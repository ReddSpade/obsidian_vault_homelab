
## Plan des VLANS

| VLAN | Zone           | Subnet         | Gateway     | Rôle                               |
| ---- | -------------- | -------------- | ----------- | ---------------------------------- |
| 10   | Management     | `10.10.0.0/24` | `10.10.0.1` | Proxmox (+PBS), switches, OPNsense |
| 20   | Infrastructure | `10.20.0.0/24` | `10.20.0.1` | Ring 0 (DNS, PKI, Vault, IdP)      |
| 30   | Compute        | `10.30.0.0/24` | `10.30.0.1` | Kubernetes nodes                   |
| 40   | Storage        | `10.40.0.0/24` | `10.40.0.1` | NAS                                |
| 50   | Users          | `10.50.0.0/24` | `10.50.0.1` | Postes clients                     |
| 60   | IoT            | `10.60.0.0/24` | `10.60.0.1` | Appareils "non fiables"            |
| 70   | Guest          | `10.70.0.0/24` | `10.70.0.1` | WiFi invités                       |

### Choix 

#### Classe A (10.x.x.x)
Vu l'utilisation du Netbird, il y'a peu de chances d'avoir des IP en 10.x.x.x dans d'autres réseaux domestiques qui sont bien plus souvent sur du 192.168.1.x, ce qui évite un conflit.

#### 2e Octet (10.10, 10.20, 10.30.)
Le choix de renseigner les IP par 10, 20, 30 ainsi de suite a été fait pour faciliter l'associations aux numéro des VLANS.

#### Gateway .1
Si jamais le réseau passe en /23, il y'aura 512 adresses, avec un .1 on s'assure que la Gateway ne se retrouve pas a un endroit aléatoire du réseau, mais **toujours** au début