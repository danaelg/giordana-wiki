---
title: Backend Terraform
description: 
published: true
date: 2023-06-22T16:37:06.962Z
tags: terraform, terraform_state, terraform_backend
editor: markdown
dateCreated: 2023-06-22T16:36:19.663Z
---

# Introduction
Terraform utilise le [state](/terraform/state) pour stocker l'état de l'infrastructure. Le backend définit où il est stocké.

# Intérêt des backend
Les states peuvent contenir des informations sensibles, il est donc préférable de ne pas stocker le state local dans un dépôt git. Se pose alors la question du travail collaboratif. C'est là que l'usage d'un backend distant prend son sens.

# Type de backend
- [S3](/terraform/state/backend/s3)
{.links-list}