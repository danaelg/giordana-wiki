---
title: Inventaires
description: 
published: true
date: 2023-06-21T06:49:31.521Z
tags: ansible, inventory
editor: markdown
dateCreated: 2023-06-21T06:49:31.521Z
---

# Introduction
Les fichiers d'inventaire au format *INI* ou *YAML* permettent de lister les hôtes, de les grouper et de leurs associer des variables.

A titre personnel, je préfère le format YAML, tous les exemples seront donc dans ce format.

# Strucuture d'un fichier d'inventaire
## Exemple simple
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

### Hôtes dans plusieurs groupes
```yaml
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

# Variables
## Priorité des variables dans les fichiers d'inventaire
Il est possible de définir des variables dans le fichier d'inventaire à plusieurs niveaux : par hôte ou par groupes.
```yaml
all:
  vars:
    var: '!'
    ansible_connection: local
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

On voit qu'un ordre de priorité s'applique. Plus la variable est définit proche de l'hôte, plus elle est prioritaires. 