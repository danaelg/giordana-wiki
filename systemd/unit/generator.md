---
title: Générateur d'unit systemd
description: 
published: true
date: 2023-06-23T06:40:56.504Z
tags: systemd, systemd.unit
editor: markdown
dateCreated: 2023-06-23T06:36:48.567Z
---

# Introduction
Les générateurs d'unit sont des exécutables placé par exempel dans `/usr/lib/systemd/system-generators/`. Systemd les exécute à un stade précoce du démarrage, avant même que les units soient chargées. Leur objectif est de générer dynamiquement des fichiers units.

# Répertoires
Les générateurs peuvent être placés dans :

- `/run/systemd/system-generators/*`
- `/etc/systemd/system-generators/*`
- `/usr/local/lib/systemd/system-generators/*`
- `/usr/lib/systemd/system-generators/*`
- `/run/systemd/user-generators/*`
- `/etc/systemd/user-generators/*`
- `/usr/local/lib/systemd/user-generators/*`
- `/usr/lib/systemd/user-generators/*`

# Référence
- [systemd.generator - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.generator.html#)
{.links-list}