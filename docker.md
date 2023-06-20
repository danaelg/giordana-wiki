---
title: Docker
description: 
published: true
date: 2023-06-20T19:55:30.536Z
tags: docker, container, software
editor: markdown
dateCreated: 2023-06-20T11:37:39.821Z
---

# Installation
- [Installation sur Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Installation sur Debian](https://docs.docker.com/engine/install/debian/)
- [Installation sur CentOS](https://docs.docker.com/engine/install/centos/)
- [Installation sur RedHat](https://docs.docker.com/engine/install/rhel/)
{.links-list}

# Exposer l'API Docker
L'exposition de l'API Docker, permet de contrôler à distance l'instance Docker.

- [Exposer l'API via systemd](/docker/api/expose-api-systemd)
- [Exposer l'API via le fichier de configuration](/docker/api/expose-api-config-file)
{.links-list}
> Veillez à sécuriser l'accès à l'API !
{.is-danger}
- [Sécuriser l'accès à l'API](/docker/api/secure-access)
{.links-list}

# Gestion des images
```bash
docker image
```

## Supprimer les images inutilisés
```bash
docker image prune [option]
```
> L'option `-a` supprime les images sans référence (sans aucun conteneur associé)
{.is-info}

> Sans option seul les images pendantes (dangling images) sont supprimés, c'est à dire les images sans tag
{.is-info}

# Gestion des conteneurs
```bash
docker container
```