---
title: Target systemd
description: 
published: true
date: 2023-06-27T15:22:04.133Z
tags: systemd, systemd.unit, systemd.target
editor: markdown
dateCreated: 2023-06-23T07:35:09.999Z
---

# Introduction
Les *targets* sont des types d'unit utilisés pour grouper des units et pour créer des étapes dans le processus de démarrage. Ils se veulent être le remplaçant des *runlevels* SysV.

# Configuration
Les fichiers de configuration target doivent être nommé avec `.target`. Le fichier ne contient pas d'options particulière par rapport aux fichiers d'units traditionnels. Référez-vous à la page des [units](/systemd/unit)

# Exemple
Voici un exemple de target : `emergency-net.target` :
```ini
[Unit]
Description=Emergency Mode with Networking
Requires=emergency.target systemd-networkd.service
After=emergency.target systemd-networkd.service
AllowIsolate=yes
```

# Références
- [systemd.target - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.target.html)
{.links-list}