---
title: OpenSSL
description: 
published: true
date: 2023-06-20T19:55:08.358Z
tags: openssl, security, cryptography, software
editor: markdown
dateCreated: 2023-06-20T11:07:27.543Z
---

# Introduction

```bash
openssl COMMAND
```

# Gestion des certificats
## Génération d'un certificat x509
```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365 -noenc
```

## Signature d'un certificat


# Formats des certificats
## PEM - Privacy Enhanced Mail
Format le plus commun pour les certificat X.509, CSR et clefs. C'est un fichier texte encodé en BASE64. 

On identifie ce format grâce à l'en-tête `-----BEGIN CERTIFICATE-----` et au pied de page `-----END CERTIFICATE-----`

### Extensions communes
`.crt`, `.pem`, `.cer`, `.key`

### Afficher les informations d'un certificat x509 au format PEM
```bash
openssl x509 -in CERT -text
```

## DER - Distinguished Encoding Rules
Format binaire

### Extensions communes
`.der`, `.cer`

### Afficher les informations d'un certificat x509 au format DER
```bash
openssl x509 -informe der -in CERT -text
```