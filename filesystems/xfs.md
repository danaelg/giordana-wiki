---
title: XFS
description: 
published: true
date: 2023-06-20T11:20:31.776Z
tags: linux, filesystem, xfs
editor: markdown
dateCreated: 2023-06-20T11:20:29.779Z
---

## Introduction
XFS est un système de fichiers journalisé intégré au noyau Linux depuis 2001.

## Formatter une partition
```bash
mkfs.xfs DEVICE
```

## Etendre le système de fichiers
```bash
xfs_growfs DEVICE
```