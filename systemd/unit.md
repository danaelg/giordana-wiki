---
title: Unit systemd
description: 
published: true
date: 2023-06-27T15:28:59.888Z
tags: systemd, systemd.unit, ini
editor: markdown
dateCreated: 2023-06-23T06:25:09.939Z
---

# Introduction
Les éléments géré par systemd sont appelés *unit*. Ce sont des fichiers textes qui décrivent la configuration de l'élément. On trouve plusieurs type d'unit, les plus courant étant les *targets* ou les *services*.

# Configuration
## Emplacement
Les fichiers d'unit sont des fichiers textes formaté en INI qui peuvent être placé dans un certain nombre de répertoire, les plus courant étant :
- `/etc/systemd/system`: pour les units systèmes
- `~/.config/systemd/user/`: pour les units utilisateurs

## Nommage
Un fichier unit doit être nommé avec des caractère ASCII auquel est ajouté l'extension correspondant à son type (cf. [type d'unit](/systemd/unit#types-dunit)). Par exemple, `network.target` est un target et `apache2.service` est un service.

## Sections
Comme pour tous les fichiers INI, les sections regroupent un ensemble d'options `key=value`. Chaque section porte un nom spécifique, s'il est préfixé de `x-` la section est ignoré par systemd, de même pour les nom des options.

Nous ne décrirons que la section `[Unit]` et `[Install]`, les autres sections sont spécifiques à chaque type d'unit (cf. [type d'unit](/systemd/unit#types-dunit)).

Pour plus d'info sur la syntaxe, référez-vous à la documentation : 
- [systemd.syntax - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.syntax.html#)
{.links-list}

### Section [Unit]
Voici quelques  options courantes pour la section `[Unit]`:
- **Description**: Chaîne de caractères servant de titre de l'unit
- **Documentation**: Liste d'URIs séparés par des espaces. Types d'URIs supportés: `http://`, `https://`, `file:`, `info:` et `man:`.
- **Wants**: Liste d'units séparés par des espaces. Définit une dépendance faible aux units déclarés. Les units déclarés seront démarrés si cet unit l'est aussi. Si le démarrage des units déclarés échoue, cela n'a pas d'impact sur le démarrage de cet unit.
- **Conflicts**: Liste d'units séparés par des espaces. Définit des dépendances négatives, c'est à dire des units qui entreraient en conflit avec cet unit. Le démarrage d'un unit déclaré dans cette option stoppera cet unit et inversement. 
- **Before** et **After**: Liste d'units séparé par des espaces. Définit une séquence de dépendance entre les units. Si l'unit `a.service` contient l'option `Before=b.service`, alors si le démarrage des deux services est lancé, `b.service` démarrera lorsque le service `a.service` aura terminé son démarrage. 

La liste complète des options se trouve ici :
- [systemd.unit - [Unit] Section Options - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#%5BUnit%5D%20Section%20Options)
{.links-list}

### Section [Install]
à compléter


# Générateur d'unit
Les générateurs d'unit sont des exécutables placé par exemple dans `/usr/lib/systemd/system-generators/`. Systemd les exécute à un stade précoce du démarrage, avant même que les units soient chargées. Leur objectif est de générer dynamiquement des fichiers units.

En savoir plus :
- [systemd.generator](/systemd/unit/generator)
{.links-list}

# Types d'unit
Il existe plusieurs type d'unit qui répondent à des besoins différents. Les type que l'on retrouve le plus courrament sont les **target** et les **service**. Chaque type d'unit peut ajouter ses propres sections. Référez-vous à la page du type d'unit que vous voulez créer.

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

# Commandes
## Lister les units
```bash
systemctl list-units
```

## Lister les répertoires de chargement des units
```bash
systemd-analyze unit-paths {--user | --system}
```
> Sans option, la commande retourne les répertoires système.
{.is-info}

## Afficher la configuration d'un unit
```bash
systemctl cat UNIT
```

# Références
- [systemd.unit - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)
{.links-list}