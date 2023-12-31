---
title: systemd-hostnamed.service
description: 
published: true
date: 2023-06-26T06:19:08.935Z
tags: systemd, systemd-hostnamed, hostnamectl
editor: markdown
dateCreated: 2023-06-23T17:59:09.765Z
---

# Introduction
Le service systemd *systemd-hostnamed* est un service permettant de gérer le nom d'hôte.

# Fonctionnement
Le service fournit une interface [D-Bus](https://en.wikipedia.org/wiki/D-Bus) pour gérer le nom d'hôte au travers de plusieurs variables :
- **Hostname**: Hostname courant 
- **StaticHostname**: Hostname statique défini dans `/etc/hostname`
- **PrettyHostname**: Hostname sour forme libre UTF8 
- **IconName**: Nom de l'icône respectant la spécification XDG
- **Chassis**: Type de d'équipement, doit être l'une des valeur suivante *desktop*", *laptop*, *server*, *tablet*, *handset*, *vm* ou *container*
- **Deployment**: Environnement de déploiement (ex: *production*, *development*, *integration*, *staging*)
- **Location**: Emplacement, doit être une chaîne de caratère sous forme libre 

Pour en savoir plus :
- [org.freedesktop.hostname1 - D-Bus Interface of systemd-hostnamed - freedesktop](https://www.freedesktop.org/software/systemd/man/org.freedesktop.hostname1.html)
- [D-Bus - Wikipedia](https://en.wikipedia.org/wiki/D-Bus)
{.links-list}

# Commandes
La commande `hostnamectl` est le programme client du service *systemd-hostnamed*.

## Changer le hostname 
```bash
hostnamectl hostname HOSTNAME
```
> Cela affecte également le hostname courant (`StaticHostname`) et le hostname sous forme libre (`PrettyHostname`) 
{.is-info}

# Références
- [systemd-hostnamed.service - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd-hostnamed.service.html)
- [hostnamectl - freedesktop](https://www.freedesktop.org/software/systemd/man/hostnamectl.html)
- [org.freedesktop.hostname1 - D-Bus Interface of systemd-hostnamed - freedesktop](https://www.freedesktop.org/software/systemd/man/org.freedesktop.hostname1.html)
{.links-list}