---
title: OpenSSH
description: 
published: true
date: 2023-06-20T14:14:35.446Z
tags: linux, windows, openssh
editor: markdown
dateCreated: 2023-06-20T14:14:35.446Z
---

# Introduction
OpenSSH est un outil de contrôle à distance basé sur le protocole SSH. Il permet de créer un tunel sécurisé entre un client et un serveur.

# Windows
OpenSSH est intégré à Windows 10 depuis la version 1803

# Utilisation
## Client
On retrouve le serveur OpenSSH sous le nom de `ssh` ou `openssh-client`
### Générer une paire de clef 
```bash
ssh-keygen -b 4096 -C 'user@hostname' -f '~/.ssh/id_rsa'
``` 
> *-b*: taille de la clef
> *-C*: Commentaire
> *-f*: Chemin du fichier de la clef privée, la clef publique sera positionné à côté avec l'extension `.pub`
{.is-info}

### Port forwarding
- [Utilisation du transfert de port SSH](/openssh/port-forwarding)

## Serveur
On retrouve le serveur OpenSSH sous le nom de `sshd` ou `openssh-server`

---
## Références
- https://en.wikipedia.org/wiki/OpenSSH
{.links-list}