---
title: systemd-timesyncd
description: 
published: true
date: 2023-06-26T10:01:26.478Z
tags: systemd, systemd-timesyncd, timedatectl
editor: markdown
dateCreated: 2023-06-26T06:27:08.128Z
---

# Introduction
L'*unit* systemd `systemd-timesyncd.service` est un service gérant la synchronisation de l'horloge par le réseau via le protocole SNTP (Simple Network Time Protocol).

> Attention à ne pas le confondre avec le service [systemd-timedated](/systemd/timedated) qui s'occupe de la gestion du temps au niveau système (date, heure, fuseau horaire, activation/désactivation de la synchronisation par le réseau).
{.is-warning}

# Configuration
Le service `systemd-timesyncd` lit sa configuration à partir des fichiers suivant :
- `/etc/systemd/timesyncd.conf`
- `/etc/systemd/timesyncd.conf.d/*.conf`
- `/run/systemd/timesyncd.conf.d/*.conf`
- `/usr/lib/systemd/timesyncd.conf.d/*.conf`

Commes pour tous les fichiers *systemd*, il s'agit d'un fichier texte au format INI.

## Section [Time]
Toutes les options du service sont groupés dans la section `[Time]`. Voici la liste des options
- **NTP**: Liste des serveurs NTP séparés par des espaces
- **FallbackNTP**: Liste des serveurs NTP secondaires séparés par des espaces
- **RootDistanceMaxSec**: Distance maximale accepté, c'est à dire le temps maximum qu'un paquet peut mettre à joindre le serveur. Si le serveur dépasse cette limite, *systemd-timesyncd* bascule sur un autre serveur.
- **PollIntervalMinSec** et **PollIntervalMaxSec**: Le temps minimum et maximum entre deux interrogation du serveur NTP.
- **ConnectionRetrySec**: Délai minimum entre deux tentatives de contact du serveur NTP.
- **SaveIntervalSec**: Période où l'heure actuelle enregistrée sur le disque, en l'absence de toute synchronisation récente à partir d'un serveur NTP. Utile lors que le système ne possède pas d'horloge RTC.

* [timesyncd.conf - freedesktop](https://www.freedesktop.org/software/systemd/man/timesyncd.conf.html)
{.links-list}

## Exemple
`/etc/systemd/timesynd.conf`
```ini
[Time]
NTP=fr.pool.ntp.org
FallbackNTP=0.fedora.pool.ntp.org 1.fedora.pool.ntp.org
```
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
- [timesyncd.conf - freedesktop](https://www.freedesktop.org/software/systemd/man/timesyncd.conf.html)
- [timedatectl - freedesktop](https://www.freedesktop.org/software/systemd/man/timedatectl.html)
{.links-list}