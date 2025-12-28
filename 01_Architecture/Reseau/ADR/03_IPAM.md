| VLAN | Zone           | Subnet          | Gateway     | Rôle                               |
| ---- | -------------- | --------------- | ----------- | ---------------------------------- |
| 10   | Management     | `10.10.0.0/24`  | `10.10.0.1` | Proxmox (+PBS), switches, OPNsense |
| 20   | Infrastructure | `10.20.0.0/24`  | `10.20.0.1` | Ring 0 (DNS, PKI, Vault, IdP)      |
| 30   | Compute        | `10.30.0.0/24`  | `10.30.0.1` | Kubernetes nodes                   |
| 40   | Storage        | `10.40.0.0/24`  | `10.40.0.1` | NAS                                |
| 50   | Users          | `10.50.0.0/24`  | `10.50.0.1` | Postes clients                     |
| 60   | IoT            | `10.60.0.0/24`  | `10.60.0.1` | Appareils "non fiables"            |
| 70   | Guest          | `10.70.0.0/24`  | `10.70.0.1` | WiFi invités                       |
| N/A  | Non Routable   | `172.16.0.0/29` | N/A         | Proxmox HA                         |

## VLAN 10 (Management)

### Pool Réseau

| IP        | Hostname           | Objet         | Fonction                 |
| --------- | ------------------ | ------------- | ------------------------ |
| 10.10.0.1 | phy-rtr-prd-opn-01 | Routeur       | Routing/Pare-Feu/Gateway |
| 10.10.0.2 | phy-mm-prd-frbx-01 | Modem         | Modem Freebox            |
| 10.10.0.3 | phy-ap-prd-unfi-01 | Point d'accès | Wifi                     |
| 10.10.0.4 | phy-sw-prd-mrlk-01 | Switch        | Connexion 2,5GbE         |
| 10.10.0.5 | phy-sw-prd-ntgr-01 | Switch        | Connexion 1GbE           |
### Pool Hyperviseurs

| IP         | Hostname       | Objet       | Fonction       |
| ---------- | -------------- | ----------- | -------------- |
| 10.10.0.20 | phy-pve-prd-01 | Hyperviseur | Virtualisation |
| 10.10.0.21 | phy-pve-prd-02 | Hyperviseur | Virtualisation |
| 10.10.0.22 | phy-pve-prd-03 | Hyperviseur | Virtualisation |
| 10.10.0.23 | phy-pbs-prd-01 | Hyperviseur | Backup         |
## VLAN 20 (Infrastructure)

### Pool Réseau

| IP        | Hostname           | Objet   | Fonction                 |
| --------- | ------------------ | ------- | ------------------------ |
| 10.20.0.1 | phy-rtr-prd-opn-01 | Routeur | Routing/Pare-Feu/Gateway |
### Pool VM

| IP         | Hostname             | Objet | Fonction                |
| ---------- | -------------------- | ----- | ----------------------- |
| 10.20.0.20 | lxc-dns-prd-deb13-01 | LXC   | DNS                     |
| 10.20.0.21 | lxc-pki-prd-deb13-01 | LXC   | PKI                     |
| 10.20.0.22 | lxc-vlt-prd-deb13-01 | LXC   | Gestionnaire de Secrets |
| 10.20.0.23 | vm-iam-prd-deb13-01  | VM    | IdP + VPN               |
| 10.20.0.24 | vm-edge-prd-deb13-01 | VM    | Reverse Proxy           |

## VLAN 30 (Compute)

### Pool Réseau

| IP        | Hostname           | Objet   | Fonction                 |
| --------- | ------------------ | ------- | ------------------------ |
| 10.30.0.1 | phy-rtr-prd-opn-01 | Routeur | Routing/Pare-Feu/Gateway |
### Pool Kubernetes (Dev + prod + mgmt)
 
À déterminer selon le nombre de nodes Kubernetes

## VLAN 40 (Storage)

### Pool Réseau

| IP        | Hostname           | Objet   | Fonction                 |
| --------- | ------------------ | ------- | ------------------------ |
| 10.40.0.1 | phy-rtr-prd-opn-01 | Routeur | Routing/Pare-Feu/Gateway |

### Pool NAS

| IP         | Hostname            | Objet | Fonction |
| ---------- | ------------------- | ----- | -------- |
| 10.40.0.20 | phy-nas-prd-trns-01 | NAS   | Stockage |

## VLAN 50 (Users)

### Pool Réseau

| IP        | Hostname           | Objet   | Fonction                 |
| --------- | ------------------ | ------- | ------------------------ |
| 10.50.0.1 | phy-rtr-prd-opn-01 | Routeur | Routing/Pare-Feu/Gateway |

### Pool PC Reduck

| IP         | Hostname       | Objet              | Fonction                           |
| ---------- | -------------- | ------------------ | ---------------------------------- |
| 10.50.0.20 | reduck         | PC Reduck          | PC Reduck NixOS (Dual Boot)        |
| 10.50.0.21 | reduck         | PC Reduck Wifi     | PC Reduck Wifi NixOS (Dual Boot)   |
| 10.50.0.22 | reduck-windows | PC Reduck          | PC Reduck Windows (Dual Boot)      |
| 10.50.0.23 | reduck-windows | PC Reduck Wifi     | PC Reduck Wifi Windows (Dual Boot) |
| 10.50.0.24 | reduck-laptop  | PC Portable Reduck | PC portable Reduck NixOS           |
| 10.50.0.25 | CHE-WK33-MRD   | PC Reduck          | PC Travail Reduck                  |

### Pool PC Fenrir

| IP         | Hostname                 | Objet          | Fonction                           |
| ---------- | ------------------------ | -------------- | ---------------------------------- |
| 10.50.0.40 | fenrir-pendragon         | PC Fenrir      | PC Fenrir CachyOS (Dual Boot)      |
| 10.50.0.41 | fenrir-pendragon         | PC Wifi Fenrir | PC Fenrir Wifi CachyOS (Dual Boot) |
| 10.50.0.42 | fenrir-pendragon-windows | PC Fenrir      | PC Fenrir Windows (Dual Boot)      |
| 10.50.0.43 | fenrir-pendragon-windows | PC Wifi Fenrir | PC Fenrir Wifi Windows (Dual Boot) |
| 10.50.0.42 | (nom inconnu)            | PC Fenrir      | PC Travail Fenrir                  |

### Pool PC autres

| IP         | Hostname    | Objet       | Fonction    |
| ---------- | ----------- | ----------- | ----------- |
| 10.50.0.60 | macbook-air | MacBook Air | MacBook Air |
### Pool Consoles

| IP         | Hostname         | Objet                  | Fonction         |
| ---------- | ---------------- | ---------------------- | ---------------- |
| 10.50.0.80 | switch-reduck    | Nintendo Switch Reduck | Nintendo Switch  |
| 10.50.0.81 | switch-fenrir    | Nintendo Switch Fenrir | Nintendo Switch  |
| 10.50.0.82 | ps4              | PS4                    | PS4              |
| 10.50.0.83 | steamdeck-reduck | SteamDeck Reduck       | SteamDeck Reduck |
| 10.50.0.84 | steamdeck-fenrir | SteamDeck Fenrir       | SteamDeck Fenrir |

## VLAN 60 (IoT)

### Pool Réseau

| IP        | Hostname           | Objet   | Fonction                 |
| --------- | ------------------ | ------- | ------------------------ |
| 10.60.0.1 | phy-rtr-prd-opn-01 | Routeur | Routing/Pare-Feu/Gateway |

### Pool HUE

Refaire inventory Philips Hue + Bridge

### Pool Smart objects

| IP         | Hostname         | Objet            | Fonction         |
| ---------- | ---------------- | ---------------- | ---------------- |
| 10.60.0.20 | phy-tv-wbos-01   | Smart TV         | Télévision       |
| 10.60.0.21 | phy-prnt-prsa-01 | Imprimante 3D    | Imprimante 3D    |
| 10.60.0.22 | phy-prnt-hp-01   | Imprimante       | Imprimante       |
| 10.60.0.23 | phy-vac-eurk-01  | Aspirateur Robot | Aspirateur Robot |
| 10.60.0.24 | phy-ha-hmpd-01   | Apple Homepod    | Home assistant   |



## VLAN 70 (Guests)

### Pool Réseau
| IP        | Hostname           | Objet   | Fonction                 |
| --------- | ------------------ | ------- | ------------------------ |
| 10.70.0.1 | phy-rtr-prd-opn-01 | Routeur | Routing/Pare-Feu/Gateway |
### Pool autre

DHCP

## Non Routable

### Pool Hyperviseurs

| IP         | Hostname       | Objet       | Fonction       |
| ---------- | -------------- | ----------- | -------------- |
| 172.16.0.1 | phy-pve-prd-01 | Hyperviseur | Virtualisation |
| 172.16.0.2 | phy-pve-prd-02 | Hyperviseur | Virtualisation |
| 172.16.0.3 | phy-pve-prd-03 | Hyperviseur | Virtualisation |
