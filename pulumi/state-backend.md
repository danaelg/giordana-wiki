---
title: State & Backend
description: 
published: true
date: 2023-08-21T12:49:00.425Z
tags: pulumi, pulumi_backend, pulumi_state
editor: markdown
dateCreated: 2023-08-21T12:49:00.425Z
---

# Introduction
Les states sont des fichiers dont se sert Pulumi pour connaître l'état de l'infrastructure. Les states sont stockés dans le *backend* qui peut être le cloud Pulumi, un stockage type S3 (tel que AWS, Azure, Minio, Ceph) ou système de fichiers local.

# Fonctionnement
Les states se matérialisent par un dossier `.pulumi` dont l'arbrescence est la suivante :
```

```

# Configuration du backend
Par défaut, c'est le Cloud Pulumi qui est utilisé comme backend, mais on peut très bien en choisir un autre :
- [Système de fichiers local](/pulumi/state-backend/filesystem)
{.links-list}

# Commandes
## Connexion à un backend
```bash
pulumi login [URL] [FLAGS]
```
> Les informations de connexion sont stockées dans le fichier `~/.pulumi/credentials.json`
{.is-info}

## Déconnexion d'un backend
```bash
pulumi logout [URL] [FLAGS]
```
> A la déconnexion, les informations de connexion stockées dans le fichier `~/.pulumi/credentials.json` sont supprimées.
{.is-info}