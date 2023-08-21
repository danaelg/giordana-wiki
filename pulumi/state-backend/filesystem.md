---
title: Backend Pulumi - Système de fichiers local
description: 
published: true
date: 2023-08-21T12:35:57.896Z
tags: pulumi, pulumi_backend, pulumi_state
editor: markdown
dateCreated: 2023-08-21T12:35:57.896Z
---

# Introduction
Le système de fichier local peut être utilisé comme un backend Pulumi. Les states seront alors matérialisé par un ensemble de fichiers.

# Configuration
La commande suivant permet de définir le système de fichiers comme backend : 
```bash
pulumi login --local
```
Par défaut, les states seront placé dans le dossier `.pulumi` dans le répertoire utilisateur (`~/`). On peut définir le dossier manuellement en spécifiant l'URL `file://PATH`, où `PATH` est le chemin du répertoire, et en enlevant le flag `--local` :
```bash
pulumi login file:///home/danael/my-project
```
Ici, les states se trouverons dans le répertoire `/home/danael/my-project/.pulumi`

