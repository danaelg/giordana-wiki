---
title: Git
description: 
published: true
date: 2024-04-04T08:34:28.653Z
tags: linux, windows, git, software, vcs, development
editor: markdown
dateCreated: 2023-06-20T19:43:57.782Z
---

# Introduction

![git-workflow.jpeg](/git-workflow.jpeg =30%x)

# Installation
- [Installation sur Windows](https://git-scm.com/download/win)
- [Installation sur Linux](https://git-scm.com/download/linux)
{.links-list}

# Fonctionnement
- [Savez-vous vraiment comment fonctionne git ? - Sébastien LECACHEUR - DevoxxFR 2023](https://youtu.be/uA2WZCQP4EI)
{.links-list}

# Dépôts
## Créer un dépôt
```bash
cd folder
git init
```
> Le dossier courant sera considéré comme le working directory git et la branche `main` sera créé automatiqument 
{.is-info}

## Cloner un dépôt
```bash
git clone URL
```
> L'URL peut être un lien *http* ou *ssh*
> http: `https://github.com/torvalds/linux.git`
> ssh: `git@github.com:torvalds/linux.git`
{.is-info}

## Ajouter un dépôt distant
```bash
git remote add origin URL
git push -u origin main
```

# Commits
## Définir les informations d'auteur des commit
Chaque commit contient un auteur qui est identifier par un nom et un email
```bash
git config --global user.name "PRENOM NOM" 
git config --global user.email "EMAIL"
```
> L'option `--global` permet d'appliquer la configuration à l'ensemble des dépôts. Sans cette option, la configuration est local au dépôt actuellement utilisé
{.is-info}

## Réaliser un commit
1. Ajouter les fichiers à la stagging area
```bash
git add FILE
```
2. Commiter les changements de la stagging area
```bash
git commit [-m DESCRIPTION]
```

## Annuler un commit
Pour annuler un commit, il faut reset le pointeur HEAD sur le commit précédent les changements que l'on souhaite annuler, on peut donc :

-  Reset HEAD d'un commit en arrière
```bash
git reset HEAD~1
```
ou

- Placer HEAD sur un commit spécifique
```bash
git reset COMMIT_HASH
```
> *COMMIT_HASH*: Hash du commit
{.is-info}

Si les changements avait été poussés sur un dépôt distant, on peut forcer la mise à jour de nos références sur le dépôt distant
```bash
git push -f 
```
> Attention, forcer un push est une opération pouvant être dangeureuse, notamment si plusieurs personnes travaillent sur une même branche. Dans un tel cas, mieux vaut réaliser un commit qui annule les changements que d'annuler un commit.
{.is-danger}

## Modifier la description du dernier commit
```bash
git commit --amend
```

# Branches
## Supprimer une branche
```bash
git branch -d BRANCH_NAME
```

## Afficher les branches supprimés dans le dépôt distant
```bash
git fetch -p
```
# .gitignore
Le fichier `.gitignore` permet de lister des dossiers ou fichier qui doivent être exclus du suivi de version (fichier de mot de passe, secrets, etc.)

## Fonctionnement par whitelist
Pour spécifier explicitement quels fichiers doivent être suivi par git et ignorer tous les autres (whitelist), on peut procéder comme suit :
```
# Exclure tous les fichiers
* 

# Inclure des fichiers et des dossiers
!config/*
!src/*
!index.html
```

# Log
## Afficher les auteurs des derniers commits
```bash
git log --format=%cn
```

## Classer les auteurs par nombre de commit
```bash
git log --format=%cn | sort | uniq -c | sort -n
```

# Références
- https://git-scm.com/docs/git-add
- https://git-scm.com/docs/git-commit
{.links-list}