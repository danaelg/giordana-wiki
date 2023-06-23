---
title: Ansible
description: 
published: true
date: 2023-06-23T12:25:12.638Z
tags: linux, windows, iac, yaml, automatisation, ansible, software
editor: markdown
dateCreated: 2023-06-20T08:37:49.880Z
---

# Introduction
Ansible est un outil d'automatisation de tâches utilisé pour gérer la configuration de serveur et déployer des applications.

Ansible se veut sans agent ne se reposant que sur Python pour l'exécution des tâches et SSH pour la connexion aux machines. 

# Installation
-   [Installer Ansible](/ansible/install)
-   [ansible vs ansible-core](/ansible/ansible-vs-ansible-core)
{.links-list}

# Fonctionnement
![ansible_architecture.svg](/ansible/ansible_architecture.svg =25%x)


- [Getting started with Ansible - Ansible Documentation](https://docs.ansible.com/ansible/latest/getting_started/index.html#getting-started-with-ansible)
{.links-list}

## Inventaire
Pour en savoir plus :
- [Inventaires](/ansible/inventory)
{.links-list}

## Playbook
Pour en savoir plus :
- [Playbooks](/ansible/playbook)
{.links-list}

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
