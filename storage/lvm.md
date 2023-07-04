---
title: LVM
description: Logical Volume Manager
published: true
date: 2023-07-04T18:56:46.730Z
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
> - D'autre suffixes existent, se référer au manuel
{.is-info}


Exemple : Créer un volume logique utilisant tout l'espace restant dans le groupe de volume
```bash
lvcreate -n LV_NAME -l 100%FREE VG 
```
### Créer un volume logique à allocation dynamique (thin-provisioning)
Les volumes à allocation dynamique écrivent les blocs en fonction de l'usage réel. Cela permet de créer des volumes avec une taille maximale mais dont seul l'espace réellement utilisé est écrit sur le disque. 

Par opposition, les volumes "classique", à allocation fixe, mettent à disposition les blocs dès leur création. L'espace défini est alors fixe et ne peux être modifié facilement.

Pour créer un volume thin, il suffit d'ajouter l'option `-T` à la commanbde de création de volume logique (voir [Créer un volume logique](/storage/lvm#créer-un-volume-logique)).

Par exemple :
```bash
lvcreate -T -n LV_NAME -L SIZE[UNIT] [--poolmetadatasize SIZE[UNIT] ] VG
```
> L'option `--poolmetadatasize` permet de définir la taille réservés aux métadonnées. Il n'y a pas de règle parfaite pour déterminer la bonne taille. [La documentation](https://man7.org/linux/man-pages/man7/lvmthin.7.html) recommande une taille de 1G et de l'augmenter en cas de besoin. Sans l'option, la taille prise réservé aux métadonnées est calculé de la façon suivante : `Pool_LV_size / Pool_LV_chunk_size * 64b`

### Etendre un volume logique
En exprimant une taille fixe
```bash
lvextend -L +SIZE VG_NAME/LV_NAME 
```
> *SIZE* doit être exprimé avec une unité K, M, G, T, P ou E
{.is-info}

En exprimant un nombre d'extent
```bash
lvextend -l EXTENT[%{FREE|PVS|VG|ORIGIN}] VG/LV 
```
> La taille d'un extent s'obtient via la commande `pvdisplay`
{.is-info}

> Cette commande est pratique pour définir une taille exprimé en pourcentage en ajoutant un suffixe.
> - *%VG*: Pourcentage de l'espace total du VG
> - *%FREE*: Pourcentage de l'espace restant dans le VG
>
> D'autre suffixes existent, se référer au manuel
{.is-info}

### Suppression d'un volume logique
Il faut d'abord désactiver le volume
```bash
lvchange -an VG_NAME/LV_NAME 
```

Ensuite, on peut supprimer le volume
```bash
lvremove VG_NAME/LV_NAME 
```
> Il faut penser à supprimer toute référence de volume dans le fichier `/etc/fstab`  
{.is-info}

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
- https://man7.org/linux/man-pages/man7/lvmthin.7.html
{.links-list}
