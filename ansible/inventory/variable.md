---
title: Définir des variables dans les inventaires Ansible
description: 
published: true
date: 2023-06-30T10:02:05.530Z
tags: ansible, ansible-inventory, ansible_variables
editor: markdown
dateCreated: 2023-06-30T09:46:27.964Z
---

# Introduction

# Variable de groupes
# Variable d'hôtes
# Dossiers *host_vars* et *group_vars*
# Priorité des variables
## Priorité dans le fichier d'inventaire
Il est possible de définir des variables dans le fichier d'inventaire à plusieurs niveaux : par hôte ou par groupes. Quelle priorité s'applique si une même variable est défini au niveau d'un hôte et au niveau d'un groupe auquel il appartient ?

Voici un exemple de fichier d'inventaire nommé `inventory.yml`
```yaml
all:
  vars:
    my_var: '!'
  children:
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
Pour voir la valeur des variables suite à l'exécution, on lance la commande :
```bash
ansible-inventory -i inventory.yml --list
```
On obtient le résultat suivant :
```json
[...]
{
  "apache-1": {
    "my_var": "Hello"
  },
  "mysql-1": {
    "my_var": "World"
  },
  "mysql-2": {
    "my_var": "World"
  },
  "proxy-1": {
    "my_var": "!"
  }
}
[...]
```

On voit qu'un ordre de priorité s'est appliqué, plus la variable est défini proche de l'hôte, plus elle est prioritaires.

## Priorité des dossiers de variables *host_vars* et *group_vars*


