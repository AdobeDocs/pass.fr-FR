---
title: Mécanisme de limitation
description: Découvrez le mécanisme de limitation utilisé dans l’authentification Adobe Pass. Découvrez un aperçu de ce mécanisme sur cette page.
exl-id: f00f6c8e-2281-45f3-b592-5bbc004897f7
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '1141'
ht-degree: 0%

---

# Mécanisme de limitation {#throttling-mechanism}

Tous les clients Pass Authentication doivent pouvoir accéder à l’API Pass Authentication pour chacun de leurs utilisateurs, conformément aux instructions et à leur business case.

L’authentification par mot de passe introduit un mécanisme de limitation pour assurer une répartition équitable des ressources entre les utilisateurs de nos clients.

Ce mécanisme est important pour plusieurs raisons :

- L’une des règles d’or de l’architecture à plusieurs clients est que le comportement d’un utilisateur ne doit pas affecter quelqu’un d’autre.
- La limitation du débit est importante pour les API, car il est facile de faire des erreurs lors de l’intégration à une API. Comme les API sont programmées, il est relativement facile d’envoyer accidentellement plus de requêtes que prévu.

## Présentation du mécanisme {#mechanism-overview}

### Implémentations client

L’authentification Pass fournit des instructions et un SDK pour interagir avec l’API, mais ne contrôle pas la manière dont le client l’utilise. Certaines implémentations peuvent avoir une implémentation rudimentaire et pourraient inonder le service de requêtes d’API inutiles, que ce soit accidentellement ou non, ce qui pourrait entraîner des ralentissements ou des problèmes de capacité pour d’autres utilisateurs et utilisatrices.

Le service lui-même devrait être en mesure de gérer toute capacité raisonnable. Mais peu importe l’évolutivité ou les performances d’un service, il y a toujours des limites. Par conséquent, des limites doivent être configurées pour le nombre d’appels acceptés dans un intervalle de temps spécifique pour le service.

### Introduction du ralentissement

L&#39;authentification Pass est basée sur l&#39;identification de l&#39;utilisateur et un algorithme de limitation du taux de compartiments de jetons avec des valeurs prédéfinies pour contrôler l&#39;accès de chaque utilisateur à notre API.

### Mécanisme d&#39;identification de dispositif

Le mécanisme de limitation proposé utilise individuellement les dispositifs identifiés, à l&#39;aide de l&#39;en-tête « X-Forwarded-For ». Les limites seront appliquées de la même manière pour chaque appareil.

### Mises à jour requises

Les implémentations serveur à serveur doivent transférer les adresses IP de leur client à l’aide du mécanisme d’en-tête « X-Forwarded-For ».

Vous trouverez plus d’informations sur la manière de transmettre l’en-tête X-Forwarded-For [ici](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md).

### Limites et points d’entrée réels {#throttling-mechanism-limits}

Actuellement, la limite par défaut autorise un maximum de 1 requête par seconde, avec un rafale initiale de 10 requêtes (autorisation unique sur la première interaction du client identifié, ce qui devrait permettre à l’initialisation de se terminer correctement). Cela ne devrait pas affecter les analyses de rentabilisation standard de tous nos clients.

Le mécanisme de limitation sera activé sur les points d’entrée suivants :

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
- /api/v1/authorize
- /api/v1/preauthorize
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/.+/profile-requests/.+
- /api/v1/identities
- /adobe-services/config/
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### Désambiguïté de l’implémentation de SDK

Comme les clients qui utilisent les SDK fournis par l’authentification Adobe Pass n’interagissent pas explicitement avec des points d’entrée, cette section présente les fonctions connues, leur comportement en cas de réponse de limitation et les actions à entreprendre.

#### setRequestor

Une fois la limite de limitation atteinte à l’aide de `setRequestor` fonction du SDK, le SDK renvoie un code d’erreur CFG429 par le biais `errorHandler` rappel .

#### getAuthorization

Une fois la limite de limitation atteinte à l’aide de `getAuthorization` fonction du SDK, le SDK renvoie un code d’erreur Z100 par le biais `errorHandler` rappel .

#### checkPreauthorizedResources

Une fois la limite de limitation atteinte à l’aide de `checkPreauthorizedResources` fonction du SDK, le SDK renvoie un code d’erreur P100 par le biais `errorHandler` rappel .

#### getMetadata

Une fois la limite de limitation atteinte à l’aide de `getMetadata` fonction du SDK, le SDK renvoie une réponse vide par le biais d’`setMetadataStatus` rappel .

Pour chaque détail d’implémentation spécifique, reportez-vous à la documentation spécifique de SDK.

- [Référence de l’API JavaScript SDK](legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
- [Référence de l’API Android SDK](legacy/sdks/android-sdk/android-sdk-api-reference.md)
- [Référence de l’API iOS/tvOS](legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

### Modifications de la réponse de l’API et réponse {#throttling-mechanism-response}

Lorsque nous identifions que la limite est dépassée, nous marquons cette requête avec un statut de réponse spécifique (HTTP 429 Too Many Requests), indiquant que vous avez consommé tous les jetons affectés à l’appareil utilisateur (adresse IP) pendant l’intervalle de temps.

La limitation expire après une seconde des 429 premières réponses. Chaque application qui reçoit une réponse 429 doit attendre au moins 1 seconde avant de générer une nouvelle requête.

Toutes les applications clientes doivent gérer de manière appropriée la réponse « 429 Too Many Requests ».

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

### Transmettre l’en-tête X-Forwarded-For

Les clients qui utilisent une implémentation personnalisée (y compris de serveur à serveur) pour interagir avec l’API Pass Authentication doivent s’assurer qu’ils peuvent capturer leur adresse IP utilisateur et la transférer correctement, en utilisant l’en-tête X-Forwarded-For suite à l’API Pass Authentication.

Voir [ici](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md) pour plus d’informations.

### Réaction au nouveau code de réponse

Les clients qui utilisent une implémentation personnalisée (y compris de serveur à serveur) pour interagir avec l’API d’authentification Pass doivent s’assurer que tout appel ultérieur effectué après réception d’une demande 429 Too many inclut une période d’attente d’au moins 1 seconde. Cette période d&#39;attente permet de modifier ce mécanisme et d&#39;obtenir une réponse commerciale valide.

## Exemple de scénario de limitation

| Temps depuis la première demande | Réponse reçue | Explication |
|--------------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------|
| Seconde 0 | L’appel reçoit le code de statut de succès. | 1 appel consommé depuis la limite |
| Deuxième 0,3 | L’appel reçoit le code de statut de succès. | 1 appels consommés à partir de la limite et 1 appels marqués comme rafale |
| Deuxième 0,6 | L’appel reçoit le code de statut de succès. | 1 appel consommé à partir de la limite et 2 appels marqués comme rafale |
| Deuxième 0,9 | L’appel reçoit le code de statut de succès. | 1 appel consommé à partir de la limite et 3 appels marqués comme rafale |
| Deuxième 1.2 | L’appel reçoit le code de statut de succès. | 2 appels consommés à partir de la limite et 3 appels marqués comme rafale |
| Deuxième 1.3 | L’appel reçoit le code de statut de succès. | 2 appels consommés à partir de la limite et 4 appels marqués comme rafale |
| Deuxième 1.4 | L’appel reçoit le code de statut de succès. | 2 appels consommés à partir de la limite et 5 appels marqués comme rafale |
| Deuxième 1,5 | L’appel reçoit le code de statut de succès. | 2 appels consommés à partir de la limite et 6 appels marqués comme rafale |
| Deuxième 1,6 | L’appel reçoit le code de statut de succès. | 2 appels consommés à partir de la limite et 7 appels marqués comme rafale |
| Deuxième 1,7 | L’appel reçoit le code de statut de succès. | 2 appels consommés à partir de la limite et 8 appels marqués comme rafale |
| Deuxième 1,8 | L’appel reçoit le code de statut de succès. | 2 appels consommés à partir de la limite et 9 appels marqués comme rafale |
| Deuxième 2.1 | L’appel reçoit le code de statut de succès. | 3 appels consommés à partir de la limite et 9 appels marqués comme rafale |
| Deuxième 2.2 | L’appel reçoit le code de statut de succès. | 3 appels consommés à partir de la limite et 10 appels marqués comme rafale |
| Deuxième 2.4 | L&#39;appel reçoit le code d&#39;état 429 | 3 appels consommés à partir de la limite et 10 appels marqués comme rafale et 1 appel reçoit « 429 demandes trop nombreuses » |
| Deuxième 2.6 | L&#39;appel reçoit le code d&#39;état 429 | 3 appels consommés à partir de la limite et 10 appels marqués comme rafale et 2 appels reçoivent « 429 demandes trop nombreuses » |
| Deuxième 2,8 | L&#39;appel reçoit le code d&#39;état 429 | 3 appels consommés à partir de la limite et 10 appels marqués comme rafale et 3 appels reçoivent « 429 demandes trop nombreuses » |
| Deuxième 3.1 | L’appel reçoit le code de statut de succès. | 4 appels consommés à partir de la limite et 10 appels marqués comme étant en rafale et 3 appels reçoivent « 429 Too many requests » |
