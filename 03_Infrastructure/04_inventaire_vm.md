
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
## VM :

**200**: vir-srv-prd-deb13-glab01
Utilité principale: SCM (Gitlab)
- Gestion de versioning

**201**: vm-iam-prd-deb13-01
Utilité principale: IdP + VPN (Zitadel + Netbird)
- Accès distant géré par IdP
- Gestion des droits et accès IAM
- Dockerisé

**202**: vm-edge-prd-deb13-01
Utilité principale: Reverse Proxy Gateway (Traefik)
- Hors hostname, `*.int.lavaduck.net` redirigé vers l'IP Traefik, qui gère ensuite via règle dynamique les redirections
- 2 TLS pour challenge DNS-01: Step-CA pour services Internes, cloudlfare pour externe.
- SSL Terminaison
- Dockerisé

## Templates

**8000**: lxc-tpl-deb13
Template "ready" (clé ssh adm, paquets à jours, sudoer adm rajouté) basée sur lxc **100** avant son installation DNS.

**9000**: vm-srv-tpl-deb12
Template oudated de Debian 12, vieux projet effectué avec terraform.
