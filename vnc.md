---
title: VNC
description: 
published: true
date: 2024-02-14T11:01:11.190Z
tags: linux, vnc, remote-control
editor: markdown
dateCreated: 2024-02-14T09:40:27.708Z
---

# Introduction
VNC (Virtual Network Computing) est un système de prise en main à distance de session graphique.


# Fonctionnement
VNC se base sur le protocole RFB (Remote Framebuffer Protocol)

# Historique
Initialement publié en 1998 par le laboratoire Oracle & Olivetti Research Laboratory en version 3.8, il a été récupéré par la société RealVNC Ltd qui a publié les version 3.7 et 3.8 en 2003 et 2007. En mars 2011, les spécifications de la version 3.8 sont publiés sous la [RFC 6143](https://datatracker.ietf.org/doc/html/rfc6143).

Depuis, Real VNC ltd continue le développement du protocole mais n'a pas publié de spécifications. On peut alors dire que le protocole RFB est propriétaire.   

La version actuelle du protocole est la version 6.

# Ressources
- [Virtual Network Computing - Wikipedia](https://en.wikipedia.org/wiki/Virtual_Network_Computing)
- [Remote Framebuffer Protocol - Wikipedia](https://en.wikipedia.org/wiki/RFB_protocol)
- [RFC 6143 - The Remote Framebuffer Protocol](https://datatracker.ietf.org/doc/html/rfc6143)
- [All you need to know about VNC remote access technology - RealVNC](https://discover.realvnc.com/what-is-vnc-remote-access-technology)
- [Virtual Network Computing - IEEE Internal Computing](https://help.realvnc.com/hc/article_attachments/6864678826397/tr.98.1.pdf)
{.links-list}


# Notes
### VNC vs Server X
En installant un serveur X sur l'ordinateur local, on peut afficher des applications qui s'exécutent sur l'ordinateur distant. En revanche, si l'ordinateur local crash, l'application meure. Avec VNC, la session serait coupé, mais l'affichage serait intacte (puisqu'il continue de tourner sur le serveur distant).
https://web.archive.org/web/20000815062533/http://www.uk.research.att.com/vnc/index.html

