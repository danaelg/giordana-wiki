---
title: Gestion des dépendances systemd
description: 
published: true
date: 2023-06-28T07:31:33.976Z
tags: systemd, work-in-progress, systemd.unit
editor: markdown
dateCreated: 2023-06-27T20:11:27.096Z
---

# Introduction
Pour mieux comprendre le système de dépendances de systemd, on va jouer avec les options de dépendances `After`, `Before` `Wants`, `Requires`, `Requisite`, `BindsTo`, `PartOf`, `Upholds` et `Conflicts` via la création de deux services `serviceA.service` et `serviceB.service` dépendant du premier.

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
StandardOutput=journal
```
et le contenu de `serviceB.service`
```ini
[Unit]
Description=Hello World Service B

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -n "Service B"
StandardOutput=journal
```
> Pour l'instant, serviceB n'a aucune dépendance, nous allons modifier cela par la suite en fonction des exemples.
{.is-info}

# Before et After
Les options `Before` et `After` sont définis de la façon suivante :
> Those two settings configure ordering dependencies between units. If unit foo.service contains the setting Before=bar.service and both units are being started, bar.service's start-up is delayed until foo.service has finished starting up. After= is the inverse of Before=[...].
> 
> When two units with an ordering dependency between them are shut down, the inverse of the start-up order is applied. [...] If two units have no ordering dependencies between them, they are shut down or started up simultaneously, and no ordering takes place.

On modifie serviceB pour définir l'option `After`
```ini
[Unit]
Description=Hello World Service B
After=serviceA.service

[Service]
Type=simple
```

Dans nos exemples on ne va utiliser que l'option `After`, car le comportement de l'option `Before` est similaire à la différence qu'il serait configuré dans serviceA plutôt que serviceB.

## Cas d'un démarrage sans erreur
### Démarrage de serviceB
Les deux services sont arrêtés, on lance le démarrage de serviceB via la commande `systemctl --user start serviceB`
Regardons alors le statut des deux services via la commande : `systemctl --user status serviceA serviceB -n 0`
Donne le résultat
```
○ serviceA.service - Hello World Service A
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
     Active: inactive (dead)

● serviceB.service - Hello World Service B
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
     Active: active (running) since Wed 2023-06-28 08:56:51 CEST; 1s ago
   Main PID: 2222 (helloWorld.sh)
      Tasks: 2 (limit: 11067)
     Memory: 540.0K
        CPU: 5ms
     CGroup: /user.slice/user-1001.slice/user@1001.service/app.slice/serviceB.service
             ├─2222 /bin/bash /home/danael/bin/helloWorld.sh -n "Service B"
             └─2223 sleep 2
```
On observe que seul serviceB est démarré. serivceA est toujours arrêté.

### Démarrage simultané
On part du principe que les deux services sont arrêtés. On lance le démarrage simultané des deux service via la commande : `systemctl --user start serviceA serviceB`
Regardons alors le statut des deux services via la commande : `systemctl --user status serviceA serviceB -n 0`

## Conclusion

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
On observeque les deux services sont démarrés en même temps (voir la ligne `active (running) since ...`) alors que nous avons lancé uniquement le démarrage de serviceB.

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
> Similar to `Wants=`, but declares a stronger requirement dependency. [...] If this unit gets activated, the units listed will be activated as well. If one of the other units fails to activate, and an ordering dependency `After=` on the failing unit is set, this unit will not be started. Besides, with or without specifying `After=`, this unit will be stopped (or restarted) if one of the other units is explicitly stopped (or restarted).

On modifie serviceB pour modifier l'option `Wants` par `Requires`.
```ini
[Unit]
Description=Hello World Service B
Requires=serviceA.service

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -n "Service B"
```

On s'assure également que l'option `ExecStart` permet au serviceA de démarrer correctement.
```ini
[Unit]
Description=Hello World Service A

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -n "Service A"
```

## Cas d'un démarrage sans erreur
Nous n'allons pas refaire cette exemple, le résultat est identique au comportement de l'option `Wants` (cf. [Wants - Cas d'un démarrage sans erreur](https://wiki.giordana.cc/systemd/unit/dependency#cas-dun-d%C3%A9marrage-sans-erreur))

## Cas d'un arrêt (ou redémarrage) de serviceA pendant que serviceB tourne
Lorsque nos deux services tournent correctement, que se passe-t-il si l'on arrête serviceA qui est déclaré en tant que dépendance de serviceB ?

On part du principe que les deux services tournent correctement. On arrête alors serviceA via la commande : `systemctl --user stop serviceA`
Regardons alors le statut des deux services via la commande : `systemctl --user status serviceA serviceB -n 0`
Donne la sortie
```
○ serviceA.service - Hello World Service A
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
     Active: inactive (dead)

○ serviceB.service - Hello World Service B
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
     Active: inactive (dead)
```

On observe que les deux services sont arrêtés alors que nous avons lancé uniquement l'arrêt de serviceA.

## Cas d'un démarrage où serviceA échoue
Le cas d'un démarrage de serviceB avec serviceA qui entre en échec va être différent si serviceB est configuré avec ou sans l'option `After`.

### Sans l'option `After`
### Avec l'option `After`

## Conclustion
L'option `Requires` permet de déclarer des dépendances et se comporte comme suit :
- Le démarrage du service entraîne le démarrage des dépendances.
- L'arrêt (ou le redémarrage) des dépendances entraîne également l'arrêt du service.
- 

# Requisite

# BindsTo

# PartOf

# Upholds

# Conflicts