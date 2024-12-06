---
title: Certificat de métadonnées utilisateur pour le chiffrement
description: Certificat de métadonnées utilisateur pour le chiffrement
exl-id: 6f5d9a31-945e-418b-a9df-985bdbf29dff
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# Certificat de métadonnées utilisateur pour le chiffrement

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Pour l’intégration de l’authentification Adobe Pass des métadonnées utilisateur chiffrées, vous devez disposer d’une paire de clés privée/publique.

Ce document décrit un processus de génération de certificats de clé publique à utiliser dans l’authentification Adobe Pass. Le processus décrit ici utilise la boîte à outils OpenSSL.

## Présentation du processus de génération de certificats (#generation)

1. Téléchargez et installez la boîte à outils OpenSSL (http://www.openssl.org).

1. Générer une demande de signature de certificat (CSR) :

   * Générez une paire de clés.  Ouvrez une fenêtre Commande / terminal et exécutez la commande suivante :

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Générez la demande de signature de certificat. Sur votre ligne de commande, exécutez :

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     Vous serez invité à saisir le mot de passe de la clé privée.

   * Créez une copie de sauvegarde de votre clé privée et de votre mot de passe. Exemple de CSR :

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. Envoyez la demande de signature de certificat à une autorité de certification (par exemple, Verisign).

1. L’autorité de certification vous enverra le certificat au format .p7b (PKCS#7, Cryptographic Message Syntax Standard).

1. Déployez le certificat .p7b. Convertissez le fichier PKCS#7 (.p7b) en fichier PKCS#12 (fichier PFX, Personal Information Exchange Syntax Standard) à l’aide de votre clé privée, et générez le fichier PEM (fichier de conteneur de certificats concaténé) :

   * Convertissez le fichier PKCS#7 en fichier PEM temporaire. Sur votre ligne de commande, exécutez :

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Convertissez le fichier PEM temporaire en fichier PFX.  Sur votre ligne de commande, exécutez :

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Convertissez le fichier PEM temporaire en fichier PEM final. Sur votre ligne de commande, exécutez :

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Envoyez le fichier PEM final à l’Adobe pour qu’il soit configuré.

   * La personne qui doit recevoir le fichier PEM est l’ingénieur d’activation d’Adobe affecté à votre intégration/validation. Si vous ne travaillez pas directement avec cette personne, vous pouvez découvrir à qui envoyer le fichier auprès de votre représentant d’Adobe.
   * Adobe prend en charge un certificat principal et un certificat de sauvegarde. Si votre certificat principal est compromis, vous pouvez le révoquer et passer au certificat secondaire pour signer l’identifiant du demandeur dans votre application. Cela permettra une transition en douceur entre les certificats en production, avec un minimum d’impact sur les clients.
   * Une fois que l’Adobe a reçu le fichier PEM, les ingénieurs d’authentification l’ajoutent à votre configuration côté serveur et confirment la réception du fichier.
