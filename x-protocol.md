---
title: X
description: 
published: true
date: 2024-02-16T08:18:49.867Z
tags: linux, x, x11
editor: markdown
dateCreated: 2024-02-16T08:18:49.867Z
---

# Introduction

X (aussi appelé X11) est le système de fenêtre que l’on retrouve sur les machines type Unix. Ce composant ajoute un niveau d’abstraction pour l’OS pour gérer l’affichage d’une GUI. On peut voir ça comme un protocole qui permettent à des clients et des serveurs l’implémentant de communiquer

# Fonctionnement
## Client X
Le client X est l’élément qui se connecte un serveur X (logique), Firefox et xterm sont des exemples d’applications intégrant un client X.

## Serveur X

Le serveur X interprète les données envoyés par les clients et rend le résultat sur l'écran qu’il gère. 


Concrètement le serveur X tourne sur une machine et les applications sont les clients X. L’utilisation du X Forwarding permet (ssh -X) permet de rediriger les requêtes des clients X de la machine distante sur notre propre serveur X, d’où, sur Mac OS, le besoin d’installer XQuartz : un serveur X. 

# Différence avec VNC

L’architecture client / serveur du protocole X permet d’afficher des fenêtres de machines distantes sur sa propre machine via l’installation d’un serveur X et l’usage du X Forwarding. En revanche, la bande passante nécessaire est importante et n’est pas adapté à un usage à grande échelle. 

VNC ne demande pas autant de bande passante, car le serveur X reste installé sur la machine distante et le rendu transmis au serveur VNC qui s’occupe de compresser les données et de les envoyer au client VNC.

Pour en savoir plus : 
- [VNC](/vnc)
{.links-list}

# Ressources
- [X Server-Client!! What the hell?](https://medium.com/mindorks/x-server-client-what-the-hell-305bd0dc857f)
- [X Window System - Wikipedia](https://en.wikipedia.org/wiki/X_Window_System)
{.links-list}