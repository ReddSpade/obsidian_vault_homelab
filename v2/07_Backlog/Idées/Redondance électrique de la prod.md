
## Idée

Afin d'éviter des corruptions sur le NAS et sur les clusters Proxmxo en cas de coupure de courant, il faut mettre en place un moyen de shutdown gracefully l'infra.

Pour ce faire, il faut un onduleur capable d'alimenter l’entièreté du cluster pour un temps donné.

Pour ne pas avoir de coupure, il faudra opter pour un onduleur de type online

Il faut donc calculer la quantité de KVA (Ou watts) que l'onduleur peut fournir:
