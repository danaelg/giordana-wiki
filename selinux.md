---
title: SELinux
description: 
published: true
date: 2023-08-02T18:58:57.052Z
tags: linux, selinux, mac
editor: markdown
dateCreated: 2023-08-02T15:07:23.662Z
---

# Introduction

# Fonctionnement
## Utilisateurs SELinux
Les utilisateurs SELinux ne sont pas équivalent aux utilisateurs linux, par exemple, il ne peut pas y avoir de changement d'utilisateur SELinux pendant une sessions, contrairement aux utilisateurs linux (via la commande `sudo` ou `su`).

Généralement plusieurs utilisateurs linux partagent le même utilisateur SELinux.

La convention de nommage des utilisateurs SELinux consiste à ajouter le suffixe *_u* (ex: *user_u*).

## Roles
Un utilisateur SELinux peut avoir un ou plusieurs roles. Les rôles sont définis par les politiques.

## Types
Les types sont le moyen principal pour déterminer l'accès. Le type d'un processus est aussi appelé *domaine*.

La convention de nommage des types consiste à ajouter le suffixe *_t* (ex: *user_t*).

# Commandes

```bash
ls -Z PATH
```

# Références
- https://debian-handbook.info/browse/fr-FR/stable/sect.selinux.html
- https://selinuxproject.org/page/BasicConcepts
{.links-list}