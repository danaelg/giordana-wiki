---
title: Ansible
description: 
published: true
date: 2023-07-02T20:11:59.481Z
tags: linux, windows, iac, yaml, automatisation, ansible, software
editor: markdown
dateCreated: 2023-06-20T08:37:49.880Z
---

# Introduction
Ansible est un outil d'automatisation de tâches utilisé pour gérer la configuration de serveur et déployer des applications. Il se veut sans agent, car il ne se repose que sur Python pour l'exécution des tâches et SSH pour la connexion aux machines.

# Fonctionnement
## Architecture
Pour se connecter aux machines, appelé *managed nodes*, Ansible va s'appuyer sur un fichier d'inventaire pour obtenir la liste des hôtes. Il est ensuite possible d'exécuter des actions sur ces dernières.
![ansible_architecture.svg](/ansible/ansible_architecture.svg =25%x)

La machine exécuant Ansible est appelé *control node*. A l'exécution d'une commande Ansible, ce dernier interprète les fichiers qui composent sa configuration pour former un script python. Ce script est transféré puis exécuté sur les *managed nodes*. Il n'y a aucun service qui tournent en arrière plan, **tout** se fait à l'exécution. 

- [Getting started with Ansible - Ansible Documentation](https://docs.ansible.com/ansible/latest/getting_started/index.html#getting-started-with-ansible)
{.links-list}

## Inventaires
La liste des hôtes (ou *managed nodes*), sur lesquels sont exécutés les actions, est défini par un ou plusieurs fichiers d'inventaire. Ce sont des fichiers INI ou YAML.

Pour en savoir plus :
- [Inventaire Ansible](/ansible/inventory)
{.links-list}

## Playbooks
Les actions exécutés sur les *managed nodes* sont définis par les playbooks. Ce sont des fichiers YAML.

Pour en savoir plus :
- [Playbook Ansible](/ansible/playbook)
{.links-list}

# Installation
-   [Installer Ansible](/ansible/install)
-   [ansible vs ansible-core](/ansible/ansible-vs-ansible-core)
{.links-list}

# Commandes
Ansible fournit un ensemble de commandes qui ont des usages précis :
- `ansible` : Permet d’exécuter une tâche sur des hôtes (aussi appelé ansible ad-hoc)
- `ansible-playbook` : Permet d’exécuter et tester des playbook
- `ansible-inventory` : Permet d’interpréter des fichiers d’inventaire et d’afficher le résultat
- `ansible-galaxy` : Permet de gérer les roles et les collections
- `ansible-vault` : Permet de (dé)chiffrer des fichiers et des variables
> Seuls les commandes les plus courrantes sont listés, il y en a d'autres (cf. [Using Ansible command line tools - Ansible Documentation](https://docs.ansible.com/ansible/latest/command_guide/index.html)).
{.is-info}

## Modules
### Lister les modules installés
```bash
ansible-doc -l
```
### Consulter la documentation d'un module Ansible
```bash
ansible-doc <module name>
```

# Références
- [Ansible Documentation](https://docs.ansible.com/ansible/latest/)
{.links-list}
