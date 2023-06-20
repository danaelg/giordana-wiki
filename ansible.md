---
title: Ansible
description: 
published: true
date: 2023-06-20T19:29:18.402Z
tags: linux, windows, iac, yaml, automatisation, ansible
editor: markdown
dateCreated: 2023-06-20T08:37:49.880Z
---

# Introduction


# Installation
-   [Installer Ansible](/ansible/install)
-   [ansible vs ansible-core](/ansible/ansible-vs-ansible-core)
{.links-list}

# Inventaires

Les fichiers d'inventaire au format *INI* ou *YAML* permettent de lister les hôtes, de les grouper et de leurs associer des variables.

A titre personnel, je préfère le format YAML, tous les exemples seront donc avec ce format.

## Strucuture d'un fichier d'inventaire
### Exemple simple
```yaml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
```

### Hôtes dans plusieurs groupes
```yaml
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
    east:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    west:
      hosts:
        bar.example.com:
        three.example.com:
    prod:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    test:
      hosts:
        bar.example.com:
        three.example.com:
```

### Groupes imbriqués
```yaml
all:
  hosts:
    mail.example.com:
  children:
    east:
      hosts:
        foo.example.com:
        one.example.com:
        two.example.com:
    west:
      hosts:
        bar.example.com:
        three.example.com:
    prod:
      children:
        east:
    test:
      children:
        west:
```
## Priorité des variables dans les fichiers d'inventaire
Il est possible de définir des variables dans le fichier d'inventaire à plusieurs niveaux : par hôte, par groupes d'hôtes ou de façon globale.
```yaml
all:
  vars:
    var: '!'
    ansible_connection: local

apache:
  hosts:
    apache-1:
      my_var: 'Hello'

mysql:
  hosts:
    mysql-1:
	mysql-2:
  vars:
    my_var: 'World'

proxy:
  hosts:
    proxy-1:
```
Si on lance la commande :
```bash
ansible -m setup -a var=my_var -i inventory.yml all
```
On obtient le résultat suivant :
> apache-1 | SUCCESS => {
> 	"my_var": "Hello"
> }
> mysql-1 | SUCCESS => {
> 	"my_var": "World"
> }
> mysql-2 | SUCCESS => {
> 	"my_var": "World"
> }
> proxy-1 | SUCCESS => {
> 	"my_var": "!"
> }

Comme on peut le voir, un ordre de priorité s'applique de la définition la plus gloable à la plus spécifique. 

# Playbooks

# Modules
## Lister les modules installés
```bash
ansible-doc -l
```
## Consulter la documentation d'un module Ansible
```bash
ansible-doc <module name>
```

