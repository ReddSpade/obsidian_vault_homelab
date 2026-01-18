## Idée


Afin de réduire la complexité de l'infrastructure niveau management. Un point critique a été identifié:

Les switchs achetés sont des Whitebox OEM, ce qui cause des problèmes de sécurité de de gestion (différentes interfaces).

Afin de régler le problème, il faut unifier les switchs via une seule marque.

Deux marques ont été identifiées:

MikroTik et Ubiquiti.
Ce choix a été fait car il y'a déjà un équipement UniFi à la maison, et que MikroTik est très réputé et accessible en homelab (que ce soit en terme d'encombrement et de prix).

## Versus

|          | Sécurité     | Prix       | Réputation       | Gestion                        | Rackable (futur rack)      |
| -------- | ------------ | ---------- | ---------------- | ------------------------------ | -------------------------- |
| MikroTik | A rechercher | Moins cher | Bonne réputation | Gestion via serveur Dude       | Oui pour les modèles visés |
| Ubiquiti | Réputé       | Plus cher  | Bonne réputation | Une seule interface (Unifi OS) | Non nativement             |



## Modèles identifiés

### Remplaçant keepLiNK

Origine:
keepLiNK 4x 2.5GbE ports + 2 SFP+, L2, 62.99€.

| Nom           | Spécificités         | Marque   | Prix    | Site        | Commentaire                                     |
| ------------- | -------------------- | -------- | ------- | ----------- | ----------------------------------------------- |
| CRS304-4XG-IN | 4x10GbE + 1 port PoE | MikroTik | 189.36€ | Amazon (fr) | Prise fibre + 2 alimentations (redondance élec) |
| USW-Flex-XG   | 4x10GbE + 1 port PoE | Ubiquiti | 324€    | UniFi       | Prise USB-C, géré par l'interface Unifi OS.     |

## Remplaçant MokerLink

Origine:
MokerLink 8x 2.5GbE ports + 1 SFP+, L2, 72.64€.

| Nom             | Spécificités                              | Marque   | Prix     | Site        | Commentaire                                                     |
| --------------- | ----------------------------------------- | -------- | -------- | ----------- | --------------------------------------------------------------- |
| CRS310-8G+2S+IN | 8x2.5GbE + 2 port SFP+                    | MikroTik | 203.56€  | Amazon (fr) | Port USB.                                                       |
| USW-Flex-2.5G-B | 8x2.5GbE + 1 port 10GbE PoE + 1 port SFP+ | Ubiquiti | 214.80 € | UniFi       | Port SFP+ et 10GbE en combo. <br>Géré par l'interface Unifi OS. |