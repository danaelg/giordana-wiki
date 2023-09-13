---
title: Cluster Map Ceph
description: 
published: true
date: 2023-09-13T20:17:23.307Z
tags: ceph
editor: markdown
dateCreated: 2023-08-30T17:53:59.842Z
---

# Introduction
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

# Détail de chaque map

## Monitor map
La monitor map contient le `fsid` (identifiant unique du cluster), l'emplacment (adresse + port) des services monitor, l'epoch, la date de création et de dernière modification de la map.

On peut afficher la map avec la commande :
```bash
ceph mon dump
```

## OSD map
L'OSD map contient également le `fsid`, la date de création et de la dernière modification de la map, la liste des [pools](/storage/ceph/ceph#pool), des tailles de replica, des numéros des PG ainsi que la liste des service [OSD](/storage/ceph/osd) avec leur statut.

On peut afficher la map avec la commande :
```bash
ceph osd dump
```

## PG map
La PG map contient la version PG, sa date, le dernier epoch de l'OSD map et des détails sur les PGs (PG ID, l'*up set* et l'*active set*, le statut des PGs et des statistiques d'usage des données pour chaque pool).

## CRUSH map
La CRUSH map contient la liste des disques, la hiérarchie du domaine de défaillance et les règles de parcours de cette hiérarchie lors du stockage des données.

On peut afficher la map en la récupérant compilé :
```bash
ceph osd getcrushmap -o COMPILED_FILENAME
```
puis en la décompilant avec la commande :
```bash
crushtool -d COMPILED_FILENAME -o FILENAME
```

## MDS map
La MDS map contient un epoch, la date de création et de la dernière modification de la map, le pool de stockage des métadonnées, l'emplacement des services [MDS](/storage/ceph/mds) et leur statut. 

On peut afficher la map avec la commande :
```bash
ceph fs dump
```