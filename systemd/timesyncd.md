---
title: systemd-timesyncd.service
description: 
published: true
date: 2023-06-26T08:23:49.169Z
tags: systemd, systemd-timesyncd, timedatectl
editor: markdown
dateCreated: 2023-06-26T06:27:08.128Z
---

# Introduction
L'*unit* systemd `systemd-timesyncd.service` est un service gérant la synchronisation de l'horloge par le réseau via le protocole SNTP (Simple Network Time Protocol).

> Attention à ne pas le confondre avec le service [systemd-timedated](/systemd/timedated) qui s'occupe de la gestion du temps au niveau système (date, heure, fuseau horaire, activation/désactivation de la synchronisation par le réseau).
{.is-warning}

# Fonctionnement

# Configuration


# Commandes
La commande `timedatectl` contrôle le service `systemd-timesyncd` via les deux sous-commandes suivantes :
- timesync-status
- show-timesync
> Ces deux sous commandes ne fonctionnent qu'avec `systemd-timesyncd`, si un autre service de synchronisation de temps par le réseau est utilisé, ces sous-commandes ne pourront pas être utilisés.
{.is-warning}

## Afficher la configuration de `systemd-timesyncd`
```bash
timedatectl show-timesync
```

## Afficher le statut de la synchronisation
```bash
timedatectl timesync-status
```

# Référence
- [systemd-timesyncd - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd-timesyncd.service.html)
- [timedatectl - freedesktop](https://www.freedesktop.org/software/systemd/man/timedatectl.html)
{.links-list}