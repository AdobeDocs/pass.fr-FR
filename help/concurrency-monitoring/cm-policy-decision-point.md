---
title: Point de décision de la stratégie
description: Point de décision de la stratégie
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 0%

---


# Point de décision de la stratégie {#policy-desc-pt}

## Modèle de domaine {#domain-model}

Cette page est destinée à servir de référence pour différents cas d’utilisation et mises en oeuvre de stratégies. Nous vous conseillons également de consulter le [Glossaire](/help/concurrency-monitoring/cm-glossary.md) partie de la documentation relative aux définitions de termes.

A **client** owned **applications** pour laquelle il souhaite appliquer **policies**. **Applications clientes** doit être configuré à l’aide de la fonction **ID d’application** (fourni par Adobe).

Le client associe ensuite chaque application à une ou plusieurs stratégies, qu’elles soient créées par lui ou créées et partagées par d’autres. Les stratégies peuvent être liées entre plusieurs clients.

La variable **activité subject** se compose de tous les flux (quelle que soit l’application) qui sont signalés à la surveillance de la simultanéité pour un certain sujet.

Lorsqu’un flux doit être autorisé pour un sujet donné, le système vérifie d’abord toutes les stratégies définies pour l’application qui a créé le flux.

Pour chacune des stratégies applicables, nous devons ensuite collecter toutes les **activité pertinente** qui sera transmis à la règle. La variable **activité pertinente** pour une stratégie, P inclura uniquement un flux S s’il répond à la condition suivante :

**Le flux &quot;S&quot; est démarré par une application qui inclut la stratégie &quot;P&quot; dans ses stratégies.**

![Le flux &quot;S&quot; est démarré par une application qui inclut la stratégie &quot;P&quot; dans ses stratégies.](assets/pdp-domain-model.png)

## Cas d’utilisation d’exécution rapide {#dry-run-use-cases}

La présentation ci-dessous vise à valider le modèle par rapport à certains cas d’utilisation. Nous le ferons progressivement, en commençant par une configuration de base et en ajoutant de la complexité de différentes manières.

### 1. Un client. Une application. Une seule politique. Un flux {#onetenant-oneapp-onepolicy-onestream}

Nous commencerons avec un seul client, associé à une seule application et à une seule stratégie. Supposons que la stratégie indique qu’il peut y avoir au plus un flux actif pour n’importe quel utilisateur (le dernier flux est autorisé à être lu).

Une fois qu’un flux est démarré, l’activité ne comprend que ce flux et il est autorisé à le lire.

![Un client. Une application. Une seule politique. Un flux](assets/onetenant-app-policy-stream.png)


### 2. Un client. Une application. Une seule politique. Deux ruisseaux. {#onetenant-oneapp-onepolicy-twostreams}

Une fois qu’un deuxième flux est lancé (par le même sujet, utilisant la même application), l’activité utilisée pour la validation se compose des deux éléments suivants : **s1** et **s2**.

La limite est dépassée, car la stratégie indique qu’un seul flux est autorisé à être lu. Nous n’autoriserons donc que le dernier flux (**s2**) à lire.

![Un client. Une application. Une seule politique. Deux ruisseaux.](assets/tenant-app-policy-twostream.png)

>[!NOTE]
>
>Les diagrammes représentent la vue système sur l’activité de l’utilisateur. Pour les tentatives d’initialisation du flux, la décision d’accès sera incluse dans la réponse. Pour les flux actifs, la décision est renvoyée sur la réponse de pulsation.

### 3. Deux locataires. Deux applications. Une seule politique. Deux ruisseaux. {#twotenant-twoapp-onepolicy-twostreams}

Supposons maintenant qu’un nouveau client souhaite appliquer la même stratégie dans ses applications :

![Deux locataires. Deux applications. Une seule politique. Deux ruisseaux.](assets/onepolicy-twotenant-app-stream.png)

Les deux clients étant liés par la même politique, la situation décrite dans le cas d&#39;utilisation 2 est applicable ici et **s3** est autorisé à jouer, car il s’agit du dernier flux.

### 4. Deux locataires. Trois applications. Deux politiques. Deux ruisseaux. {#twotenants-threeapps-twopolicies-twostreams}

Maintenant, supposons que le deuxième client déploie une nouvelle application et souhaite définir une nouvelle stratégie qui sera partagée entre **app2** et **app3**.

![Deux locataires. Trois applications. Deux politiques. Deux ruisseaux.](assets/twotenant-policies-streams-threeapps.png)

En ce moment, les flux actifs **s3** et **s4** sont toutes les deux autorisées. Pour **s3**, lorsque la stratégie **P1** est évalué, le système ne comptera que **s3** as **activité pertinente** (**s4** n’est en aucun cas lié à la stratégie ; **P1**), il n’y a donc pas de violation.

Stratégie **P2** s’applique aux deux diffusions et inclut les deux **s3** et **s4** comme activité pertinente. Comme cette activité se trouve dans les limites de deux diffusions, les deux diffusions sont autorisées.

### 5. Deux locataires. Trois applications. Deux politiques. Trois ruisseaux. {#twotenants-threeapps-twopolicies-threestreams}

Maintenant, en supposant qu’une nouvelle tentative d’initialisation de flux soit effectuée à l’aide de **app2**:

![Deux locataires. Trois applications. Deux politiques. Trois ruisseaux.](assets/twotenants-policies-threeapps-streams.png)

**s5** est autorisé à commencer par **P1** (ce qui permet aux nouveaux flux de prendre le relais), mais il est refusé par **P2**, donc ça ne commencera pas.

La même chose se produit si une initialisation de flux est tentée avec app3 : la même stratégie P2 lui refuse l’accès.

![](assets/stream-init-attempted-app3.png)

Maintenant, voyons ce qui se passerait si l’utilisateur tentait de créer un flux à l’aide d’app1 :

![](assets/new-stream-with-app1.png)

L’application app1 n’a aucun lien avec la stratégie. **P2**, de sorte qu’il ne s’applique qu’à la stratégie. **P1**: permet au nouveau flux de démarrer et refuse l’ancien (**s3** dans ce cas).

