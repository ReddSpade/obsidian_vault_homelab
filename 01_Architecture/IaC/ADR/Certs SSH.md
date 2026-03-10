Afin d'avoir une meilleure sécurisation de l'infrastructure, il a été décidé d'utiliser les certificats SSH provisionnés par OIDC pour les utilisateurs.

Voici comment ils sont provisionnés sur les VM:

**Packer** : Installe `step-cli` (binaire pinned), bake `@cert-authority` dans `known_hosts` global
**OpenTofu**: Déploie la VM, passe un `user_data` cloud-init avec hostname + token Step-CA
**cloud-init**: Au boot : appelle `step ssh certificate --host --principal <hostname>`, configure `sshd_config`
**Ansible**: Configuration métier post-boot, VM déjà sécurisée