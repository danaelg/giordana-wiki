---
title: Unit systemd
description: 
published: true
date: 2023-06-30T07:13:53.850Z
tags: systemd, systemd.unit, ini
editor: markdown
dateCreated: 2023-06-23T06:25:09.939Z
---

# Introduction
Les éléments géré par systemd sont appelés *unit*. Ce sont des fichiers textes qui décrivent la configuration de l'élément. On trouve plusieurs type d'unit, les plus courant étant les *targets* ou les *services*.

# Configuration
## Emplacement
Les fichiers d'unit sont des fichiers textes INI qui peuvent être placés dans un certain nombre de répertoires, les plus courant étant :
- `/etc/systemd/system`: pour les units systèmes
- `~/.config/systemd/user/`: pour les units utilisateurs

## Nommage
Un fichier unit doit être nommé avec des caractère ASCII auquel est ajouté l'extension correspondant à son type (cf. [type d'unit](/systemd/unit#types-dunit)). Par exemple, `network.target` est une target et `apache2.service` est un service.

## Sections
Comme pour tous les fichiers INI, les sections regroupent un ensemble d'options `key=value`. Chaque section porte un nom entre crochets : `[Section]`, si le nom de la section est préfixé de `x-` la section est ignoré par systemd, de même pour le nom des options.

Toutes les units ont en commun les sections `[Unit]` et `[Install]`. On trouve d'autres sections spécifique à chaque type (cf. [type d'unit](/systemd/unit#types-dunit)).

Pour plus d'info sur la syntaxe, référez-vous à la documentation : 
- [systemd.syntax - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.syntax.html#)
{.links-list}

### Section [Unit]
Voici quelques  options courantes pour la section `[Unit]`:
- **Description**: Chaîne de caractères définissant un titre à l'unit
- **Documentation**: Liste d'URIs séparés par des espaces mettant en lien la documentation. Types d'URIs supportés: `http://`, `https://`, `file:`, `info:` et `man:`.
- **Wants**: Liste d'units séparé par des espaces. Définit une dépendance faible aux units déclarées. Les units déclarées seront démarrés si cette unit l'est aussi. Si le démarrage des units déclarées échoue, cela n'a pas d'impact sur le démarrage de cette unit.
- **Conflicts**: Liste d'units séparé par des espaces. Définit des dépendances négatives, c'est à dire des units qui entreraient en conflit avec cette unit. Le démarrage de cette unit entraînera l'arrêt des units déclarées et inversement. 
- **Before** et **After**: Liste d'units séparé par des espaces. Définit un ordonnancement entre les units. Si l'unit `a.service` contient l'option `Before=b.service`, alors si le démarrage des deux services est lancé, `b.service` démarrera lorsque le service `a.service` aura terminé son démarrage. 

> Des exemples d'utilisation des différentes options de dépendance et d'ordonnancement sont listés ici : [Gestion des dépendances systemd](/systemd/unit/dependency)
{.is-info}

La liste complète des options se trouve ici :
- [systemd.unit - [Unit] Section Options - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#%5BUnit%5D%20Section%20Options)
{.links-list}



### Section [Install]
à compléter

## Exemple
Voici l'exemple d'une unit de type service nommée `hello-world.service`
```ini
[Unit]
Description=Hello World Service

[Service]
Type=simple
ExecStart=/usr/local/bin/helloWorld.sh
```
On remarque la présence de la section `[Unit]` que nous avons décrit plus haut et la section `[Service]` qui est spécifique aux units de type service (cf. [systemd.service](/systemd/unit)).

# Générateur d'unit
Les générateurs d'unit sont des exécutables placé par exemple dans `/usr/lib/systemd/system-generators/`. Systemd les exécute à un stade précoce du démarrage, avant même que les units soient chargées. Leur objectif est de générer dynamiquement des fichiers units.

En savoir plus :
- [systemd.generator](/systemd/unit/generator)
{.links-list}

# Types d'unit
Il existe plusieurs type d'unit qui répondent à des besoins différents. Les types que l'on retrouve le plus courrament sont les **target** et les **service**. Chaque type d'unit peut ajouter ses propres sections. Référez-vous à la page du type d'unit que vous voulez créer.

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
Systemd offre plusieurs commandes, la plus courante étant `systemd`. Cette commande peut prendre les options `--user` ou `--system` pour traîter les units système ou utilisateur. Sans options, on traîte par défaut les units système.

Dans la suite nous ne spécifierons pas ces deux options pour ne pas surcharger les exemples.

## Lister les units
```bash
systemctl list-units [--type=UNIT_TYPE]
```
> L'option `--type` permet de filtrer les units par type. `UNIT_TYPE` doit être remplacé par le type d'unit, par exemple `service`, `target` ou `timer` (cf. [type d'unit](/systemd/unit#types-dunit))
{.is-info}

## Lister les répertoires de chargement des units
```bash
systemd-analyze unit-paths
```

## Afficher la configuration d'une unit
```bash
systemctl cat UNIT
```

# Références
- [systemd.unit - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)
{.links-list}