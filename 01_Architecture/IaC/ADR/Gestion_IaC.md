## Architecture Gitlab

L'architecture choisie pour Gitlab a été choisie comme suit:

```txt
.
├── infrastructure 
└── modules
    └── deploy-vm
```

Infrastructure: Contient l'infrastructure (Appels aux différents modules) comme les VM et autres.

Modules: Contient les modules réutilisables appelés dans l'infrastructure.

## Backend

Le backend va être géré par Gitlab qui utilisera un S3 (Garage) stocké sur le NAS. Il va également gérer le côté chiffrement server-side

OpenBao s'occupera du chiffrement slient-side.