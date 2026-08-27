
# LDC (Lavaduck Datacenter)

🚧 Ce projet et cette documentation sont en cours de construction et peuvent
changer à tout moment.

## Table des matières

- [Introduction](#introduction)
- [Chapitrage](#chapitrage)
- [La place de l'IA](#la-place-de-lia)
- [Vos retours](#vos-retours)

## Introduction

Ce projet recueille l'ensemble des notes, schémas, procédures et projtes de mon
homelab, il est utilisé avant tout comme un vault Obsidian.

## Chapitrage

### 00_Images

Ce dossier contient toutes les images qui sont collées à travers le dépôt.

### 01_Architecture

Le dossier architecture comporte la documentation décisionnelle (ADR) de chaque
outil utilisé dans le Lab.

Il contient également des les informations sur le matériel (composants,
dépenses)

Le plan d'adressage doit être à terme, bougé sur Netbox.

### 02_Roadmap

La roadmap contient une liste de phases à compléter pour faire en sorte que le
lab soit prêt pour la production.

Elle est accompagnée d'une Todolist décrivant les étapes nécessaire à la
complétion de chaque brique.

### 03_Tracking

Le Tracking contient des dossiers classés par années, au début par jour
et désormais par semaine afin d'éviter le surplus de fichiers.

Ce dernier décrit les actions effectuées dans le lab, à but de suivi
d'évolution.
### 04_Runbooks

Le dossier Runbook contient actuellement des procédures utiles permettant
d'effectuer des tâches précises sur des plateforme comme Gitlab, qui seraient
faciles à oublier si elles ne sont pas notées.

### 05_Incidents

Les incidents recueillent les problèmes rencontrés lors du fonctionnement
normal du lab, elles permettent de définir:

- Quand un problème survient.
- Pourquoi il s'est produit.
- Quelles sont les actions effectuées.
- Si le problème est réglé.
- Si il peut se reproduire, ou quelles sont les actions préventives.

Ce dossier est encore en cours de travail sur le format de l'incidentologie.

### 06_Evolution

Le dossier évolution permet de noter les changements pertinents et/ou
importants du lab existant.

Par exemple, upgrade de matériel, migration d'un workload...

### 07_Backlog

Le backlog permet de garder une trace de deux éléments qui ne sont pas
prioritaires sur le reste, ou ne mettent pas en pause l'existant:

- Les idées
- Les projets

#### Idées

Il s'agit d'idées permettant d'améliorer le lab dans son ensemble, qui
impliquent souvent un achat, une livraison et une mise en place.
  
Par exemple, on peut y retrouver des idées pour améliorer la redondance
électrique du lab, ou encore la gestion des sauvegardes de ce dernier.

#### Projets

Les projets quant à eux visent une amélioration côté applicative du lab.

Que ce soit un ajout d'un nouveau type de gestion pour les appareils IoT,
des idées pour l'amélioration du flux GitOps ou autres.

## 08_Troubleshooting

Ce dossier est encore en construction, il devrait contenir les problèmes
rencontrés lors de l'administration du lab.

## La place de l'IA

Au début, ce projet de documentation a été mis en place avec de l'asssistance
de la part de Claude (Pro) avec un profil type Socratique (Qui force
l'utilisateur à s'interroger).

Désormais, aucune IA n'est utilisée pour coder, documenter, valider ni quoi que
ce soit.

Les actions sont effectuées à la suite de recherches et ne seront donc pas
toujours parfaites.

## Vos retours

Je suis ouvert à tout retour que vous pourriez me proposer, que cela soit sur
du réseau, de la méthode de documentation, du choix de matériel 
questionnement sur mes choix, ils sont les bienvenues.