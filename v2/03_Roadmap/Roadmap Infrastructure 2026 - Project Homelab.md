## Vision Stratégique

À disposition: 2 domaines:

`lavaduck.net` -> Domaine publique sur Cloudflare, il exposera:
- Worload applicatif Kube
- VPN
- IdP

`int.lavaduck.net` -> Sous domaine privé dans le homelab, il exposera:
- Composants interne

---

## Phase 1: Fondation réseau

**Objectif final**: Avoir un réseau propre, séparé, maîtrisé et documenté sur l'ensemble de la maison.
### 1.0 Réseau Théorique

**Objectif:** Cette partie consiste à rédiger un plan clair servant de présentation lors de la mise en place des équipements réseau.

**Critères de complétion:** 
- Livrables vérifiables [[01_VLANS]], [[02_Matrice_de_flux_inter-VLAN]], [[03_IPAM]]. 

**Dépendance:** Aucune.
### 1.1 Installation des équipements réseaux

**Objectif:** Cette partie contient la listes des équipements réseau à installer.

**Critère de complétion:**
- Chacun des équipements est atteignable sur le réseau.

**Dépendance:** Aucune.

### 1.2 Paramétrage des équipements réseau

**Objectif:** Après installation, le paramétrage des équipements réseaux est nécessaire au bon fonctionnement de l'infrastructure.

**Critère de complétion:**
- Un client sur VLAN 50 (Users) peut résoudre `dns.int.lavaduck.net`.
- Un client sur VLAN 70 (Guest) ne peut PAS ping le VLAN 20 (Infra).
- Les règles firewall bloquent par défaut et loguent les refus.
- Les règles NAT redirigent le trafic 80/443 vers l'IP du Reverse Proxy.
- Les règles NAT redirigent le trafic (port à définir) vers l'IP de la VM VPN.

**Dépendance:** 
Étape achevées:
- [[#1.0 Réseau Théorique]] 
- [[#1.1 Installation des équipements réseaux]]

## Phase 2: Infrastructure critique

**Objectif final:** Définir et mettre en production une infrastructure critique, elle doit toujours être up, être en HA ou avoir un failsafe.

### 2.0 DNS + DHCP (Technitium)

**Objectif:** Avoir un DNS contenant une zone d'Autorité, et qui fasse Récursivité et Filtrage.

**Type de virtualisation:** LXC (Pas de db).

**Critères de complétion:** 
- Zone `int.lavaduck.net` atteignable par tout le réseau.
- Baux DHCP et réservations définies.
- Requêtes de pubs et télémétries bloquées.

**Dépendance:** Hyperviseur Proxmox en ligne.

### 2.1 PKI (Step-CA)

**Objectif:** Disposer d'une entité de certification permettant de sécuriser les connexions internes.

**Type de virtualisation:** LXC (Pas de db).

**Critères de complétion:** 
- Root CA et Intermediate CA définies.
- Serveur ACME disponible.
- Root CA stockée offline sur support chiffré, hors du LXC Step-CA.

**Dépendance:** Hyperviseur Proxmox en ligne.

### 2.2 Reverse Proxy (Traefik)

**Objectif:** Disposer d'un Reverese Proxy redirigeant les requêtes vers les bonnes IP, interne comme externe.

**Type de virtualisation:** Machine Virtuelle (Nécessite Docker).

**Critères de complétion:** 
- TLS Resolver Cloudflare paramétré via challenge DNS-01.
- TLS Resolver Interne Step-CA paramétré via challenge DNS-01.
- Fichiers dynamique défini.

**Dépendance:** 
Étapes achevées:
- Hyperviseur Proxmox en ligne.
- [[#2.0 DNS + DHCP (Technitium)]]
- [[#2.1 PKI (Step-CA)]] 

### 2.3 IdP (Zitadel)

**Objectif:** Avoir un IdP sécurisant les connexion via protocole OIDC quand c'est possible.

**Type de virtualisation:** Machine Virtuelle (Nécessite Docker + Db).

**Critères de complétion:** 
- Groupes créés afin de respecter le modèle RBAC.
- L'endpoint `.well-known/openid-configuration` répond.

**Dépendance:** Hyperviseur Proxmox en ligne.

### 2.4 VPN (Netbird)

**Objectif:** Mettre en place une solution de VPN Mesh flexible permettant différent type d'accès externe.

**Type de virtualisation:** Machine Virtuelle (Nécessite Docker).

**Critères de complétion:** 
- Mode "Mesh" testé permettant une connexion entre plusieurs PC.
- Mode point à site testé permettant une connexion aux ressources du réseau via NAT.
- Nom DNS défini afin de résoudre les FQDN distants.

**Dépendance:** 
Étapes achevées:
- Hyperviseur Proxmox en ligne.
- [[#2.0 DNS + DHCP (Technitium)]]
- [[#2.3 IdP (Zitadel)]]

## Phase 3: Infrastructure non critique

**Objectif final:** La phase 3 consiste à la mise en place des éléments du cluster qui ne sont pas indispensable à son démarrage. Comme des outils d'inventoring et SCM.
### 3.0 Gitlab

**Objectif:** Avoir en place un Gitlab privé qui contiendra la majeure partie de l'IaC de l'infrastructure.

**Type de virtualisation:** Machine Virtuelle (due à la DB).

**Critères de complétion:**
- Gitlab exposé en interne et atteignable à `gitlab.int.lavaduck.net`.
- Accès par OIDC configuré.
- Utilisateur et clé SSH créés.
- Groupes créés.

**Dépendance:** 
- Hyperviseur Proxmox en ligne.
- Zitadel en ligne.
- Traefik en ligne.

### 3.1 Sémaphore [EN RÉFLEXION]

**Objectif:** Disposer d'une machine avec UI et API dédiée à l'orchestration des outils Terraform, Ansible et Packer.

**Type de virtualisation:** Machine Virtuelle (due à la DB).

**Critères de complétion:** 
- Sémaphore atteignable sur `semaphore.int.lavaduck.net`.
- Accès par OIDC configuré.
- La machine peut communiquer avec les clusters Proxmox, le DNS, et autres outils.
- Premier flow de test créé et fonctionnel.
- Technitium (résolution DNS côté client)

**Dépendance:**
- Hyperviseur Proxmox en ligne.
- Gitlab en ligne.
- Zitadel en ligne.
- Traefik en ligne.
- Technitium (résolution DNS côté client)

### 3.2 Netbox

**Objectif:** Avoir une inventorisation complète du parc informatique de la maison, avec un supplément sur le référencement de l'IPAM comme seule source de vérité.

**Type de virtualisation:** Machine Virtuelle (due à la PostgreSQL).

**Critères de complétion:** 
- [EN RÉFLEXION, SOFTWARE INCONNU POUR LE MOMENT]

**Dépendance:** 
- Hypervisuer Proxmox en ligne.
- Zitadel en ligne.
- Traefik en ligne.
- Technitium (résolution DNS côté client)

## Phase 4: Automatisation et IaC

**Objectif final:** Avoir une infrastructure immuable et automatisée, gérée par code conformément aux principes GitOps.
### 4.0 Packer

**Objectif:** Avoir un builder de Golden Images pour les machines virtuelles, dans l'idéal prêt pour de l'OS Wide.

**Critères de complétion:**
- Peut créer une template Debian 13 déployable.
- Peut facilement ajouter une autre source d'OS en renseignant un autre fichier de vars.

**Dépendance:** 
- Gitlab en ligne.
### 4.1 Cloud-Init

**Objectif:** Injecter les informations lors du déploiement Terraform/OpenTofu (IP, Hostname, Clé SSH, Users, compte de service, ...)

**Critères de complétion:**
- [EN RÉFLEXION]

**Dépendance:** 
- Gitlab en ligne.
### 4.2 Terraform/OpenTofu

**Objectif:** Déployer des VM et LXC prod ready à partir des templates Packer généré, le but définitif est de moins possible toucher à l'UI Proxmox.

**Critères de complétion:**
- VM déployée et prête sans intervention manuelle sur l'UI.
- Template de déploiement pour Talos Linux afin de préparer la phase 6 Kubernetes.
- LXC déployé et prêt sans intervention manuelle sur l'UI.

**Dépendance:** 
- Gitlab en ligne.
### 4.3 Ansible

**Objectif:** Préparer des playbooks divers permettant de gérer la configuration des machines (mise à jour, installation de paquets, ...)

**Critères de complétion:**
- Playbook de mise à jour complet pour Debian.
- Playbook d'installation de paquets.

**Dépendance:** 
- Gitlab.

## Phase 5: Mise en HA du Cluster Proxmox + Proxmox Backup Server 

## Phase 6: Kubernetes

