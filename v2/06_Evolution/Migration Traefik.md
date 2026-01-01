## Timestamp

30/12/2025
## Actions

Migration de la VM `vm-edge-prd-deb13-01` vers un LXC.
Changement de Traefik: Docker -> Binaire.

## Justification

Traefik étant isolé au sein d'une seule VM Docker, les labels ne sont pas utilisés ce qui contre tout intérêt d'avoir la solution sur Docker.
Gain de ressources.

## Étapes

1. Actions à faire en local (pas via VPN).
2. `scp` du contenu de `opt/docker/traefik` dans la VM `vm-edge-prd-deb13-01` sur PC local.
3. Éteindre la VM.
4. Mettre à jour le fichier de configuration en local (clé API Technitium DNS, Cloudflare Token, ...).
5. Déploiement d'un LXC `lxc-edge-prd-deb13-01` cloné de la template ID 8000.
6. `scp` des nouveaux fichiers le conteneur LXC.
7. Création de l'utilisateur `traefik`.
8. Installation du binaire traefik statique.
9. Mise en place des bonnes permissions sur les fichiers copiés:
	-  `acme.json` en 600
	- Propriétaire `traefik:traefik` sur les fichiers
	- `User=traefik` et `Group=traefik` dans l'unit systemd
10. Création d'un service systemd pour traefik afin qu'il démarre au boot.
11. Mise en place des fichiers et certificats.
12. Test de chaque redirection (fichiers dynamique) manuellement.
13. Kill de la VM si chaque fichier dynamique traefik est validé.