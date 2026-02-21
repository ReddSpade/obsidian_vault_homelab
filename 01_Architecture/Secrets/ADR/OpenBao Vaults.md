
Création de 4 vaults type Key Value (KV) séparés:

- CI -> Vault type KV dédié à tout type de tokens utilisés par Gitlab.
- Servers -> Vault type KV dédié au stockage des credentials des serveurs de l'infrastructure.
- Services -> Vault type KV dédié aux services (password bdd d'une app).
- Infrastructure -> Vault type KV critique dédié aux services pouvant modifier l'infrastructure (API Proxmox, DNS, TFState.)