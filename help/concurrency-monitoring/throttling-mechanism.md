---
title: Mécanisme de ralentissement
description: Mécanisme de ralentissement
exl-id: 15236570-1a75-42fb-9bba-0e2d7a59c9f6
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '624'
ht-degree: 1%

---

# Mécanisme de ralentissement {#throttling-mechanism}

## Introduction {#introduction}

Adobe, en tant qu’entité de traitement des données, doit prendre les mesures appropriées pour s’assurer que les utilisateurs de nos clients utilisent équitablement les ressources et que le service ne soit pas inondé de requêtes d’API inutiles. Pour cela, nous avons mis en place un mécanisme de ralentissement.
Une application de surveillance de la simultanéité peut être utilisée par plusieurs utilisateurs et un utilisateur peut avoir plusieurs sessions. Par conséquent, les limites du nombre d’appels acceptés par utilisateur/session seront configurées pour le service dans un intervalle de temps spécifique.
Une fois la limite atteinte, les requêtes sont marquées avec un état de réponse spécifique (requêtes HTTP 429 Too many). Tout appel ultérieur effectué après la réception d’une réponse &quot;429 Too many requests&quot; doit être effectué avec une période de refroidissement d’au moins une minute pour s’assurer qu’il obtiendra une réponse commerciale valide.

## Présentation du mécanisme {#mechanism-overview}

Le mécanisme détermine le nombre maximal d’appels acceptés pour chaque point de terminaison de surveillance de la simultanéité au cours d’un intervalle de temps spécifique.
Une fois ce nombre maximum d&#39;appels atteint, notre service répondra avec &#39;429 Too many requests&#39;. L’en-tête de réponse 429 &quot;Date d’expiration&quot; inclut l’horodatage lorsque l’appel suivant est considéré comme valide ou lorsque le ralentissement expire. Pour l’instant, le ralentissement expire après l’un d’eux.   minute à partir de la première réponse 429.

Les points de terminaison configurés avec le ralentissement sont les suivants :
1. Créer une nouvelle session : POST /sessions/{idp}/{subject}
2. Appel Heartbeat : POST /sessions/{idp}/{subject}/{sessionId}
3. Arrêter une session : DELETE /sessions/{idp}/{subject}/{sessionId}

Le ralentissement est configuré à deux niveaux :
1. session : même paramètre {sessionId} unique envoyé dans l&#39;appel `Heartbeat` et l&#39;appel `Terminate a session`.
2. user : même paramètre {subject} unique envoyé dans l&#39;appel `Create a new session`.

La limite de limitation au niveau de la session est définie sur 200 requêtes en une minute.\
La limite de ralentissement au niveau de l’utilisateur est définie sur 200 requêtes en une minute.\
Ces deux limites (ralentissement au niveau de la session et ralentissement au niveau de l’utilisateur) sont configurables et nous les mettrons à jour au cas où elles seraient atteintes par des scénarios d’intégration valides. Pour ce faire, nous vous recommandons de contacter l’équipe d’assistance.

**Scénario pour le ralentissement au niveau de la session :**

| Heure | Demande d’envoi à CM | Nombre de requêtes | Réponse reçue de CM | Explication |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 10 secondes | POST /sessions/idp1/subject1/session1 | 50 | Tous les appels reçoivent &quot;202 Accepted&quot; | 50 appels consommés à partir de la limite |
| 50 secondes | POST /sessions/idp1/subject1/session1 | 151 | 150 appels reçoivent &quot;202 acceptés&quot; et 1 appel reçoit &quot;429 demandes en trop&quot; | 200 appels sont consommés à partir de la limite et 1 appel reçoit une réponse 429 |
| Second 61 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 appel reçoit &quot;429 demandes trop nombreuses&quot; | Aucun appel dans la limite disponible pour le moment |
| 70 secondes | DELETE /sessions/idp1/subject1/session1 | 1 | 1 appel reçoit &quot;202 Accepted&quot; | Limite à 200 appels disponibles, car 60 secondes se sont écoulées depuis les 10 secondes |

**Scénario pour le ralentissement au niveau de l’utilisateur :**

| Heure | Demande d’envoi à CM | Nombre de requêtes | Réponse reçue de CM | Explication |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 10 secondes | POST /sessions/idp1/subject1 | 50 | 50 appels reçoivent &quot;202 Accepted&quot; | 50 appels consommés à partir de la limite |
| 50 secondes | POST /sessions/idp1/subject1 | 151 | 150 appels reçoivent &quot;202 acceptés&quot; et 1 appel reçoit &quot;429 demandes en trop&quot; | 200 appels sont consommés à partir de la limite et 1 appel reçoit une réponse 429 |
| Second 61 | POST /sessions/idp1/subject1 | 1 | 1 appel reçoit &quot;429 demandes trop nombreuses&quot; | Aucun appel dans la limite disponible pour le moment |
| 70 secondes | POST /sessions/idp1/subject1 | 1 | 1 appel reçoit &quot;202 Accepted&quot; | Limite à 200 appels disponibles, car 60 secondes se sont écoulées depuis les 10 secondes |

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

## Recommandations relatives à l’intégration des clients {#customer-integration-recommendations}

Avec une mise en oeuvre correcte, les clients ne recevront pas la réponse &quot;429 Too many requests&quot;.
Néanmoins, Adobe recommande que chaque client traite la réponse &quot;429 Too many requests&quot; de manière appropriée à l’aide des détails techniques présentés ci-dessus. Lors de la gestion de la réponse, l’en-tête &quot;Expires&quot; doit être utilisé pour déterminer quand envoyer la prochaine requête valide.
