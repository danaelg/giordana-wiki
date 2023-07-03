---
title: Inventaires Ansible
description: 
published: true
date: 2023-07-03T17:50:40.549Z
tags: yaml, ansible, work-in-progress, ansible-inventory
editor: markdown
dateCreated: 2023-06-21T06:49:31.521Z
---

# Introduction
Les fichiers d'inventaire au format *INI* ou *YAML* permettent de lister les hôtes, de les grouper et de leurs associer des variables.

A titre personnel, je préfère le format YAML, certains exemples ne seront que dans ce format.

# Inventaires statiques
## Exemple simple
### Tabs {.tabset}
#### YAML
```yaml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
         one.example.com:
    dbservers:
      hosts:
        two.example.com:
```
#### INI
```ini
mail.example.com

[webservers]
one.example.com

[dbservers]
two.example.com
```

## Hôtes dans plusieurs groupes
### Tabs {.tabset}
#### YAML
```yaml
all:
  children:
    webservers:
      hosts:
        one.example.com:
    dbservers:
      hosts:
      	two.example.com:
    east:
      hosts:
        one.example.com:
    west:
      hosts:
        two.example.com:
    prod:
      hosts:
        one.example.com:
        two.example.com:
```
> A titre personnel, je n'aime pas faire ce genre de chose, car cela peut surcharger l'inventaire. Je préfère alors imbriquer les groupes (voir exemple ci-dessous)

#### INI
```ini
[webservers]
one.example.com

[dbservers]
two.example.com

[east]
one.example.com

[west]
two.example.com

[prod]
one.example.com
two.example.com
```
> A titre personnel, je n'aime pas faire ce genre de chose, car cela peut surcharger l'inventaire. Je préfère alors imbriquer les groupes (voir exemple ci-dessous)

## Groupes imbriqués
### Tabs {.tabset}
#### YAML
```yaml
all:
  hosts:
    mail.example.com:
  children:
    prod:
      children:
        east:
          hosts:
            foo.example.com:
            one.example.com:
            two.example.com:
    test:
      children:
        west:
          hosts:
            bar.example.com:
            three.example.com:
```
#### INI
```ini
mail.example.com

[east]
foo.example.com
one.example.com
two.example.com

[west]
bar.example.com
three.example.com

[prod:children]
east

[test:children]
west
```

## Exemple complexe
Mon expérience m'a confronté à un cas un peu complexe où je devais avoir trois niveaux :
- L'environnement: `prod`, `preprod` ou `dev`
- L'application: Spécifique au métier, on définira `APP1`, `APP2`
- La fonction: `db`, `front`, `proxy`, etc.

Nous devions avoir la possibilité d'appliquer des actions sur :
- Tous les hôtes d'un environnement
- Tous les hôtes d'une application
- Tous les hôtes d'une fonction
- Tous les hôtes d'une même application et d'une même fonction (ex: *Les front de APP1*)

Cala a donné une structure d'inventaire relativement complexe mais assez simple à exploiter.

Les hôtes de chaque environnement ont été déclaré dans des fichiers différent. On a donc un fichier d'inventaire par environnement. Pour le reste du découpage, un groupe d'hôtes nommé `APPLICATION_FONCTION`. Ce group est ensuite membre de groupes nommé `APPLICATION` et `FONCTION` correspondant.

> `APPLICATION` et `FONCTION` sont remplacés par les valeurs adéquate
{.is-info}

Voici un exemple suivant :
```yaml
all:
  children:
    app1_front:
      hosts:
        prodapacheapp1:
        prodhttpdapp1:
    app2_front:
      hosts:
        prodapacheapp2:
        prodhttpdapp2:
    app1_bdd:
      hosts:
        prodmariadbapp1:    
    app2_bdd:
      hosts:
      	prodmariadbapp2:
  
  # Groupes de fonction
    front:
      children:
        app1_front:
        app2_front:
    bdd:
      children:
        app1_bdd:
        app2_bdd:
  # Groupes d'application  
    app1:
      children:
        app1_front:
        app1_bdd:
    app2:
      cildren:
        app2_front:
        app2_bdd:
```

# Variables
Les fichiers d'inventaire peuvent contenir des variables au niveau des hôtes ou des groupes.

Pour en savoir plus :
- [Définir des variables d'inventaire](/ansible/inventory/variable)
{.links-list}

# Inventaires dynamiques
Les inventaires dynamiques ammènent une approches différentes qui consiste, non pas lister les hôtes et les regrouper, mais à utiliser un plugin qui s'interface avec un système externe (par exemple, VMWare, vSphere, Nutanix, GLPI, Netbox, etc.).

Le contenu du fichier d'inventaire dynamique varie en fonction du plugin utilisé, mais généralement il regroupe un ensemble de requête pour obtenir une liste d'hôte et une façon de les regrouper.

Voici un exemple de fichier d'inventaire dynamique utilisant le plugin netbox.
```yaml
---
plugin: netbox.netbox.nb_inventory
api_endpoint: https://netbox.example.org
token: SECRET_TOKEN
query_filters:
  - status: active
  - role: container
  - role: server
group_by:
  - device_roles
```
Dans cet exemple, on récupère l'ensemble des hôtes portant le rôle container ou server et étant actif. Les hôtes sont regroupés par rôle.

Il n'est pas possible d'être exhaustif tellement il existe de plugins.

# Commande
La commande `ansible-inventory` permet de lister les hôtes, groupes et variables des fichiers d'inventaires.
## Lister toutes les informations des fichiers d'inventaires
```bash
ansible-inventory [-i INVENTORY] --list
```
> On peut définir autant de fois l'option `-i` que nécessaire si on a plusieurs fichiers d'inventaires.
{.is-info}

# Références
- [Building Ansible inventories - Ansible Documentation](https://docs.ansible.com/ansible/latest/inventory_guide/index.html)
{.links-list}