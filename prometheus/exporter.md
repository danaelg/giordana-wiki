---
title: Prometheus exporter
description: 
published: true
date: 2024-02-23T22:29:45.455Z
tags: prometheus, supervision
editor: markdown
dateCreated: 2024-02-23T22:29:45.455Z
---

# Introduction
Les exporteur permettent de d'exporter des données d'un système local en métrique utilisable par [Prometheus](/prometheus). En quelque sorte, les exporteurs agissent comme un traducteur pour Prometheus 

# Fonctionnement
Les exporters représentent les targets que les [serveurs Prometheus](/prometheus/server) vont requêter. Concrètement un exporter est un binaire qui s’exécute sur un serveur. Celui-ci peut être intégré à une application existante (Grafana, Traefik, Gitlab, etc.) ou être un binaire à part entière dédié à un usage précis. On peut citer par exemple : 
- node_exporter : pour la récupération des métriques OS
- snmp_exporter : pour la récupération de métriques SNMP
- blackbox_exporter : pour la récupération de métriques ICMP, HTTP, DNS

On peut voir un exporter comme l'équivalent des agents dans les autres systèmes de supervision.

L’instance d’un exporter s’appelle une target. On accède à une target via l’association de l’IP et du port de l’exporter s’exécutant sur la machine.

# Ressources
- - [Prometheus - Exporters and integration](https://prometheus.io/docs/instrumenting/exporters/#exporters-and-integrations)
{.links-list}