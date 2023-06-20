---
title: Commandes cool 😎
description: Listes de commandes utiles qui doivent encore être triées
published: true
date: 2023-06-20T13:55:16.001Z
tags: linux, command, tips&tricks
editor: markdown
dateCreated: 2023-06-20T13:25:40.644Z
---

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