---
title: Raspberry Pi
description: 
published: true
date: 2023-07-04T15:16:22.617Z
tags: hardware, raspberry-pi
editor: markdown
dateCreated: 2023-06-20T19:20:19.517Z
---

# Introduction
La Rapsberry Pi est un nano-ordinateur monocarte intégrant un processeur ARM. Son format compact et sa faible consommation en font un candiat parfait pour des projets d'hébergement à bas coût ou pour de l'embarqué.

# Matériel
| Modèle | CPU | Architecture | Coeurs | Fréquence (GHz) | RAM (Go)
| --- | --- | --- | --- | --- | --- |
| 2B+ | ARM Cortex-A7 | ARMv7 | 4 | 0.9 | 1 |
| 3B | ARM Cortex-A53 | ARMv8 (64bits) | 4 | 1.2 | 1 |
| 3A+ | ARM Cortex-A53 | ARMv8 (64bits) | 4 | 1.4 | 0.5 |
| 3B+ | ARM Cortex-A53 | ARMv8 (64bits) | 4 | 1.4 | 1 |
| 4B | ARM Cortex-A72 | ARMv8 (64bits) | 4 | 1.5 | 1, 2, 4 ou 8 |

# Configuration sans redémarrage
Les configurations suivante peuvent être faites directement après avoir insérer la carte SD dans un ordinateur.
## Activer le serveur SSH sans démarrage
1. Montez la partition `boot`
2. Créez un fichier vide `ssh` à la racine : `touch ssh`
3. Démontez la partition

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
{.is-info}