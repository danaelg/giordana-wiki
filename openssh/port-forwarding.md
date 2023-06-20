---
title: Utilisation du transfert de port SSH
description: 
published: true
date: 2023-06-20T14:56:16.760Z
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

Ainsi, lorsque l'on appellera `localhost:8080` cela passera par notre tunnel SSH vers `server:80`.

Pour cela la commande est la suivante :
```bash
ssh -L localhost:8080:server:80 user@bastion
```
> On peut ajouter l'option `-T` pour ne pas associer de terminal
{.is-info}

Voici une image d'illustration :
![port-forwarding-local.png](/openssh/port-forwarding-local.png =50%x)
*[Ivan Velichko - A Visual Guide to SSH Tunnels: Local and Remote Port Forwarding](https://iximiuz.com/en/posts/ssh-tunnels/)*


Bien entendu, on peut remplacer `server:80` par `localhost:80` si le serveur web n'est pas sur un serveur distant mais sur le serveur SSH.

# Transfert de port distant (remote forwarding)
Ce cas de figure est exactement l'inverse du cas ci-dessus. Il sert à donner l'accès à une ressource auquel on à accès (par exemple une application en cours de développement localement) mais auquel le serveur distant n'a pas accès directement. C'est exactement ce principe qui est utilisé par des outils tel que [ngrok](https://ngrok.com/).

Pour cela la commande est la suivante :
```bash
ssh -R 0.0.0.0:8080:server:80 user@gateway
```
> On peut ajouter l'option `-T` pour ne pas associer de terminal
{.is-info}

Voici une image d'illustration :
![port-forwarding-remote.png](/openssh/port-forwarding-remote.png =50%x)
*[Ivan Velichko - A Visual Guide to SSH Tunnels: Local and Remote Port Forwarding](https://iximiuz.com/en/posts/ssh-tunnels/)*

Bien entendu, on peut remplacer `server:80` par `localhost:80` si le serveur web n'est sur un serveur distant mais en local.


# Références
- https://iximiuz.com/en/posts/ssh-tunnels/
- https://linux.die.net/man/5/sshd_config
{.links-list}