---
title: Point de décision de politique
description: Point de décision de politique
exl-id: 94bc638c-bef8-45ea-b20a-9b7038adecdd
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 0%

---

# Point de décision de politique {#policy-desc-pt}

## Modèle de domaine {#domain-model}

Cette page est destinée à servir de référence pour différents cas d’utilisation et implémentations de politiques. Nous vous conseillons également de consulter la partie [Glossaire](/help/concurrency-monitoring/cm-glossary.md) de la documentation pour obtenir des définitions de termes.

Un **client** possède des **applications** pour lesquelles il souhaite appliquer des **politiques**. **Applications clientes** doit être configuré avec l’**ID d’application** (fourni par Adobe).

Le client associe ensuite chaque application à une ou plusieurs politiques, qu’elles soient créées par lui ou créées et partagées par d’autres. Les politiques peuvent être liées entre plusieurs clients.

L’**activité du sujet** se compose de tous les flux (quelle que soit l’application) qui sont signalés à la surveillance de simultanéité pour un sujet donné.

Lorsqu’un flux doit être autorisé pour un objet donné, le système vérifie d’abord toutes les politiques définies pour l’application qui a créé le flux.

Pour chacune des politiques applicables, nous devons ensuite collecter toutes les **activités pertinentes** qui seront transmises à la règle. L&#39;**activité pertinente** d&#39;une politique P ne comprendra un flux S que si elle remplit la condition suivante :

**Le flux « S » est démarré par une application qui inclut la politique « P » parmi ses politiques.**

![Le flux « S » est démarré par une application qui inclut la politique « P » parmi ses politiques.](assets/pdp-domain-model.png)

## Cas D’Utilisation De L’Exécution D’Essai {#dry-run-use-cases}

La présentation ci-dessous vise à valider le modèle par rapport à certains cas d’utilisation. Nous le ferons progressivement, en commençant par une configuration de base et en ajoutant de la complexité de différentes manières.

### &#x200B;1. Un locataire. Une application. Une seule politique. Un flux {#onetenant-oneapp-onepolicy-onestream}

Nous allons commencer avec un seul client, avec une seule application et une seule politique associée. Supposons que la politique indique qu’il peut y avoir au plus un flux actif pour n’importe quel utilisateur (le dernier flux est autorisé à être lu).

Une fois qu’un flux est démarré, l’activité ne consiste qu’en ce flux et sa lecture est autorisée.

![Un client. Une application. Une seule politique. Un flux](assets/onetenant-app-policy-stream.png)


### &#x200B;2. Un locataire. Une application. Une seule politique. Deux courants. {#onetenant-oneapp-onepolicy-twostreams}

Une fois qu’un second flux est démarré (par le même sujet utilisant la même application), l’activité utilisée pour la validation se compose de **s1** et **s2**.

La limite est dépassée, car la politique indique qu’un seul flux est autorisé à être lu. Nous n’autoriserons donc que le dernier flux (**s2**) à être lu.

![Un client. Une application. Une seule politique. Deux flux.](assets/tenant-app-policy-twostream.png)

>[!NOTE]
>
>Les diagrammes représentent la vue du système sur l’activité de l’utilisateur. Pour les tentatives d’initialisation de flux, la décision d’accès sera incluse dans la réponse. Pour les flux actifs, la décision est renvoyée lors de la réponse de pulsation.

### &#x200B;3. Deux locataires. Deux applications. Une seule politique. Deux courants. {#twotenant-twoapp-onepolicy-twostreams}

Supposons maintenant qu’un nouveau client souhaite appliquer la même politique dans ses applications :

![Deux locataires. Deux applications. Une seule politique. Deux flux.](assets/onepolicy-twotenant-app-stream.png)

Comme les deux clients sont liés par la même politique, la situation décrite dans le cas d’utilisation 2 s’applique ici et **s3** est autorisé à être lu, car il s’agit du dernier flux.

### &#x200B;4. Deux locataires. Trois applications. Deux politiques. Deux courants. {#twotenants-threeapps-twopolicies-twostreams}

Maintenant, supposons que le deuxième client déploie une nouvelle application et souhaite définir une nouvelle politique qui sera partagée entre **app2** et **app3**.

![Deux locataires. Trois applications. Deux politiques. Deux flux.](assets/twotenant-policies-streams-threeapps.png)

A ce moment, les flux actifs **s3** et **s4** sont tous deux autorisés. Pour **s3**, lorsque la politique **P1** est évaluée, le système ne comptabilise que **s3** comme **activité pertinente** (**s4** n’est en aucun cas lié à la politique **P1**), il n’y a donc aucune violation.

La politique **P2** s&#39;applique aux deux volets et inclut les **s3** et **s4** comme activité pertinente. Comme cette activité se trouve dans les limites de deux flux, les deux flux sont autorisés.

### &#x200B;5. Deux locataires. Trois applications. Deux politiques. Trois courants. {#twotenants-threeapps-twopolicies-threestreams}

En supposant maintenant qu’une nouvelle tentative d’initialisation de flux soit effectuée à l’aide de **app2** :

![Deux locataires. Trois applications. Deux politiques. Trois flux.](assets/twotenants-policies-threeapps-streams.png)

**s5** est autorisé à commencer par **P1** (ce qui permet à de nouveaux flux de prendre le relais) mais il est refusé par **P2**, donc il ne commencera pas.

La même chose se produit si une init de flux est tentée avec app3 : la même politique P2 refuse l’accès pour elle.

![](assets/stream-init-attempted-app3.png)

Maintenant, voyons ce qui se passerait si l’utilisateur ou l’utilisatrice tentait de créer un flux à l’aide d’app1 :

![](assets/new-stream-with-app1.png)

L’application app1 n’est en aucun cas liée à la politique **P2**, elle appliquera donc uniquement la politique **P1** : qui permet au nouveau flux de démarrer et refuse l’ancien (**s3** dans ce cas).
