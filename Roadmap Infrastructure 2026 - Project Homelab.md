## Vision Stratégique

Déploiement d'une infrastructure "On-Premise" résiliente, segmentée et automatisée. L'architecture repose sur un principe de **Ring 0** (Services Critiques) découplé du **Runtime** (Kubernetes) pour éliminer les dépendances circulaires lors d'un Cold Start.

À disposition: 2 domaines:

`lavaduck.net` -> Domaine publique sur Cloudflare, il exposera:
- Worload applicatif Kube
- VPN
- IdP

`int.lavaduck.net` -> Sous domaine privé dans le homelab, il exposera:
- Composants interne

---

## Phase 1 : Network Foundation & Segmentation

_Objectif : Établir une couche réseau robuste et isolée avant tout déploiement de services._

### 1.1 IPAM & Adressage

- Définition des plages CIDR (RFC 1918) pour éviter les collusions.

### 1.2 Segmentation (VLANs)

- Configuration Switch/Routeur (Layer 2/3).
    
- Mise en place des ACLs/Firewalling inter-VLAN (Refus par défaut).
    
- Isolation stricte des flux IoT et Guest.
    

### 1.3 Core Services (Contenu dans Proxmox)
- **DNS Infrastructure :** (Technitium) Résolution autoritaire pour le domaine `.int.lavaduck.net`. Doit être up avant toute VM.
    
- **DHCP :** (Technitium) Baux statiques pour l'infrastructure critique.
        

---

## Phase 2 : Compute Layer

_Objectif : Fournir une couche de virtualisation standardisée et "Cloud-Ready"._

### 2.1 Stockage Centralisé (NAS)

- **TrueNAS Scale :**
    
    - **Rôle :** Stockage "Cold" (Backups, ISOs, Archives) et "Warm" (NFS partagé pour les VMs Proxmox si besoin).
        
    - **Réseau :** Doit impérativement être connecté en **2.5GbE** (ou agrégation de liens) sur le switch MoerLink pour ne pas étrangler les backups du cluster.
        
    - **Services :** Serveur NFS (pour Proxmox ISOs/Backups) et S3 (via Garage/SeaweedFS intégré, utile pour Terraform State ou backups database).
### 2.2 Hyperviseur

- **Proxmox VE :** Installation sur ZFS (RAIDZ/Mirror selon stockage).
    
- **Networking :** Configuration Linux Bridge (ou OVS) mappée sur les VLANs de la Phase 1.
-  **Proxmox Backup Server**: Implémentation du Proxmox Backup Server
    

### 2.2 Golden Images LXC + VM

- **OS Standard :** Debian 13 (Trixie).
    
- **Templating :** Création d'une image Cloud-Init générique (Nettoyage machine-id, clés SSH injectées, qemu-guest-agent).
    
- _Politique :_ Aucune installation manuelle via ISO. Instanciation uniquement par clonage/IaC.
    

---

## Phase 3 : Ring 0 (Critical Infrastructure)

_Objectif : Déployer les services de sécurité et d'identité nécessaires au bootstrapping du cluster._ _Contrainte : Ces services tournent en VM ou Docker Compose isolé (Hors-Kubernetes)._

### 3.1 Identité & Accès

- **IdP :** (Zitadel) Fournisseur d'identité central (OIDC/SAML). Gestion du SSO pour Proxmox, ArgoCD et K8s API.
- **VPN**: (Netbird) Accès externe mesh géré par IdP.
    
### 3.2 Gestion des Secrets

- **Gestion secret :** (OpenBao) Coffre-fort centralisé.
    
- **PKI :** (Step-CA) Gestion des certificats internes et autorité de certification.
    
### 3.3 Inventoring 

- **DCIM+IPAM**: (Netbox): Inventoring de l'infrastructure complète
### 3.3 Observabilité "Day 0"

- Stack de logs minimale (ex: Loki/Promtail isolé) pour diagnostiquer les échecs de démarrage du cluster K8s.
    

---

## Phase 4 : Automation Factory

_Objectif : Industrialiser le déploiement et la configuration._

### 4.1 Infrastructure as Code (IaC)

- **OpenTofu / Terraform :** Provisioning des ressources Compute (VMs, LXC) sur Proxmox.
    
- State file stocké sur backend sécurisé (S3/Consul/Git).
    

### 4.2 Configuration Management

- **Ansible :** Configuration post-provisioning des OS (Hardening, Docker, Kubelet, Join Token).
    
- **Ansible Semaphore :** UI Web pour l'orchestration des playbooks (Maintenance, Rolling Updates).
    

---

## Phase 5 : Runtime (Kubernetes Cluster)

_Objectif : Orchestration des charges de travail applicatives._

### 5.1 Bootstrap Cluster

- Déploiement des nœuds Control Plane et Workers.
    
- **CNI (Network) :** Cilium (eBPF) pour la performance et la sécurité réseau (Network Policies).
    
- **CSI (Storage) :** Longhorn ou Rook-Ceph pour le stockage persistant distribué.
    

### 5.2 GitOps Delivery

- **ArgoCD :** Déploiement déclaratif des applications depuis Git.
    
- Structure "App of Apps".
    

### 5.3 Intégration Ring 0

- **OIDC :** Connexion de l'API Server K8s à Zitadel (Rôles RBAC mappés sur Groupes OIDC).
    
- **External Secrets Operator :** Synchronisation des secrets applicatifs depuis OpenBao vers les Secret K8s.