---
title: Systemd
description: 
published: true
date: 2023-06-26T12:06:40.448Z
tags: linux, systemd, software
editor: markdown
dateCreated: 2023-06-22T18:49:04.579Z
---

# Introduction
Systemd est un ensemble composants logiciel destiné aux systèmes Linux. Il s'agit avant tout d'un gestionnaire de système et de services s'exécutant en tant que PID 1 et assurant le démarrage du système. Il se veut être le remplaçant SysV.

Il propose également des services permettant la gestion de la journalisation des services du nom d'hôte, de la date, des locales, de la connexion utilisateur, du réseau, etc.

# Fonctionnalités
L'une des fonctionnalité phare de systemd est la parallélisation du démarrage des services. C'est ce qui a permis d'accélérer significativement le démarrage du système comparé à SysV (en plus de ne pas se baser sur des scripts shell).

# Fonctionnement
Systemd est construit autour des *units*. Il en existe de nombreux types, on peut citer par exemple les *service*, *socket*, *périphérique* ou *target*.

En savoir plus : 
- [systemd.unit](/systemd/unit)
{.links-list}

# Autres services
Systemd se voulant être une suite logiciels permettant de gérer le système dans son ensemble, ce dernier intègre des services supplémentaires pour gérer par exemple la journalisation et ou le nom d'hôte.

Vous pouvez retrouver les pages de ces différents services :

- [systemd-journald](/systemd/journald)
- [systemd-hostnamed](/systemd/hostnamed)
- [systemd-timedated](/systemd/timedated)
- [systemd-timesyncd](/systemd/timesyncd)
{.links-list}

# Commandes
systemd peut se contrôler avec la commande `systemctl`

## Afficher le version de systemd
```bash
systemctl --version
```

## Afficher la configuration d'une unit
```bash
systemctl cat UNIT
```

# Critiques
Bien que systemd soit devenu incourtounable sous Linux, il est souvent sujet à des critiques certains se tournant alors vers des distributions sans systemd (*Systemd-Free Linux Distributions*) voire même vers des système BSD.

A ce sujet, les discussions sur le choix du système de démarrage par défaut de Debian sont très intéressantes : https://wiki.debian.org/Debate/initsystem
Lennart Poettering, créateur de systemd, exprime son point de vue dans un article répondant aux critiques qui lui sont faites : http://0pointer.de/blog/projects/the-biggest-myths.html

# Références
- https://systemd.io/
- [Rethinking PID 1 - Pid Eins](http://0pointer.de/blog/projects/systemd.html)
- [systemd - Wikipedia](https://en.wikipedia.org/wiki/Systemd)
{.links-list}
