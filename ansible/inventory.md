---
title: Inventaires Ansible
description: 
published: true
date: 2023-06-30T09:47:00.633Z
tags: yaml, ansible, ansible-inventory
editor: markdown
dateCreated: 2023-06-21T06:49:31.521Z
---

# Introduction
Les fichiers d'inventaire au format *INI* ou *YAML* permettent de lister les hôtes, de les grouper et de leurs associer des variables.

A titre personnel, je préfère le format YAML, tous les exemples seront donc dans ce format.

# Strucuture d'un fichier d'inventaire
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

# Variables
Les fichiers d'inventaire peuvent contenir des variables au niveau des hôtes ou des groupes.

Pour en savoir plus :
- [Définir des variables d'inventaire](/ansible/inventory/variable)
{.links-list}

# Commande
La commande `ansible-inventory` permet de lister les hôtes, groupes et variables des fichiers d'inventaires.