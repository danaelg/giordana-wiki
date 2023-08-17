---
title: Playbooks Ansible
description: 
published: true
date: 2023-08-17T20:38:22.853Z
tags: ansible, work-in-progress, ansible-playbook
editor: markdown
dateCreated: 2023-07-03T17:34:59.507Z
---

# Introduction
Les playbooks sont des fichiers texte YAML qui servent de plan pour exécuter des tâches sur une liste d'hôte définie par les fichiers d'[inventaire](/ansible/inventory). Comme pour des script *bash* ou *powershell* classique, quasiment toutes les actions peuvent être décrite dans des playbooks. La différence réside dans l'approche descriptive qui s'oppose à une approche plus itérative que l'on retrouve dans les scripts.

# Syntaxe
Les playbooks sont écris au format YAML et sont composés d'un ou plusieurs *plays* qui sont eux-même composés d'une ou plusieurs tâches. Chaque tâche appelle un module qui contient un ensemble de paramètres.
Voici un exemple :
![playbook-example.png](/ansible/playbook-example.png =40%x)

## Mot clefs
Les mots clefs peuvent être considéré comme des attributs. On peut les utiliser à différent niveau. Dans l'exemple ci-dessus, on trouve les mots clefs `name`, `hosts`, `tasks`, `remote_user` Référez-vous à la documentation pour obtenir la liste complète des mots clefs :
- [Playbook Keyword - Ansible Documentation](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html)
{.links-list}


# Commandes
## Exécuter un playbook
```bash
ansible-playbook [-i INVENTORY] PLAYBOOK
```

# Références
- [Using Ansible playbooks - Ansible Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/index.html)
{.links-list}