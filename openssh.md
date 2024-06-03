---
title: OpenSSH
description: 
published: true
date: 2024-06-03T08:21:01.872Z
tags: linux, windows, openssh, software
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

### Afficher la clef publique à partir de la clef privée
```
ssh-keygen -yf PRIVATE_KEY
```

## Serveur
On retrouve le serveur OpenSSH sous le nom de `sshd` ou `openssh-server`

### Afficher le hash d'une clef publique
Les logs de connexion affiche le hash de la clef publique utilisé pour se connecter : 
```
Accepted publickey for jdoe from 192.0.2.10 port 54460 ssh2: ED25519 SHA256:U6IKv4ADYNnK63QFd6KzzQDqLhDJ3cR4QuN+lJNjrRk
```

Pour obtenir ce hash à partir du fichier `authorized_keys` de l'utilisateur, on peut utiliser cette commande :
```bash
while IFS= read -r line; do echo -n "$line" | awk '{print $2}' | base64 -d | sha256sum | awk '{print $1}' | xxd -r -p | base64 ; done < /home/jdoe/.ssh/authorized_keys
```
1. On lit le fichier line par ligne en utilisant `while IFS= read -r line`
2. Pour chaque ligne on exécute le commande suivante :
    1. `awk '{print $2}'`: Récupération chaîne base64 (sans commentaire ni préfixe ssh-XXX)
    2. `base64 -d`: Décodage chaine
    3. `sha256sum`: Calcule hash sha256 
    4. `awk '{print $1}'`: Récupération du hash uniquement
    5. `xxd -r -p`: Convertion en octets
    6. `base64`: Encodage en base64

---
## Références
- https://en.wikipedia.org/wiki/OpenSSH
{.links-list}