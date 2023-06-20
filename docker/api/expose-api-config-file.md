---
title: Exposer l'API docker via le fichier de configuration
description: 
published: true
date: 2023-06-20T12:42:17.203Z
tags: docker, api
editor: markdown
dateCreated: 2023-06-20T12:42:15.156Z
---

# Introduction
Pour exposer l'API Docker, on peut utiliser le fichier de configuration `daemon.json`

# Configuration
## Editer le fichier `/etc/docker/daemon.json`
Editer le fichier `/etc/docker/daemon.json` et ajouter les lignes suivantes :
```json
{
  "hosts": ["unix:///var/run/docker.sock", "tcp://LISTEN_IP:2375"]
}
```
> LISTEN_IP correspond à l'adresse IP sur laquel l'instance Docker écoutera. `0.0.0.0` signifie que l'API sera exposé sur toutes les interfaces. Mais on peut aussi spéicifier l'adresse ip d'une interface précisé : `127.0.0.1` 
{.is-info}

## Rédamarrer le service
```bash
systemctl restart docker
```
> Pensez à [sécuriser l'accès à l'API](/docker/api/secure-access) !
{.is-danger}

# Reférences
- https://docs.docker.com/engine/install/linux-postinstall/#configuring-remote-access-with-daemonjson
{.links-list}