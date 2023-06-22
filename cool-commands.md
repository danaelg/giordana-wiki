---
title: Commandes cool 😎
description: Listes de commandes utiles qui doivent encore être triées
published: true
date: 2023-06-22T15:41:29.479Z
tags: linux, command, tips&tricks
editor: markdown
dateCreated: 2023-06-20T13:25:40.644Z
---

# La pire page
Cette page se veut d'être mal construite. L'objectif est de me permettre de déposer des commandes utiles en vue de créer des pages plus complètes. C'est une page fourre-tout.

## Obtenir la date de création du système
```bash
df / | awk '{print $1}' | grep dev | xargs tune2fs -l | grep create
```

## Top 10 des plus gros dossiers et fichiers
```bash
du -ahx MOUNTPOINT | sort -hr | head -10
```

## Recherche d'une chaîne de caractère dans tous les fichiers d'un répertoire
```bash
grep -rn PATERN PATH
```
> L'option -n permet d'afficher le numéro de ligne
{.is-info}

## Lister les fichiers supprimés toujours ouvert
```bash
lsof | grep "deleted"
```

## TOP 6 des processus consommateur de CPU
```bash
ps aux --sort pcpu | tail -n 6
```

## Afficher la crontab de tous les utilisateurs
```bash
grep . /var/spool/cron/crontabs/*
```

## Activer le versioning sur un bucket S3 avec aws-cli
```bash
aws s3api put-bucket-versioning --bucket BUCKET_NAME --versioning-configuration Status=Enabled
```

## Afficher la version de Debian
```bash
cat /etc/debian_version
```

## Afficher les information du noyau
```bash
uname -a
```
> -a : All
> -r : Kernel version
> -i : Hardware plateform
{.is-info}

## Afficher les informations Spanning Tree sous Cisco IOS
```
show spanning-tree
```

## Afficher les logs avec `journalctl`
```
journalctl
```
> --system : Journaux système
> -k : Journaux du noyaux
> -b : Journaux de démarrage
> -S --since=DATE : Journaux depuis DATE
> -U --until=DATE : Journaux jusqu'à DATE
> -u --unit=UNIT : Jounaux de l'unité UNIT
> -f --follow : Suivi en temps réel
{.is-info}

## Afficher les version et la distribution Linux
```bash
lsb_release -a
```

