---
title: Prometheus
description: 
published: true
date: 2023-10-01T10:45:21.411Z
tags: prometheus, supervision
editor: markdown
dateCreated: 2023-10-01T09:19:46.375Z
---

# Introduction
Prometheus est outil libre de supervision et d'alerte. Il récupère des métriques qu'il stocke dans une base de données de série chronologique (timeseries database).

# Notes en vrac
Ecrit en go. Il inclus un serveur Web, un moteur et une base de donnée. Prometheus va chercher les métriques. Les métriques sont mise à disposition sur un fichier texte via HTTP. 

## Configuration
Fichier de configuration *prometheus.yml* décomposé en plusieurs parties :
- global : configuration globale
- rule_files : configuration des alertes
- scrape_configs : configuration du scrapping

## Instances et Jobs
Une instance correspond un couple ip:port, on peut voir ça comme le noeud ou le serveur que l'on souhaite surveiller.
Le job correspond à un processus dont le rôle est défini et peut être répliqué sur plusieurs instances. 
For example, an API server job with four replicated instances:
- job: api-server
		- instance 1: 1.2.3.4:5670
		- instance 2: 1.2.3.4:5671
		- instance 3: 5.6.7.8:5670
		- instance 4: 5.6.7.8:5671

## Métriques
Les métriques sont toujours associé à une série chronologique unique qui est identifié par son nom et possède des labels (paire clef/valeur) optionnelles. Le nom de la métrique indique la fonction de l'élement surveiller, par exemple `prometheus_http_requests_total` correspond au nombre total de requêtes HTTP reçu par l'application prometheus.
Les labels permettent d'ajouter des dimensions à la métrique, dans notre exemple précédent, la métrique `prometheus_http_requests_total` offre les labels `handler` et `code` qui permettent de spécifier la ressource qui a été requêté (handler) et le code de retour (code). On peut alors accéder au nombre total de requête http par l'application prometheus reçu uniquement sur la ressource `/graph` via la métrique : `prometheus_http_requests_total{handler="/graph"}`.

## Exporteur
Les exporteur permettent de d'exporter des données d'un système local en métrique utilisable par prometheus. En quelque sorte, les exporteurs agissent comme un traducteur pour prometheus 