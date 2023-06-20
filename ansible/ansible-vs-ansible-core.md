---
title: ansible vs ansible-core
description: 
published: true
date: 2023-06-20T09:18:06.586Z
tags: ansible
editor: markdown
dateCreated: 2023-06-20T09:18:04.631Z
---

# ansible-core vs ansible
Depuis la version 2.10, Ansible est distribué sous deux paquets `ansible` et `ansible-core`, chacun possède son propre versionnage et cycle de vie

## ansible-core
ansible-core est le paquet le plus petit qui ne contient que le langage, le runtime et la collection ansible.builtin. 

Le numéro de version suit la convention déjà établie : (2.11, 2.12, etc.).

Le support est assuré de la version N à N-2

## ansible
ansible est le paquet communautaire, plus gros, il contient des collections en plus des éléments du paquet ansible-core. Le numéro de version suit une nouvelle convention (3.x, 4.x, etc.).

Le support est assuré uniquement pour la version N

## Repérer sa version
Vous pouvez voir la version d'ansible-core qui est installé :
```
ansible --version
ansible [core 2.14.0]
...
```

Cela n'indique pas si vous avez installer le paquet `ansible` ou `ansible-core`. Une façon de repérer quel paquet est installé est en listant les collections : `ansible-galaxy collection list`