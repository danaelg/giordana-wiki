---
title: Définir des variables dans les inventaires Ansible
description: 
published: true
date: 2023-06-30T09:46:27.964Z
tags: ansible, ansible-inventory, ansible_variables
editor: markdown
dateCreated: 2023-06-30T09:46:27.964Z
---

# Introduction


## Priorité des variables dans les fichiers d'inventaire
Il est possible de définir des variables dans le fichier d'inventaire à plusieurs niveaux : par hôte ou par groupes.
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
Si on lance la commande :
```bash
ansible-inventory -i INVENTORY_FILE --list
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

On voit qu'un ordre de priorité s'applique. Plus la variable est défini proche de l'hôte, plus elle est prioritaires.