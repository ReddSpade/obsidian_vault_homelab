## Timestamp

08/01/2026

## Actions

Remplacement des 3 Dell OptiPlex Micro par des Shuttle XH510S.
Achat de 3 cartes réseau X520-DA2 (2 ports SFP+ 10 GbE).
Achat de 3 cables DAC d'1 mètre.
Transférer chaque composant des Optiplex vers les XH510S (RAM, disques, CPU).

## Justification

Afin de mettre en place un cluster CEPH viable (et non une réplication ZFS), Proxmox recommande de mettre en place des cartes réseau de 10GbE.
Les 3 OptiPlex étaient limités au niveau des cartes réseau, seulement 1 slot 1GbE, possibilité de rajouter une carte à travers le slot WLAN en 2,5GbE max.

## Étapes

Chercher un modèle compatible, celui ci devait proposer:
- Un format mini.
- CPU, RAM et Disques amovibles (non soudés).
- Un port PCIe x8 ou x16 disponible pour rajouter une carte 10Gig.
- Génération PCIe 3 ou plus.
- RAM en DDR4.
- Socket LGA 1200.
- CPU compatible officiellement dans les specs du PC.

Trois PC répondait à tous les prérequis: le Shuttle XH510G et XH410G et le Lenovo M90q Gen1.

Le Lenovo a été écraté car nécessitant un raiser pour le port PCIe, propriétaire et potentiellement complexe à trouver.

Le XH510G a été trouvé en barebone (ce qui était voulu) pour 180€ unité. L'avantage supplémentaire de ce PC là est qu'il est compatible pour du Intel Gen 11 (les CPU des optiplex étant du Gen 10), ouvrant potentiellement la route pour une upgrade futur, rallongeant la durée de vie du matériel.

La suite des opérations est de revendre les OptiPlex sous forme de Barebone afin de couvrir une partie des frais engagés, ce processus pourrait s'avérer long due au fait que ce soit niche d'acheter ce genre de serveurs sans aucun composants.