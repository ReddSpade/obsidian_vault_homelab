Afin de connecter Gitlab avec Bao, il faut le faire grace à un JWT.

1. Activer l'auth JWT: `bao auth enable -path=jwt-gitlab jwt`.
2. Dans la configuration de l'auth, définir le `OIDC Discovery URL` sur `https://gitlab.int.lavaduck.net` et donner comme default role `gitlab-ci`.
3. Créer une Policy permettant à Gitlab de lire, écrire et update les secrets sur le vault type kv nommé `ci`.
4. Créer un rôle sous le chemin `auth/jwt-gitlab/role` nommé `gitlab-ci` avec le code suivant:
	- `bound_claims.namespace_path="iac,kubernetes,platform"`: Définis quel groupe Gitlab est autorisé a accéder à la ressource
	- `bound_audiences="https://bao.int.lavaduck.net"`: Définis la cible autorisé pour ce JWT.
	- `policies="gitlab-ci-policy"`: Permet de lier la policy au rôle.
	- `user_claim="sub"`: Champ JWT qu'OpenBao utilise comme identifiant.
	- `role_type=jwt` : définis le type du rôle.


## Commandes

```sh
# Étape 1
bao auth enable path=jwt-gitlab jwt

# Étape 2
bao write auth/jwt-gitlab/config \
  oidc_discovery_url="https://gitlab.int.lavaduck.net" \
  default_role="gitlab-ci"
  
# Étape 3
bao policy write gitlab-ci-policy gitlab-ci-policy.hcl

# gitlab-ci-policy.hcl
# path "ci/*" {
#  capabilities = ["create", "read", "update", "list"]
# }

# Étape 4
bao write auth/jwt-gitlab/role/gitlab-ci \
  bound_claims.namespace_path="iac,kubernetes,platform" \
  bound_audiences="https://bao.int.lavaduck.net" \
  policies="gitlab-ci-policy" \
  user_claim="sub" \
  role_type=jwt
```
