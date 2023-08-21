---
title: Pulumi
description: 
published: true
date: 2023-08-21T14:30:40.921Z
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
## Projet
Pulumi fonctionne sous forme de *projets*. Un projet est une arborescence de fichiers regroupant le code de l'infrastructure (*les programmes*) et les métadonnées. Chaque programme contient un ensemble de *ressources* qui correspondent à des éléments de l'infrastructure, par exemple une instance AWS EC2, un bucket S3 ou un disque virtuel sont autant de ressources possible. Chaque ressource possède des *propriétés d'entrées et de sorties*.

Voici un schéma issu de la documentation qui montre le lien entre les différents composants.
![](https://www.pulumi.com/images/docs/pulumi-programming-model-diagram.svg =30%x)
*[Schéma d'interraction entre les composants Pulumi - Pulimi Docs](https://www.pulumi.com/docs/concepts/)*

Pour en savoir plus :
- [Projets](/pulumi/project)
{.links-list}

## Fournisseurs de ressources
Pour gérer l'infrastructure, Pulumi s'appuie sur des fournisseurs de ressources, retrouvez des pages dédié pour certains d'entre-eux :
- [Docker](/pulumi/resource-provider/docker)
{.links-list}

## State
Pour géré l'infrastructure, Pulumi a besoin de connaître l'état de cette dernière. C'est ce qu'on appelle les *states*. Chaque stack possèse son propre state. Les states sont stockés dans le *backend* qui peut être le cloud Pulumi, un stockage type S3 (tel que AWS, Azure, Minio, Ceph) ou système de fichiers local.

Pour en savoir plus :
- [States & Backend](/pulumi/state-backend)
{.links-list}

# Ressources
- [Pulumi Docs](https://www.pulumi.com/docs/)
{.links-list}