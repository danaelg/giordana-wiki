---
title: Pulumi
description: 
published: true
date: 2023-08-21T08:21:27.470Z
tags: iac, automatisation, work-in-progress, infrastructure
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
## 

## Travailler avec Pulumi
Pulumi fonctionne sous forme de *projets*. Un projet est une arborescence de fichiers regroupant le code de l'infrastructure (*les programmes*) et les métadonnées. Chaque programme contient un ensemble de ressources qui correspondent à des éléments de l'infrastructure, par exemple une instance AWS EC2, un bucket S3 ou un disque virtuel sont autant de ressources possible. Chaque ressource possède des propriétés d'entrées et de sorties.

Voici un schéma issu de la documentation qui montre le lien entre les différents composants.
![](https://www.pulumi.com/images/docs/pulumi-programming-model-diagram.svg =50%x)
*[Schéma d'interraction entre les composants Pulumi - Pulimi Docs](https://www.pulumi.com/docs/concepts/)*

Pour en savoir plus :
- [Projet](/pulumi/project)
- [Programme](/pulumi/program)
- [Ressource](/pulumi/resource)
- [Stack](/pulumi/stack)
{.links-list}

# Arborescence des projets
Le choix de l'arborscence des projets Pulumi est fondamentale puisque c'est elle qui impactera le flux de travail, il n'y a pas de solution parfaite qui conviendra à tout le monde. De la même manière qu'un projet de développement, il faut trouver une structure qui correspond le mieux au besoin. La documentation oppose deux approches différentes : monolithique et micro-stack, il faut s'inspirer de ces méthodes pour trouver le compromis le plus adapté.

## Approche monolithique
Une arborescence monolithique consiste à n'avoir qu'une structure 



# Ressources
- [Pulumi Docs](https://www.pulumi.com/docs/)
{.links-list}