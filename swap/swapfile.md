---
title: Créer un fichier swapfile
description: 
published: true
date: 2023-06-20T13:15:46.525Z
tags: linux, swap
editor: markdown
dateCreated: 2023-06-20T13:03:14.301Z
---

# Introduction
On peut vouloir créer un fichier swap (swapfile) à la place d'une partition

# Configuration
## Création d'un fichier
```bash
fallocate -l SIZE PATH
```
> Il faut définir une unité à `SIZE`, par exemple G, GB, M, MB, K, KB  
{.is-info}

## Modification des permissions du fichier
```bash
chmod 600 PATH
```

## Configuration du fichier en tant que swap
```bash
mkswap PATH
```

## Activation du swap
```bash
swapon PATH
```

## Montage automatique du SWAP
Voir [Montage des système de fichiers sous Linux](/filesystems/linux-mounts)
> dans /etc/fstab définissez `MOUNTPOINT` et `TYPE` à `swap`)
{.is-info}
  
# Références
- https://man7.org/linux/man-pages/man1/fallocate.1.html
{.links-list}