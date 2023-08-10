---
title: iproute2
description: 
published: true
date: 2023-08-10T11:46:40.825Z
tags: linux, networking, iproute2, ss
editor: markdown
dateCreated: 2023-06-20T19:05:17.799Z
---

## Introduction
`iproute2` est un ensemble d'outils permettant la gestion réseaux niveau 3 et 4 du modèle [OSI](/network/osi).

Il est censé remplacé la suite d'outils `net-tools`. Le tableau suivant montre la correspondance des commandes entre les deux collections :
| Commande legacy (`net-tools`)       | Commande de remplacement (`iproute2`) | Utilité                                            |
| -------------------------------- | ---------------------------------- | -------------------------------------------------- |
| `ifconfig`                         | `ip address`, `ip link`                | Gestion des adresses IP et des interfaces          |
| `route`                            | `ip route`                           | Gestion des tables de routage                      |
| `arp`                              | `ip neighbour`                       | Gestion des voisins (ARP & ICMPv6)                 |
| `iptunnel`                         | `ip tunnel`                          | Gestion des tunnels                                |
| `nameif`, `ifrename`                 | `ip link set name`                   | Gestion des noms des interfaces                    |
| `ipmaddr`                          | `ip maddr`                           | Gestion du multicast                               |
| `netstat`                          | `ss`, `ip route`                       | Affichage d'informations et de statistiques réseau |
| `brctl`                            | `bridge`                             | Gestion des bridges                                |

# Installation
## Debian & dérivé
```bash
apt install iproute2
```

# Commandes
## Gestion des adresses, interfaces, tunnels et du routage - `ip`
### Afficher les interfaces
```bash
ip link show
```

### Allumer/éteindre une interface
```bash
ip link set DEVICE {up|down}
```
> *DEVICE*: Nom de l'interface associé à la route
{.is-info}

### Activer le mode promiscuous
```bash
ip link set DEVICE promisc on
```
> *DEVICE*: Nom de l'interface associé à la route
{.is-info}

### Afficher les adresses IP des interfaces
```bash
ip addr show
```

### Ajouter une adresse IP à une interface
```bash
ip addr add IP_CIDR dev DEVICE
```
> IP_CIRD: Adresse IP au format CIDR (ex: 192.0.2.10/24)
> DEVICE: Nom de l'interface
{.is-info}

### Afficher la table de routage
```bash
ip route show
```

### Ajouter une route
```bash
ip route add NETWORK_CIDR via GATEWAY_IP dev DEVICE
```
> *NETWORK_CIRD*: Adresse IP du réseau au format CIDR (ex: 192.0.2.0/24). Vous pouvez indiquer `default` pour configurer une route par défaut
> *GATEWAY_IP*: Adresse IP de la passerelle
> *DEVICE*: Nom de l'interface associé à la route
{.is-info}

### Supprimer une route
```bash
ip route del NETWORK_CIDR
```
> *NETWORK_CIRD*: Adresse IP du réseau au format CIDR (ex: 192.0.2.0/24). Vous pouvez indiquer `default` pour configurer une route par défaut
{.is-info}

### Afficher les sockets en écoute
```bash
ss -l
```
> Les options suivantes peuvent être utiles :
> `-t`: Affiche les sockets TCP
> `-u`: Affiche les sockets UDP
> `-p`: Affiche le processus associé à la socket (nécessite les droits root)
> `-n`: Empèche la résolution des noms de service 
{.is-info}

# Reférences
- https://en.wikipedia.org/wiki/Iproute2
- https://linux.die.net/man/8/ip
- https://linux.die.net/man/8/ss
{.links-list}