---
title: systemd-journald.service
description: 
published: true
date: 2024-03-19T15:48:11.663Z
tags: systemd, systemd-journald
editor: markdown
dateCreated: 2024-03-19T15:46:05.778Z
---

# Introduction

# Configuration
## Rotation de log par date
`MaxFileSec` : Définit le temps maximal des entrées présentes dans un seul fichier journal avant d'en faire une rotation.
`MaxRetentionSec` : Définit le temps de conservation maximal des entrées.

Exemple 
```
MaxFileSec=1day
MaxRetentionSec=1month
```
Avec cette configuration, chaque fichier de journal contient des entrées sur une journée. Les fichiers de journal contenant des entrées de plus de 1 mois sont supprimés. 

# Ressources
- [journald.conf](https://www.freedesktop.org/software/systemd/man/latest/journald.conf.html)
{.links-list}