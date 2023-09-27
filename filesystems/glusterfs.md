---
title: GlusterFS
description: 
published: true
date: 2023-09-27T20:36:44.711Z
tags: filesystem, glusterfs
editor: markdown
dateCreated: 2023-09-27T18:39:39.916Z
---

# Introduction
GlusterFS est un système de fichier distribué libre qui peut être utilisé pour des tâches avec un usage intensif des données (stockage cloud, streaming).

# Fonctionnalités
GlusterFS est un système de fichier distribué par le réseau qui est évolutif (scalable). Il offre les fonctionnalités suivantes :
- Support de plusieurs pétaoctet
- Support des centaines de clients
- Compatible POSIX
- Supporte du matériel standard
- Réplication
- Quota
- Géo-réplication
- Snapshots

# Fonctionnement
GlusterFS n'est pas vraiment un système de fichier, il fonctionne par dessus un système de fichiers existant ([la doc recommande XFS](https://docs.gluster.org/en/latest/Install-Guide/Common-criteria/#general-setup-principles)). C'est à dire que n'importe quel espace disponible sur un hôte peut potentiellement être utilisé. GlusterFS s'occupe de répartir les blocs de données entre les hôtes. Il aggrége les systèmes de fichiers des différents hôte pour n'en fournir qu'un seul :

![glusterfs.png](/filesystems/glusterfs/glusterfs.png =25%x)

# Configuration
## Installation
- [Install - Gluster Docs](https://docs.gluster.org/en/latest/Install-Guide/Install/#glusterfs)
{.links-list}

## Création du pool de serveur (TSP)
Le pool de serveur appelé Trusted Storage Pool (TSP) contient l'ensemble des noeuds d'un cluster GlusterFS. Sans TSP, aucun volume ne peut être créé.

Pour ajouter un noeud au TSP, il suffit d'exécuter la commande : 
```bash
gluster peer probe SERVER
```
> La commande ci-dessus ajoute le serveur défini par SERVER dans le TSP. Il est possible d'indiquer une adresse IP ou un nom DNS. Un serveur déjà présent dans un TSP ne peut pas être ajouté.
{.is-info}

# Commandes
## TSP
### Ajout d'un serveur au TSP
```
gluster peer probe SERVER
```
### Afficher le statut des hôtes du TSP
```
gluster peer status
```

### Lister les serveurs présent dans un TSP
```
gluster pool list
```


# Ressources
- [Gluster Docs](https://docs.gluster.org/en/latest/)
{.links-list}