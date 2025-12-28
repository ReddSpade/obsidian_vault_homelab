```txt
Nomenclature du Lab :

[VIRT]-[FUNC]-[ENV]-[OS]-[ID]

Type :
Type d’Hardware de la machine
    • phy : Machine physique
    • vir : Machine virtuelle
    • ctr : lxc
    • vps : Machine cloud

Fonction :
Fonction de la machine
    • k8s : Kubernetes
    • cli : clients
    • vlt : vault
    • pve : proxmox
Ainsi de suite

Env :
Environnement de la machine
    • prd : Production
    • dev : Developpment (avant prod)
    • tst : VM de tests
    • bst : Bastion

OS + Ver :
Type d'OS et version
    • deb12: Debian 12
    • ws2025: Windows Server 2025

ID :
Informations optionelles + numéro :
    • cp01 : Control Plane 01
    • wk02 : Worker Node 02
    • nix01 : NixOS 01


ID des VM Proxmox :
1xx - Containers 
2xx - Linux VM
3xx - Windows VM 
4xx - Tests 
5xx - Fonctions spécfiques 
9xx - Templates
```