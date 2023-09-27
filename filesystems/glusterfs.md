---
title: GlusterFS
description: 
published: true
date: 2023-09-27T19:19:25.385Z
tags: filesystem, glusterfs
editor: markdown
dateCreated: 2023-09-27T18:39:39.916Z
---

# Introduction
GlusterFS est un système de fichier distribué libre qui peut être utilisé pour des tâches avec un usage intensif des données (stockage cloud, streaming).

# Fonctionnalités
GlusterFS est un système de fichier distribué par le réseau qui est évolutif (scalable). Il offre les fonctionnalités suivantes :
- Support de plusieurs pétaoctet
- Support des centaines de clients
- Compatible POSIX
- Supporte du matériel standard
- Réplication
- Quota
- Géo-réplication
- Snapshots

# Fonctionnement
GlusterFS n'est pas vraiment un système de fichier, il fonctionne par dessus un système de fichiers existant ([la doc recommande XFS](https://docs.gluster.org/en/latest/Install-Guide/Common-criteria/#general-setup-principles)). C'est à dire que n'importe quel espace disponible sur un hôte peut potentiellement être utilisé. GlusterFS s'occupe de répartir les blocs de données entre les hôtes. Il aggrége les systèmes de fichiers des différents hôte pour n'en fournir qu'un seul :

![glusterfs.png](/filesystems/glusterfs/glusterfs.png =25%x)

# Configuration


# Ressources
- [Gluster Docs](https://docs.gluster.org/en/latest/)
{.links-list}