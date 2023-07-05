---
title: Commandes cool 😎  | En vrac
description: Listes de commandes utiles qui doivent encore être triées
published: true
date: 2023-07-05T14:12:10.277Z
tags: linux, command, tips&tricks, work-in-progress
editor: markdown
dateCreated: 2023-06-20T13:25:40.644Z
---

# La pire page
Cette page se veut d'être mal construite. L'objectif est de me permettre de déposer des commandes ou des liens utiles en vue de créer des pages plus complètes. C'est une page fourre-tout.

# Liens en vrac
## SNMP MIB Explorer
- https://mib-explorer.com
- https://cric.grenoble.cnrs.fr/Administrateurs/Outils/MIBS/

## Système
- https://brendangregg.com/usemethod.html

# Commandes En vrac
## Désactiver le mode visuel de VIM
En mode commande
```
set mouse-=a
```

## Sauvegarder un fichier
```bash
cp /path/to/file.ext{,.$(date +%F)}
```
> Créer une copie du fichier /path/to/file.ext dans /path/to/file.ext.2023-06-28 (dans le cas où la date du jour est le 28 juin 2023).
{.is-info}

## Obtenir la date de création du système
```bash
df / | awk '{print $1}' | grep dev | xargs tune2fs -l | grep create
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

## Changer de hostname sous Linux avec `hostname`
```bash
hostname HOSTNAME
```

## Définir la date ou l'heure manuellement avec `date`
```bash
date --set {HH:mm:ss | YYYY-MM-DD}
```
> Remplacer `HH:mm:ss` par l'heure, par exemple `22:04:25` pour 22h04 et 25s
> Remplacer `YYYY-MM-DD` par la date, par exemple `2023-06-23` pour le 23 juin 2023
{.is-info}

# Tar
## Créer une archive avec Tar
```bash
tar -cf FILE FOLDER
```
> *FILE*: nom de l'archive (par convention on met l'extension .tar) 
> *FOLDER*: dossier à archiver
{.is-info}

## Extraire une archive avec Tar
```bash
tar -xf FILE
```
> *FILE*: nom de l'archive (par convention on met l'extension .tar) 
> *FOLDER*: dossier à archiver
{.is-info}

## Afficher le contenu du d'une archive avec Tar
```bash
tar -tf FILE
```
> *FILE*: nom de l'archive (par convention on met l'extension .tar) 
> *FOLDER*: dossier à archiver
{.is-info}

# Top
## Trier les processus en fonction d'un champ (colonne)
1. Entrer dans la fenêtre de gestion des champs en appuyant sur `f`
2. Sélectionner le champs sur lequel on veut fitrer avec les flèches
3. Appuyer sur `s`
4. Quitter la gestion des champs en appuyant sur `q`

## Afficher la commande complète des processus
Appuyer sur `c`

# Cisco IOS
## Afficher les informations Spanning Tree sous Cisco IOS
```
show spanning-tree
```

## Sauvegarder la configuration d'un équipement Cisco IOS
```
# copy running-config startup-config
```

## Annuler les changements effectué sous Cisco IOS
```
# copy startup-config running-config
```

## Réinitialiser la configuration d'un équipement Cisco IOS
```
# erase startup-config
```