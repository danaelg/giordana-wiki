---
title: GlusterFS
description: 
published: true
date: 2023-09-28T21:45:13.420Z
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

## Brique

## Types de volumes
GlusterFS propose cinq types de volumes :
- Volume distribué
- Volume répliqué
- Volume répliqué distribué
- Volume dispersé
- Volume dispersé distribué

### Volume distribué
Un volume distribué est un volume dans lequel les fichiers sont répartis sur les différentes briques. Il n'y a pas de réplication ou de distribution des fichiers. Un fichier se trouve sur une brique. Ce type de volume permet d'utiliser l'espace de tous les disques du cluster.

![glusterfs-distributed_volume.png](/filesystems/glusterfs/glusterfs-distributed_volume.png =35%x)
*Illustration d'un volume distribué - [Creating Distributed Volumes Red Hat Gluster Storage 3.5](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.5/html/administration_guide/creating_distributed_volumes)*

Si l'on fait une analogie avec les systèmes [RAID](/storage/raid), ce type de volume serait équivalent à un RAID 0.

### Volume répliqué
Un volume répliqué est un volume dont les fichiers sont répliqués entre les briques. Il y a une réplication complète des fichiers sur chaque briques. Le nombre de réplica est déterminé à la création du volume et doit être égal au nombre de briques.

![glusterfs-replicated_volume.png](/filesystems/glusterfs/glusterfs-replicated_volume.png =35%x)
*Illustration d'un volume répliqué - [Creating Replicated Volumes Red Hat Gluster Storage 3.5](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.5/html/administration_guide/sect-creating_replicated_volumes)*

> Pour éviter les scénarios de split-brain, il est recommandé d'avoir au moins trois briques. Sinon, il est possible de créer un volume d'arbritage. [Arbiter volumes and quorum options in gluster - Gluster Docs](https://docs.gluster.org/en/latest/Administrator-Guide/arbiter-volumes-and-quorum/)
{.is-info}

Si l'on fait une analogie avec les systèmes [RAID](/storage/raid), ce type de volume serait équivalent à un RAID 1.

### Volume répliqué distribué
Un volume répliqué distribué est un volume qui tire partie des fonctionnalités des volumes distribué et répliqué. Cela nécessite un grand nombre de brique mais permet d'améliorer les performances de lecture.

![glusterfs-distributed_replicated_volume.png](/filesystems/glusterfs/glusterfs-distributed_replicated_volume.png =35%x)
*Illustration d'un volume répliqué distribué - [Creating Distributed Replicated Volumes Red Hat Gluster Storage 3.5](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.5/html/administration_guide/sect-creating_distributed_replicated_volumes)*

Si l'on fait une analogie avec les systèmes [RAID](/storage/raid), ce type de volume serait équivalent à un RAID 10.

### Volume dispersé
Un volume dispersé est un volume dans lequel les fichiers sont découpés en morceau sur le principe du code d'effacement (Erasure Coding) et où chaque morceau est distribué sur les différentes briques du cluster. Cela permet d'obtenir une optimisation de l'espace tout en permettant la perte d'une ou plusieurs briques.

![glusterfs-dispersed_volume.png](/filesystems/glusterfs/glusterfs-dispersed_volume.png =35%x)
*Illustration d'un volume dispersé - [Creating Dispersed Volumes Red Hat Gluster Storage 3.5](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.5/html/administration_guide/chap-red_hat_storage_volumes-creating_dispersed_volumes_1)*

Si l'on fait une analogie avec les systèmes [RAID](/storage/raid), ce type de volume serait équivalent à un RAID 5.

## Volume dispersé distribué

![glusterfs-distributed_dispersed_volume.png](/filesystems/glusterfs/glusterfs-distributed_dispersed_volume.png =35%x)
*Illustration d'un volume dispersé distribué - [Creating Distributed Dispersed Volumes Red Hat Gluster Storage 3.5](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.5/html/administration_guide/sect-creating_distributed_dispered_volumes_1)*

Si l'on fait une analogie avec les systèmes [RAID](/storage/raid), ce type de volume serait équivalent à un RAID 50.

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
- [Administration Guide Red Hat Gluster Storage 3.5](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.5/html/administration_guide/index)
{.links-list}