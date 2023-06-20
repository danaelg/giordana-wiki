---
title: Utilisation du transfert de port SSH
description: 
published: true
date: 2023-06-20T14:30:43.363Z
tags: openssh, networking
editor: markdown
dateCreated: 2023-06-20T14:23:47.026Z
---

# Introduction
Le transfert de port (port forwarding) est une technique visant à rendre accessible une utiliser SSH pour transférer des ports soit, d'une machine local vers une ressource distante (local forwarding), soit d'une machines distante vers une ressource locale (remote forwarding).

L'illustration suivante résume très bien les différents cas de figure
![port-forwarding.png](/openssh/port-forwarding.png)
*[Ivan Velichko - A Visual Guide to SSH Tunnels: Local and Remote Port Forwarding](https://iximiuz.com/en/posts/ssh-tunnels/)*

# Pré-requis
Les directives `AllowTcpForwarding` (local forwarding) et `GatewayPorts` (remote forwarding) du fichier de configuration sshd contrôlent si le transfert de port est autorisé.

# Transfert de port local (local forwarding)
![port-forwarding-local.png](/openssh/port-forwarding-local.png =50%x)

# Références
- https://iximiuz.com/en/posts/ssh-tunnels/
- https://linux.die.net/man/5/sshd_config
{.links-list}