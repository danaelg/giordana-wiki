---
title: AppArmor
description: 
published: true
date: 2023-08-04T08:42:10.932Z
tags: linux, security, work-in-progress, selinux, mac, apparmor
editor: markdown
dateCreated: 2023-08-04T06:37:55.463Z
---

# Introduction

# Fonctionnement
AppArmor ne s'applique que sur les applications sur lequelles un profil est actif. Un profil peut être dans deux états : 
- **enforcement**: le profil applique les politiques en avertissant et en bloquant.
- **complain**: le profile applique les politiques en avertissant mais sans bloquer.

# Commandes
## Afficher le statut d'AppArmor
```bash
aa-status
```
Voici un exemple de retour :
```
    apparmor module is loaded.
    13 profiles are loaded.
    12 profiles are in enforce mode.
       /usr/lib/connman/scripts/dhclient-script
       /usr/share/gdm/guest-session/Xsession
       /usr/bin/evince-previewer
       /usr/sbin/tcpdump
       /usr/lib/cups/backend/cups-pdf
       /usr/bin/evince-thumbnailer
       /sbin/dhclient3
       /usr/bin/evince
       /usr/bin/virt-aa-helper
       /usr/sbin/cupsd
       /usr/sbin/libvirtd
       /usr/lib/NetworkManager/nm-dhcp-client.action
    1 profiles are in complain mode.
       /bin/foobash
    3 processes have profiles defined.
    3 processes are in enforce mode :
       /sbin/dhclient3 (3043) 
       /usr/sbin/libvirtd (2370) 
       /usr/sbin/cupsd (2425) 
    0 processes are in complain mode.
    0 processes are unconfined but have a profile defined.
```

# Références
- [Documentation - AppArmor](https://gitlab.com/apparmor/apparmor/-/wikis/Documentation)
- [apparmor - man page](http://man.he.net/?topic=apparmor&section=all)
{.links-list}
