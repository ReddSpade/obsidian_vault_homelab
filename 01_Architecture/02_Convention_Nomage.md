
## Virtuel

```txt
Nomenclature du Lab :

[VIRT]-[FUNC]-[ENV]-[OS]-[ID]

Type :
Type d’Hardware de la machine
    • vm  : Machine virtuelle
    • lxc : Linux Container
    • vps : Machine cloud

Fonction :
Fonction de la machine
    • k8s : Kubernetes
    • cli : clients
    • vlt : vault
Ainsi de suite

Env :
Environnement de la machine
    • prd : Production
    • dev : Developpment
    • tst : VM de tests

OS + Ver :
Type d'OS et version
    • deb13: Debian 13
    • ws2025: Windows Server 2025

ID :
Informations optionelles + numéro :
    • 01 : Instance 1 de cet objet
    • cp01 : Control Plane 01
    • wk02 : Worker Node 02
    • nix01 : NixOS 01

ID des VM Proxmox :
1xx  - LXC 
2xx  - Linux VM
3xx  - Windows VM 
4xx  - Tests 
5xx  - Fonctions spécfiques
8xxx - Templates LXC
9xxx - Templates VM
```


## Physique

### Compute

```txt
Nomenclature du Compute divers:

[TYPE]-[FUNC]-[ID]

Type :
Type d’Hardware de la machine
    • phy : Machine physique

Fonction :
Fonction de la machine
    • pve : proxmox
	• pbs : proxmox backup server
	• nas : NAS	
Ainsi de suite

Env :
Environnement de la machine
    • prd : Production
    • dev : Developpment

ID :
Informations optionelles + numéro :
    • 01 : Instance 1 de cet objet

```
### Réseau

```txt
Nomenclature des équipements réseau:

[TYPE]-[TYPE2]-[LOC]-[ID]

Type :
Type d’Hardware de la machine
    • phy : Machine physique

Type 2 :
Type d'équipement
    • sw  : Switch
    • rtr : Router
    • ap  : Point d'accès
    • mdm : modem
Ainsi de suite

Loc :
Localisation physique de l'appareil
    • lvrm : living room (salon)
    • gtec  : gaine technique
    • ofce : office (bureau)

ID :
Informations optionelles + numéro :
    • 01 : Instance 1 de cet objet
```