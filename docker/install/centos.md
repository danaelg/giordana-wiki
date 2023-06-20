---
title: Installation de Docker sur CentOS
description: 
published: true
date: 2023-06-20T11:50:36.962Z
tags: linux, docker, centos
editor: markdown
dateCreated: 2023-06-20T11:46:44.939Z
---

# Installation du dépôt
```bash
yum install -y yum-utils
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
# Installation de Docker
```bash
yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

# Référence
- https://docs.docker.com/engine/install/centos/
{.links-list}