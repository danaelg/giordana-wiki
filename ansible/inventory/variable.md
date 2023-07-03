---
title: Définir des variables dans les inventaires Ansible
description: 
published: true
date: 2023-07-03T14:37:18.733Z
tags: ansible, ansible-inventory, ansible_variables
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
Les variables peuvent être défini dans répertoire `group_vars` et `host_vars` situé à côté des fichiers d'inventaires. Ansible chargera alors les fichiers YAML présent des ces dossiers (respectivement les variables de groupe dans le dossier `group_vars` et les variables d'hôte dans le dossier `host_vars`).

Les fichiers YAML présent dans ces dossiers doivent porter le nom du groupe ou de l'hôte. Par exemple si on à un groupe *apache*, *mysql* et *proxy* ainsi qu'un hôte *foo.example.com* et *bar.example.com*, voici la structure possible
```bash
hosts # Fichier d'inventaire contenant la liste des hôtes et les groupes.
host_vars/foo.example.com # Fichier de variable YAML
group_vars/apache # Fichier de variable YAML
group_vars/mysql # Fichier de variable YAML
group_vars/proxy # Fichier de variable YAML
```

> Les fichier de variables peuvent prendre l'extension `.yml` ou `.yaml`
{.is-info}

Notez que pour les variables de groupes, il est possible de créer un sous-dossier portant le nom du groupe peut être créé. Ansible chargera l'ensemble des fichiers se trouvant à l'intérieur. Voici un exemple de structure :
```bash
group_vars/apache/apache_settings.yml # Fichier de variable YAML
group_vars/apache/connection_settings.yml # Fichier de variable YAML
...
```

Voici un exemple de fichier de variable au format YAML
```yaml
---
my_var: coucou
environment: prod
users:
  - name: John
    lastname: Doe
```

# Priorité des variables
Comme on l'a vu, les variables peuvent être défini à différents niveaux. Si un même variable est défini plusieurs fois quelle priorité s'applique ?
## Priorité dans le fichier d'inventaire
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
Pour voir quelle valeur est prise en compte au moment de l'exécution, on lance la commande :
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
Avec le même fichier d'inventaire `inventory.yml` que précédemment :
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
On définit les fichiers suivants :
`group_vars/all.yml`
```yaml
---
my_var: 'var from group_vars/all.yml'
```
`group_vars/mysql.yml`
```yaml
---
my_var: 'var from group_vars/mysql.yml'
```
`host_vars/apache-1.yml`
```yaml
---
my_var: 'var defined from host_vars/apache-1.yml'
```

Si on exécute la commande suivante :
```bash
ansible-inventory -i inventory.yml --list
```
On obtient le résultat suivant :
```json
[...]
{
	"apache-1": {
		"my_var": "var defined from host_vars/apache-1.yml"
	},
	"mysql-1": {
		"my_var": "var from group_vars/mysql.yml"
	},
	"mysql-2": {
		"my_var": "var from group_vars/mysql.yml"
	},
	"proxy-1": {
		"my_var": "var from group_vars/all.yml"
	}
}
[...]
```

On observe que les variables définies directement dans le fichier d'inventaire sont écrasés et que la même priorité "*au plus proche de l'hôte*" s'applique.

# Références
- [How to build your inventory - Adding variables to inventory - Ansible Documentation](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#adding-variables-to-inventory)
{.links-list}
