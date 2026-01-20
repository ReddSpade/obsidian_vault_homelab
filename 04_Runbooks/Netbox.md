Ce runbook détaille l'installation de Netbox, de l'installation des services nécessaires au premier login.

## Installation 

[Lien de la documentation d'installation.](https://netbox.readthedocs.io/en/stable/installation/)

### Informations spécifiques

#### Mot de passe postgresql db

Mot de passe de la base de donnée: `CREATE USER netbox WITH PASSWORD 'password'` mot de passe disponible sur Bitwarden.

#### ALLOWED_HOSTS

Variables permettant les connexions externes de ces IPs:
`ALLOWED_HOSTS = ['netbox.int.lavaduck.net', '192.168.1.2', '192.168.1.8']` (FQDN accessible, IP de traefik, IP localhost).

#### Configuration Traefik

```yaml
---
http:
  routers:
    netbox:
      entryPoints:
        - websecure
      rule: "Host(`netbox.int.lavaduck.net`)"
      service: netbox-svc
      tls:
        certResolver: step-ca

  services:
    netbox-svc:
      loadBalancer:
        servers:
          - url: http://192.168.1.8
        passHostHeader: true
```
## Troubleshoot

### Dossier media manquant

Créer le dossier manuellement: `sudo mkdir -p /opt/netbox/netbox/media`.

### Erreur 403 CSRF

![[Pasted image 20251230162157.png]]

**Cause** : Lorsque Traefik termine le TLS et forward en HTTP vers Netbox, l'origine de la requête (HTTPS) diffère du backend (HTTP). Django rejette la requête CSRF car l'origine n'est pas dans la liste de confiance.

**Résolution** : 
1. Afin de pouvoir lister Traefik comme étant une source autorisée, rajouter dans `/opt/netbox/netbox/netbox/configuration.py`, rajouter la variable `CSRF_TRUSTED_ORIGINS = ['https://netbox.int.lavaduck.net']`.
2. Redémarrer le service: `sudo systemctl restart netbox`.