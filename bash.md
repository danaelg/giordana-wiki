---
title: Bash
description: 
published: true
date: 2024-02-21T15:17:57.666Z
tags: linux, bash
editor: markdown
dateCreated: 2024-02-21T15:17:57.666Z
---

# Introduction
Bash (Bourne-again shell) est un shell Unix développé pour le projet GNU qui vise à remplacer Bourne shell (sh). 

# Commandes
## Redirection de stderr et stdout dans un fichier et sur stdout
La commande suivante permet de rediriger la sortie des erreurs et la sortie standard à la fois dans un fichier et sur l'écran (sortie standard)
```bash
command 2>&1 | tee -a file.log 
```
> L'option `-a` permet d'ouvrir le fichier *file.log* en mode *append* (ajout, équivalement à `>>`).
{.is-info}

# Ressources
- [Bash (Unix shell)](https://en.wikipedia.org/wiki/Bash_(Unix_shell))
{.links-list}