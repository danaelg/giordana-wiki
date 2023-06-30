---
title: systemd-timedated.service
description: 
published: true
date: 2023-06-30T06:55:47.332Z
tags: systemd, timedatectl, systemd-timedated.service
editor: markdown
dateCreated: 2023-06-30T05:55:35.329Z
---

# Introduction
Le service systemd *systemd-timedated.service* permet de gérer l'heure local et le fuseaux horaire du système. Il est également capable d'activer ou de désactiver la synchronisation temporelle par le réseau. Cette synchronisation n'est cependant pas géré par *systemd-timedated*, cette fonction est délégué à d'autres services compétent (par exemple *ntpd*, *chronyd* ou *[systemd-timesyncd](/systemd/timesyncd))*.

# Fonctionnement
## Interface D-Bus
Le service fournit une interface [D-Bus](https://en.wikipedia.org/wiki/D-Bus) pour gérer l'horloge du système au travers de plusieurs variables :
- **Timezone**: Fuseau horaire du système.
- **LocalRTC**: Indique si l'horloge [RTC](https://en.wikipedia.org/wiki/Real-time_clock) (heure du BIOS) est configuré pour utilisé le fuseau horaire UTC (`False`) ou le fuseau du système (`True`).
- **CanNTP**: Indique si un service permettant la synchronisation temporelle par le réseau est disponible.
- **NTP**: Indique si un service permettant la synchronisation temporelle par le réseau est activé.
- **NTPSynchronized**: Indique si le noyau retourne l'horloge comme synchronisé.
- **TimeUSec**: Heure du système
- **RTCTimeUSec**: Heure de la RTC.

Pour en savoir plus :
- [D-Bus - Wikipedia](https://en.wikipedia.org/wiki/D-Bus)
- [org.freedesktop.timedate1 - D-Bus interface of systemd-timedated](https://www.freedesktop.org/software/systemd/man/org.freedesktop.timedate1.html#)
{.links-list}

## Intéraction avec des services de synchronisation temporelle par le réseau
Pour savoir si un service NTP est disponible, *systemd-timedated* surveille les fichiers `*.list` présents dans le répertoire `ntp-units.d`. Ce dossier peut se trouver dans l'un des répertoire systemd (par exemple `/etc/systemd/`, `/run/systemd`, `/usr/lib/`, etc.). Chaque fichier contient le nom d'une *[unit](/systemd/unit)* par ligne.

Voici un exemple sur un système avec `systemd-timesyncd` et `chrony`.

```
# ls /usr/lib/systemd/ntp-units.d/
50-chronyd.list  80-systemd-timesync.list
```
Voici le contenu du fichier `50-chronyd.list`
```
chronyd.service
```

et le contenu du fichier `80-systemd-timesync.list`
```
systemd-timesyncd.service
```

Ainsi, *systemd-timedated* sait quels services sont disponibles et lequel il doit activer si on lui fait la demande (le premier de la liste).

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
- [org.freedesktop.timedate1 - D-Bus interface of systemd-timedated](https://www.freedesktop.org/software/systemd/man/org.freedesktop.timedate1.html#)
{.links-list}