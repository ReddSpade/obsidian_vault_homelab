Stocker l'état dans un bucket S3 dans le NAS -> Garage, gitlab gère l'état mais le stocke de manière distante.

- **GitLab** → locking + versioning + API
- **Garage** → stockage physique hors Proxmox
- **GitLab** → chiffrement server-side
- **OpenBao** → chiffrement client-side via OpenTofu
- **PBS** → backup de GitLab qui backup le state