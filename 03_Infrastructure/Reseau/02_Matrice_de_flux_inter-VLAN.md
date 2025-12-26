
## Légende

- ✅ = Accès complet
- ❌ = Accès bloqué
- 🔶 = Accès partiel (certains ports/services uniquement)

| Source ↓ / Dest → | Management | Infra | Compute | Storage | Users | IoT | Guest |
| ----------------- | ---------- | ----- | ------- | ------- | ----- | --- | ----- |
| Management        | -          | ✅     | ✅       | ✅       | ✅     | ✅   | ✅     |
| Infrastructure    | 🔶         | -     | ❌       | 🔶      | ❌     | ❌   | ❌     |
| Compute           | ❌          | ✅     | -       | 🔶      | ❌     | ✅   | ❌     |
| Storage           | ❌          | ❌     | ❌       | -       | ❌     | ❌   | ❌     |
| Users             | ✅          | ✅     | ✅       | ✅       | -     | ✅   | ❌     |
| IoT               | ❌          | ❌     | ❌       | 🔶      | ❌     | -   | ❌     |
| Guest             | ❌          | ❌     | ❌       | ❌       | ❌     | ❌   | -     |

### Choix

#### Management
- Management doit pouvoir accéder à tout, il gère l'infrastructure

#### Infrastructure
- Infrastructure doit pouvoir faire du monitoring sur le management au besoin
- Infrastructure doit pouvoir initier des backups sur le storage

#### Compute
- Le Compute doit accéder à l'Infra (écriture et lecture de secrets, externalDNS)
- Le Compute doit accéder au NAS 
- Le Compute doit pouvoir accéder à l'IoT (HomeAssistant)

#### Storage
- Le Storage n'initie aucune connexion.

#### Users
- Les users agréés membre du domicile sont adimistrateurs, ils ont accès à tout, sauf aux guests car ils ne les gèrent pas.

#### IoT
- L'IoT doit pouvoir accéder au Storage qui contient Jellyfin.

#### Guest 
- Ne fait que recevoir une IP de la part du DHCP.