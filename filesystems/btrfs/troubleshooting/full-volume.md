---
title: Corriger volume plein
description: 
published: true
date: 2023-06-20T10:25:13.989Z
tags: filesystem, btrfs
editor: markdown
dateCreated: 2023-06-20T10:19:27.355Z
---

# Corriger les problèmes de volume BTRFS plein

## Cas 1 : Déséquilibre des blocs de données
Dans l'example suivant, la commande :
```bash
btrfs filesystem show
```
donne le résultat :
```
Label: btrfs_pool1  uuid: 4850ee22-bf32-4131-a841-02abdb4a5ba6
	Total devices 1 FS bytes used 441.69GiB
	devid    1 size 865.01GiB used 751.04GiB path /dev/mapper/cryptroot
```
On voit que seulement 50% (441.69 sur 865.01GiB) de l'espace est utilisés alors que le disque apparaît comme remplis à 87% (751.04 sur 865.01GiB).

Lancer un [rééquilibrage des blocs de données](/filesystems/btrfs#rééquilibrer-un-volume-btrfs) (`-dusage`) peut aider à mieux répartir les blocs pour libérer de l'espace.
> Plus la valeur `dusage` et `musage` est grande, plus cela sollicitera  les disques, car cela entrainera le déplacement d'un plus grand nombre de données.
> L'idéal est de commencer avec une valeur basse, ici 50, car le disque est utilisé à 50% donc nous pouvons rééquilibré uniquement les groupes de blocs qui ont un espace utilisé inférieur à 50%. Cela provoquera le déplacement des données vers des blocs plus remplis ce qui libérera des blocs.
> Si cela ne suffit pas, la valeur peut être augmenté au fur et à mesure.
{.is-info}


## Cas 2 : Déséquilibre des blocs de metadonnées
Si l'espace dédié aux métadonnées est remplis, il est impossible d'écrire de nouvelles données. 

Lancer un [rééquilibrage des blocs de metadonnées](/filesystems/btrfs#rééquilibrer-un-volume-btrfs) (`-musage`) peut aider à mieux répartir les blocs, sinon lancer un rééquilibrage complet.

## Cas 3 : Le rééquilibrage ne peut pas se lancer car le système de fichiers est plein

Si les éléments ci-desus n'ont pas aidé (c'est la merde hein...), on peut [Ajouter un disque au volume](/filesystems/btrfs#ajouter-un-disque-à-un-volume-btrfs).

L'exemple suivant permet de créer un disque à l'aide d'un fichier. Il peut ensuite être ajouté au volume BTFS `/mnt/btrfs_pool2` temporairement.


```bash
dd if=/dev/zero of=/var/tmp/btrfs bs=1G count=5
losetup -v -f /var/tmp/btrfs
btrfs device add /dev/loop0 /mnt/btrfs_pool2
```
Vous pouvez alors faire de l'espace sur le volume puis supprimer le disque temporaire.

---
## Reférences
- http://marc.merlins.org/perso/btrfs/post_2014-05-04_Fixing-Btrfs-Filesystem-Full-Problems.html
- https://btrfs.readthedocs.io/en/latest/Balance.html 
{.links-list}