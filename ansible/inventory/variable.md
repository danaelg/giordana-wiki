---
title: Définir des variables dans les inventaires Ansible
description: 
published: true
date: 2023-07-03T09:59:45.701Z
tags: ansible, work-in-progress, ansible-inventory, ansible_variables
editor: markdown
dateCreated: 2023-06-30T09:46:27.964Z
---

# Introduction
Les variables peuvent être défini dans de nombreux endroits dont les fichiers d'inventaires. Les variables d'inventaire peuvent être défini au niveau des hôtes, des groupes ou dans des dossiers `host_vars` et `group_vars`.

# Variable de groupes
On peut définir des variables au niveau des groupes en ajoutant le mot clef `vars:` dans la cas du YAML et en ajoutant le suffixe `:vars` à la section INI
## Tabs {.tabset}
### YAML
```yaml
all:
  vars:
    my_var: Hello
    my_var2: World!
  hosts:
    foo.example.com:
    bar.example.com:
```
### INI
```ini
[all:vars]
my_var=Hello
my_var2=World!

foo.example.com
bar.example.com
```
# Variable d'hôtes
On peut définir des variables au niveau des hôtes, pour du YAML, elles sont ajoutésen en tant que couple `key: value` de l'hôte, pour du INI, elles sont ajoutés sur la même ligne que l'hôte en étant séparé par des espaces.
## Tabs {.tabset}
### YAML
```yaml
all:
  hosts:
    foo.example.com:
      my_var: Hello
      my_var2: World!
    bar.example.com:
      my_var: Coucou
```
### INI
```ini
foo.example.com my_var=Hello my_var2=World!
bar.example.com my_var=Coucou
```
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

# Références
- [How to build your inventory - Adding variables to inventory - Ansible Documentation](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#adding-variables-to-inventory)
{.links-list}
