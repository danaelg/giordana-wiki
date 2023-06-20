---
title: Exposer l'API docker via systemd
description: 
published: true
date: 2023-06-20T12:43:22.259Z
tags: linux, docker, systemd, api
editor: markdown
dateCreated: 2023-06-20T12:21:40.070Z
---

# Introduction
Pour exposer l'API Docker, on peut utiliser le fichier de configuration du service systemd pour passer des paramètres au binaire `dockerd`

# Configuration
## Modifier le fichier de configuration du service
```bash
systemctl edit docker.service
```
Puis ajouter les lignes suivantes :
```ini
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://LISTEN_IP:2375
```
Exemple :
```ini
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375
```
> LISTEN_IP correspond à l'adresse IP sur laquel l'instance Docker écoutera. `0.0.0.0` signifie que l'API sera exposé sur toutes les interfaces. Mais on peut aussi spéicifier l'adresse IP d'une interface précise : `127.0.0.1` 
{.is-info}

## Redémarrer le service
```bash
systmectl daemon-reload
systemctl restart docker
```


> Pensez à [sécuriser l'accès à l'API](/docker/api/secure-access) !
{.is-danger}


---
## Reférences
- https://docs.docker.com/engine/install/linux-postinstall/#configuring-remote-access-with-systemd-unit-file
{.links-list}