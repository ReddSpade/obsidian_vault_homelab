## Incident Traefik OOM 

### Timeline
- 16:30 : Détection perte accès Netbird + Zitadel
- 18:00 : Vérification logs → Traefik OOM Killed
- 18:03 : Analyse Docker stats → 750 MiB avant crash
- 18:04 : Suppression gitlab.yaml (ERREUR : analyse non faite)
- 18:07 : Redémarrage Traefik → Services restaurés
- 18:30 : Ajout RAM 1Go → 2Go

### Cause Racine
Fichier `gitlab.yaml` contenant une erreur syntaxique (`router` au lieu de `routers`). 
Traefik file watcher a détecté le fichier, tenté de le parser en boucle, 
générant un spam inotify qui a saturé la mémoire du conteneur (limite 960 MiB).

### Résolution
1. Suppression `/etc/traefik/dynamic/gitlab.yaml`
2. Redémarrage conteneur
3. Augmentation RAM VM (1Go → 2Go)

### Faille humaine
- **Pas de validation avant deploy** : fichier copié en prod sans test syntaxe
- **Destruction de preuve** : suppression du fichier AVANT analyse
- **Pas de staging** : modification directe en production
- **Monitoring absent** : OOM détecté par indisponibilité, pas par alerte

### Actions Préventives
1. **Validation syntaxe** : `yamllint` avant tout `cp` vers `/etc/traefik/dynamic/`
2. **Test local** : config Traefik en dev avec `--dry-run` si disponible
3. **Observabilité** : monitoring RAM Traefik (seuil alerte à 70%)
4. **Backup config** : Git pour versionner les fichiers dynamiques