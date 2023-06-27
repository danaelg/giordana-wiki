---
title: Gestion des dépendances systemd
description: 
published: true
date: 2023-06-27T20:11:27.096Z
tags: systemd, work-in-progress, systemd.unit
editor: markdown
dateCreated: 2023-06-27T20:11:27.096Z
---

# Introduction
Pour mieux comprendre le système de dépendances de systemd, on va jouer avec les options de dépendances `Wants`, `Requires`, `Requisite`, `BindsTo`, `PartOf`, `Upholds`, `Conflicts`

# Création des services
Script `/home/danael/bin/helloWorld.sh`
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

Sans option, le script affiche "*Hello World!*", si l'option `-n` est défini, le script affiche "*Hello ARGUMENT*" avec `ARGUMENT`, l'argument défini via l'option `-n`.

Si une autre option inconnu est utilisé, le script affiche "*INvalid syntaxe*" et s'arrête avec le code d'erreur 1. 


ServiceA n'a aucune dépendance, on créera un autre service qui dépendra de celui-ci :
`/home/danael/.config/systemd/user/serviceA.service`
```ini
[Unit]
Description=Hello World Service A

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -n "Service A"
```

Pour l'instant serviceB n'a aucune dépendance, on modifiera ceci par la suite
`/home/danael/.config/systemd/user/serviceB.service`
```ini
[Unit]
Description=Hello World Service B

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -n "Service B"
```

# Wants
La doc dit :
> Configures (weak) requirement dependencies on other units. [...] Units listed in this option will be started if the configuring unit is. However, if the listed units fail to start or cannot be added to the transaction, this has no impact on the validity of the transaction as a whole, and this unit will still be started.


On crée serviceB qui dépend de serviceA.
`/home/danael/.config/systemd/user/serviceB.service`
```ini
[Unit]
Description=Hello World Service B
Wants=serviceA.service

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -n "Service B"
```

## Cas d'un démarrage sans erreur
Jouons, nos deux service sont arrêtés. On démarre serviceB
`systemctl --user start serviceB`


Nos deux services ont démarrés :
`systemctl --user status serviceA serviceB -n 0`
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

On remarque que bien qu'on ait démarré uniquement serviceB, serviceA a aussi démarré.

## Cas d'un arrêt (ou redémarrage) de serviceA pendant que serviceB tourne

Nos deux services tournent, que se passe-t-il si l'on arrête serviceA ?
`systemctl --user stop serviceA`

Regardons le statut de nos services
`systemctl --user status serviceA serviceB -n 0`
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
Seul serviceA est arrêté, serviceB tourne correctement.

## Cas d'un démarrage où serviceA échoue
On va forcer l'échec du démarrage de serviceA pour voir comment serviceB se comporte. Pour cela on modifie l'option `ExecStart` de serviceA
`/home/danael/.config/systemd/user/serviceA.service`
```ini
[Unit]
Description=Hello World Service

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld.sh -x
```
L'option `-x` utilisé fera échoué l'exécution du script

En partant du principe que nos deux services sont arrêtés. On démarre serviceB (ce qui déclenchera automatiquement le démarrage de serviceA, comme vu ci-dessus).
`systemctl --user start serviceB`

Regardons maintenant le statut des services
`systemctl --user status serviceA serviceB -n 0`
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

On voit que serviceB est démarré tandis que le démarrage de serviceA est en échec.

## Conclusion
L'option `Wants` permet de déclarer des dépendances. Le démarrage du service entraînera alors le démarrage des dépendances. L'échec du démarrage d'une dépendances n'a pas d'influence sur le démarrage du service.
L'arrêt (ou le redémarrage) des dépendances, n'a pas d'influence sur le service.

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