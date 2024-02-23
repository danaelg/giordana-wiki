---
title: Prometheus server
description: 
published: true
date: 2024-02-23T22:31:38.749Z
tags: prometheus, supervision
editor: markdown
dateCreated: 2024-02-23T22:27:24.153Z
---

# Introduction
Prometheus server est le composant principal de la solution de supervision et d'alerte [Prometheus](/prometheus).

# Fonctionnement
Prometheus server contient la base de données chronologique (TSDB : TimeSeries DataBase) et se charge d’aller récupérer les métriques des différents [exporters](/prometheus/exporter).

Il intègre également service web et une API (optionnel) basique permettant de visualiser les données en base.

Les serveurs prometheus sont très simple. A titre d'exemple, on ne peut pas faire de clusters de serveurs prometheus. Si le nombre d’exporter à requêter est trop grand, il faut diviser son parc par groupe et mettre en place un serveur prometheus par groupe.

# Configuration
Fichier de configuration *prometheus.yml* décomposé en plusieurs parties :
- global : configuration globale
- rule_files : configuration des alertes
- scrape_configs : configuration du scrapping

# Ressources
- [Prometheus - Configuration](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)