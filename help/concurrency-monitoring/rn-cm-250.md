---
title: Notes de mise à jour d’Adobe Pass Concurrency Monitoring 2.5.0
description: Notes de mise à jour d’Adobe Pass Concurrency Monitoring 2.5.0
exl-id: da392b18-a2aa-4f51-a75f-2c5b65b2b073
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 2%

---

# Notes de mise à jour d’Adobe Pass Concurrency Monitoring 2.5.0 {#cm-250}

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

Date de publication : 04/21/2016

## Nouvelles fonctionnalités {#new-features}

L’API de surveillance de la simultanéité v2.0 est désormais disponible en production.

La version V2 unifie les appels de pulsation et de requête et simplifie considérablement la mise en oeuvre lorsque ces deux API sont utilisées simultanément.



### Cycle de vie de la session {#session-lifecycle}

* API en écriture seule pour la création, la persistance et la fin d’une session : en d’autres termes, plus de requêtes, la décision est renvoyée pour les appels d’initialisation de session et de pulsation.
* L’intervalle de pulsation est désormais piloté par le serveur via les en-têtes Expires définis sur les appels init de session et keep-alive. Cela permet la configuration côté serveur des intervalles de pulsation et une valeur codée en moins dans les applications.
* Le chemin d’accès heureux (comportement conforme de l’utilisateur et de l’application) est &quot;pavé&quot; avec des codes d’état 2XX.

### Gestion des erreurs {#error-handling}

* Les décisions de refus, les métadonnées manquantes ou le mauvais comportement de l’application sont marqués comme réponses 4XX (Conflit, Bad Request, Unauthorized, Gone, etc.)

* Chaque fois que cela a du sens, la réponse comprend :

   * conseil associé : explication(s) détaillée(s) de l’échec, à inviter à l’utilisateur.

   * obligations : actions obligatoires que l’application doit entreprendre (par exemple : actualisation des métadonnées, déconnexion d’Adobe Pass).

### Métadonnées {#metadata}

* Plutôt que des métadonnées personnalisées, cette API permet d’identifier si des métadonnées incorrectes sont envoyées ou si des métadonnées requises par les règles sont manquantes.
* Les attributs requis pour chaque application sont désormais fournis par un appel API et doivent être mis en cache par les clients.
Notes

>[!NOTE]
>
>La version V1 reste disponible. Si les clients doivent utiliser des requêtes en dehors de l’appel de pulsation, la version V1 peut être utilisée.




### Correctifs {#bug-fixes}

S/O

### Problèmes connus {#known-issues}

S/O
