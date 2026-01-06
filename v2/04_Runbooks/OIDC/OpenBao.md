## Prérequis

- OpenBao unsealed
- Projet Zitadel ayant la case "Affirmer les rôles lors de l'authentification" cochée.
- Action `flatRoles` créée.
- Flux `Compléter Token` avec `Pré Userinfo création` et `Pré access token création` contenant l'action `flatRoles`.
- Rôle `admin_ring` créé dans le Projet.
- Autorisations au rôle `admin_ring` donné aux utilisateurs voulus.

## Zitadel

### Créer l'Application

1. `Projet > Infrastructure > Applications > Nouveau`.
2. Choisir le nom "OpenBao" et cliquer sur le type "Web".
3. Choisir le type "CODE".
4. Activer le "Mode développement".
5. Rentrer ces URIs de redirection:
	1. https://bao.int.lavaduck.net/ui/vault/auth/oidc/oidc/callback
	2. https://bao.int.lavaduck.net/v1/auth/oidc/*
	3. http://localhost:8250/oidc/callback
6. Rentrer cette URI de post-déconnexion: https://bao.int.lavaduck.net/ui/
7. Cliquer sur "Créer".
8. Copier le `Client ID` et le `Client Secret` et les noter dans un endroit sécurisé.
9. Un moyen de se connecter au vault via `bao login`.

### Paramétrage de l'Application

Dans l'onglet `Configuration` s'assurer que:

- Type d'application =  `Web`.
- Types de réponse =`Code`.
- Méthode d'authentification = `Basic`.
- Type d'octroi = `Code d'autorisation` uniquement.

## OpenBao

### Activer l'OIDC

Pour activer la méthode de connexion OIDC (si elle n'est pas activée).

`bao auth enable oidc`

### Créer le rôle `default`

```bash
bao write auth/oidc/role/default \ 
allowed_redirect_uris="https://bao.int.lavaduck.net/ui/vault/auth/oidc/oidc/callback,http://localhost:8250/oidc/callback" \ 
oidc_scopes="openid,profile,email,urn:zitadel:iam:org:project:roles" \ 
groups_claim="groups" \ 
user_claim="email"
```
### Configurer l'OIDC

Afin de pouvoir se connecter, il faut configurer la méthode de connexion OIDC.

```bash
bao write auth/oidc/config \  
  oidc_client_id="<CLIENT_ID>" \
  oidc_client_secret="<CLIENT_SECRET>" \ 
  default_role="default" \
  oidc_discovery_url="https://auth.lavaduck.net" 
```
### Configurer la Policy pour le groupe SSO

Pour que les membres puissent avoir des accès, il faut créer une policy.
Cela peut être fait en linkant un fichier HCL contenant ce code:

```hcl
path "*" {
  capabilities = ["create", "read", "update", "delete", "list", "sudo"]
}
```

Puis créé via `bao policy write admin-policy /ma/policy.hcl`.

### Groupe Externe

Il faut maintenant lier la policy à un groupe, ce groupe sera lié au groupe de l'utilisateur Zitadel.

Créer le groupe de type `external` et l'associer à la policy `admin-policy`:
`bao write identity/group name="admin_ring" type="external" policies="admin-policy"`

Récupérer l'accessor du mount OIDC:
`bao auth list`

Récupérer le `group_id`
`bao read identity/group/name/admin_ring` (récupérer le champ `id`). 

Créer l'alias (lien entre le groupe et le mount OIDC) :
`bao write identity/group-alias name="admin_ring" mount_accessor="<accessor>" canonical_id="<group_id>"`

## Vérification

Afin de vérifier si ça fonctionne, se log avec un utilisateur faisant partie du groupe Zitadel `admin_ring`.

Se connecter:
`bao login -method=oidc`.

Vérifier que le token contient `admin-policy` sur le champ `identity_policies`:
`bao token lookup`.