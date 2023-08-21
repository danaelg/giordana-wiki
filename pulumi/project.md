---
title: Projet Pulumi
description: 
published: true
date: 2023-08-21T14:26:45.228Z
tags: pulumi, pulumi_project
editor: markdown
dateCreated: 2023-08-21T13:59:51.971Z
---

# Introduction
Un projet Pulumi est un dossier contenant un fichier `Pulumi.yaml`. Ce fichier peut être créé manuellement ou à partir d'un template via la commande `pulumi new`. Un projet est écrit dans l'un des langages supporté par Pulumi (NodeJS, Python, Go, Java, .NET ou YAML). 

# Arborescence
## Pulumi.yaml
Le fichier `Pulumi.yaml` est le fichier de configuration du projet. Il doit être écrit en YAML et être nommé avec un `P` majuscule avec l'extention `.yaml` ou `.yml`.

Voici un exemple de fichier `Pulumi.yaml` :
```yaml
name: test-project
runtime: python
```
## Fichiers runtime {.tabset}
D'autres fichiers et dossiers accompagnent le fichier `Pulumi.yaml`. Le nom de ces derniers diffèrent  en fonction du langage défini par la variable `runtime`

### Python
- `__main__.py` : Point d'entrée du programme
- `venv` : Environnement Python virtuelle (cf. [virtualenv](https://virtualenv.pypa.io/en/latest/))
- `requirements.txt` : Fichier de dépendances Python du projet.

### Test
coucou

# Création d'un projet
Pour créer un projet il est plus simple de passer par la commande `pulumi new`, la commande suivante permet de créer un projet `my-project` avec le runtime Python :
```bash
mkdir my-project && mv $_
pulumi new python
```

# Organisation des projets
Un projet se matérialisant par arborescence de fichiers, il est important de réfléchir à sa structure puisque c'est elle qui impactera le flux de travail. Il n'y a pas de solution parfaite, de la même manière qu'un projet de développement, il faut trouver une structure qui correspond le mieux au besoin. La documentation oppose deux approches différentes : monolithique et micro-stack, il faut s'inspirer de ces méthodes pour trouver le compromis le plus adapté.

## Approche monolithique
L'approche monolithique consiste à n'avoir qu'un projet pour l'ensemble de l'infrastructure ou d'un service de celle-ci. Les stack Pulumi correspondent alors un environnement spécifique (production, préproduction, développement, etc.). Cette approche à le mérite d'être très simple.

## Approche micro-stack
De façon similaire à une architecture microservices, l'approche micro-stack consiste à découper un projet en un ensemble de petits projets.

## La gestion des versions
Qui dit infrastructure as code, dit gestion de version (ex: [git](/git)). La façon dont sont organisés les dépôts dans l'orgnisation peut influencer l'arborescence des projets Pulumi. En effet, si l'organisation travaille de façon mono-dépôt (ex: un dépôt pour toute l'infrastructre), alors on préférera peut être une approche monolithique, à l'inverse, si l'organisation travaille avec beaucoup de dépôt (ex: un dépôt par service de l'infrastructure), alors préférera peut-être une approche micro-stack.

Ceci dit, on peut tout aussi bien créer un projet par dépôt ou créer plusieurs projets dans un seul et même dépôt.

## Sécurité
Les exigences de sécurité déterminent généralement le positionnement du curseur entre les deux approches. En effet, si l'on veut contrôler finement qui déploit quoi, alors il est pertinent que les équipes n'aient accès qu'aux dépôts correspondant aux services de l'infrastructre qu'elles maintiennent.

# Ressources
- [Project - Pulumi Docs](https://www.pulumi.com/docs/concepts/projects/)
- [Creating a Pulumi Project - Lean Pulumi](https://www.pulumi.com/learn/pulumi-fundamentals/create-a-pulumi-project/)
{.links-list}