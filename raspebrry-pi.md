---
title: Raspberry Pi
description: 
published: true
date: 2023-06-27T14:29:44.411Z
tags: hardware, raspberry-pi
editor: markdown
dateCreated: 2023-06-20T19:20:19.517Z
---

# Introduction

# Configuration
## Activer le serveur SSH sans démarrage
1. Insérer la carte SD dans un ordinateur
2. Montez la partition `boot`
3. Créez un fichier vide `ssh` à la racine
4. Démonter la partition

## Définir l'adresse IP sans démarrage
1. Montez la partition `rootfs`
2. Modifiez le fichier `/etc/dhcpcd.conf`
```
interface eth0
static ip_address=IP_CIDR
static routers=GATEWAY_IP
static domain_name_servers=DNS_IP
```
> *IP_CIDR*: IP au format CIDR que l'on souhaite configurer, ex: `192.0.2.10/24`
> *GATEWAY_IP*: IP de la passerelle, ex: `192.0.2.1`
> *DNS_IP*: IP du serveur DNS, ex: `1.1.1.1`