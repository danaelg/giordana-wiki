---
title: Ceph
description: 
published: true
date: 2023-07-10T19:51:35.736Z
tags: storage, work-in-progress, block_storage, object_storage
editor: markdown
dateCreated: 2023-07-06T08:50:12.878Z
---

# Introduction
Ceph est une technologie de stockage distribué libre qui fournit du [stockage objet](/storage/object), [bloc](/storage/block) et sous forme de [système de fichiers](/filesystems) ([CephFS](/filesystem/cephfs)).

# Fonctionnalités
Ceph fournit trois méthodes de stockage différentes que sont : le stockage object, le stockage block et le stockage sous forme de système de fichiers. Les fonctionnalités de ces trois méthodes sont résumés dans les onglets ci-dessous :
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

On observe que les I/Os se font par l'intermédiaire d'un client qui récupère la [cluster map](/storage/ceph#cluster-map) auprès du service [monitor](/storage/ceph/monitor). C'est grâce à cette dernière qu'il va pouvoir créer les objects qui seront affecté à un [pool](/storage/ceph/pool). Le pool détermine le nombre de [placement group](/storage/ceph/placement-group) .

> Lorsque l'on parle d'objects au niveau d'un cluster Ceph, on parle en fait d'un objet RADOS.
{.is-info}

Pour en savoir plus sur chaque service :
- [Monitors (`ceph-mon`)](/storage/ceph/monitor)
- [Managers (`ceph-mgr`)](/storage/ceph/manager)
- [OSDs (`ceph-osd`)](/storage/ceph/osd)
- [MDSs (`ceph-mds`)](/storage/ceph/mds)
{.links-list}

## Cluster Map
## Placement Group


# Références
- [Ceph Documentation](https://docs.ceph.com/en/latest/)
- [Ceph - Wikipedia](https://en.wikipedia.org/wiki/Ceph_(software))
{.links-list}