---
title: Mécanisme de limitation
description: Mécanisme de limitation
exl-id: 15236570-1a75-42fb-9bba-0e2d7a59c9f6
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 1%

---

# Mécanisme de limitation {#throttling-mechanism}

## Introduction {#introduction}

Adobe, en tant que responsable du traitement des données, doit prendre les mesures appropriées pour s’assurer que les utilisateurs et utilisatrices de nos clients utilisent équitablement les ressources et que le service n’est pas inondé de requêtes d’API inutiles. Pour cela, nous avons mis en place un mécanisme de limitation.
Une application de surveillance simultanée peut être utilisée par plusieurs utilisateurs et un utilisateur peut avoir plusieurs sessions. Par conséquent, des limites seront configurées pour le nombre d’appels acceptés par utilisateur/session dans un intervalle de temps spécifique.
Lorsque la limite est atteinte, les requêtes sont marquées avec un statut de réponse spécifique (HTTP 429 Too Many Requests). Tout appel ultérieur effectué après réception d’une réponse « 429 Too Many Requests » doit être effectué avec une période de refroidissement d’au moins une minute afin de s’assurer qu’il obtiendra une réponse commerciale valide.

## Présentation du mécanisme {#mechanism-overview}

Le mécanisme détermine le nombre maximal d’appels acceptés pour chaque point d’entrée de la surveillance d’accès simultané dans un intervalle de temps spécifique.
Une fois ce nombre maximum d&#39;appels atteint, notre service répondra avec &#39;429 Too many requests&#39;. L’en-tête « Expires » de la réponse 429 inclut la date et l’heure de validité de l’appel suivant ou de l’expiration de la limitation. Pour l’instant, la limitation expire après une   minute depuis la première réponse 429.

Les points d’entrée configurés avec le ralentissement sont les suivants :
1. Créez une session : POST /sessions/{idp}/{subject}
2. Appel de pulsation : POST /sessions/{idp}/{subject}/{sessionId}
3. Terminer une session : DELETE /sessions/{idp}/{subject}/{sessionId}

La limitation est configurée à deux niveaux :
1. session : même paramètre de {sessionId} unique envoyé dans `Heartbeat` appel et `Terminate a session` appel .
2. utilisateur : même paramètre de {subject} unique envoyé dans l’appel `Create a new session`.

La limite de la limitation au niveau de la session est définie sur 200 requêtes en une minute.\
La limite de la limitation au niveau de l’utilisateur est définie sur 200 requêtes en une minute.\
Ces deux limites (limitation au niveau de la session et limitation au niveau de l’utilisateur) sont configurables et nous les mettrons à jour au cas où elles seraient atteintes par le biais de scénarios d’intégration valides. Pour cela, nous vous recommandons de contacter l’équipe d’assistance.

**Scénario de limitation au niveau de la session :**

| Heure | Demande d’envoi à CM | Nombre de demandes | Réponse reçue de CM | Explication |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Second 10 | POST /sessions/idp1/subject1/session1 | 50 | Tous les appels reçoivent le numéro « 202 Accepted » | 50 appels consommés depuis la limite |
| Deuxième 50 | POST /sessions/idp1/subject1/session1 | 151 | 150 appels reçoivent « 202 acceptés » et 1 appel reçoit « 429 demandes trop nombreuses » | 200 appels consommés depuis la limite et 1 appel recevra une réponse 429 |
| Deuxième 61 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 appel reçoit « 429 Too many requests » | Aucun appel dans la limite disponible pour le moment |
| Deuxième 70 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 appel reçoit le numéro « 202 Accepted » | Limite définie sur 200 appels disponibles car 60 secondes se sont écoulées depuis la seconde 10 |

**Scénario de limitation au niveau de l’utilisateur :**

| Heure | Demande d’envoi à CM | Nombre de demandes | Réponse reçue de CM | Explication |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Second 10 | POST /sessions/idp1/subject1 | 50 | 50 appels reçoivent le numéro « 202 Accepted » | 50 appels consommés depuis la limite |
| Deuxième 50 | POST /sessions/idp1/subject1 | 151 | 150 appels reçoivent « 202 acceptés » et 1 appel reçoit « 429 demandes trop nombreuses » | 200 appels consommés depuis la limite et 1 appel recevra une réponse 429 |
| Deuxième 61 | POST /sessions/idp1/subject1 | 1 | 1 appel reçoit « 429 Too many requests » | Aucun appel dans la limite disponible pour le moment |
| Deuxième 70 | POST /sessions/idp1/subject1 | 1 | 1 appel reçoit le numéro « 202 Accepted » | Limite définie sur 200 appels disponibles car 60 secondes se sont écoulées depuis la seconde 10 |

**429 Exemple de réponse :**

```
HTTP/2 429
date: Thu, 15 Feb 2024 07:54:20 GMT
content-length: 0
vary: Origin
vary: Access-Control-Request-Method
vary: Access-Control-Request-Headers
cache-control: no-store
expires: Thu, 15 Feb 2024 07:54:41 GMT
strict-transport-security: max-age=31536000 ; includeSubDomains
x-xss-protection: 1; mode=block
x-frame-options: DENY
x-content-type-options: nosniff
```

## Recommandations d’intégration client {#customer-integration-recommendations}

Avec une implémentation correcte, les clients ne recevront pas de réponse « 429 Too Many Requests ».
Néanmoins, Adobe recommande à chaque client de traiter la réponse « 429 Too Many Requests » de manière appropriée à l’aide des détails techniques présentés ci-dessus. Lors de la gestion de la réponse, l’en-tête « Expires » doit être utilisé pour déterminer quand envoyer la requête valide suivante.
