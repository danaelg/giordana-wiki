---
title: Installation de Docker sur Debian ou Ubuntu
description: 
published: true
date: 2023-06-20T11:49:33.080Z
tags: linux, docker, ubuntu
editor: markdown
dateCreated: 2023-06-20T11:41:00.623Z
---

Sous Ubuntu, l'installation se fait de la façon suivante

# Installation des dépendances
```bash
apt install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

# Installation de la clef GPG du dépôt
```bash
mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/DISTRIB/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
> Pour une Debian, remplacer `DISTRIB` par `debian`
> Pour une Ubuntu, remplacer `DISTRIB` par `ubuntu`
{.is-info}

# Ajout du dépôt
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/DISTRIB \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
```
> Pour une Debian, remplacer `DISTRIB` par `debian`
> Pour une Ubuntu, remplacer `DISTRIB` par `ubuntu`
{.is-info}

# Installation de Docker
```bash
apt update
apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```


# Références
- https://docs.docker.com/engine/install/ubuntu/
- https://docs.docker.com/engine/install/debian/
{.links-list}