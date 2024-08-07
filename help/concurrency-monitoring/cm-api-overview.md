---
title: Présentation des API
description: API - Aperçu de la surveillance de la simultanéité
exl-id: eb232926-9c68-4874-b76d-4c458d059f0d
source-git-commit: dd370b231acc08ea0544c0dedaa1bdb0683e378f
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 0%

---

# Présentation de l’API {#api-overview}

Pour plus d’informations, consultez la [documentation de l’API en ligne](http://docs.adobeptime.io/cm-api-v2/) .

## Objectif et conditions préalables {#purpose-prerequisites}

Ce document aide les développeurs d’applications à utiliser notre spécification API Swagger lors de l’implémentation d’une intégration avec la surveillance de la simultanéité. Il est vivement recommandé que le lecteur ait une compréhension antérieure des concepts définis par le service avant de suivre cette directive. Pour que cette compréhension soit possible, il est nécessaire d’avoir une vue d’ensemble de la [documentation du produit](/help/concurrency-monitoring/cm-home.md) et de la [spécification de l’API Swagger](http://docs.adobeptime.io/cm-api-v2/).


## Introduction {#api-overview-intro}

Au cours du processus de développement, la documentation publique Swagger représente la ligne directrice de référence pour comprendre et tester les flux d’API. Il s’agit d’un excellent point de départ pour une approche pratique et pour vous familiariser avec le comportement des applications réelles dans différents scénarios d’interaction utilisateur.

Envoyez un ticket dans [Zendesk](mailto:tve-support@adobe.com) pour enregistrer votre société et vos applications dans la surveillance de la simultanéité. Adobe attribuera un ID d’application à chaque entité. Dans ce guide, nous utiliserons deux applications de référence avec les identifiants **demo-app** et **demo-app-2** qui seront sous l’Adobe du client.


## Cas d’utilisation {#api-use-case}

La première étape du test d’un flux à l’aide de Swagger consiste à saisir l’ID d’application en haut à droite de la page comme suit :

![](assets/setting-app-id.png)

Ensuite, appuyez sur **Explorer** pour définir l’identifiant qui sera utilisé dans l’en-tête d’autorisation pour tous les appels effectués vers l’API REST.  Chaque appel API exige que l’ID de l’application soit transmis via l’authentification de base HTTP. Le nom d’utilisateur correspond à l’ID de l’application et le mot de passe est vide.


### Première application {#first-app-use-cases}

L’équipe d’Adobe a attribué à l’application avec l’ID **demo-app** une stratégie comportant une règle qui limite à 3 le nombre de diffusions simultanées. Une stratégie est affectée à une application spécifique en fonction de la demande envoyée dans Zendesk.


#### Récupération des métadonnées {#retrieve-metadata-use-case}

Le premier appel que nous effectuons concerne la ressource Métadonnées afin d’obtenir la liste des attributs de métadonnées à transmettre en tant que données de formulaire lors de l’initialisation de la session. Ces métadonnées seront utilisées pour évaluer les stratégies affectées à cette application.

![](assets/retrieving-metadata.png)

Après avoir appuyé sur &quot;Essayez-le&quot;, pour l’application avec l’ID **demo-app**, nous obtiendrons le résultat suivant :

![](assets/empty-metadata-call.png)

Comme nous pouvons le constater dans le champ de corps de la réponse, la liste des attributs de métadonnées est vide. Cela signifie que les attributs requis par la conception sont suffisants pour évaluer la stratégie des 3 diffusions affectée à cette application. Voir aussi la [documentation sur les champs de métadonnées standard](/help/concurrency-monitoring/standard-metadata-attributes.md). Après cet appel, nous pouvons continuer et créer une session sur la ressource Sessions REST .


#### Initialisation de la session {#session-initial}

L’appel d’initialisation de session est effectué par une application après avoir obtenu toutes les informations nécessaires pour l’exécuter.

![](assets/session-init.png)

Il n’est pas nécessaire de fournir un code de terminaison au premier appel, car nous n’avons pas d’autres flux actifs. Et aucun attribut de métadonnées, car aucun n’a été renvoyé à partir de l’appel Récupération des métadonnées .

Les paramètres **subject** et **idp** sont obligatoires et seront spécifiés comme des variables de chemin d’accès URI. Vous pouvez obtenir les paramètres **subject** et **idp** en effectuant un appel pour les champs de métadonnées **mvpd** et **amontUserID** depuis l’authentification Adobe Pass. Voir également la [présentation des API de métadonnées](https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/user-metadat/user-metadata-feature.html?lang=en#). Pour cet exemple, nous allons fournir la valeur &quot;12345&quot; comme objet et &quot;adobe&quot; comme idp.


![](assets/session-init-params-frstapp.png)

Effectuez l’appel d’initialisation de session. Vous obtiendrez la réponse suivante :


![](assets/session-init-result-first-app.png)


Toutes les données dont nous avons besoin sont contenues dans les en-têtes de réponse. L’en-tête **Location** représente l’identifiant de la nouvelle session créée et les en-têtes **Date** et **Expires** représentent les valeurs utilisées pour planifier votre application pour effectuer la prochaine pulsation afin de maintenir la session active.

#### Heartbeat {#heartbeat}

Effectuez un appel de pulsation. Fournissez l’ **ID de session** obtenu dans l’appel d’initialisation de session, ainsi que les paramètres **subject** et **idp** utilisés.

![](assets/heartbeat.png)


Si la session est toujours valide (elle n’a pas expiré ou a été supprimée manuellement), vous obtiendrez un résultat positif :

![](assets/heartbeat-succesfull-result.png)

Comme dans le premier cas, nous utiliserons les en-têtes **Date** et **Expire** pour planifier une autre pulsation pour cette session particulière. Si la session n’est plus valide, cet appel échoue avec un code d’état HTTP 410 GONE.

Vous pouvez utiliser l’option &quot;Keep the stream alive&quot; disponible dans l’interface utilisateur de Swagger afin d’exécuter des pulsations automatiques sur une session spécifique. Cela peut vous aider à tester une règle sans avoir à vous soucier du modèle standard nécessaire pour effectuer des pulsations de session opportunes. Ce bouton est placé à côté du bouton &quot;Essayez-le&quot; dans l’onglet Swagger Heartbeat . Pour définir une pulsation automatique pour toutes les sessions créées, vous devez les programmer chacune d’elles dans une interface utilisateur Swagger distincte ouverte dans un onglet de navigateur web.

![](assets/keep-stream-alive.png)

#### Arrêt de la session {#session-termination}

Le cas d’entreprise de votre entreprise peut nécessiter la surveillance simultanée pour mettre fin à une session spécifique lorsqu’un utilisateur cesse, par exemple, de regarder une vidéo. Pour ce faire, vous pouvez effectuer un appel DELETE sur la ressource Sessions .

![](assets/delete-session.png)

Utilisez les mêmes paramètres pour l’appel que pour la pulsation de session. Les codes d’état HTTP de réponse sont les suivants :

* 202 ACCEPTÉ pour une réponse réussie
* 410 GONE si la session était déjà arrêtée.

#### Obtenir tous les flux d’exécution {#get-all-running-streams}

Ce point de terminaison offre toutes les sessions en cours d’exécution pour un client spécifique sur toutes ses applications. Utilisez les paramètres **subject** et **idp** pour l’appel :

![](assets/get-all-running-streams-parameters.png)

Lorsque vous effectuez l’appel, vous obtenez la réponse suivante :

![](assets/get-all-running-streams-success.png)

Notez l’en-tête **Expires**. C’est l’heure à laquelle la première session doit expirer, sauf si une pulsation est envoyée. OtherStreams a la valeur 0, car aucun autre flux n’est en cours d’exécution pour cet utilisateur sur les applications d’un autre client.
Le champ de métadonnées contient toutes les métadonnées envoyées au démarrage de la session. Nous ne le filtrons pas, vous recevrez tout ce que vous avez envoyé.
S’il n’existe aucune session en cours d’exécution pour un utilisateur spécifique lorsque vous effectuez l’appel, vous obtenez cette réponse :

![](assets/get-all-running-streams-empty.png)

Notez également que dans ce cas, l’en-tête **Expires** n’est pas présent.

#### Rompre la politique {#breaking-policy-app-first}


Afin de simuler le comportement de notre application lorsque la stratégie des 3 diffusions qui lui est affectée est rompue, nous devons effectuer 3 appels pour l’initialisation de la session. Pour que la stratégie prenne effet, les appels doivent être effectués avant l’expiration de l’une des sessions en raison d’un manque de pulsations. Nous verrons que ces appels réussissent tous, mais si nous en faisons un quatrième, il échouera avec l’erreur suivante :

![](assets/breaking-policy-frstapp.png)


Nous obtenons une réponse CONFLICT 409 ainsi qu’un objet de résultat d’évaluation dans la payload. Lisez une description complète du résultat de l’évaluation dans la [spécification de l’API Swagger](http://docs.adobeptime.io/cm-api-v2/#evaluation-result).

L’application peut utiliser les informations du résultat de l’évaluation pour afficher un certain message à l’utilisateur lors de l’arrêt de la vidéo et pour prendre d’autres mesures si nécessaire. Un cas d’utilisation peut être l’arrêt d’autres flux existants afin d’en démarrer un nouveau. Pour ce faire, utilisez la valeur **terminationCode** présente dans le champ **conflict** pour un attribut de conflit spécifique. La valeur sera fournie sous la forme de l’en-tête HTTP X-Terminate dans l’appel d’une nouvelle initialisation de session.

![](assets/session-init-termination-code.png)

Lorsque vous fournissez un ou plusieurs codes d’arrêt à l’initialisation de la session, l’appel réussit et une nouvelle session est générée. Ensuite, si nous tentons d’effectuer une pulsation avec l’une des sessions qui ont été arrêtées à distance, nous obtiendrons une réponse 410 GONE avec un payload de résultat de l’évaluation qui décrit le fait que la session a été arrêtée à distance, comme dans l’exemple :

![](assets/remote-termination.png)

### Deuxième application {#second-application}

L’autre exemple d’application que nous utiliserons est celui avec l’ID **demo-app-2**. Cette règle a été affectée à celle-ci avec une règle qui limite le nombre de diffusions disponibles pour un canal à un maximum de 2.   Vous devez fournir la variable channel pour évaluer cette stratégie.

#### Récupération des métadonnées {#retrieving-metadata}

Définissez le nouvel ID d’application dans le coin supérieur droit de la page et appelez la ressource Métadonnées . Vous obtiendrez la réponse suivante :

![](assets/non-empty-metadata-secndapp.png)

Cette fois, le corps de la réponse n’est plus une liste vide, comme dans l’exemple de la première application. Désormais, le service de surveillance de la simultanéité indique dans le corps de la réponse que les métadonnées **channel** sont requises lors de l’initialisation de la session pour évaluer la stratégie.

Si vous effectuez un appel sans fournir de valeur pour le paramètre **channel** , vous obtiendrez :

* Code de réponse - DEMANDE 400 MAUVAISE
* Corps de la réponse : payload des résultats d’évaluation qui décrit dans le champ **obligations** ce qui est attendu dans la demande d’initialisation de session pour que l’opération réussisse.

![](assets/metadata-request-secndapp.png)


#### Initialisation de la session {#session-init}

Attribuez une valeur à la clé de métadonnées requise et définissez-la comme paramètre de formulaire dans la demande d’initialisation de session, comme illustré ci-dessous :

![](assets/session-init-params-secndapp.png)

L’appel réussira et une nouvelle session sera générée.


#### Rompre la politique {#breaking-policy-second-app}

Afin de rompre la règle qui figure dans la stratégie affectée à cette application, nous devons effectuer 2 appels avec la même valeur de canal. Comme dans le premier exemple, le second appel doit être effectué pendant que la première session générée est toujours valide.

![](assets/breaking-policy-secnd-app.png)

Si nous utilisons des valeurs différentes pour les métadonnées de canal chaque fois que nous créons une nouvelle session, tous les appels réussiront car le seuil de 2 est inclus individuellement à chaque valeur.

Comme dans le premier exemple, nous pouvons utiliser le code de terminaison afin d’arrêter à distance les flux en conflit ou nous pouvons attendre que l’un des flux arrive à expiration, en supposant qu’aucune pulsation ne sera utilisée sur eux.
