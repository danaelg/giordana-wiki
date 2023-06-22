---
title: State Terraform
description: 
published: true
date: 2023-06-22T16:36:45.420Z
tags: terraform, terraform_state
editor: markdown
dateCreated: 2023-06-22T16:27:57.068Z
---

# Introduction
Le state est l'élément qui contient les informations de l'infrastructure géré par Terraform. 

La méthode de stockage est défini par le [backend](/terraform/state/backend). Par défaut, c'est le backend local qui est utilisé. 

# Fonctionnement
Lorsque qu'un `plan` est exécuté, Terraform fait une comparaison entre le fichiers de configuration et le state pour définir ce qui va être modifié. Lors d'un `apply`, les modifications sont appliqué sur l'infrastructure puis sont stockées dans le state.

# Références
- [Backend Configuration - HashiCorp Developer](https://developer.hashicorp.com/terraform/language/settings/backends/configuration)
{.links-list}