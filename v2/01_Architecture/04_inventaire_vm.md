
## LXC :

**100**: lxc-dns-prd-deb13-01
Utilitée principale:  DNS (Technitium DNS) 
- DNS Autorité (int.lavaduck.net), Récursif, Filtrage.
- DHCP

**101**: lxc-pki-prd-deb13-01
Utilité principale: PKI (Step-CA)
- CA Root (Step-CA)
- CA Intérmédiaire
- ACME Server 

**102**: lxc-vlt-prd-deb13-01
Utilité principale: Vault (OpenBao)
- Gestion des secrets

**103**: lxc-edge-prd-deb13-01
Utilité principale: Reverse Proxy Gateway (Traefik)
- Hors hostname, `*.int.lavaduck.net` redirigé vers l'IP Traefik, qui gère ensuite via règle dynamique les redirections
- 2 TLS pour challenge DNS-01: Step-CA pour services Internes, cloudlfare pour externe.
- SSL Terminaison
## VM :

**200**: vm-glab-prd-deb13-01
Utilité principale: SCM (Gitlab)
- Gestion de versioning

**201**: vm-iam-prd-deb13-01
Utilité principale: IdP + VPN (Zitadel + Netbird).
- Accès distant géré par IdP
- Gestion des droits et accès IAM
- Dockerisé

**203**: vm-ntbx-prd-deb13-01
Utilité principale: DCIM + IPAM (Netbox).
- Source de vérité matérielle.
- Source de vérité réseau.

**204**: vm-grun-prd-deb13-01
Utilité principale: Gitlab Runner.
- Runner pour les CI/CD Gitlab.
- Contient Docker pour faire du DinD (Docker in Docker).

**205**: vm-ubqt-prd-deb13-01
Utilité principale: Contrôleur Ubiquiti. 
- Gestion via UI du matériel Ubiquiti dans la maison.
## Templates

**8000**: lxc-tpl-deb13
Template "ready" (clé ssh adm, paquets à jours, sudoer adm rajouté) basée sur lxc **100** avant son installation DNS.

