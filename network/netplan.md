---
title: Netplan
description: 
published: true
date: 2023-06-20T15:10:12.234Z
tags: linux, networking, netplan
editor: markdown
dateCreated: 2023-06-20T15:02:33.838Z
---

# Introduction
Netplan sert à la configuration réseau en pilotant directement NetworkManger ou systemd-networkd. Il permet ainsi d'ajouter une couche d'abstraction qui peut s'avérer très utiles.

![netplan-architecture.svg](/network/netplan-architecture.svg =25%x)

# Configuration
Les fichiers de configuration Netplan sont au format YAML dans `/etc/netplan`

## Définission d'une adresse IP statique
Voici un exemple de fichier de configuration permettant de configurer une adresse IP statique à l'interface enp0s3 :
```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      addresses:
        192.168.1.10/24
      nameservers:
        search: [example.org, example.com]
        addresses: [8.8.8.8, 8.8.4.4]
      routes:
        - to: default
          via: 192.168.1.1
```

# Références
- https://netplan.io/
{.links-list}