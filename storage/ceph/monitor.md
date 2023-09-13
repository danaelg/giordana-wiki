---
title: Ceph Monitor
description: 
published: true
date: 2023-09-13T20:15:36.772Z
tags: ceph, ceph-mon
editor: markdown
dateCreated: 2023-09-13T20:11:49.460Z
---

# Introcution
Le service Monitor d'un cluster ceph maintient la [cluster-map](/storage/ceph/cluster-map) que recupère le client.

# Usage
Avant toute oppération, un client doit récupérer la cluster map aurpès d'un service Monitor. Un cluster Ceph doit donc contenir au moins un service Monitor, même si la documentation recommande d'en avoir au moins trois. Lorsque plusieurs service monitor cohabitent, l'algorithme [paxos](https://en.wikipedia.org/wiki/Paxos_(computer_science)) est utilisé pour obtenir un consensus.  

# Ressources
- [Architecture - Ceph Documentation](https://docs.ceph.com/en/latest/architecture/)
- [Ceph Glossary - Ceph Monitor - Ceph Documentation](https://docs.ceph.com/en/latest/glossary/#term-Ceph-Monitor)
- [Monitor Config Reference - Ceph Documentation](https://docs.ceph.com/en/latest/rados/configuration/mon-config-ref/#monitor-config-reference)
- [Paxos - Wikipedia](https://en.wikipedia.org/wiki/Paxos_(computer_science))
{.links-list}