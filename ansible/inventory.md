---
title: Inventaires Ansible
description: 
published: true
date: 2023-06-30T10:23:29.393Z
tags: yaml, ansible, ansible-inventory
editor: markdown
dateCreated: 2023-06-21T06:49:31.521Z
---

# Introduction
Les fichiers d'inventaire au format *INI* ou *YAML* permettent de lister les hôtes, de les grouper et de leurs associer des variables.

A titre personnel, je préfère le format YAML, tous les exemples seront donc dans ce format.

# Structure d'un fichier d'inventaire
## Exemple simple
```yaml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
         one.example.com:
    dbservers:
      hosts:
        two.example.com:
```

### Hôtes dans plusieurs groupes
```yaml
  children:
    webservers:
      hosts:
        one.example.com:
    dbservers:
      hosts:
      	two.example.com:
    east:
      hosts:
        one.example.com:
    west:
      hosts:
        two.example.com:
    prod:
      hosts:
        one.example.com:
        two.example.com:
```
> A titre personnel, je n'aime pas faire ce genre de chose, car cela peut surcharger l'inventaire. Je préfère alors imbriquer les groupes (voir exemple ci-dessous)

### Groupes imbriqués
```yaml
all:
  hosts:
    mail.example.com:
  children:
    east:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    west:
      hosts:
        bar.example.com:
        three.example.com:
    prod:
      children:
        east:
    test:
      children:
        west:
```

## Exemple complexe
Mon expérience m'a confronté à un cas un peu complexe où je devais avoir trois niveaux :
- L'environnement: `prod`, `preprod` ou `dev`
- L'application: Spécifique au métier, on définira `APP1`, `APP2`, `APP3`
- La fonction: `db`, `front`, `proxy`, etc.

Nous devions avoir la possibilité d'appliquer des actions sur :
- Tous les hôtes d'un environnement
- Tous les hôtes d'une application
- Tous les hôtes d'une fonction
- Tous les hôtes d'une même application et d'une même fonction (ex: *Les front de APP1*)

Cala a donné une structure d'inventaire relativement complexe mais assez simple à exploiter.

# Variables
Les fichiers d'inventaire peuvent contenir des variables au niveau des hôtes ou des groupes.

Pour en savoir plus :
- [Définir des variables d'inventaire](/ansible/inventory/variable)
{.links-list}

# Inventaires dynamiques

# Commande
La commande `ansible-inventory` permet de lister les hôtes, groupes et variables des fichiers d'inventaires.

# Références
- [Building Ansible inventories - Ansible Documentation](https://docs.ansible.com/ansible/latest/inventory_guide/index.html)
{.links-list}