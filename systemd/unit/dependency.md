---
title: Gestion des dépendances systemd
description: 
published: true
date: 2023-06-27T20:36:21.300Z
tags: systemd, work-in-progress, systemd.unit
editor: markdown
dateCreated: 2023-06-27T20:11:27.096Z
---

# Introduction
Pour mieux comprendre le système de dépendances de systemd, on va jouer avec les options de dépendances `Wants`, `Requires`, `Requisite`, `BindsTo`, `PartOf`, `Upholds`, `Conflicts` via la création de deux services `serviceA.service` et `serviceB.service` dépendant du premier.

# Création des services
Pour créer un service il nous faut avant tout un programme. On va donc créer un script simple `helloWorld.sh`. Dans notre exemple il sera placé dans `/home/danael/bin/helloWorld.sh` :
```bash
#!/bin/bash

while getopts "n:" opt; do
	case $opt in 
		n)
			name=$OPTARG
			;;
		*)
			echo "Invalid syntax"
			exit 1
			;;
	esac
done

if [ -z "$name" ]; then
	name="World!"
fi

while $(sleep 2); do
	echo "Hello $name"
done
```
Sans option, le script affiche "*Hello World!*". Ce script peut prendre l'option `-n` auquel il faut ajouter un arguement. Si l'option est défini, le script affiche "*Hello ARGUMENT*" avec `ARGUMENT`, l'argument défini via l'option `-n`.

Si une autre option inconnu est utilisé, le script affiche "*Invalid syntaxe*" et s'arrête avec le code d'erreur 1. 

Maintenant que nous avons notre programme, nous pouvons créer nos services `serviceA.service` et `serviceB.service`. Dans notre exemple, nous les placerons dans le répertoire `/home/danael/.config/systemd/user/` 

Voici le contenu du fichier `serviceA.service`
```ini
[Unit]
Description=Hello World Service A

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -n "Service A"
```
et le contenu de `serviceB.service`
```ini
[Unit]
Description=Hello World Service B

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -n "Service B"
```
> Pour l'instant, serviceB n'a aucune dépendance, nous allons modifier cela par la suite en fonction des exemples.
{.is-info}

# Wants
L'option `Wants` est défini de la façon suivante :
> "Configures (weak) requirement dependencies on other units. [...] Units listed in this option will be started if the configuring unit is. However, if the listed units fail to start or cannot be added to the transaction, this has no impact on the validity of the transaction as a whole, and this unit will still be started."
>
> *[systemd.unit - Wants - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Wants=)*

On modifie serviceB pour déclaré serviceA en tant que dépendance.
```ini
[Unit]
Description=Hello World Service B
Wants=serviceA.service

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -n "Service B"
```

## Cas d'un démarrage sans erreur
Commençons par un exemple simple, nos deux services sont arrêté et on démarre serviceB.
`systemctl --user start serviceB`

Regardons ce qu'il se passe en affichant le statut des deux services via la commande `systemctl --user status serviceA serviceB -n 0` :
Donne la sortie
```
● serviceA.service - Hello World Service
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
    Drop-In: /usr/lib/systemd/user/service.d
             └─10-timeout-abort.conf
     Active: active (running) since Tue 2023-06-27 21:47:19 CEST; 1s ago
   Main PID: 50590 (helloWorld.sh)
      Tasks: 2 (limit: 9270)
     Memory: 576.0K
        CPU: 7ms
     CGroup: /user.slice/user-1000.slice/user@1000.service/app.slice/serviceA.service
             ├─50590 /bin/bash /home/danael/bin/helloWorld.sh
             └─50591 sleep 2

● serviceB.service - Hello World Service B
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
    Drop-In: /usr/lib/systemd/user/service.d
             └─10-timeout-abort.conf
     Active: active (running) since Tue 2023-06-27 21:47:19 CEST; 1s ago
   Main PID: 50592 (helloWorld.sh)
      Tasks: 2 (limit: 9270)
     Memory: 564.0K
        CPU: 8ms
     CGroup: /user.slice/user-1000.slice/user@1000.service/app.slice/serviceB.service
             ├─50592 /bin/bash /home/danael/bin/helloWorld.sh -n "Service B"
             └─50593 sleep 2
```
On observeque les deux services sont démarrés alors que nous avons seul le démarrage de serviceB a été explicité.

## Cas d'un arrêt (ou redémarrage) de serviceA pendant que serviceB tourne
Lorsque nos deux services tournent correctement, que se passe-t-il si l'on arrête serviceA qui est déclaré en tant que dépendance de serviceB ?

On arrête serviceA via la commande : `systemctl --user stop serviceA`
Regardons alors le statut des deux services via la commande : `systemctl --user status serviceA serviceB -n 0`
Donne la sortie
```
○ serviceA.service - Hello World Service
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
    Drop-In: /usr/lib/systemd/user/service.d
             └─10-timeout-abort.conf
     Active: inactive (dead) since Tue 2023-06-27 21:51:00 CEST; 24s ago
   Duration: 3min 40.629s
    Process: 50590 ExecStart=/home/danael/bin/helloWorld.sh (code=killed, signal=TERM)
   Main PID: 50590 (code=killed, signal=TERM)
        CPU: 290ms

● serviceB.service - Hello World Service B
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
    Drop-In: /usr/lib/systemd/user/service.d
             └─10-timeout-abort.conf
     Active: active (running) since Tue 2023-06-27 21:47:19 CEST; 4min 5s ago
   Main PID: 50592 (helloWorld.sh)
      Tasks: 2 (limit: 9270)
     Memory: 576.0K
        CPU: 328ms
     CGroup: /user.slice/user-1000.slice/user@1000.service/app.slice/serviceB.service
             ├─50592 /bin/bash /home/danael/bin/helloWorld.sh -n "Service B"
             └─51454 sleep 2
```
On observice que seul serviceA est arrêté, serviceB continu de tourner correctement.

## Cas d'un démarrage où serviceA échoue
On va maintenant complexifier un peu, on va modifier serviceA pour que son démarrage échoue. Pour cela on modifie l'option `ExecStart` de serviceA :
```ini
[Unit]
Description=Hello World Service

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -x
```
> L'option `-x` fera échoué l'exécution du script
{.is-info}

En partant du principe que nos deux services sont arrêtés. On démarre serviceB (ce qui déclenchera automatiquement le démarrage de serviceA, comme vu ci-dessus) via la commande : `systemctl --user start serviceB`
Regardons maintenant le statut des deux services via la commande : `systemctl --user status serviceA serviceB -n 0`
Donne la sortie
```
× serviceA.service - Hello World Service
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
    Drop-In: /usr/lib/systemd/user/service.d
             └─10-timeout-abort.conf
     Active: failed (Result: exit-code) since Tue 2023-06-27 21:58:25 CEST; 2s ago
   Duration: 5ms
    Process: 52558 ExecStart=/home/danael/bin/helloWorld.sh -x (code=exited, status=1/FAILURE)
   Main PID: 52558 (code=exited, status=1/FAILURE)
        CPU: 2ms

● serviceB.service - Hello World Service B
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
    Drop-In: /usr/lib/systemd/user/service.d
             └─10-timeout-abort.conf
     Active: active (running) since Tue 2023-06-27 21:58:25 CEST; 2s ago
   Main PID: 52559 (helloWorld.sh)
      Tasks: 2 (limit: 9270)
     Memory: 560.0K
        CPU: 5ms
     CGroup: /user.slice/user-1000.slice/user@1000.service/app.slice/serviceB.service
             ├─52559 /bin/bash /home/danael/bin/helloWorld.sh -n "Service B"
             └─52563 sleep 2
```
On observce que serviceB est démarré tandis que le démarrage de serviceA est en échec.

## Conclusion
L'option `Wants` permet de déclarer des dépendances et se comporte comme suit :
- Le démarrage du service entraîne le démarrage des dépendances.
- L'échec du démarrage d'une dépendances n'a pas d'influence sur le démarrage du service.
- L'arrêt (ou le redémarrage) des dépendances, n'a pas d'influence sur le service.

La documentation recommande l'utilisation de `Wants` pour lié le démarrage des services tout en assurant la fiabilité du système en cas de défaillance de certains services. 

# Requires
La doc dit :
>Similar to `Wants=`, but declares a stronger requirement dependency. [...] If this unit gets activated, the units listed will be activated as well. If one of the other units fails to activate, and an ordering dependency `After=` on the failing unit is set, this unit will not be started. Besides, with or without specifying `After=`, this unit will be stopped (or restarted) if one of the other units is explicitly stopped (or restarted).

On ne vas pas refaire le cas simple, vu que le résultat serait identique à que nous avons déjà observé avec `Wants`. On va continuer avec notre serviceA qui échoue à démarrer.

On modifie donc uniquement serviceB pour remplacer l'option `Wants` par `Requires`
```ini
[Unit]
Description=Hello World Service B
Requires=serviceA.service

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -n "Service B"
```

Nos services sont arrêtés :
`systemctl status serviceA`
>○ serviceA.service - Hello World Service
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
    Drop-In: /usr/lib/systemd/user/service.d
             └─10-timeout-abort.conf
     Active: inactive (dead)

`systemctl status serviceB`
> ○ serviceB.service - Hello World Service A
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
    Drop-In: /usr/lib/systemd/user/service.d
             └─10-timeout-abort.conf
     Active: inactive (dead)

Jouons et démarrons serviceB
`systemctl start serviceB`

On vérifie son statut


# Requisite

# BindsTo

# PartOf

# Upholds

# Conflicts