
## Plan des VLANS

| VLAN | Zone           | Subnet         | Gateway     | Rôle                                                          |
| ---- | -------------- | -------------- | ----------- | ------------------------------------------------------------- |
| 10   | Management     | `10.0.10.0/24` | `10.0.10.1` | Équipements réseau                                            |
| 20   | Infrastructure | `10.0.20.0/24` | `10.0.20.1` | Hyperviseurs, serveur de backup, et services d'infrastructure |
| 30   | Compute        | `10.0.30.0/24` | `10.0.30.1` | Nodes Kubernetes et workload                                  |
| 40   | Storage        | `10.0.40.0/24` | `10.0.40.1` | NAS et stockages centralisés                                  |
| 50   | Users          | `10.0.50.0/24` | `10.0.50.1` | Utilisateurs permantents                                      |
| 60   | IoT            | `10.0.60.0/24` | `10.0.60.1` | Objets connectés avec connexion externe                       |
| 70   | NoT            | `10.0.70.0/24` | `10.0.70.1` | Objets connectés sans connexion externe                       |
| 80   | Guests         | `10.0.80.0/24` | `10.0.80.1` | WiFi invités                                                  |
| 99   | DMZ            | `10.0.99.0/24` | `10.0.99.1` | DMZ pour le reverse proxy                                     |

### Choix 

#### Classe A (10.x.x.x)
Vu l'utilisation du Netbird, il y'a peu de chances d'avoir des IP en 10.x.x.x dans d'autres réseaux domestiques qui sont bien plus souvent sur du 192.168.1.x, ce qui évite un conflit.

#### 2e Octet (10.10, 10.20, 10.30.)
Le choix de renseigner les IP par 10, 20, 30 ainsi de suite a été fait pour faciliter l'associations aux numéro des VLANS.