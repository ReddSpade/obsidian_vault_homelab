## Provider

Utilisation du Provider "bpg/proxmox" -> plus complet que Telmate (Permet de gérer toute l'API et non seulement les VM)

## Resources

`proxmox_virtual_environment_vm` ---> Ressource legacy.

Ressource utilisée car la nouvelle `cloned_vm` ne permet pas de charger un Userdata cloud-init de quelconque manière de ce soit pour l'instant.