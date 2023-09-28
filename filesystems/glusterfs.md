---
title: GlusterFS
description: 
published: true
date: 2023-09-28T20:37:28.882Z
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

## Trusted Storage Pool (TSP)
Les serveurs faisant partie d'un cluster GlusterFS sont regroupés dans un Trusted Storage Pool (TSP). Un TSP peut contenir un ou plusieurs serveurs de stockage.

# Configuration
## Installation
- [Install - Gluster Docs](https://docs.gluster.org/en/latest/Install-Guide/Install/#glusterfs)
{.links-list}

## Configuration du TSP
Un serveur qui démarre le service *glusterd* fait partie de son propre TSP, pour ajouter un serveur à un TSP, il suffit [d'ajouter un serveur au TSP](/filesystems/glusterfs#ajout-dun-serveur-au-tsp)

## Configuration des disques
### Paritionnement et formatage
GlusterFS fonctionne par dessus un système de fichier existant. Il est donc nécessaire de partitionner le disque et de formater la ou les partitions qui seront alloué au volume GlusterFS. GlusterFS recommande l'usage de [XFS](/filesystems/xfs).

### Montage automatique
Tout comme NFS, les partitions mises à disposition de GlusterFS doivent être monté automatiquement (Voir [Montage des systèmes de fichiers sous Linux](/filesystems/linux-mounts)).

## Configuration d'un volume GlusterFS


# Commandes
## TSP
### Ajout d'un serveur au TSP
```
gluster peer probe SERVER
```
> La commande ci-dessus ajoute le serveur défini par SERVER dans le TSP. Il est possible d'indiquer une adresse IP ou un nom DNS. Un serveur déjà présent dans un TSP ne peut pas être ajouté.
{.is-info}

### Afficher le statut des hôtes du TSP
```
gluster peer status
```

### Lister les serveurs présent dans un TSP
```
gluster pool list
```


# Ressources
- [Gluster Docs](https://docs.gluster.org/en/latest/)
{.links-list}