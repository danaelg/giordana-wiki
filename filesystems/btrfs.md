---
title: BTRFS
description: 
published: true
date: 2023-06-20T13:55:34.512Z
tags: linux, filesystem, btrfs
editor: markdown
dateCreated: 2023-06-20T10:03:07.577Z
---

# BTRFS
## Introduction
BTRFS (prononcé "*Better FS*", "*Butter FS*" ou "*B-Tree FS*") est un système de fichier développé par Oracle qui assemble de la copy-on-write et un gestionnaire de volume logique.

L'objectif de BTRFS est d'apporté les fonctionnalité de :
- Snapshots
- RAID
- D'auto-rétablissement (à l'aide de checksum, de détection et de correction de corruptions).

## Création de volumes
### Créer une partition BTRFS
```bash
mkfs.btrfs DEVICE
```
> Il est possible de formater un disque entier sans créer de partition (ex: /dev/sda plutôt que /dev/sda1). Cependant, cela n'est pas commun et peut être sujet à des erreurs. 
{.is-info}

### Créer un volume RAID avec BTRFS
```bash
mkfs.btrfs -m raid{0|1|5|6|10} -d raid{0|1|5|6|10} DEVICE [DEVICE ...]
```

## Gestion de volumes
### Convertir un volume BTRFS
```bash
btrfs balance start -mconvert=raid{0|1|5|6|10} -dconvert=raid{0|1|5|6|1}0 MOUNTPOINT
```
> Il est possible de convertir un RAID 1 en RAID 5 si le volume possède au moins 3 disques
{.is-info}

### Ajouter un disque à un volume BTRFS
```
btrfs device add DEVICE MOUNPOINT
```

### Rééquilibrer un volume BTRFS
```bash
btrfs balance start -dusage=0 MOUNTPOINT
btrfs balance start -musage=0 MOUNTPOINT
```
> - dusage : déplace les groupes blocs de données ayant une utilisation inférieur à 0
> - musage : déplace les groupes blocs de métadonnées ayant une utilisation inférieur à 0
{.is-help}

> La valeur de 0 peut être utilisé dans un cas où le rééquilibrage renvoit des erreurs ENOSPC alors que le système de fichiers n'est pas plein
{.is-info}

- https://btrfs.readthedocs.io/en/latest/Balance.html
{.links-list}

## Troubleshooting
### Changer un disque d'un volume BTRFS
```bash
btrfs replace start FAULTY_DEVICE NEW_DEVICE MOUNTPOINT
```
> Utiliser l'option `-r` s'il y a beaucoup d'erreurs en lecture sur le disque source
{.is-info}

### Monter un volume en mode dégradé
```bash
mount -o degraded DEVICE MOUNTPOINT
```

### Voir aussi
- [Corriger volume plein](/filesystems/btrfs/troubleshooting/full-volume)
{.links-list}


---

## Ressources
- https://btrfs.wiki.kernel.org
- https://en.wikipedia.org/wiki/Btrfs

{.links-list}