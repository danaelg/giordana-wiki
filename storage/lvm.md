---
title: LVM
description: Logical Volume Manager
published: true
date: 2023-06-20T15:37:26.474Z
tags: linux, storage, lvm
editor: markdown
dateCreated: 2023-06-20T15:37:26.474Z
---

# Introduction
LVM (Logical Volume Manager) ajoute une couche d'abstraction entre les disques et les systèmes de fichiers. LVM permet alors de grouper des disques dans des groupes de volumes qui peuvent alors servir un ou plusieurs volume logiques.

![lvm-scheme.jpg](/storage/lvm-scheme.jpg =50%x)

# Utilisation
## Volumes physiques (PV)
### Création d'un volume physique
```bash
pvcreate DEVICE
```

### Etendre un volume physique
```bash
pvresize DEVICE
```

### Supprimer un volume physique
```bash
pvremove DEVICE
```

## Groupes de volumes (VG)
### Création d'un groupe de volumes
```bash
vgcreate VG_NAME PV_PATH [PV_PATH ...]
```

### Etendre un groupe de volumes
```bash
vgextend VG_NAME PV_PATH [PV_PATH ...]
```

### Supprimer un groupe de volumes
```bash
```

## Volumes logiques (LV)
### Créer un volume logique
En exprimant une taille fixe
```bash
lvcreate -n LV_NAME -L SIZE VG 
```
> *SIZE* doit être exprimé avec une unité K, M, G, T, P ou E
{.is-info}

En exprimant un nombre d'extent
```bash
lvcreate -n LV_NAME -l EXTENT[%{FREE|PVS|VG|ORIGIN}] VG 
```
> La taille d'un extent s'obtient via la commande `pvdisplay`
{.is-info}

> Cette commande est pratique pour définir une taille exprimé en pourcentage en ajoutant un suffixe.
> - *%VG*: Pourcentage de l'espace total du VG
> - *%FREE*: Pourcentage de l'espace restant dans le VG
> - D'autre suffixe existe, se référer au manuel.
{.is-info}

Exemple : Créer un volume logique utilisant tout l'espace restant dans le groupe de volume
```bash
lvcreate -n LV_NAME -l 100%FREE VG 
```

# Références

- https://linux.die.net/man/8/pvcreate
- https://linux.die.net/man/8/pvextend
- https://linux.die.net/man/8/pvremove
- https://linux.die.net/man/8/vgcreate
- https://linux.die.net/man/8/vgextend
- https://linux.die.net/man/8/vgremove
- https://linux.die.net/man/8/lvcreate
- https://linux.die.net/man/8/lvextend
- https://linux.die.net/man/8/lvremove
{.links-list}
