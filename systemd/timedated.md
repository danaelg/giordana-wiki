---
title: systemd-timedated.service
description: 
published: true
date: 2023-06-30T05:55:35.329Z
tags: systemd, systemd-timedated.service
editor: markdown
dateCreated: 2023-06-30T05:55:35.329Z
---

# Introduction

# Configuration

# Commandes
Le commande `timedatectl` est un client pour ce service

## Changer le fuseau horaire
```
timedatectl set-timezone TIMEZONE
```

## Lister les fuseaux horaire
```bash
timedatectl list-timezones
```

## Définir l'heure manuellement
```bash
timedatectl set-time HH:mm:ss
```
> Remplacer `HH:mm:ss` par l'heure, par exemple `22:04:25` pour 22h04 et 25s
{.is-info}

# Références
- [systemd-timedated.service - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd-timedated.service.html)
{.links-list}