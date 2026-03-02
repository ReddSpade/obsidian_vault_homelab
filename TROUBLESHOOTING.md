# Troubleshooting: Cloud-Init et Packer sur Proxmox

## Problème Initial

Packer ne pouvait pas se connecter en SSH aux VMs clonées depuis une template Debian. Le build échouait systématiquement avec un timeout SSH.

## Environnement

- **Proxmox VE** : phy-pve-01w
- **Image source** : Debian 13 (Trixie) cloud image
- **Packer plugin** : proxmox-clone v1.2.3
- **Template ID** : 9000
- **VM de build** : 9001

---

## Diagnostic

### Étape 1 : Vérification de la configuration Packer

Configuration initiale dans `build.pkr.hcl` :
```hcl
cloud_init              = true
cloud_init_storage_pool = "local"
cloud_init_disk_type    = "ide"
ssh_username            = "debian"
```

**Observation** : Packer génère bien une clé SSH éphémère mais n'arrive pas à se connecter.

### Étape 2 : Vérification de la template 9000

```bash
qm config 9000
```

```
agent: 1
bios: ovmf
ide2: local:9000/vm-9000-cloudinit.qcow2,media=cdrom
scsi0: vm:base-9000-disk-1,discard=on,iothread=1,size=3G,ssd=1
```

**Observation** : Le disque cloud-init est sur `ide2`.

### Étape 3 : Test de clonage manuel

```bash
qm clone 9000 9999 --name test-cloudinit --full true
qm set 9999 --ciuser debian --sshkeys /tmp/test_key.pub --ipconfig0 ip=192.168.1.230/24,gw=192.168.1.254
qm start 9999
```

**Résultat** : La VM démarre mais le ping échoue. Pas de réseau.

### Étape 4 : Vérification de cloud-init

```bash
qm cloudinit dump 9999 user
```

```yaml
#cloud-config
hostname: test-cloudinit
user: debian
ssh_authorized_keys:
  - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5...
```

**Observation** : La configuration cloud-init semble correcte.

### Étape 5 : Analyse du réseau cloud-init

```bash
qm cloudinit dump 9999 network
```

```yaml
version: 1
config:
    - type: physical
      name: eth0
      mac_address: 'bc:24:11:a9:f2:af'
      subnets:
      - type: static
        address: '192.168.1.230'
        netmask: '255.255.255.0'
        gateway: '192.168.1.254'
```

**Observation** : Proxmox génère une config pour `eth0`, mais les images Debian modernes utilisent des noms d'interface prédictibles (`ens18`, `enp0s18`).

### Étape 6 : Tentative de forcer eth0

Modification du grub dans l'image pour ajouter `net.ifnames=0 biosdevname=0` :

```bash
sed -i 's|consoleblank=0|consoleblank=0 net.ifnames=0 biosdevname=0|g' /boot/grub/grub.cfg
```

**Résultat** : Le réseau ne fonctionne toujours pas.

### Étape 7 : Vérification des logs cloud-init

Montage du disque VM pour inspecter les logs :

```bash
qemu-nbd --connect=/dev/nbd0 /dev/vm/vm-9999-disk-1 --format=raw
mount /dev/nbd0p1 /tmp/vm-mount
cat /tmp/vm-mount/var/log/cloud-init.log
```

**Résultat** : Aucun log cloud-init. Cloud-init ne s'exécute pas du tout.

### Étape 8 : Vérification des services systemd

```bash
journalctl --root=/tmp/vm-mount -u ssh.service
```

```
sshd: no hostkeys available -- exiting.
```

**Observation** : SSH échoue car pas de clés host. Ces clés sont normalement générées par cloud-init.

### Étape 9 : Vérification du journal systemd général

```bash
journalctl --root=/tmp/vm-mount | grep -i cloud
```

**Résultat** : Aucune mention de cloud-init dans les logs. Le service ne démarre pas.

### Étape 10 : Investigation du datasource

Cloud-init utilise `ds-identify` pour détecter le datasource. Création d'un script de debug :

```bash
#!/bin/bash
exec > /var/log/debug-cloud-init.log 2>&1
echo '=== Checking block devices ==='
lsblk
echo '=== Checking for cidata ==='
blkid | grep -i cidata
echo '=== Checking sr0/cdrom ==='
ls -la /dev/sr0 /dev/cdrom
```

**Résultat critique** :
```
ls: cannot access '/dev/sr0': No such file or directory
ls: cannot access '/dev/cdrom': No such file or directory
```

**Le disque cloud-init (ide2) n'est pas visible dans la VM !**

---

## Cause Racine

L'image **Debian 13 genericcloud** n'inclut pas les modules kernel IDE. Le disque cloud-init configuré sur `ide2` est donc invisible pour le système d'exploitation de la VM.

Sans accès au disque cloud-init :
1. `ds-identify` ne trouve pas de datasource
2. Cloud-init est désactivé par le generator systemd
3. Pas de configuration réseau
4. Pas de clés SSH générées
5. Packer ne peut pas se connecter

---

## Solution

### 1. Changer le type de disque cloud-init

**Avant** (ne fonctionne pas) :
```python
# Script Python
ide2='local:cloudinit'
```

```hcl
# Packer
cloud_init_disk_type = "ide"
```

**Après** (fonctionne) :
```python
# Script Python
sata0='local:cloudinit,media=cdrom'
```

```hcl
# Packer
cloud_init_disk_type = "sata"
```

### 2. Utiliser l'image genericcloud (recommandé)

L'image `debian-13-genericcloud-amd64.qcow2` est optimisée pour les environnements cloud, contrairement à `debian-13-generic-amd64.qcow2`.

```python
servers = [
    {
        "filename": "debian-13-genericcloud-amd64.qcow2",
        "url": "https://cloud.debian.org/images/cloud/trixie/latest/debian-13-genericcloud-amd64.qcow2",
        "vmid": 9000
    },
]
```

### 3. Spécifier l'IP SSH dans Packer

Packer ne peut pas obtenir l'IP automatiquement sans qemu-guest-agent. Il faut spécifier `ssh_host` :

```hcl
ssh_host    = "192.168.1.229"
ssh_timeout = "10m"
```

---

## Configuration Finale

### `scripts/proxmox.py`

```python
create_vm = proxmox.nodes(node).qemu.post(
    vmid=server['vmid'],
    agent='enabled=1',
    net0='virtio,bridge=vmbr0',
    ostype='l26',
    keyboard='en-us',
    cpu='cputype=host',
    cores=2,
    memory=2048,
    serial0='socket',
    vga='serial0',
    bios='ovmf',
    efidisk0='efitype=4m,pre-enrolled-keys=0,file=vm:0',
    scsihw='virtio-scsi-single',
    scsi0=f'file=vm:0,import-from={vol_name},discard=on,iothread=1,ssd=1',
    boot='order=scsi0',
    # IMPORTANT: Utiliser SATA au lieu de IDE
    sata0='local:cloudinit,media=cdrom'
)
```

### `packer/build.pkr.hcl`

```hcl
source "proxmox-clone" "vm-template" {
  proxmox_url              = var.proxmox_api_url
  username                 = var.proxmox_api_token_id
  token                    = var.proxmox_api_token_secret
  node                     = var.proxmox_node
  insecure_skip_tls_verify = false

  clone_vm_id             = 9000
  cloud_init              = true
  cloud_init_storage_pool = "local"
  cloud_init_disk_type    = "sata"  # IMPORTANT: pas "ide"
  full_clone              = true
  scsi_controller         = "virtio-scsi-single"

  ssh_username = "debian"
  ssh_timeout  = "10m"
  ssh_host     = "192.168.1.229"  # IP statique configurée

  vm_id   = 9001
  vm_name = "vir-tpl-deb13-${local.timestamp}"

  network_adapters {
    model  = "virtio"
    bridge = "vmbr0"
  }

  ipconfig {
    ip      = "192.168.1.229/24"
    gateway = "192.168.1.254"
  }
}
```

---

## Vérification

Après les corrections, le build Packer réussit :

```
==> vm-template.proxmox-clone.vm-template: Creating VM
==> vm-template.proxmox-clone.vm-template: Starting VM
==> vm-template.proxmox-clone.vm-template: Using SSH communicator to connect: 192.168.1.229
==> vm-template.proxmox-clone.vm-template: Connected to SSH!
==> vm-template.proxmox-clone.vm-template: Converting VM to template
Build 'vm-template.proxmox-clone.vm-template' finished after 1 minute 837 milliseconds.
```

---

## Points Clés

| Problème | Cause | Solution |
|----------|-------|----------|
| Cloud-init ne s'exécute pas | Disque IDE invisible | Utiliser SATA |
| Réseau non configuré | Cloud-init désactivé | Corriger le disque cloud-init |
| SSH timeout | Packer ne connaît pas l'IP | Ajouter `ssh_host` |
| Image incompatible | generic vs genericcloud | Utiliser genericcloud |

---

## Commandes Utiles pour Debug

```bash
# Vérifier la config cloud-init d'une VM
qm cloudinit dump <vmid> user
qm cloudinit dump <vmid> network

# Monter un disque VM pour inspection
qemu-nbd --connect=/dev/nbd0 /dev/vm/vm-<vmid>-disk-1 --format=raw
mount /dev/nbd0p1 /tmp/vm-mount

# Vérifier les logs cloud-init
cat /tmp/vm-mount/var/log/cloud-init.log
cat /tmp/vm-mount/var/log/cloud-init-output.log

# Vérifier le journal systemd
journalctl --root=/tmp/vm-mount --no-pager | grep cloud

# Vérifier les périphériques block dans la VM (via script)
lsblk
blkid | grep cidata

# Nettoyer après inspection
umount /tmp/vm-mount
qemu-nbd --disconnect /dev/nbd0
```
