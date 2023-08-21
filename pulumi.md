---
title: Pulumi
description: 
published: true
date: 2023-08-21T12:42:19.179Z
tags: iac, automatisation, work-in-progress, infrastructure, pulumi
editor: markdown
dateCreated: 2023-08-17T20:52:15.943Z
---

# Introduction
Pulumi est un outil d'[Infrastructure as Code](/iac) qui peut être utilisé sur divers language de programmation tel que TypeScript, JavaScript, Python, Go, .NET, Java ou sous forme de fichiers YAML. Le code, interprété par Pulumi, décrit l'état souhaité d'une infrastructure qui est déployé sur le fournisseur de service souhaité (AWS, Azure, Proxmox, etc.).

Pulumi possède de nombreux plugin pour intéragir avec les fournisseurs de service, la liste complète se trouve ici : [Pulumi Registry](https://www.pulumi.com/registry/).

# Installation
- [Download & install Pulumi - Pulumi Docs](https://www.pulumi.com/docs/install/)
{.links-list}


# Fonctionnement
Pulumi fonctionne sous forme de *projets*. Un projet est une arborescence de fichiers regroupant le code de l'infrastructure (*les programmes*) et les métadonnées. Chaque programme contient un ensemble de *ressources* qui correspondent à des éléments de l'infrastructure, par exemple une instance AWS EC2, un bucket S3 ou un disque virtuel sont autant de ressources possible. Chaque ressource possède des *propriétés d'entrées et de sorties*.

Voici un schéma issu de la documentation qui montre le lien entre les différents composants.
![](https://www.pulumi.com/images/docs/pulumi-programming-model-diagram.svg =50%x)
*[Schéma d'interraction entre les composants Pulumi - Pulimi Docs](https://www.pulumi.com/docs/concepts/)*

> Les *ressources* sont fournit par les *fournisseurs de ressources*. Ces fournisseurs sont matérialisés par des bibliothèques importés dans le programme et des plugins.
{.is-info}

Si l'on prend l'arborescence du projet suivant :
```

```

## State
Pour géré l'infrastructure, Pulumi a besoin de connaître l'état de cette dernière. C'est ce qu'on appelle les *states*. Chaque stack possèse son propre state. Les states sont stockés dans le *backend* qui peut être le cloud Pulumi, un stockage type S3 (tel que AWS, Azure, Minio, Ceph) ou système de fichiers local.

Pour en savoir plus :
- [States & Backend](/pulumi/state-backend)
{.links-list}

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

# Gérer l'infrastructure
Pour gérer l'infrastructure, Pulumi s'appuie sur des fournisseurs de ressources, retrouvez des pages dédié pour certains d'entre-eux :
- [Docker](/pulumi/resource-provider/docker)
{.links-list}

# Ressources
- [Pulumi Docs](https://www.pulumi.com/docs/)
{.links-list}