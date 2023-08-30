---
title: Ceph
description: 
published: true
date: 2023-08-30T19:19:46.985Z
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
La cluster map est l'élément que partagent le client et les OSDs pour permettre une communication directe.

La *Cluster Map* est composé de cinq maps :
- La monitor map 
- L'OSD map
- La PG (Placement Group) map
- la CRUSH map
- la MDS map

Pour en savoir plus : 
- [Cluster map](/storage/ceph/cluster-map)
{.links-list}

## Pool
## Placement Group
## Bluestore
## Réseau
Pour permettre aux clients de communiquer directement avec les différents services du cluster, ils doivent être configurés sur un même réseau, appelé *Public Network*. Un second réseau dédié aux oppérations entres les OSDs peut être configuré afin d'alléger la charge sur le *Public Network*, ce réseau est appelé *Cluster Network*.

L'illustration suivante montre comment les deux réseaux sont utilisés :
![ceph-networks.png](/storage/ceph/ceph-networks.png =50%x)
*Schéma d'interconnexion des services Ceph aux réseaux Public et Cluster - [Ceph Documentation](https://docs.ceph.com/en/latest/rados/configuration/network-config-ref/)*

La documentation recommande des vitesses de réseau de 10Gb/s minimum.

# Recommandations matérielles
Les recommandations de configuration hardware de la documentation Ceph me semble plutôt adapté à de gros cluster de stockage (plusieurs dizaines d'OSDs par machine). Les recommandations Proxmox pour un cluster de virtualisation hyperconvergé me semblent être une bonne alternative pour des clusters Ceph plus modeste.

- [Deploy Hyper-Converged Ceph Cluster - Precondition - Proxmox VE Administration](https://pve.proxmox.com/pve-docs/pve-admin-guide.html#_precondition)
- [Hardware Recommendations - Ceph Documentation](https://docs.ceph.com/en/latest/start/hardware-recommendations/)
{.links-list}

# Ressources
- [Ceph Documentation](https://docs.ceph.com/en/latest/)
- [Architecture - Ceph Documentation](https://docs.ceph.com/en/latest/architecture/)
- [Ceph - Wikipedia](https://en.wikipedia.org/wiki/Ceph_(software))
{.links-list}