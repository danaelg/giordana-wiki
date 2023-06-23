---
title: Unit systemd
description: 
published: true
date: 2023-06-23T07:52:04.131Z
tags: systemd, systemd.unit, ini
editor: markdown
dateCreated: 2023-06-23T06:25:09.939Z
---

# Introduction
Les éléments géré par systemd sont appelés *unit*. Ce sont des fichiers textes qui décrivent l'élément. On trouve plusieurs type d'unit, les plus courant étant les *targets* ou les *services*.

# Configuration
Les fichiers d'unit sont des fichiers textes formaté en INI qui peuvent être placé dans un certain nombre de répertoire :
**Unit système**:
- `/etc/systemd/system.control/*`
- `/run/systemd/system.control/*`
- `/run/systemd/transient/*`
- `/run/systemd/generator.early/*`
- `/etc/systemd/system/*`
- `/etc/systemd/system.attached/*`
- `/run/systemd/system/*`
- `/run/systemd/system.attached/*`
- `/run/systemd/generator/*`
- …
- `/usr/lib/systemd/system/*`
- `/run/systemd/generator.late/*`

**Unit utilisateur**:
- `~/.config/systemd/user.control/*`
- `$XDG_RUNTIME_DIR/systemd/user.control/*`
- `$XDG_RUNTIME_DIR/systemd/transient/*`
- `$XDG_RUNTIME_DIR/systemd/generator.early/*`
- `~/.config/systemd/user/*`
- `$XDG_CONFIG_DIRS/systemd/user/*`
- `/etc/systemd/user/*`
- `$XDG_RUNTIME_DIR/systemd/user/*`
- `/run/systemd/user/*`
- `$XDG_RUNTIME_DIR/systemd/generator/*`
- `$XDG_DATA_HOME/systemd/user/*`
- `$XDG_DATA_DIRS/systemd/user/*`
- …
- `/usr/lib/systemd/user/*`
- `$XDG_RUNTIME_DIR/systemd/generator.late/*`

Un fichier unit doit être nommé avec des caractère ASCII auquel est ajouté l'extension correspondant à son type (cf. [type d'unit](/systemd/unit#types-dunit)).

## Types d'unit
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
Les générateurs d'unit sont des exécutables placé par exemple dans `/usr/lib/systemd/system-generators/`. Systemd les exécute à un stade précoce du démarrage, avant même que les units soient chargées. Leur objectif est de générer dynamiquement des fichiers units.

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