---
title: Installer Ansible
description: 
published: true
date: 2023-06-20T09:18:07.620Z
tags: linux, ansible
editor: markdown
dateCreated: 2023-06-20T08:49:44.566Z
---

# Installer Ansible

Ansible est distribué sous deux formes différente : [ansible & ansible-core](/ansible/ansible-vs-ansible-core). L'installation n'est nécessaire que sur le *control-node*, car ansible est agentless.

Ces deux paquets sont souvent disponibles dans les gestionnaires de paquets. Mais la méthode officielle recommande l'utilisation de `pip`. C'est la méthode que nous décrirons ici, car cela assure d'avoir la version la plus à jour, alors que les paquets dans les gestionnaires peuvent être EOL.

## Debian/Ubuntu

### Dépendance
```
apt install python3-pip
```

### Installation
```
pip install --user ansible-core
```

