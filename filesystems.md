---
title: Filesystems
description: 
published: true
date: 2023-09-28T19:12:46.292Z
tags: filesystem, storage
editor: markdown
dateCreated: 2023-06-20T09:29:09.640Z
---

# Introduction
Le système de fichiers est l'intermédiaire entre le stockage physique et le système d'exploitation qui défini la structure des données sur le stockage physique.

Il existe de nombreux type de système de fichiers qui offrent des fonctionnalités et répondent à des besoins différents. Sous Linux le système de fichier le plus courant est EXT4, mais certaine distributions (RHEL7+) tendent à le remplacer par XFS.

# Type de systèmes de fichiers
- [BTRFS](/filesystems/btrfs)
- [EXT4](/filesystems/ext4)
- [XFS](/filesystems/xfs)
- [GlusterFS](/filesystems/glusterfs)
- [CephFS](/filesystem/cephfs)
{.links-list}

# Et LVM ?
LVM (Logical Volume Manager) est un système de gestion de volumes. Il intervient donc avant le système de fichiers, au même titre que le RAID logiciel. C'est pourquoi il est classé dans la page [Storage](/storage) 

# Montage d'un système de fichiers
- [Montage des système de fichiers sous Linux](/filesystems/linux-mounts)
{.links-list}

# Commandes
## Déterminer le système de fichiers d'une partition
```bash
file -s -L DEVICE
```
> L'option `-L` permet de suivre les liens symbolique (pratique pour les volumes logiques)
{.is-info}

## Top 10 des plus gros dossiers et fichiers
```bash
du -ahx MOUNTPOINT | sort -hr | head -10
```

# Références
- [File system - Wikipedia](https://en.wikipedia.org/wiki/File_system)
- [FileSystem - Debian Wiki](https://wiki.debian.org/FileSystem)
- [Storage Administration Guide Red Hat Entreprise Linux 7](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/index)
- [Storage Administration Guide Red Hat Entreprise Linux 6](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/storage_administration_guide/index)
{.links-list}