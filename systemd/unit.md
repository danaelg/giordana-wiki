---
title: Unit systemd
description: 
published: true
date: 2023-06-23T06:32:54.965Z
tags: systemd, systemd_unit
editor: markdown
dateCreated: 2023-06-23T06:25:09.939Z
---

# Introduction
Les éléments géré par systemd sont appelés *unit*. Ce sont des fichiers textes qui décrivent l'élément. On trouve plusieurs type d'unit, les plus courant étant les *targets* ou les *services*.


sont les fichiers contient les informations à propos d'un *service*, *socket*, *périphérique*, *point de montage*, *point de montage automatique*, *partitions*, *fichier swap*, *target*, *timer ou d'une surveillance de fichier ou dossier*.

En d'autres termes, un élément géré par systemd s'appelle un *unit*.

# Types d'unit
Il existe plusieurs type d'unit qui répondent à des besoins différents. Les type que l'on retrouve le plus courrament sont les **target** et les **service**.

- [systemd.service](/systemd/unit/service)
- [systemd.socket](/systemd/unit/socket)
- [systemd.device](/systemd/unit/device)
- [systemd.mount](/systemd/unit/mount)
- [systemd.automount](/systemd/unit/automount)
- [systemd.swap](/systemd/unit/swap)
- [systemd.target](/systemd/unit/target)
- [systemd.path](/systemd/unit/path)
- [systemd.timer](/systemd/unit/timer)
- [systemd.slice](/systemd/unit/slice)
- [systemd.scope](/systemd/unit/scope)
{.links-list}

# Générateur d'unit
Les générateurs d'unit sont des exécutables placé par exempel dans `/usr/lib/systemd/system-generators/`. Systemd les exécute à un stade précoce du démarrage, avant même que les units soient chargées. Leur objectif est de générer dynamiquement des fichiers units.

En savoir plus :
- [systemd.generator](/systemd/unit/generator)
{.links-list}

# Commandes
## Lister les units
```bash
systemctl list-units
```

# Références
- [systemd.unit - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)
{.links-list}