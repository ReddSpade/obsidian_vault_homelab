## Idée

Le but est de mettre en place un cluster Kubernetes de management.
Ce cluster comportera:

- Cluster API afin de provisionner d'autres clusters Kubernetes sur le cluster Proxmox.
- Crossplane afin de provisionner en GitOps les machines virtuelles dans Proxmox et autres, permettant de se débarasser de manière effective de Terraform et d'avoir de l'auto rémédiation.

Niveau hardware, ce dernier devrait se composer de 3 PC, la solution la plus économe en électricité et prix identifié serait:

- 3 Raspberry Pi 5 avec 8Go de RAM.
- 3 modules HAT.
- 3 SSD NVMe (récup).

Cela permettrait d'avoir 3 Control Plane faisant aussi du Worker.