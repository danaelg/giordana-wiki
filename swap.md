---
title: Swap
description: 
published: true
date: 2023-06-20T13:55:28.381Z
tags: linux, swap, memory
editor: markdown
dateCreated: 2023-06-20T12:49:41.800Z
---

# Introduction
L'espace d'échange (SWAP) est une partie de la mémoire virtuelle qui se trouve sur un disque (via une partition dédié ou un fichier). Du point de vue de l'application, cet espace n'a aucune différence avec de la mémoire se trouvant en mémoire vive (RAM).

Le système d'exploitation s'occupe de placer des pages mémoire dans cet espace quand la mémoire vive vient à manquer.

# Recommandations
D'après, red-hat la taille du SWAP doit se faire de la façon suivante :
| Quantité de RAM	| Taille de SWAP recommandé |
| --------------- | ------------------------- |
|<= 2G            | 2 fois la quantité de RAM |
| > 2G & <8G      |	1 fois la quantité de RAM |
| > 8G            |	Au moins 4G               |
{.dense}

# Création de l'espace SWAP
- [Créer un fichier swapfile](/swap/swapfile)
- [Créer une partition SWAP](/swap/swap-part)
{.links-list}

# Troubleshooting
## Vidage de l'espace SWAP
```bash
swapoff -a && swapon -a
```
> Veillez à ce qu'il y ait suffisement de mémoire disponible
{.is-warning}

# Références
- https://access.redhat.com/documentation/fr-fr/red_hat_enterprise_linux/8/html/managing_storage_devices/getting-started-with-swap_managing-storage-devices
{.links-list}
