Afin de standardiser les pipelines CI/CD, un Catalogue a été mis en place avec les éléments suivants:

- Connexion à OpenBao pour récupérer les secrets et les stocker dans des variables.

## Flux

### Connexion OpenBao

Démarrage d'une Pipeline avec les éléments à récupérer --> Gitlab génère un JWT --> Le job de la pipeline le présente à OpenBao --> OpenBao vérifie la signature et les claims (audience, user, issuer, ...) --> OpenBao renvoie un token à Gitlab --> Gitlab s'en sert pour lire le ou les secrets.