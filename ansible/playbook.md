---
title: Playbooks Ansible
description: 
published: true
date: 2023-07-05T07:44:58.432Z
tags: ansible, work-in-progress, ansible-playbook
editor: markdown
dateCreated: 2023-07-03T17:34:59.507Z
---

# Introduction
Les playbooks sont des fichiers texte YAML qui servent de plan pour exécuter des tâches sur une liste d'hôte définie par les fichiers d'[inventaire](/ansible/inventory). Comme pour des script *bash* ou *powershell* classique, quasiment toutes les actions peuvent être décrite dans des playbooks. La différence réside dans l'approche descriptive qui s'oppose à une approche plus itérative que l'on retrouve dans les scripts.

# Syntaxe
Voici un exemple de playbooks :
![playbook-example.png](/ansible/playbook-example.png)

# Commandes
## Exécuter un playbook
```bash
ansible-playbook [-i INVENTORY] PLAYBOOK
```

# Références
- [Using Ansible playbooks - Ansible Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/index.html)
{.links-list}