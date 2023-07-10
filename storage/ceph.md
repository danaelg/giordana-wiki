---
title: Ceph
description: 
published: true
date: 2023-07-10T20:20:54.088Z
tags: storage, work-in-progress, block_storage, object_storage
editor: markdown
dateCreated: 2023-07-06T08:50:12.878Z
---

# Introduction
Ceph est une technologie de stockage distribué libre qui fournit du [stockage objet](/storage/object), [bloc](/storage/block) et sous forme de [système de fichiers](/filesystems) ([CephFS](/filesystem/cephfs)).

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

Sur l'illustration, les rectangles bleu représentent des services internes à Ceph qui doivent s'exécuter sur un ou plusieurs noeuds. 

On observe que les I/Os se font par l'intermédiaire d'un client qui représente les données sous forme d'objets. Il récupère également la [cluster map](/storage/ceph#cluster-map) auprès du service [monitor](/storage/ceph/monitor) qui, avec les informations du [pool](/storage/storage/ceph#pool), permettent de déterminer le [placement groups (PG)](/storage/ceph#placement-group) dans lequel l'objet doit se trouver. L'algorithme [CRUSH](https://ceph.io/assets/pdfs/weil-crush-sc06.pdf) permet de définir dans quel OSD doit être placé un PG.

> Lorsque l'on parle d'objets au niveau d'un cluster Ceph, on parle en fait d'un objet RADOS.
{.is-info}

L'une des forces de Ceph est la communication directe entre le client et les OSDs, il n'y a pas de service central qui pourrait faire goulot d'étranglement.

Pour en savoir plus sur chaque service :
- [Monitors (`ceph-mon`)](/storage/ceph/monitor)
- [Managers (`ceph-mgr`)](/storage/ceph/manager)
- [OSDs (`ceph-osd`)](/storage/ceph/osd)
- [MDSs (`ceph-mds`)](/storage/ceph/mds)
{.links-list}

## Processus de lecture/écriture
## Objet (RADOS)
## Cluster Map
## Pool
## Placement Group
## Bluestore

# Références
- [Ceph Documentation](https://docs.ceph.com/en/latest/)
- [Architecture - Ceph Documentation](https://docs.ceph.com/en/latest/architecture/)
- [Ceph - Wikipedia](https://en.wikipedia.org/wiki/Ceph_(software))
{.links-list}