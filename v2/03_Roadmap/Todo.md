## Phase 1: Fondation réseau

### 1.0 Réseau Théorique

- [ ] Plan d'adressage documenté.
- [ ] VLANS.
- [ ] Flux Inter-VLAN.
- [ ] [EN RÉFLEXION] Règles pare feu.

### 1.1 Installation des équipements réseaux 

- [ ] Routeur Protectli OPNsense.
- [ ] Switch 2,5GbE MokerLink.
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

## Phase 2: Infrastructure Critique

### 2.0 DNS + DHCP (Technitium)

#### DNS

- [ ] Création de la Zone `int.lavaduck.net`.
- [ ] Installation d'SQLite pour les logs.
- [ ] Mise en place des liste de bloqueurs de pubs les plus connues (Multi PRO ++).
- [ ] [EN RÉFLEXION] création de la zone upstream/stub `lavaduck.net` ?

#### DHCP

- [ ] Définition des réservation des baux statiques.
- [ ] Création de la zone dynamique pour le VLAN Guest.
- [ ] Distribution de la Gateway et DNS dans le DHCP.

### 2.1 PKI (Step-CA)

- [ ] Création de l'autorité de certification.
- [ ] Sécurisation du Root CA key dans une clé USB chiffrée.
- [ ] Activation du serveur ACME.
- [ ] Définition d'une entrée A spécifique dans Technitium DNS pour pointer sur Step-CA.

### 2.2 Reverse Proxy (Traefik)

- [ ] Déploiement de Traefik sur Docker.
- [ ] Mise en place du Resolver DNS-01 Cloudflare.
- [ ] Création d'une clé API sur Technitium DNS pour les entrées TXT.
- [ ] Mise en place du Resolver DNS-01 Step-CA.
- [ ] Création des fichiers dynamiques.

### 2.3 IdP (Zitadel)

- [ ] Déploiement de Zitadel sur Docker.
- [ ] Création des Projets.
- [ ] Mise en place d'un compte Resend pour les invitations mails via Cloudflare.
- [ ] Invitation des utilisateurs.

### 2.4 VPN (Netbird)

- [ ] Déploiement de Netbird sur Docker.
- [ ] Ajout de la connexion OIDC avec Zitadel.
- [ ] Acivation du champ "groups" du JWT Zitadel.
- [ ] Création d'un Network avec adresse du réseau (.0) pour l'accès admin.
- [ ] Création d'un Network avec accès aux PVE (x.x.x.x/32) pour accès homelab pour projet groupe.
- [ ] Mise en place du Nameserver redirigeant au Serveur DNS.
- [ ] Invitation des utilisateurs.

## Phase 3: Infrastructure non critique
## Phase 4: Automatisation et IaC

## Phase 5: Mise en HA du Cluster Proxmox + Proxmox Backup Server 

## Phase 6: Kubernetes
