## Idée

Afin de sécuriser l'Infrastructure, il faut mettre en place une stratégie de backup. 

Pour ce faire il a fallu calculer les éléments suivants 

## RPO - **Recovery Point Objective**

Le RPO permet de quantifier dans le temps la quantité de données étant perdues. Ici j’accepte une perte de 24h.
Ce qui signifie que les backups sont faites chaque jour, et que si un problème arrive, je perds donc le travail de la journée.

## RTO - **Recovery Time Objective**

Le RTO permet quant à lui de définir combien de temps un processus peut être indisponible, et combien de temps il doit être pris pour le restaurer.
Je ne peux ici pas le définir.

## Règle 3-2-1

### Machines virtuelles

La règle 3-2-1 permet d'obtenir une bonne résilience des backups, permettant de tolérer la compromission d'une ou plusieurs backups.

3: 3 copies de la donnée.
2: Sur 2 supports (type de média) différents.
1: Dont 1 hors site.

Le 3 est respecté car la working copy est la VM sur le Proxmox lui-même, la 2e étant sur un Proxmox Backup Server Local et la dernière sur un Proxmox Backup Server distant.

Le 2 est respecté car:
- Support 1 : NVMe (Proxmox nodes)
- Support 2 : SSD (PBS local)
- Support 3 : Volume Hetzner (cloud)

Le 1 est respecté car le 2e Proxmox Backup Server est un VPS Hetzner.

Pour les VM avec une base de donnée, le cron de backup inclus un dumping de la base de donnée pré-backup afin qu'elle puisse être restaurée.

```txt
AI GENERATED

┌─────────────────────────────────────────────────────────────┐
│                        DONNÉES                              │
├─────────────────────────────────────────────────────────────┤
│  VM/LXC sans DB        │  VM avec DB (Netbox, Zitadel)      │
│  (DNS, Traefik, PKI)   │                                    │
└──────────┬─────────────┴──────────────┬─────────────────────┘
           │                            │
           │ vzdump                     │ 1. pg_dump (cron 23h00)
           │ (snapshot)                 │    → /var/backups/pg/
           │                            │ 2. vzdump (minuit)
           │                            │    inclut le dump
           ▼                            ▼
┌─────────────────────────────────────────────────────────────┐
│              PBS LOCAL (HP EliteDesk)                       │
│              Rétention : 7 daily, 4 weekly                  │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           │ Sync PBS natif (1×/jour, 02h00)
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              PBS DISTANT (VPS Hetzner)                      │
│              Rétention : 7 daily, 4 weekly, 3 monthly       │
└─────────────────────────────────────────────────────────────┘
```

### Kubernetes

[EN RÉFLEXION] (Velero + TrueNAS + Bucket S3 Hetzner)

## Coûts estimés Hetzner

À calculer.


## Questions ouvertes

- Stratégie backup applications stateful Kubernetes (Nextcloud, bases de données dans Kube)
- RTO à mesurer après premier test de restore
- Dimensionnement exact stockage Hetzner après mise en place PBS local