---
title: Ceph
description: 
published: true
date: 2023-07-06T08:53:21.485Z
tags: storage, work-in-progress, block_storage, object_storage
editor: markdown
dateCreated: 2023-07-06T08:50:12.878Z
---

# Introduction
Ceph est une technologie de stockage distribué libre qui fournit du stockage objet, block et sous forme de système de fichiers ([CephFS](/filesystem/cephfs)).

# Fonctionnalités
Ceph fournit trois méthodes de stockage différentes que sont : le stockage object, le stockage block et le stockage système de fichiers. Les fonctionnalités de ces trois méthodes sont résumés dans les onglets ci-dessous :
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

### Système de fichiers
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


# Références
- [Ceph Documentation](https://docs.ceph.com/en/latest/)
- [Ceph - Wikipedia](https://en.wikipedia.org/wiki/Ceph_(software))
{.links-list}