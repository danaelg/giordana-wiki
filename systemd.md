---
title: Systemd
description: 
published: true
date: 2023-06-23T06:27:35.892Z
tags: linux, systemd, software
editor: markdown
dateCreated: 2023-06-22T18:49:04.579Z
---

# Introduction
Systemd est un ensemble composants logiciel destiné aux systèmes Linux. Il s'agit avant tout d'un gestionnaire de système et de services s'exécutant en tant que PID 1 et assurant le démarrage du système. Il se veut être le remplaçant SysV.

Les autres composants de systemd permettent la gestion de journalisation des services ou la gestion basique du système tel que le nom d'hôte, la date, les locales, la connexion utilisateur, le réseau, la synchronisation temporelle par le réseau, les points de montage, la résolution de nom...

# Fonctionnalités
L'une des fonctionnalité phare de systemd est la parallélisation du démarrage des services. C'est ce qui a permis d'accélérer significativement le démarrage du système compaéré à SysV.

# Critiques
Bien que systemd soit devenu incourtounable sous Linux, il est souvent sujet à des critiques certains se tournant alors vers distribution sans systemd (*Systemd-Free Linux Distributions*) voire même vers des système BSD.

A ce sujet, les discussions sur le choix du système de démarrage par défaut de Debian sont très intéressantes : https://wiki.debian.org/Debate/initsystem
Lennart Poettering, créateur de systemd, exprime son point de vue dans un article répondant aux critiques qui lui sont faites : http://0pointer.de/blog/projects/the-biggest-myths.html

# Fonctionnement
A compléter
## Unit
Les éléments gérés par systemd sont appelé des **unit**. Il s'agit d'un fichier texte qui contient les informations à propos d'un *service*, *socket*, *périphérique*, *point de montage*, *point de montage automatique*, *partitions*, *fichier swap*, *target*, *timer* ou d'une *surveillance de fichier ou dossier*

En savoir plus : 
- [systemd.unit](/systemd/unit)
{.links-list}

# Autres composants
- [systemd-journald](/systemd/journald)
- [systemd-hostnamed](/systemd/hostnamed)
- [systemd-timedated](systemd/timedated)
- [systemd-timesyncd](systemd/timesyncd)
{.links-list}

# Références
- https://systemd.io/
- [Rethinking PID 1 - Pid Eins](http://0pointer.de/blog/projects/systemd.html)
- [systemd - Wikipedia](https://en.wikipedia.org/wiki/Systemd)
{.links-list}