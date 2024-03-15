---
title: Mécanisme de ralentissement
description: Découvrez le mécanisme de limitation utilisé dans l’authentification Adobe Pass. Consultez un aperçu de ce mécanisme dans cette page.
source-git-commit: 4f81f39427d87e4274c27d8f1b4bd1eb366d9abb
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 0%

---


# Mécanisme de ralentissement {#throttling-mechanism}

Tous les clients Pass Authentication doivent pouvoir accéder à l’API Pass Authentication pour chacun de leurs utilisateurs, conformément aux instructions et à leurs analyses de cas.

L’authentification par col introduit un mécanisme de limitation pour assurer une distribution équitable des ressources entre les utilisateurs de nos clients.

Ce mécanisme est important pour plusieurs raisons :

- L’une des règles d’or de l’architecture multi-locataires est que le comportement d’un utilisateur ne doit pas affecter quelqu’un d’autre.
- La limitation de débit est importante pour les API, car il est facile de faire des erreurs lors de l’intégration à une API. Comme les API sont programmées, il est relativement facile d’envoyer accidentellement plus de requêtes que prévu.

## Présentation du mécanisme {#mechanism-overview}

### Implémentations client

L’authentification par transmission fournit des instructions et un SDK pour interagir avec l’API, mais ne contrôle pas la manière dont le client l’utilise. Certaines mises en oeuvre peuvent avoir une mise en oeuvre rudimentaire et peuvent inonder le service de requêtes d’API inutiles, accidentellement ou non, ce qui peut entraîner des ralentissements ou des problèmes de capacité pour d’autres utilisateurs.

Le service lui-même doit être en mesure de gérer toute capacité raisonnable. Mais quel que soit l’évolutivité ou les performances d’un service, il y a toujours des limites. Par conséquent, les limites du nombre d’appels acceptés doivent être configurées pour le service dans un intervalle de temps spécifique.

### Présentation du ralentissement

L’authentification par transmission est basée sur l’identification de l’utilisateur et un algorithme de limitation de débit de jeton avec des valeurs prédéfinies pour contrôler l’accès de chaque appareil d’utilisateur à notre API.

### Mécanisme d&#39;identification des périphériques

Le mécanisme de ralentissement proposé utilise les périphériques identifiés individuellement, à l’aide de l’en-tête &quot;X-Forwarded-For&quot;. Les limites seront appliquées de la même manière pour chaque appareil.

### Mises à jour requises

Les implémentations serveur à serveur doivent transférer les adresses IP de leur client à l’aide du mécanisme d’en-tête &quot;X-Forwarded-For&quot;.

Vous trouverez plus d’informations sur la manière de transmettre l’en-tête X-Forwarded-For [here](rest-api-cookbook-servertoserver.md).

### Limites et points de fin réels

Actuellement, la limite par défaut autorise un maximum de 1 requête par seconde, avec une première rafale de 3 requêtes (allocation unique lors de la première interaction du client identifié, ce qui devrait permettre à l’initialisation de se terminer avec succès). Cela ne devrait affecter aucune analyse de performances classique de tous nos clients.

Le mécanisme de ralentissement sera activé sur les points de terminaison suivants :

- /o/client/register
- /o/client/token
- /o/client/scopes
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authn
- /api/v1/tokens/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthn
- /api/v1/logout
- /api/v1/authorized
- /api/v1/preauthorized
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/.+/profile-requests/.+
- /api/v1/identities
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### clarification de l&#39;implémentation du SDK

Les clients qui utilisent les SDK fournis par l’authentification Adobe Pass n’interagissent pas explicitement avec les points de terminaison, cette section présente les fonctions connues, leur comportement en cas de réponse de ralentissement et les actions à entreprendre.

#### setRequestor

Lorsque vous atteignez la limite de ralentissement en utilisant `setRequestor` à partir du SDK, le SDK renvoie un code d’erreur CFG429 via `errorHandler` rappel.

#### getAuthorization

Lorsque vous atteignez la limite de ralentissement en utilisant `getAuthorization` à partir du SDK, le SDK renvoie un code d’erreur Z100 via `errorHandler` rappel.

#### checkPreauthorizedResources

Lorsque vous atteignez la limite de ralentissement en utilisant `checkPreauthorizedResources` à partir du SDK, le SDK renvoie un code d’erreur P100 via `errorHandler` rappel.

#### getMetadata

Lorsque vous atteignez la limite de ralentissement en utilisant `getMetadata` à partir du SDK, le SDK renvoie une réponse vide via `setMetadataStatus` rappel.

Pour chaque détail de mise en oeuvre spécifique, reportez-vous à la documentation spécifique du SDK.

- [Référence de l’API du SDK JavaScript](javascript-sdk-api-reference.md)
- [Référence de l’API du SDK Android](android-sdk-api-reference.md)
- [Référence de l’API iOS/tvOS](iostvos-sdk-api-reference.md)

### Modifications et réponse de la réponse de l’API

Lorsque nous identifions que la limite est atteinte, nous marquons cette requête avec un état de réponse spécifique (requêtes HTTP 429 Too many), indiquant que vous avez consommé tous les jetons affectés à l’appareil de l’utilisateur (adresse IP) pendant l’intervalle de temps.

Le ralentissement expire après une seconde des 429 premières réponses. Chaque application qui reçoit une réponse 429 doit attendre au moins 1 seconde avant de générer une nouvelle requête.

Toutes les applications client doivent gérer correctement la réponse &quot;429 Too many requests&quot;.

Voici un exemple de message de réponse 429 :

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## Impact et modifications requises

### Transfert de l’en-tête X-Forwarded-For

Les clients qui utilisent une mise en oeuvre personnalisée (y compris de serveur à serveur) pour interagir avec l’API Pass Authentication doivent s’assurer qu’ils peuvent capturer leur adresse IP utilisateur et la transférer correctement, en utilisant l’en-tête X-Forwarded-For plus loin dans l’API Pass Authentication.

Voir [here](rest-api-cookbook-servertoserver.md) pour plus d’informations.

### Réaction au nouveau code de réponse

Les clients qui utilisent une mise en oeuvre personnalisée (y compris celle du serveur à serveur) pour interagir avec l’API Pass Authentication doivent s’assurer que tout appel ultérieur effectué après la réception d’une demande 429 Too Many comprend une période d’attente d’au moins 1 seconde. Cette période d’attente permet de modifier ce mécanisme et d’obtenir une réponse commerciale valide.

## Exemple de scénario pour le ralentissement

| Temps écoulé depuis la première requête | Réponse reçue | Explication |
|--------------------------|-----------------------------------|----------------------------------------------------------------------------------------------------------|
| Second 0 | L’appel reçoit le code d’état de réussite | 1 appel consommé à partir de la limite |
| Second 0.3 | L’appel reçoit le code d’état de réussite | 1 appel consommé à partir de la limite et 1 appel marqués comme rafistolés |
| Second 0.6 | L’appel reçoit le code d’état de réussite | 1 appel consommé à partir de la limite et 2 appels marqués comme ayant été ouverts |
| Deuxième 0,9 | L’appel reçoit le code d’état de réussite | 1 appel consommé à partir de la limite et 3 appels marqués comme ayant été ouverts |
| Second 1.2 | L’appel reçoit le code d’état de réussite | 2 appels sont consommés à partir de la limite et 3 appels marqués comme perdants |
| Second 1.4 | L’appel reçoit le code d’état 429 | 2 appels sont consommés à partir de la limite et 3 appels marqués comme perdants et 1 appels reçoivent &quot;429 Too many requests&quot; |
| Second 1.6 | L’appel reçoit le code d’état 429 | 2 appels sont consommés à partir de la limite et 3 appels marqués comme perdants et 2 appels reçoivent ‘429 Too many requests’ |
| Second 1.8 | L’appel reçoit le code d’état 429 | 2 appels sont consommés à partir de la limite et 3 appels marqués comme perdants et 3 appels reçoivent ‘429 Too many requests’ |
| Second 2.1 | L’appel reçoit le code d’état de réussite | 3 appels sont consommés à partir de la limite et 3 appels marqués comme perdants et 3 appels reçoivent &quot;429 Too many requests&quot; (429 demandes excessives) |
