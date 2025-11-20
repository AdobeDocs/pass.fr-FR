---
title: Notes de mise à jour de la surveillance simultanée 2.5.0 d’Adobe Pass
description: Notes de mise à jour de la surveillance simultanée 2.5.0 d’Adobe Pass
exl-id: da392b18-a2aa-4f51-a75f-2c5b65b2b073
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 2%

---

# Notes de mise à jour de la surveillance simultanée 2.5.0 d’Adobe Pass {#cm-250}

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

Date de publication : 04/21/2016

## Nouvelles fonctionnalités {#new-features}

L’API de surveillance d’accès simultané v2.0 est désormais disponible en production.

La version V2 unifie les appels de pulsation et de requête et simplifie considérablement l’implémentation lorsque ces deux API sont utilisées simultanément.



### Cycle de vie d’une session {#session-lifecycle}

* API en écriture seule pour la création, la conservation en vie et la fin d’une session ; c’est-à-dire qu’aucune autre requête n’est nécessaire, la décision est renvoyée lors des appels d’initialisation et de pulsation de la session
* L’intervalle de pulsation est désormais piloté par le serveur via les en-têtes Expires définis sur les appels d’initialisation et de maintien en vie de session. Cela permet de configurer côté serveur les intervalles de pulsation et une valeur codée en dur de moins dans les applications.
* Le chemin heureux (comportement conforme de l’utilisateur et de l’application) est « pavé » de codes d’état 2XX

### Gestion des erreurs {#error-handling}

* Les décisions de refus, les métadonnées manquantes ou les comportements incorrects de l’application sont signalés comme des réponses 4XX (conflit, requête incorrecte, non autorisée, disparue, etc.).

* Lorsque cela est logique, la réponse inclut :

   * conseils associés — explication(s) détaillée(s) de l&#39;échec, à demander à l&#39;utilisateur.

   * obligations : actions obligatoires que l’application doit effectuer (par exemple, actualisation des métadonnées, déconnexion d’Adobe Pass).

### Métadonnées {#metadata}

* Métadonnées standard plutôt que personnalisées : cela permet à l’API d’identifier si des métadonnées incorrectes sont envoyées ou si des métadonnées requises par des règles sont manquantes.
* Les attributs requis pour chaque application sont désormais fournis par un appel API et doivent être mis en cache par les clients.
Notes

>[!NOTE]
>
>La version V1 reste disponible. Si les clients doivent utiliser des requêtes en dehors de l’appel de pulsation, la version V1 peut être utilisée.




### Correctifs {#bug-fixes}

S/O

### Problèmes connus {#known-issues}

S/O
