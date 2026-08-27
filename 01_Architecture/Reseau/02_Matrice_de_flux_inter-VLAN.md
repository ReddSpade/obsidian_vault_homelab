
## Légende

- O = Accès complet
- X = Accès bloqué
- / = Accès partiel (certains ports/services uniquement)
- - = Non applicable

| Source  / Dest --> | Management | Infra | Compute | Storage | Users | IoT | NoT | Guest | DMZ |
| ------------------ | ---------- | ----- | ------- | ------- | ----- | --- | --- | ----- | --- |
| Management         | -          | O     | O       | O       | O     | O   |     | O     |     |
| Infrastructure     | /          | -     | X       | X       | X     | X   |     | X     |     |
| Compute            | X          | O     | -       | /       | X     | O   |     | X     |     |
| Storage            | X          | X     | X       | -       | X     | X   |     | X     |     |
| Users              | O          | O     | O       | O       | -     | O   |     | X     |     |
| IoT                | X          | X     | X       | /       | X     | -   |     | X     |     |
| NoT                |            |       |         |         |       |     | -   |       |     |
| Guest              | X          | X     | X       | X       | X     | X   |     | -     |     |
| DMZ                |            |       |         |         |       |     |     |       | -   |

## Choix

### Management
### Infrastructure
### Compute
### Storage
### Users
### IoT
### NoT
### Guest 
### DMZ