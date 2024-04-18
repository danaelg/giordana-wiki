---
title: Prometheus
description: 
published: true
date: 2024-04-18T13:34:35.326Z
tags: prometheus, supervision
editor: markdown
dateCreated: 2023-10-01T09:19:46.375Z
---

# Introduction
Prometheus est outil libre de supervision et d'alerte. Il récupère des métriques qu'il stocke dans une base de données chronologique (timeseries database).

# Fonctionnement
Prometheus est écrit en Go et inclus plusieurs éléments : 
![](https://prometheus.io/assets/architecture.png)
*Schéma d'architecture Prometheus - [Prometheus](https://prometheus.io/docs/introduction/overview/)*

Comme on peut le voir, Prometheus se découpe en quatres éléments 
- [Prometheus server](/prometheus/server)
- [Alertmanager](/prometheus/alertmanager)
- [Exporter](/prometheus/exporter)
- [Pushgateway](/prometheus/pushgateway)
{.links-list}


# Vocabulaire
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

## Cardinalité
En mathématique, la cardinalité représent le nombre d'élément dans un ensemble. Dans Prometheus, on peut identifier plusieurs cardinalités : 
- Cardinalité des labels : Nombre de valeurs possible d'un label
- Cardinalité des métriques : Nombre de série d'une métrique

Il est recommandé de conserver une cardinalité des métriques basse. Cela passe par une diminution de la cardinalité des labels.
En d'autres termes, il faut éviter, à tout prix, d'avoir trop de label différents ou de valeurs différentes par label.

Il faut se rappeler que, pour chaque valeur de label, on crée une nouvelle série. Multiplier par un nombre de target important, cela peut faire exploser la base Prometheus.  

# Ressources
- [Prometheus - Overview](https://prometheus.io/docs/introduction/overview/)
- [Prometheus - Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/)
{.links-list}