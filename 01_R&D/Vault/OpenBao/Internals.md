[Documentation](https://openbao.org/docs/internals)





## Architecture

![[01_R&D-Vault-OpenBao-Architecture-01.png]]

OpenBao montre 3 niveaux sur ce schéma:

1. HTTP/S API: Tout ce qui vient de dehors, requêtes et autres, considéré **untrusted**.
2. La barrière responsable du chiffrement et déchiffrement des données.
3. Le Storage Backend, ou OpenBao stocke les données, il considère **untrusted**.

Le Storage Backend n'est pas une entité de confiance pour OpenBao, il chiffre donc les données qu'il y envoie, si le Storage Backend est compromis, il n'est pas utilisable car seul OpenBao peut déchiffrer les données.
OpenBao fonctionne en RAM, et se sert du Storage Backend pour stocker à longue durée les secrets.

### Sealed/Unsealed

Quand OpenBao démarre, il est dans l'état "Scellé" et doit être descellé sans quoi il n'est pas utilisable.
On utilise les clés de descellement pour desceller OpenBao.
À l'initialisation d'OpenBao, ce dernier génère une 