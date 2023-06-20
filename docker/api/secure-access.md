---
title: Sécuriser l'accès à l'API Docker
description: 
published: true
date: 2023-06-20T12:38:14.942Z
tags: security, docker, api
editor: markdown
dateCreated: 2023-06-20T12:38:12.849Z
---

# Introduction
Pour sécuriser l'API Docker, nous allons utiliser des certificats TLS. Cela va nécessiter trois choses :
1. Générer une authorité de certification (CA) qui signera les certificats TLS
2. Générer une paire de certificats TLS pour chiffrer les communication avec l'API Docker (paire serveur)
3. Générer une paire de certificats TLS pour authentifier le client auprès de l'API Docker (paire client)

# Création de l'autorité de certification
## Génération de la clef privé
```bash
openssl genrsa -aes256 -out ca-key.pem 4096
```

## Génération du certificat public
```bash
openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
```

> Sauvegarder précieusement ces deux certificats. Le certificat public `ca.pem` devra être copié sur l'hôte Docker et sur le client.
{.is-info}

# Création des certificats de l'hôte Docker
## Génération de la clef privée
```bash
openssl genrsa -out server-key.pem 4096
```

## Génération du certificat public
```bash
openssl req -subj "/CN=HOST" -sha256 -new -key server-key.pem -out server.csr
```
> HOST correspond au nom ou à l'adreese IP de l'hôte docker par laquelle le client accèdera.
{.is-info}

## Ajout de la propriété `serverAuth`
```bash
echo extendedKeyUsage = serverAuth > extfile.cnf
```

### (facultatif) Ajout de noms alternatifs pour le serveur
Si vous souhaitez utiliser plusieurs moyen pour accéder à l'hôte Docker (plusieurs IP ou plusieurs FQDN) qui sont différent du nom commun (CN), il faut ajouter des noms alternatifs : 
```bash
echo subjectAltName = DNS:docker-host.example.com,IP:192.0.2.10,IP:127.0.0.1 >> extfile.cnf
```

## Signature du certificat public
```bash
openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -extfile extfile.cnf
```

> Sauvegarder précieusement les deux certificats `server-key.pem` et `server-cert.pem` . Ils doivent se trouver uniquement sur l'hôte Docker.
{.is-info}

# Création des certificats du client
## Génération de la clef privée
```bash
openssl genrsa -out key.pem 4096
```

## Génération du certificat public 
```bash
openssl req -subj '/CN=HOST' -new -key key.pem -out client.csr
```
> Remplacer `HOST` par le nom ou l'adresse IP du client.
{.is.info}

## Ajout de la propriété `clientAuth`
```bash
echo extendedKeyUsage = clientAuth > extfile-client.cnf
```

## Signature du certificat public
```bash
openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out cert.pem -extfile extfile-client.cnf
```

> Sauvarger précieusement les deux certificats `cert.pem` et `key.pem`. Ils doivent se trouver uniquement sur le client.
{.is-info}

# Récapitulatif
Nous avons générés de nombreux fichiers qui doivent être stockés sur des machines différentes. Le tableau suivant à pour but de résumer l'emplacement de chaque fichier. Tous les autres fichiers peuvent être supprimés.

|             | ca.pem | ca-key.pem | server-cert.pem | server-key.pem | cert.pem | cert-key.pem |
| ----------- | ------ | ---------- | --------------- | -------------- | -------- | ------------ |
| CA          | ✅     | ✅         | ❌              | ❌             | ❌       | ❌           |
| Hôte Docker | ✅     | ❌         | ✅              | ✅             | ❌       | ❌           |
| Client      | ✅     | ❌         | ❌              | ❌             | ✅       | ✅           |

# Configuration de Docker
Pour configurer Docker en mode TLS, il faut ajouter les options suivantes à la commande d'exécution de `dockerd` :
```bash
dockerd \
	--tlsverify \
    --tlscacert=ca.pem \
    --tlscert=server-cert.pem \
    --tlskey=server-key.pem \
    --H=HOST:2376
```
> Docker sur TLS devrait écouter sur le port 2376
{.is-info}

# Configuration du client
La configuration du client dépend... du client... mais voici un exemple avec `curl`
```bash
curl \
	--cert cert.pem \
	--key cert-key.pem \
	--cacert ca.pem \
	https://HOST:2376
```

---
## Reférences
- https://docs.docker.com/engine/security/protect-access/#use-tls-https-to-protect-the-docker-daemon-socket
{.links-list}