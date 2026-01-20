
Ce runbook doit être utilisé lors d'un clonage manuel de VM.

1. Faire un full clone
2. Éteindre la VM source après clonage (Conflit d'ip avec systemd-networkd)
3. Exécuter la commande suivante `sudo hostnamectl set-hostname <new-hostname>`
4. Modifier l'IP via `sudo vim /etc/systemd/network/<nic_name>.network`
5. Changer le `machine-id` de la VM:
```bash
rm -f /etc/machine-id /var/lib/dbus/machine-id
systemd-machine-id-setup
reboot
```

- [ ] Automatiser via Cloud-Init 