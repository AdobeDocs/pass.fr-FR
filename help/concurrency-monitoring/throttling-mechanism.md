---
title: Mécanisme de ralentissement
description: Mécanisme de ralentissement
source-git-commit: 1390c09d3de6b4608cdcc97b3f053cce71e84639
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 1%

---


# Mécanisme de ralentissement {#throttling-mechanism}

## Introduction {#introduction}

Adobe, en tant qu’entité de traitement des données, doit prendre les mesures appropriées pour s’assurer que les utilisateurs de nos clients utilisent équitablement les ressources et que le service ne soit pas inondé de requêtes d’API inutiles. Pour cela, nous avons mis en place un mécanisme de ralentissement.\
Une application de surveillance de la simultanéité peut être utilisée par plusieurs utilisateurs et un utilisateur peut avoir plusieurs sessions. Par conséquent, les limites du nombre d’appels acceptés par utilisateur/session seront configurées pour le service dans un intervalle de temps spécifique.\
Une fois la limite atteinte, les requêtes sont marquées avec un état de réponse spécifique (requêtes HTTP 429 Too many). Tout appel ultérieur effectué après la réception d’une réponse &quot;429 Too many requests&quot; doit être effectué avec une période de refroidissement d’au moins 1 minute pour s’assurer qu’il obtiendra une réponse commerciale valide.

## Présentation du mécanisme {#mechanism-overview}

Le mécanisme détermine le nombre maximal d’appels acceptés pour chaque point de terminaison de surveillance de la simultanéité au cours d’un intervalle de temps spécifique.
Une fois ce nombre maximum d&#39;appels atteint, notre service répondra avec &#39;429 Too many requests&#39;. Le service a besoin de 60 secondes supplémentaires pour réinitialiser la limite à sa valeur maximale.

Les points de terminaison configurés avec le ralentissement sont les suivants :
1. Créer une nouvelle session : POST /sessions/{idp}/{subject}
2. Appel Heartbeat : POST /sessions/{idp}/{subject}/{sessionId}
3. Arrêt d’une session : DELETE /sessions/{idp}/{subject}/{sessionId}

Le ralentissement est configuré à deux niveaux :
1. session : même unique {sessionId} paramètre envoyé `Heartbeat` appelez et `Terminate a session` appelez .
2. user : même unique {subject} paramètre envoyé `Create a new session` appelez .

La limite de limitation au niveau de la session est définie sur 200 requêtes en une minute.\
La limite de ralentissement au niveau de l’utilisateur est définie sur 200 requêtes en une minute.\
Ces deux limites (ralentissement au niveau de la session et ralentissement au niveau de l’utilisateur) sont configurables et nous les mettrons à jour au cas où elles seraient atteintes par des scénarios d’intégration valides. Pour ce faire, nous vous recommandons de contacter l’équipe d’assistance.

Voici un scénario pour le ralentissement au niveau de la session :

| Heure | Demande d’envoi à CM | Nombre de requêtes | Réponse reçue de CM | Explication |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 10 secondes | POST /sessions/idp1/subject1/session1 | 50 | Tous les appels reçoivent &quot;202 Accepted&quot; | 50 appels consommés à partir de la limite |
| 50 secondes | POST /sessions/idp1/subject1/session1 | 151 | 150 appels reçoivent &quot;202 acceptés&quot; et 1 appel reçoit &quot;429 demandes en trop&quot; | 200 appels sont consommés à partir de la limite et 1 appel reçoit une réponse 429 |
| Second 61 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 appel reçoit &quot;429 demandes trop nombreuses&quot; | Aucun appel dans la limite disponible pour le moment |
| 70 secondes | DELETE /sessions/idp1/subject1/session1 | 1 | 1 appel reçoit &quot;202 Accepted&quot; | Limite à 200 appels disponibles, car 60 secondes se sont écoulées depuis les 10 secondes |

et un scénario pour le ralentissement au niveau de l’utilisateur :

| Heure | Demande d’envoi à CM | Nombre de requêtes | Réponse reçue de CM | Explication |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 10 secondes | POST /sessions/idp1/subject1 | 50 | 50 appels reçoivent &quot;202 Accepted&quot; | 50 appels consommés à partir de la limite |
| 50 secondes | POST /sessions/idp1/subject1 | 151 | 150 appels reçoivent &quot;202 acceptés&quot; et 1 appel reçoit &quot;429 demandes en trop&quot; | 200 appels sont consommés à partir de la limite et 1 appel reçoit une réponse 429 |
| Second 61 | POST /sessions/idp1/subject1 | 1 | 1 appel reçoit &quot;429 demandes trop nombreuses&quot; | Aucun appel dans la limite disponible pour le moment |
| 70 secondes | POST /sessions/idp1/subject1 | 1 | 1 appel reçoit &quot;202 Accepted&quot; | Limite à 200 appels disponibles, car 60 secondes se sont écoulées depuis les 10 secondes |


## Recommandations relatives à l’intégration des clients {#customer-integration-recommendations}

Avec une mise en oeuvre correcte, les clients ne recevront pas la réponse &quot;429 Too many requests&quot;.\
Néanmoins, Adobe recommande que chaque client traite la réponse &quot;429 Too many requests&quot; de manière appropriée à l’aide des détails techniques présentés ci-dessus.
