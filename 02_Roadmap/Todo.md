## Phase 1: Fondation réseau

### 1.0 Réseau Théorique

- [x] Plan d'adressage documenté.
- [x] VLANS.
- [x] Flux Inter-VLAN.
- [ ] [EN RÉFLEXION] Règles pare feu.

### 1.1 Installation des équipements réseaux 

- [ ] Routeur Protectli OPNsense.
- [ ] Switch 1GbE Netgear.
- [ ] Point d'accès Ubiquiti.

### 1.2 Paramétrage des équipements réseaux 

#### OPNsense

- [ ] Définition de l'IP.
- [ ] Définition des VLANS et leur communications, documentations visible ici [[01_VLANS]], [[02_Matrice_de_flux_inter-VLAN]].
- [ ] Règles de pare-feu IPv4.
- [ ] Règles de redirections IPv4.
- [ ] [EN RÉFLEXION] paramétrage supplémentaire.

#### Switchs et Point d'accès

- [ ] Définition de l'IP fixe.
- [ ] Tagging de VLANS.

## Phase 2: Infrastructure critique

### 2.0 DNS + DHCP (Technitium)

#### DNS

- [x] Création de la Zone `int.lavaduck.net`.
- [x] Installation d'SQLite pour les logs.
- [x] Mise en place des liste de bloqueurs de pubs les plus connues (Multi PRO ++).
- [ ] [EN RÉFLEXION] création de la zone upstream/stub `lavaduck.net` ?

#### DHCP

- [ ] Définition des réservation des baux statiques.
- [ ] Création de la zone dynamique pour le VLAN Guest.
- [ ] Distribution de la Gateway et DNS dans le DHCP.

### 2.1 PKI (Step-CA)

- [x] Création de l'autorité de certification.
- [ ] Sécurisation du Root CA key dans une clé USB chiffrée.
- [x] Activation du serveur ACME.
- [x] Définition d'une entrée A spécifique dans Technitium DNS pour pointer sur Step-CA.

### 2.2 ~~Reverse Proxy (Traefik)~~

- [x] Déploiement de Traefik sur Docker.
- [x] Mise en place du Resolver DNS-01 Cloudflare.
- [x] Création d'une clé API sur Technitium DNS pour les entrées TXT.
- [x] Mise en place du Resolver DNS-01 Step-CA.
- [x] Création des fichiers dynamiques.

### 2.3 ~~IdP (Zitadel)~~

- [x] Déploiement de Zitadel sur Docker.
- [x] Création des Projets.
- [x] Mise en place d'un compte Resend pour les invitations mails via Cloudflare.
- [x] Invitation des utilisateurs.

### 2.4 ~~VPN (Netbird)~~

- [x] Déploiement de Netbird sur Docker.
- [x] Ajout de la connexion OIDC avec Zitadel.
- [x] Acivation du champ "groups" du JWT Zitadel.
- [x] Création d'un Network avec adresse du réseau (.0) pour l'accès admin.
- [x] Création d'un Network avec accès aux PVE (x.x.x.x/32) pour accès homelab pour projet groupe.
- [x] Mise en place du Nameserver redirigeant au Serveur DNS.
- [x] Invitation des utilisateurs.

### 2.5 Secret Manager (OpenBao)

- [x] Déploiement d'OpenBao.
- [x] Ajout de la connexion OIDC avec Zitadel
- [ ] [EN RÉFLEXION]
## Phase 3: Infrastructure non critique
### 3.0 ~~Gitlab~~

- [x] Déploiement de Gitlab Omnibus.
- [x] Optimisation de la consommation des ressources de Gitlab dans le fichier `/etc/gitlab/gitlab.rb`.
- [x] Création des groupes, sous groupes et projets.
- [x] Setup de la clé SSH.

### 3.1 ~~Gitlab Runner~~

- [x] Installation du dépôt de gitlab runner.
- [x] Installation de Docker.
- [x] Connexion à Gitlab.
### 3.2 Sémaphore

- [ ] [EN RÉFLEXION, SOFTWARE INCONNU]

### 3.3 Netbox

- [x] Déploiement de Netbox.
- [ ] [EN RÉFLEXION, SOFTWARE INCONNU]

### 3.3 ~~Unifi Controller~~

- [x] Déploiement du contrôleur.
- [x] Initialisation du point d'accès.
## Phase 4: Automatisation et IaC

### 4.0 Packer
 
- [ ] Mise en place d'un projet de templatisation LXC.
- [ ] Mise en place d'un projet de templatisation VM OS wide.

### 4.1 Cloud-Init

- [ ] [EN RÉFLEXION, SOFTWARE INCONNU]

### 4.2 Terraform/OpenTofu

- [ ] Mise en place d'un projet de déploiement de template LXC.
- [ ] Mise en place d'un projet de déploiement de template VM OS wide.

### 4.3 Ansible

- [ ] [EN RÉFLEXION SUR LES PLAYBOOKS À CRÉER]

## Phase 5: Mise en HA du Cluster Proxmox + Proxmox Backup Server 

## Phase 6: Kubernetes
