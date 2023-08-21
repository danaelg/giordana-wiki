---
title: Fournisseur de ressources Pulumi - Docker
description: 
published: true
date: 2023-08-21T14:46:43.661Z
tags: docker, pulumi, pulumi_resource_provider
editor: markdown
dateCreated: 2023-08-21T12:53:52.930Z
---

# Introduction
Pulumi peut gérer des ressources Docker via le fournisseur de ressources `Docker`

# Installation 
## Tabs {.tabset}
### Python
```bash
pip install pulumi-docker
```
> Sur un projet existant, il est préférable d'éditer le fichier `requirements.txt`
{.is-info}

# Configuration
Pour permettre à Pulumi de gérer ressources Docker, il faut lui donner accès à la socket du service via la commande :
```bash
pulumi config set docker:host unix:///var/run/docker.sock
```
> Il peut être nécessaire d'adapter les droits d'accès à la socket pour permettre à d'autres utilisateurs que root d'accéder à la socket docker
{.is-info}

> L'URL de la socket peut changer en fonction de la configuration, notamment si l'[API docker est exposé](/docker/api/expose-api-systemd)
{.is-info}

# Utilisation
Pour plus d'information sur l'utilisation de ce fournisseur : [Docker - Pulumi Registry](https://www.pulumi.com/registry/packages/docker/)



# Ressources
- [Docker - Pulumi Registry](https://www.pulumi.com/registry/packages/docker/)