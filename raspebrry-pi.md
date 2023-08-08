---
title: Raspberry Pi
description: 
published: true
date: 2023-08-08T19:39:46.697Z
tags: hardware, raspberry-pi
editor: markdown
dateCreated: 2023-06-20T19:20:19.517Z
---

# Introduction
La Rapsberry Pi est un nano-ordinateur monocarte intégrant un processeur ARM. Son format compact et sa faible consommation en font un candiat parfait pour des projets d'hébergement à bas coût ou pour de l'embarqué.

# Matériel
| Modèle | CPU | Architecture | Coeurs | Fréquence (GHz) | RAM (Go)
| --- | --- | --- | --- | --- | --- |
| 2B+ | ARM Cortex-A7 | ARMv7 (32bits) | 4 | 0.9 | 1 |
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

# Configuration d'une clef Wifi Asus usb-ac53 nano
La clef [Asus USB AC53 Nano](https://www.amazon.fr/Usb-ac53-Adaptateur-Wi-FI-Mu-MIMO-Double/dp/B06XQ2V4QM) est un carte wifi qui peut être utilisé sur une Raspberry Pi à condition de compiler le driver. Voici la procédure suivi sur une Rapsberry Pi 2B+ avec un kernel en version 6.1.21.

Récupération du code source :
```bash
git clone https://github.com/RinCat/RTL88x2BU-Linux-Driver
cd RTL88x2BU-Linux-Driver
```

Installation des dépendances :
```bash
apt install raspberrypi-kernel-headers
```

Compilation :
```bash
make ARCH=arm
```

Installation :
```bash
make install
``` 

Redémarrage
```bash
reboot
```

Après le redémarrage, la carte apparaît avec l'identifiant `wlan0`

- https://edimax.freshdesk.com/support/solutions/articles/14000062079-how-to-install-ew-7822ulc-adapter-on-raspberry-pi
- https://github.com/RinCat/RTL88x2BU-Linux-Driver
{.links-list}