---
title: EXT4
description: 
published: true
date: 2023-06-20T13:55:36.513Z
tags: linux, filesystem, ext4
editor: markdown
dateCreated: 2023-06-20T11:14:33.980Z
---

## Introduction
EXT4 est un système de fichier journalisé

## Formatter une partition
```bash
mkfs.ext4 DEVICE
```

## Etendre le système de fichier
```bash
resize2fs DEVICE
```

## Modifier le pourcentage de blocs réservés
Par défaut, le système de ficher EXT réserve des bloc pour les processus privilégiés (root). Lorsqu'une parition est utilisé à 100%, il peut s'avérer utile de diminuer cet espace temporairement.
```bash
tune2fs -m RESERVED_BLOCK_PERCENT DEVICE
```