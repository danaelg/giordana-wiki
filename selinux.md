---
title: SELinux
description: 
published: true
date: 2023-08-17T20:11:30.146Z
tags: linux, security, selinux, mac, apparmor
editor: markdown
dateCreated: 2023-08-02T15:07:23.662Z
---

# Introduction
SELinux (Security Enhanced Linux) est un module de sécurité du noyau Linux qui permet de contrôler l'accès aux ressources système en se basant sur [MAC (Mandatory Access Control)](/dac)

# Fonctionnement
## Utilisateurs SELinux
Les utilisateurs SELinux ne sont pas équivalent aux utilisateurs linux, par exemple, il ne peut pas y avoir de changement d'utilisateur SELinux pendant une sessions, contrairement aux utilisateurs linux (via la commande `sudo` ou `su`).

Généralement plusieurs utilisateurs linux partagent le même utilisateur SELinux.

La convention de nommage des utilisateurs SELinux consiste à ajouter le suffixe *_u* (ex: *user_u*).

## Roles
Un utilisateur SELinux peut avoir un ou plusieurs roles. Les rôles sont définis par les politiques.

### object_r
Le role `object_r` est un role générique qui regroupe l'ensemble des objets du système.

## Types
Les types sont le moyen principal pour déterminer l'accès.

La convention de nommage des types consiste à ajouter le suffixe *_t* (ex: *user_t*).

## Contexte
Tous les objets et processus du système ont un contexte. C'est ce qui permet de déterminer si l'accès entre un objet et un processus est autorisé.

Le contexte s'écris de la façon suivante :
```
user:role:type:range
 |    |    |    |
 |    |    |    └── (optionnel) range MLS
 |    |    └─────── type SELinux
 |    └──────────── role SELinux
 └───────────────── utilisateur SELinux
```

## Règles
Les règles permettent de définir les autorisations par type. Voici un exemple de règle :
```
allow user_t user_home_t:file { create read write unlink };
```
Cette règle autoriser le type *user_t* à créer (*create*), lire (*read*), écrire (*write*) et supprimer (*delete*) des objets de classe fichier (*file*) ayant pour type SELinux *user_home_t*.

## Booléen
Pour faciliter l'administration de SELinux, les développeurs d'application ou de distribution regroupent des règles sous forme de booléen.

La liste des booléens s'obtient avec la commande :
```bash
semanage boolean -l
```
Voici un extrait de booléens existant :
```
SELinux boolean                State  Default Description
privoxy_connect_any            (on   ,   on)  Allow privoxy to connect any
smartmon_3ware                 (off  ,  off)  Allow smartmon to 3ware
mpd_enable_homedirs            (off  ,  off)  Allow mpd to enable homedirs
xdm_sysadm_login               (off  ,  off)  Allow xdm to sysadm login
xen_use_nfs                    (off  ,  off)  Allow xen to use nfs
mozilla_read_content           (off  ,  off)  Allow mozilla to read content
ssh_chroot_rw_homedirs         (off  ,  off)  Allow ssh to chroot rw homedirs
mount_anyfile                  (on   ,   on)  Allow mount to anyfile
cron_userdomain_transition     (on   ,   on)  Allow cron to userdomain transition
xdm_write_home                 (off  ,  off)  Allow xdm to write home
openvpn_can_network_connect    (on   ,   on)  Allow openvpn to can network connect
```

L'activation ou la désactivation d'un booléen se fait via la commande :
```
semanage boolean -m {--on|--off} BOOLEAN
```

## Classe d'objet
SELinux possède de nombreuses classes d'objet, on peut citer par exemple les répertoire ou les fichiers (*dir* et *file*). Chaque classe possède un certain nombre de permissions, par exemple pour les fichiers, on trouvera les permissions *read*, *write*, *create* et *unlink*, tandis que pour les sockets on trouvera *connect*, *create* et *sendto*.

la liste complète des objets avec leur permission se trouve ici :
- [ObjectClassesPerms - SELinux Wiki](https://selinuxproject.org/page/ObjectClassesPerms)
{.links-list}

# Commandes
## Désactiver SELinux
```bash
setenforce 0
```
> Cela ne désactive pas SELinux mais le passe en mode permissif
{.is-info}

> La désactivation de SELinux doit être une opération de debug ! Penser à réactiver SELinux
> https://stopdisablingselinux.com/
{.is-warning}

## Activer SELinux
```bash
setenforce 1
```

## Changement de contexte temporaire `chcon`
La commande `chcon` permet de modifier le contexte d'un fichier de façon temporaire. Ainsi, les fichiers peuvent être recontextualisé à leur contexte d'origine.

## Changement de contexte permanent `semanage fcontext`
```bash
semanage fcontext -a -t TYPE FILE
```

## Afficher le contexte d'un objet
La plupart des commandes d'affichage des objets (quel que soit leur classe) permettent d'afficher le contexte de ce dernier avec le paramètre `-Z`. Par exemple `ls -Z` pour les fichiers ou `ps -Z` pour les processus.

## Recontextualier un fichier
Si un changement de contexte temporaire a été réalisé sur un fichier (via `chcon`), on peut le recontextualisé avec la commande :
`restorecon FILE`

## Recontextualiser tous les fichiers du système
Si des changements de contexte temporaires ont été réalisés sur plusieurs fichiers (via `chcon`), on peut les recontextualisé en faisant :
1. `touch /.autorelabel`
2. Redémarrer le système

# Références
- https://debian-handbook.info/browse/fr-FR/stable/sect.selinux.html
- [SELinux Wiki](https://selinuxproject.org/page/Main_Page)
{.links-list}