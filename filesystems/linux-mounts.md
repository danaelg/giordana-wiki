---
title: Montage des système de fichiers sous Linux
description: 
published: true
date: 2023-06-22T16:17:47.992Z
tags: linux, filesystem, fstab
editor: markdown
dateCreated: 2023-06-20T13:15:17.460Z
---

# Introduction
Pour monter un système de fichiers sous Linux, on passe par la commande `mount` et le fichier `/etc/fstab` pour le montage au démarrage du système.

# Commande mount
La commande `mount` permet de monter un système de fichier à chaud sans que cela ne soit persistant après un redémarrage.

```bash
mount [-o OPTIONS] DEVICE MOUNTPOINT
```
> *MOUNTPOINT*: doit être un dossier existant
{.is-info}


# Fichier /etc/fstab
Le fichier `/etc/fstab` est une table de tableau où chaque ligne correspond à un montage. Les montages seront alors montées automatiquement au démarrage du système.

## Syntaxe
```
DEVICE    MOUNTPOINT    TYPE    OPTIONS    DUMP    PASS
```
- *DEVICE*:	Indique la partition à monter. Il y a plusieurs solutions, mais les 3 principales sont :
  - l'UUID (Universal Unique Identifier) de la partition : `UUID=UUID_VALUE` 
	- La référence directe à la partition : `/dev/XXX` 
	- Le label de la partition à monter. `LABEL=PART_LABEL`
- *MOUNTPOINT*: indique le répertoire de montage (celui-ci doit exister).
- *TYPE*:	indique le système de fichiers de la partition montée (ext4, ntfs, btrfs, etc.).
- *OPTIONS*:	indique les options au montage.
- *DUMP*:	définit les sauvegardes via l'utilitaire dump. La valeur classique est 0 (pas de dump).
- *PASS*:	règle la priorité de vérification des erreurs éventuelles du système de fichiers au démarrage. (0 : pas de vérification, 1 priorité haute, 2 priorité moins haute, etc.)