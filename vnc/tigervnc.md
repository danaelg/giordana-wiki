---
title: TigerVNC
description: 
published: true
date: 2024-02-16T12:32:59.768Z
tags: work-in-progress, vnc
editor: markdown
dateCreated: 2024-02-14T14:35:15.243Z
---

# Notes
TigerVNC est un couple de client/serveur VNC 

- Seule solution supporté par Red Hat
- Extension disponible pour améliorer la sécurité de l’authentification et de la session
- Authentification PAM disponible
- Déjà utilisé sur le R920 (amélioration possible en se plongeant dans la doc et en faisant des POC)
- Peu documenté
- Support multi-écran

TigerVNC est une implémentation de VNC qui intègre les programmes suivant : 
| Programme | Description |
| --- | --- |
| `Xvnc` | Serveur X VNC. Composant principal permettant d’assurer la liaison entre les clients X et les clients VNC. C’est un serveur 2 en 1 (serveur X et serveur VNC). A la différence des serveurs X traditionnel, Xvnc possède un écran virtuelle. Dans les versions récente de TigerVNC, Xvnc est appelé via vncsession (précédemment c'était avec vncserver) |
| `x0vncserver` | Serveur VNC s’interfaçant avec un serveur X existant (il n’intègre pas de serveur X comme le fait Xvnc). Cela permet de partager l'écran physique plutôt que d’en créer un virtuelle. |
| `vncsession` | Programme permettant de démarrer en environnement VNC. Il s’occupe de lire les fichiers de configuration et d’exécuter les programmes nécessaire à la création des sessions VNC. Généralement, c’est ce programme qui est lancé via systemd. |
| `vncserver` | Script perl anciennement utilisé pour démarrer une session VNC. Il était lancé par un utilisateur. Aujourd’hui remplacé par vncserver pour s'adapter à l'usage de SELinux et de systemd, son usage est déprécié. |
| `vncconfig` | Programme permettant de configurer et de contrôler une instance Xvnc en cours d’exécution |
| `vncpasswd` | Programme permettant de définir un mot de passe VNC. Il ne faut pas considérer ce mot de passe comme un mécanisme fiable |
| `vncviewer` | Client VNC |