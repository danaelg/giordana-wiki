---
title: Gestion des dépendances systemd
description: 
published: true
date: 2023-06-30T17:54:41.719Z
tags: systemd, work-in-progress, systemd.unit
editor: markdown
dateCreated: 2023-06-27T20:11:27.096Z
---

# Introduction
Pour mieux comprendre le système de dépendances de systemd, on va jouer avec les options de dépendances `Wants`, `Requires`, `Requisite`, `BindsTo`, `PartOf`, `Upholds` et `Conflicts` ainsi que les options de séquençage de démarrage `Before` et `After` via la création de deux services `serviceA.service` et `serviceB.service` dépendant du premier.

# Création des services
Pour créer un service il nous faut avant tout un programme. On va donc créer deux scripts simples, un par service. Dans notre exemple il sera placé dans `/home/danael/bin/` :

Commençons par `helloWorld-ServiceA.sh`
```bash
#!/bin/bash

# On accepte l'option facultative -n qui attend un argument
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

# On gère le cas où aucune option n'a été passé
if [ -z "$name" ]; then
    name="World!"
fi

sleep 2
systemd-notify --ready

# Boucle d'affichage du Hello
while $(sleep 2); do
        echo "Hello $name"
done
```

Maintenant créons `helloWorld-ServiceB.sh`
```bash
#!/bin/bash

# On accepte l'option facultative -n qui attend un argument
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

# On gère le cas où aucune option n'a été passé
if [ -z "$name" ]; then
    name="World!"
fi

# Boucle d'affichage du Hello
while $(sleep 2); do
        echo "Hello $name"
done
```

Les deux scripts sont très similaire. Sans option, ils affichent "*Hello World!*". Avec l'option `-n` qui attend un arguement, ils affichent "*Hello ARGUMENT*" avec `ARGUMENT`, l'argument défini via l'option `-n`.

Si une autre option inconnu est utilisé, les scripts affichent "*Invalid syntaxe*" et s'arrêtent avec le code d'erreur 1.

Le script `helloWorld-ServiceA.sh` contient deux lignes supplémentaires :
```bash
sleep 2
systemd-notify --ready
```
Ces lignes nous servirons pour démontrer si les services démarrent en même temps ou non. Pour faire simple, on attend 2 second avant d'envoyer une notification à systemd indiquant que le service est prêt et peut être considéré comme démarré.

Maintenant que nous avons notre programme, nous pouvons créer nos services `serviceA.service` et `serviceB.service`. Dans notre exemple, nous les placerons dans le répertoire `/home/danael/.config/systemd/user/` 

Voici le contenu du fichier `serviceA.service`
```ini
[Unit]
Description=Hello World Service A

[Service]
Type=notify
NotifyAccess=all
ExecStart=/home/danael/bin/helloWorld-ServiceA.sh -n "Service A"
StandardOutput=journal
```
et le contenu de `serviceB.service`
```ini
[Unit]
Description=Hello World Service B

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld-ServiceB.sh -n "Service B"
StandardOutput=journal
```
> Pour l'instant, serviceB n'a aucune dépendance, nous allons modifier cela par la suite en fonction des exemples.
{.is-info}

### Différence entre serviceA et serviceB

Comme on peut le voir, il y a une légère différence entre les deux services. ServiceA est configuré avec l'option `Type=notify` et l'option `NotifyAccess=all`. ServiceB, quant à lui est configuré avec l'option `Type=simple`.

Le type `simple` indique à systemd que le service doit être considéré comme démarré lorsque ce dernier viens de créer le processus exécutant la commande défini par `ExecStart`. Le service est donc considéré comme démarré quasi immédiatement après l'instruction de démarrage. 

Le type `notify` indique à systemd que le service doit être considéré comme démarré lorsqu'il reçoit le statut `READY=1` de la part du processus exécutant la commande défini par `ExecStart`. Dans le script  créé précédemment, cette notification est envoyé via la commande `systemd-notify --ready`. On ajoute l'option `NotifyAccess=all` pour permettre à tous les sous-processus d'envoyer la notification à systemd. Cela est nécessaire, car la commande `systemd-notify --ready` est exécuté dans un processus enfant du processus exécutant le script. (voir [cette issue pour les détails](https://github.com/systemd/systemd/issues/24516#issuecomment-1233032190))

Pour faire très faire simple, on simule un long temps de démarrage de serviceA tandis que serviceB démarre immédiatement. Cela a pour but de mettre en évidence l'ordonnancement de démarrage des services. Si serviceA et serviceB sont démarrés en même temps, l'heure de démarrage du serviceA sera après celle du serviceB (puisqu'il met plus de temps à démarrer). Si serviceB démarre après serviceA, l'heure de démarrage des deux services sera quasiment identique.

On peut illustrer cela dans des diagrammes de temps. Voici ce qui serait attendu en cas de démarrage parallèle des deux services :
```kroki
plantuml
scale 1 as 80 pixels

robust "helloWorld-serviceA.sh" as serviceAscript
robust "helloWorld-serviceB.sh" as serviceBscript

concise "serviceA.service" as serviceA
concise "serviceB.service" as serviceB

@0
serviceA is Starting
serviceB is Starting


@+1
serviceA -> serviceAscript : ExecStart=helloWorld-serviceA.sh
serviceAscript is "sleep 2"
serviceBscript is "boucle echo Hello ..."

serviceB -> serviceBscript : ExecStart=helloWorld-serviceB.sh
serviceB is Started

@+5
serviceAscript is "systemd-notify --ready"

@+1
serviceAscript is "boucle echo Hello ..."
serviceA is Started
```

et ce qui serait attendu en cas de démarrage séquentiel.
```kroki
plantuml
scale 1 as 80 pixels

robust "helloWorld-serviceA.sh" as serviceAscript
robust "helloWorld-serviceB.sh" as serviceBscript

concise "serviceA.service" as serviceA
concise "serviceB.service" as serviceB

@0
serviceA is Starting

@+1
serviceA -> serviceAscript : ExecStart=helloWorld-serviceA.sh
serviceAscript is "sleep 2"

@+5
serviceAscript is "systemd-notify --ready"

@+1
serviceA is Started
serviceB is Starting

serviceAscript is "boucle echo Hello ..."

@+1
serviceB -> serviceBscript : ExecStart=helloWorld-serviceB.sh
serviceB is Started

serviceBscript is "boucle echo Hello ..."
```

> Ne faites pas attention à l'unité de temps qui ne correspond à aucune réalité. Elle est là en guise d'illustration.
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
> Pensez à exécuter la commande `systemctl --user daemon-reload` après chaque modification de l'unit
{.is-info}

Dans nos exemples on ne va utiliser que l'option `After`, car le comportement de l'option `Before` est similaire à la différence qu'il serait configuré dans serviceA plutôt que serviceB.

## Cas d'un démarrage sans erreur
### Démarrage de serviceB
Les deux services sont arrêtés, on lance le démarrage de serviceB via la commande `systemctl --user start serviceB`
Regardons alors le statut des deux services via la commande : `systemctl --user status serviceA serviceB -n 0`
```
○ serviceA.service - Hello World Service A
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
     Active: inactive (dead)

● serviceB.service - Hello World Service B
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
     Active: active (running) since Wed 2023-06-28 11:26:10 CEST; 8s ago
   Main PID: 4047 (helloWorld-Serv)
      Tasks: 2 (limit: 11067)
     Memory: 548.0K
        CPU: 14ms
     CGroup: /user.slice/user-1001.slice/user@1001.service/app.slice/serviceB.service
             ├─4047 /bin/bash /home/danael/bin/helloWorld-ServiceB.sh -n "Service B"
             └─4056 sleep 2
```
On observe que seul serviceB est démarré. serivceA est toujours arrêté.

### Démarrage simultané
On part du principe que les deux services sont arrêtés. On lance le démarrage simultané des deux service via la commande : `systemctl --user start serviceA serviceB`
Regardons alors le statut des deux services via la commande : `systemctl --user status serviceA serviceB`
```
● serviceA.service - Hello World Service A
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
     Active: active (running) since Wed 2023-06-28 11:27:19 CEST; 6s ago
   Main PID: 4087 (helloWorld-Serv)
      Tasks: 2 (limit: 11067)
     Memory: 572.0K
        CPU: 21ms
     CGroup: /user.slice/user-1001.slice/user@1001.service/app.slice/serviceA.service
             ├─4087 /bin/bash /home/danael/bin/helloWorld-ServiceA.sh -n "Service A"
             └─4102 sleep 2

Jun 28 11:27:17 ansible systemd[1387]: Starting Hello World Service A...
Jun 28 11:27:19 ansible systemd[1387]: Started Hello World Service A.
Jun 28 11:27:21 ansible helloWorld-ServiceA.sh[4087]: Hello Service A

● serviceB.service - Hello World Service B
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
     Active: active (running) since Wed 2023-06-28 11:27:19 CEST; 6s ago
   Main PID: 4091 (helloWorld-Serv)
      Tasks: 2 (limit: 11067)
     Memory: 564.0K
        CPU: 12ms
     CGroup: /user.slice/user-1001.slice/user@1001.service/app.slice/serviceB.service
             ├─4091 /bin/bash /home/danael/bin/helloWorld-ServiceB.sh -n "Service B"
             └─4101 sleep 2

Jun 28 11:27:19 ansible systemd[1387]: Started Hello World Service B.
Jun 28 11:27:21 ansible helloWorld-ServiceB.sh[4091]: Hello Service B
```
On observe que les deux services sont démarrés et que la ligne `Active: active (running) since [...]` indique une heure de démarrage identique pour les deux services (cf. ligne 3 et 18). Le log d'exécution des services indique que serviceA passe dans le statut démarré à 11h27 et 17 secondes. serviceB passe dans ce statut 2 secondes plus tart à 11h27 et 19 secondes (cf. ligne 13 et 27). Cela indique que le démarrage de serviceB a été lancé après serviceA. 

## Conclusion
Les options `Before` et `After` permettent de définir un ordre de démarrage des services sans que cela n'implique de dépendances (par exemple, le démarrage de serviceA lorsque l'instruction de démarrage du serviceB est lancé). Pour cela il faut utiliser une option de dépedance `Wants`, `Requires`, `Requisite`, etc. 

# Wants
L'option `Wants` est défini de la façon suivante :
> "Configures (weak) requirement dependencies on other units. [...] Units listed in this option will be started if the configuring unit is. However, if the listed units fail to start or cannot be added to the transaction, this has no impact on the validity of the transaction as a whole, and this unit will still be started."
>
> *[systemd.unit - Wants - freedesktop](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Wants=)*

On modifie serviceB pour déclaré serviceA en tant que dépendance (on enlève l'option `After`).
```ini
[Unit]
Description=Hello World Service B
Wants=serviceA.service

[Service]
Type=simple
ExecStart=/home/danael/bin/helloWorld-ServiceB.sh -n "Service B"
StandardOutput=journal
```
> Pensez à exécuter la commande `systemctl --user daemon-reload` après chaque modification de l'unit
{.is-info}

## Cas d'un démarrage sans erreur
Commençons par un exemple simple, **les deux services sont arrêtés** et on démarre serviceB via la commande : `systemctl --user start serviceB`
Regardons maintenant le statut des deux services via la commande `systemctl --user status serviceA serviceB` :
```
● serviceA.service - Hello World Service A
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
     Active: active (running) since Wed 2023-06-28 11:44:39 CEST; 395ms ago
   Main PID: 5137 (helloWorld-Serv)
      Tasks: 2 (limit: 11067)
     Memory: 564.0K
        CPU: 15ms
     CGroup: /user.slice/user-1001.slice/user@1001.service/app.slice/serviceA.service
             ├─5137 /bin/bash /home/danael/bin/helloWorld-ServiceA.sh -n "Service A"
             └─5145 sleep 2

Jun 28 11:44:37 ansible systemd[1387]: Starting Hello World Service A...
Jun 28 11:44:39 ansible systemd[1387]: Started Hello World Service A.

● serviceB.service - Hello World Service B
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
     Active: active (running) since Wed 2023-06-28 11:44:37 CEST; 2s ago
   Main PID: 5138 (helloWorld-Serv)
      Tasks: 2 (limit: 11067)
     Memory: 556.0K
        CPU: 8ms
     CGroup: /user.slice/user-1001.slice/user@1001.service/app.slice/serviceB.service
             ├─5138 /bin/bash /home/danael/bin/helloWorld-ServiceB.sh -n "Service B"
             └─5144 sleep 2

Jun 28 11:44:37 ansible systemd[1387]: Started Hello World Service B.
Jun 28 11:44:39 ansible helloWorld-ServiceB.sh[5138]: Hello Service B
```
On observe que les deux services sont démarrés et que la ligne `Active: active (running) since [...]` de service indique une heure de démarrage postérieur à celle de serviceB (cf. ligne 3 et 17). Le log d'exécution des services indique que le lancement du démarrage du serviceA est fait à  11h44 et 37 secondes tandis que serviceB passe dans le statut démarré à ce même moment (cf. ligne 12 et 26). Cela indique que le démarrage des deux services est fait en même temps (serviceA fini son démarrage après serviceB)

## Cas d'un arrêt (ou redémarrage) de serviceA pendant que serviceB tourne
Lorsque nos deux services tournent correctement, que se passe-t-il si l'on arrête serviceA qui est déclaré en tant que dépendance de serviceB ?

On arrête serviceA via la commande : `systemctl --user stop serviceA`
Regardons alors le statut des deux services via la commande : `systemctl --user status serviceA serviceB -n 0`
Donne la sortie
```
○ serviceA.service - Hello World Service A
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
     Active: inactive (dead) since Wed 2023-06-28 15:14:57 CEST; 7s ago
   Duration: 3.695s
    Process: 27291 ExecStart=/home/danael/bin/helloWorld-ServiceA.sh -n Service A (code=killed, signal=TERM)
   Main PID: 27291 (code=killed, signal=TERM)
        CPU: 15ms

● serviceB.service - Hello World Service B
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
     Active: active (running) since Wed 2023-06-28 15:14:51 CEST; 12s ago
   Main PID: 27292 (helloWorld-Serv)
      Tasks: 2 (limit: 11067)
     Memory: 568.0K
        CPU: 17ms
     CGroup: /user.slice/user-1001.slice/user@1001.service/app.slice/serviceB.service
             ├─27292 /bin/bash /home/danael/bin/helloWorld-ServiceB.sh -n "Service B"
             └─27306 sleep 2
```
On observe que que seul serviceA est arrêté, serviceB continu de tourner correctement.

## Cas d'un démarrage où serviceA échoue
On va maintenant complexifier un peu, on va modifier serviceA pour que son démarrage échoue. Pour cela on modifie l'option `ExecStart` de serviceA :
```ini
[Unit]
Description=Hello World Service A

[Service]
Type=notify
NotifyAccess=all
ExecStart=/home/danael/bin/helloWorld-ServiceA.sh -x
StandardOutput=journal
```
> L'option `-x` fera échoué l'exécution du script
{.is-info}

> Pensez à exécuter la commande `systemctl --user daemon-reload` en cas de modification de l'unit
{.is-info}

En partant du principe que nos deux services sont arrêtés. On démarre serviceB (ce qui déclenchera automatiquement le démarrage de serviceA, comme vu ci-dessus) via la commande : `systemctl --user start serviceB`
Regardons maintenant le statut des deux services via la commande : `systemctl --user status serviceA serviceB -n 0`
Donne la sortie
```
× serviceA.service - Hello World Service A
     Loaded: loaded (/home/danael/.config/systemd/user/serviceA.service; static)
     Active: failed (Result: exit-code) since Wed 2023-06-28 15:16:35 CEST; 3s ago
    Process: 27353 ExecStart=/home/danael/bin/helloWorld-ServiceA.sh -x (code=exited, status=1/FAILURE)
   Main PID: 27353 (code=exited, status=1/FAILURE)
        CPU: 3ms

● serviceB.service - Hello World Service B
     Loaded: loaded (/home/danael/.config/systemd/user/serviceB.service; static)
     Active: active (running) since Wed 2023-06-28 15:16:35 CEST; 3s ago
   Main PID: 27354 (helloWorld-Serv)
      Tasks: 2 (limit: 11067)
     Memory: 556.0K
        CPU: 6ms
     CGroup: /user.slice/user-1001.slice/user@1001.service/app.slice/serviceB.service
             ├─27354 /bin/bash /home/danael/bin/helloWorld-ServiceB.sh -n "Service B"
             └─27356 sleep 2
```
On observce que serviceB est démarré tandis que le démarrage de serviceA est en échec.

## Conclusion
L'option `Wants` permet de déclarer des dépendances et se comporte comme suit :
- Le démarrage du service entraîne le démarrage des dépendances.
- Le démarrage du services et des dépendances est fait en même temps. Il n'y a pas d'ordonnancement (pour cela il faut utiliser les option `Before` ou `After`).
- L'échec du démarrage d'une dépendances n'a pas d'influence sur le démarrage du service.
- L'arrêt (ou le redémarrage) des dépendances, n'a pas d'influence sur le service.

La documentation recommande l'utilisation de `Wants` pour lier le démarrage des services tout en assurant la fiabilité du système en cas de défaillance de certains services. 

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
ExecStart=/home/danael/bin/helloWorld-ServiceB.sh -n "Service B"
StandardOutput=journal
```
> Pensez à exécuter la commande `systemctl --user daemon-reload` en cas de modification de l'unit
{.is-info}

On s'assure également que l'option `ExecStart` permet au serviceA de démarrer correctement.
```ini
[Unit]
Description=Hello World Service A

[Service]
Type=notify
NotifyAccess=all
ExecStart=/home/danael/bin/helloWorld-ServiceA.sh -n "Service A"
StandardOutput=journal
```
> Pensez à exécuter la commande `systemctl --user daemon-reload` en cas de modification de l'unit
{.is-info}

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