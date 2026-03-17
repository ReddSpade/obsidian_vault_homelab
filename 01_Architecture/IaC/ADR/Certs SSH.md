Les clés SSH sont de natures complexe à gérer, elles n'ont pas de durées limites de vie, peuvent être volées, et la vérification du host (le serveur auquel on se connecte) n'est pas native.
Les certificats SSH permettent de gérer ce problème, et Step-CA (la PKI de cette infrastructure) gère les certificats SSH.
Il a été décidé d'utiliser les certificats SSH provisionnés par OIDC pour les utilisateurs.

Voici comment ils sont provisionnés sur les VM:

**Packer** :
- Installe le binaire Step-CLI dépendant de l'OS
- Setup d'une partie du Host certificate: le TrustedCaKeys contenant un chemin vers une clé publique qui permet de dire "Cette CA vérifie les certificats clients" avec:
```
# /etc/ssh/sshd_config.d/10-ssh-certs.conf
TrustedUserCAKeys /etc/ssh/ssh_user_key.pub
```

**OpenTofu**: 
- Déploie la VM
- Passe un `user_data` cloud-init avec hostname + token Step-CA 

**cloud-init** :
-  `step ssh certificate --host --principal <hostname>` afin de générer les certificats hosts de la VM
- Déplacement des certificats ddans `/etc/ssh/*_key` et `/etc/ssh/*_pub`
- Ajout des valeurs via:
```
# /etc/ssh/sshd_config.d/10-ssh-certs.conf

HostKey /etc/ssh/ssh_host_ecdsa_key
HostCertificate /etc/ssh/ssh_host_ecdsa_key-cert.pub
```

**Ansible**: 
- Configuration métier post-boot, VM déjà sécurisée
- Setup de la partie client dans `~/.ssh/known_hosts` permettant de dire "Pour tout ce qui est `*.int.lavaduck.net`, utilise cette pubkey pour vérifier le certificat host (à qui je me connecte ? )" Uniquement pour les VM ayant besoin d'initier des connexions SSH comme gitlab-runner.


## Host Certificate

Afin de gérer le Host Certificate, qui est un certificat SSH, il faut générer ce dernier.
Pour ce faire, on peut utiliser `step ssh certificate` afin de se faire attribuer un certificat par la CA:

```sh
step ssh certificate --host --provisioner pki@lavaduck.net pubkey privkey --provisioner-password-file <(echo '[PASSWORD]')
```

Un problème se pose, la documentation explicite:

**--insecure**
**--no-password** Do not ask for a password to encrypt a private key. Sensitive key material will be written to disk unencrypted. This is not recommended. Requires **--insecure** flag.

Il faut donc chiffrer les futur certificats avec un mot de passe.

Voici une idée de workflow qui pourrait satisfaire cette nécessité:

1. Clé host générée + mot de passe → stockés dans OpenBao
2. GitLab CI fetch les secrets
3. OpenTofu les injecte dans cloud-init
4. cloud-init utilise la clé + mot de passe pour faire signer le host cert par Step-CA