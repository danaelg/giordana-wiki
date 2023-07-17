---
title: Ceph
description: 
published: true
date: 2023-07-17T20:18:23.737Z
tags: storage, work-in-progress, block_storage, object_storage
editor: markdown
dateCreated: 2023-07-06T08:50:12.878Z
---

# Introduction
Ceph est une technologie de stockage distribué libre qui fournit des interfaces de stockage  [objet S3/Swifft](/storage/object), [bloc](/storage/block) ou [système de fichiers](/filesystems) ([CephFS](/filesystem/cephfs)).

# Fonctionnalités
Ceph fournit trois méthodes de stockage différentes : le stockage objet, le stockage bloc et le stockage sous forme de système de fichiers. Les fonctionnalités de ces trois méthodes sont résumés dans les onglets ci-dessous :
## Tabs {.tabset}
### Stockage objet
- RESTful Interface
- S3- and Swift-compliant APIs
- S3-style subdomains
- Unified S3/Swift namespace
- User management
- Usage tracking
- Striped objects
- Cloud solution integration
- Multi-site deployment
- Multi-site replication

### Stockage block
- Thin-provisioned
- Images up to 16 exabytes
- Configurable striping
- In-memory caching
- Snapshots
- Copy-on-write cloning
- Kernel driver support
- KVM/libvirt support
- Back-end for cloud solutions
- Incremental backup
- Disaster recovery (multisite asynchronous replication)

### CephFS (système de fichiers)
- POSIX-compliant semantics
- Separates metadata from data
- Dynamic rebalancing
- Subdirectory snapshots
- Configurable striping
- Kernel driver support
- FUSE support
- NFS/CIFS deployable
- Use with Hadoop (replace HDFS)

# Fonctionnement
Ceph est une solution de stockage distribué qui s'appuie sur [RADOS](https://ceph.io/assets/pdfs/weil-rados-pdsw07.pdf). Il s'architecture sous forme de cluster que l'on nomme *Ceph Storage Cluster*. Voici une illustration libre des différents éléments que l'on trouve dans un cluster Ceph avec leurs interractions :

![architecture_ceph.svg](/storage/ceph/architecture_ceph.png =50%x)
*Schéma d'interraction entre les éléments d'un cluster Ceph - Danaël Giordana*

Sur l'illustration, les rectangles bleu représentent les services internes à Ceph qui doivent s'exécuter sur un ou plusieurs noeuds. 

Les I/Os sont réalisés par un [client](/storage/ceph#client) qui représente les données sous forme d'objets. Il récupère également la [cluster map](/storage/ceph#cluster-map) auprès du service [monitor](/storage/ceph/monitor) qui, avec les informations du [pool](/storage/storage/ceph#pool), permettent de déterminer le [placement groups (PG)](/storage/ceph#placement-group) dans lequel l'objet doit se trouver. L'algorithme [CRUSH](https://ceph.io/assets/pdfs/weil-crush-sc06.pdf) permet de définir dans quel OSD doit être placé un PG. Enfin, un disque est associé à un OSD, ce dernier a la charge de représenter les objets sur le disque. Il effectue cette action à l'aide du backend [Bluestore](/storage/ceph#bluestore).

> Lorsque l'on parle d'objets au niveau d'un cluster Ceph, on parle en fait d'un objet RADOS.
{.is-info}

Bien que cela ne ressorte pas sur l'illustration, il faut bien comprendre que le client communique **directement** avec les OSDs. C'est l'une des forces de Ceph ! Cela évite d'avoir un service central qui pourrait faire goulot d'étranglement.

Pour en savoir plus sur chaque service :
- [Monitors (`ceph-mon`)](/storage/ceph/monitor)
- [Managers (`ceph-mgr`)](/storage/ceph/manager)
- [OSDs (`ceph-osd`)](/storage/ceph/osd)
- [MDSs (`ceph-mds`)](/storage/ceph/mds)
{.links-list}

## Client
Le client est l'élément qui interragi avec un cluster Ceph pour réaliser des I/Os. Ce dernier communique directement avec les OSDs grâce à la [cluster map](/storage/ceph#cluster-map). C'est donc un programme externe au cluster. Le client utilise des librairies différente pour chaque interface de stockage (object S3/Swift, Bloc ou système de fichier). Voici un schéma d'architecture haut niveau :

![ceph-client-architecture.webp](/storage/ceph/ceph-client-architecture.webp =50%x)
*Schéma d'architecture haut niveau d'un client Ceph - [Ceph Documentation](https://docs.ceph.com/en/latest/architecture/#ceph-clients)*



* [Ceph clients - Architecture - Ceph Documentation](https://docs.ceph.com/en/latest/architecture/#ceph-clients)
{.links-list}

## Processus de lecture/écriture
## Objet (RADOS)

## Cluster Map
La cluster map doit être connu des clients et des OSDs, c'est ce qui permet la communication directe entre les deux éléments. C'est le service [Monitor](/storage/ceph/monitor) qui se charge de maintenir une copie de la cluster map.

La *Cluster Map* est composé de cinq maps :
- La monitor map 
- L'OSD map
- La PG (Placement Group) map
- la CRUSH map
- la MDS map

En d'autres termes, elle contient l'ensemble des informations du cluster, elle est donc centrale dans le fonctionnement de ce dernier. C'est pourquoi, il est recommandé d'avoir au moins trois services [monitor](/storage/ceph/monitor).

* [Cluster Map - Architecture - Ceph Documentation](https://docs.ceph.com/en/latest/architecture/#architecture-cluster-map)
{.links-list}

### Monitor map
La monitor map contient le `fsid` (identifiant unique du cluster), l'emplacment (adresse + port) des services monitor, l'epoch, la date de création et de dernière modification de la map.

On peut afficher la map avec la commande :
```bash
ceph mon dump
```

### OSD map
L'OSD map contient également le `fsid`, la date de création et de la dernière modification de la map, la liste des [pools](/storage/ceph/ceph#pool), des tailles de replica, des numéros des PG ainsi que la liste des service [OSD](/storage/ceph/osd) avec leur statut.

On peut afficher la map avec la commande :
```bash
ceph osd dump
```

### PG map
La PG map contient la version PG, sa date, le dernier epoch de l'OSD map et des détails sur les PGs (PG ID, l'*up set* et l'*active set*, le statut des PGs et des statistiques d'usage des données pour chaque pool).

### CRUSH map
La CRUSH map contient la liste des disques, la hiérarchie du domaine de défaillance et les règles de parcours de cette hiérarchie lors du stockage des données.

On peut afficher la map en la récupérant compilé :
```bash
ceph osd getcrushmap -o COMPILED_FILENAME
```
puis en la décompilant avec la commande :
```bash
crushtool -d COMPILED_FILENAME -o FILENAME
```

### MDS map
La MDS map contient un epoch, la date de création et de la dernière modification de la map, le pool de stockage des métadonnées, l'emplacement des services [MDS](/storage/ceph/mds) et leur statut. 

On peut afficher la map avec la commande :
```bash
ceph fs dump
```

## Pool
## Placement Group
## Bluestore

# Références
- [Ceph Documentation](https://docs.ceph.com/en/latest/)
- [Architecture - Ceph Documentation](https://docs.ceph.com/en/latest/architecture/)
- [Ceph - Wikipedia](https://en.wikipedia.org/wiki/Ceph_(software))
{.links-list}