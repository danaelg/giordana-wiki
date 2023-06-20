---
title: Utilisation du transfert de port SSH
description: 
published: true
date: 2023-06-20T14:41:59.582Z
tags: openssh, networking
editor: markdown
dateCreated: 2023-06-20T14:23:47.026Z
---

# Introduction
Le transfert de port (port forwarding) est une technique visant à rendre accessible une utiliser SSH pour transférer des ports soit, d'une machine local vers une ressource distante (local forwarding), soit d'une machines distante vers une ressource locale (remote forwarding).

# Pré-requis
Les directives `AllowTcpForwarding` (local forwarding) et `GatewayPorts` (remote forwarding) du fichier de configuration sshd contrôlent si le transfert de port est autorisé.

# Transfert de port local (local forwarding)
Dans ce cas de figure, on a une ressource distante, par exemple un serveur web sur le port 80 auquel on n'accède pas. Grâce à SSH, on peut réaliser un transfert de port local, par exemple 8080, vers la ressource distante sur sur le port 80.

Ainsi, lorsque l'on appellera `localhost:8080` cela passera par notre tunnel SSH vers `80`.

Pour cela la commande est la suivante :
```bash
ssh -L localhost:8080:localhost:80 user@server
```
> le premier localhost correspond à notre machine, le second correspond à l'adresse de la resource distante, ici un serveur web local
{.is-info}

> On peut ajouter l'option `-T` pour ne pas associer de terminal
{.is-info}

Voici une image d'illustration :
![port-forwarding-local.png](/openssh/port-forwarding-local.png =50%x)
*[Ivan Velichko - A Visual Guide to SSH Tunnels: Local and Remote Port Forwarding](https://iximiuz.com/en/posts/ssh-tunnels/)*

Dans ce cas la ressource distante est un serveur web présent sur le même serveur que celui auquel on se connecte. Mais cela n'est pas forcément le cas. Par exemple, on peut avoir un serveur web auquel le serveur SSH peut se connecter mais pas nous. Le serveur SSH est alors un **bastion**.

Dans un tel cas, 

# Références
- https://iximiuz.com/en/posts/ssh-tunnels/
- https://linux.die.net/man/5/sshd_config
{.links-list}