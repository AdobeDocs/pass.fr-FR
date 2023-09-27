---
title: Flux de droits du programmeur
description: Flux de droits du programmeur
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1824'
ht-degree: 0%

---

# Flux de droits du programmeur {#prog-entitlement-flow}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Présentation {#overview}

Ce document décrit le flux de droits de base du point de vue du programmeur.  Pour plus d’informations sur les fonctionnalités et les cas d’utilisation au-delà de l’intégration TVE de base, voir [Cas d’utilisation des programmeurs](/help/authentication/programmer-use-cases.md).

L’authentification Adobe Pass facilite le flux de droits entre les programmeurs et les MVPD en fournissant des interfaces sécurisées et cohérentes pour les deux parties.  Du côté du programmeur, l’authentification Adobe Pass fournit deux types généraux d’interface de droits :

1. AccessEnabler : composant client qui fournit une bibliothèque d’API pour les applications sur les appareils qui peuvent effectuer le rendu des pages web (applications web, applications pour smartphone/tablette, par exemple).
2. API sans client - services Web RESTful pour les appareils qui ne peuvent pas effectuer le rendu des pages web (par exemple, décodeurs, consoles de jeux, télévisions dynamiques). L’exigence de rendu des pages web provient de l’exigence du MVPD selon laquelle les utilisateurs doivent s’authentifier sur le site web du MVPD.

Outre la présentation sans plateforme présentée ici, vous trouverez une présentation spécifique à l’API sans client ici : Documentation sur l’API sans client. AccessEnabler s’exécute en mode natif sur les plateformes prises en charge (AS/JS sur le Web, Objective-C sur iOS et Java sur Android). Les API AccessEnabler sont cohérentes entre les plateformes prises en charge. Toutes les plateformes qui ne prennent pas en charge AccessEnabler utilisent la même API sans client.

Pour les deux types d’interface, l’authentification Adobe Pass permet de arbitrer en toute sécurité le flux de droits entre l’application du programmeur et le MVPD de l’utilisateur :

![](assets/prog-entitlement-flow.png)


*Figure : Écosystème d’authentification Adobe Pass*

>[!IMPORTANT]
>
>Notez dans le diagramme ci-dessus qu’il existe une partie du flux de droits qui ne passe pas par les serveurs d’authentification Adobe Pass : la connexion MVPD. Les utilisateurs doivent se connecter à la page de connexion de leur MVPD. En raison de cette exigence, sur les appareils qui ne peuvent pas effectuer le rendu des pages web, l’application du programmeur doit demander aux utilisateurs de basculer vers un appareil compatible web pour se connecter avec leur MVPD, après quoi ils retournent sur l’appareil d’origine pour le reste du flux de droits.

## Flux de droits {#entitlement-flow}

Quatre sous-flux distincts composent le flux de droits de base :

1. [Flux de démarrage](/help/authentication/entitlement-flow.md#startup)
1. [Flux d’authentification](/help/authentication/entitlement-flow.md#authentication)
1. [Flux d’autorisation](/help/authentication/entitlement-flow.md#authorization)
1. [Déconnexion FLow](/help/authentication/entitlement-flow.md#logout)

Lors de la première visite d’un utilisateur sur le site d’un programmeur, le flux de droits se poursuit dans l’ordre ci-dessus. Cependant, lors de visites ultérieures, selon que les jetons d’authentification et d’autorisation ont expiré ou non, ou selon les stratégies d’affichage, un utilisateur peut uniquement passer par un ou deux des sous-flux.

### Flux de démarrage {#startup}

Établit l’identité du programmeur et de l’appareil, effectue des tâches d’initialisation. Il s’agit d’une condition préalable à tous les appels de droits suivants.

**AccessEnabler**

* **`setRequestor()`** - Établit votre identification avec AccessEnalber, et par extension, les serveurs d’authentification Adobe Pass. Cet appel est un précurseur du reste du flux de droits. Par exemple, dans JavaScript :

  ```JavaScript
    /* Define the requestor ID (Programmer/aggregator ID). */
      var requestorID = "sample_requestor_Id";
      ...
      // Callback indicating that the AccessEnabler swf has initialized
      function swfLoaded() {
          // AccessEnabler is loaded so we can use the API function it provides
          accessEnablerObject.setRequestor(requestorID); 
      ...
      }
  ```

**API sans client**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** - Selon la plateforme, il peut y avoir des tâches préalables à effectuer avant que votre application n’appelle le regcode. Voir **Documentation de l’API sans client** pour plus d’informations. Par exemple, les plateformes Xbox nécessitent que vous effectuiez les étapes de sécurité prescrites avant d’appeler le regcode.

### Flux d’authentification {#authentication}

Une authentification réussie génère un jeton AuthN lié à l’appareil et au demandeur. Une authentification réussie est un prérequis pour l’autorisation.

**AccessEnabler**

* `checkAuthentication()` - Vérifie si un jeton d’authentification mis en cache valide existe dans le cache de jeton local, sans déclencher réellement le flux d’authentification complet. Cela déclenche l’événement `setAuthenticationStatus()` fonction de rappel.
* `getAuthentication()` - Lance le flux d’authentification complet. En cas de réussite, l’authentification Adobe Pass génère un jeton AuthN et le met en cache sur le client. L’utilisateur se connecte au site des MVPD sélectionnés, présenté dans un iFrame, une fenêtre contextuelle ou un affichage Web, selon la plateforme. Cela déclenche displayProviderDialog().

**API sans client**

* `<FQDN>/.../checkauthn` - Version du service Web de `checkAuthentication()` ci-dessus.
* `<FQDN>/.../config` - Renvoie la liste des MVPD pour l’application 2e écran.
* `<FQDN>/.../authenticate` - Lance le flux d’authentification à partir de l’application du deuxième écran, redirigeant les utilisateurs vers le MVPD sélectionné pour la connexion. En cas de succès, l’authentification Adobe Pass génère un jeton AuthN et le stocke sur le serveur, et l’utilisateur revient sur son appareil d’origine pour terminer le flux de droits.

Un jeton AuthN est considéré comme valide si les deux points suivants sont vrais :

* Le jeton AuthN n’a pas expiré
* Le MVPD associé au jeton AuthN figure sur la liste des MVPD autorisés pour l’ID de demandeur actuel.

#### Processus d’authentification initiale d’AccessEnabler générique {#generic-ae-initial-authn-flow}

1. Votre application lance le processus d’authentification avec un appel à `getAuthentication()`, qui recherche un jeton d’authentification mis en cache valide. Cette méthode est facultative. `redirectURL` si vous ne fournissez pas de valeur pour `redirectURL`, après une authentification réussie, l’utilisateur est renvoyé à l’URL à partir de laquelle l’authentification a été initialisée.
1. AccessEnabler détermine l’état d’authentification actuel. Si l’utilisateur est actuellement authentifié, AccessEnabler appelle votre `setAuthenticationStatus()` fonction de rappel, transmission d’un état d’authentification indiquant la réussite.
1. Si l’utilisateur n’est pas authentifié, AccessEnabler poursuit le flux d’authentification en déterminant si la dernière tentative d’authentification de l’utilisateur a réussi avec un MVPD donné. Si un ID MVPD est mis en cache ET que la variable `canAuthenticate` L’indicateur est défini sur true OU un MVPD a été sélectionné à l’aide de `setSelectedProvider()`, l’utilisateur n’est pas invité à saisir la boîte de dialogue de sélection MVPD. Le flux d’authentification continue à utiliser la valeur mise en cache du MVPD (c’est-à-dire le même MVPD qui a été utilisé lors de la dernière authentification réussie). Un appel réseau est effectué au serveur principal et l’utilisateur est redirigé vers la page de connexion MVPD.

1. Si aucun ID MVPD n’est mis en cache ET qu’aucun MVPD n’a été sélectionné à l’aide de `setSelectedProvider()` OU le `canAuthenticate` est défini sur false, l’indicateur `displayProviderDialog()` callback est appelé. Ce rappel demande à votre application de créer l’interface utilisateur qui présente à l’utilisateur une liste de MVPD parmi lesquelles choisir. Un tableau d’objets MVPD est fourni, contenant les informations nécessaires à la création du sélecteur MVPD. Chaque objet MVPD décrit une entité MVPD et contient des informations telles que l’identifiant du MVPD et l’URL où se trouve le logo MVPD.

1. Une fois qu’un MVPD est sélectionné, votre application doit informer AccessEnabler du choix de l’utilisateur. Pour les clients non Flashs, une fois que l’utilisateur a sélectionné le MVPD souhaité, vous informez AccessEnabler de la sélection de l’utilisateur via un appel au `setSelectedProvider()` . Les clients Flash distribuent plutôt un partage `MVPDEvent` de type &quot;`mvpdSelection`&quot;, transmission du fournisseur sélectionné.

1. Lorsque AccessEnabler est informé de la sélection du MVPD de l’utilisateur, un appel réseau est effectué au serveur principal et l’utilisateur est redirigé vers la page de connexion MVPD.

1. Dans le workflow d’authentification, AccessEnabler communique avec l’authentification Adobe Pass et le MVPD sélectionné pour solliciter les informations d’identification de l’utilisateur (identifiant utilisateur et mot de passe) et vérifier son identité. Bien que certains MVPD redirigent vers leur propre site pour la connexion, d’autres vous demandent d’afficher leur page de connexion dans un iFrame. Votre page doit inclure le rappel qui crée un iFrame, au cas où le client choisirait l’un de ces MVPD.<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->.

1. Une fois l’utilisateur connecté, AccessEnabler récupère le jeton d’authentification et informe votre application que le flux d’authentification est terminé. AccessEnabler appelle la fonction `setAuthenticationStatus()` rappel avec un code d’état de 1, indiquant la réussite. En cas d’erreur lors de l’exécution de ces étapes, la variable `setAuthenticationStatus()` Le rappel est déclenché avec un code d’état de 0, indiquant l’échec de l’authentification, ainsi qu’un code d’erreur correspondant.

>[!IMPORTANT]
>Comcast est le seul MVPD pour le moment qui ne fournit pas d’URL statique pour le logo. Les programmeurs doivent extraire les derniers logos à jour à partir de [Portail des développeurs XFINITY](https://developers.xfinity.com/products/tv-everywhere).
>

### Flux d’autorisation {#authorization}

L’autorisation est un prérequis pour l’affichage de contenu protégé. Une autorisation réussie génère un jeton AuthZ, ainsi qu’un jeton multimédia de courte durée fourni à l’application du programmeur à des fins de sécurité. Notez que, pour prendre en charge le workflow d’autorisation, vous devez avoir préalablement effectué la configuration du demandeur nécessaire et avoir intégré la [Vérificateur de jeton multimédia](/help/authentication/media-token-verifier-int.md). Une fois ces tâches terminées, vous pouvez lancer l’autorisation.

Votre application lance une autorisation lorsqu’un utilisateur demande l’accès à une ressource protégée. Vous transmettez un ID de ressource spécifiant la ressource demandée (par exemple, un canal, un épisode, etc.). Votre application commence par rechercher un jeton d’authentification stocké. Si l’un d’eux est introuvable, vous lancez le processus d’authentification.

**AccessEnabler**

* `checkAuthorization()` - Vérifie l’autorisation sans initialiser le flux d’autorisation complet. Il est souvent utilisé pour mettre à jour les informations d’état affichées dans l’interface utilisateur de l’application de programmation.

* `getAuthorization()` - Lance le flux d’autorisation complet.

Vous fournissez les fonctions de rappel suivantes pour gérer les résultats de l’appel d’autorisation :

* `setToken()` - Si l’authentification a réussi précédemment et que l’autorisation réussit, AccessEnabler appelle votre `setToken()` fonction de rappel, transmission du jeton multimédia de courte durée, indiquant une conclusion réussie du flux de droits d’authentification Adobe Pass. (Avant de permettre à l’utilisateur d’afficher du contenu protégé, l’application du programmeur vérifie la validité du jeton multimédia à l’aide du vérificateur de jeton multimédia.

* `tokenRequestFailed()` - Si l’utilisateur n’est pas autorisé pour la ressource demandée (ou si la requête échoue pour une autre raison), AccessEnabler appelle cette fonction de rappel (ainsi que vos propres fonctions de reporting d’erreur), en transmettant des détails sur l’échec.

**API sans client**

* `\<FQDN\>/.../authorize` - Lance le flux d’autorisation complet.

#### Workflow d’autorisation d’AccessEnabler générique {#generic-ae-authr-wf}

1. Fournissez une fonction de rappel qui enregistre votre GUID de programmeur assigné avec l’Access Enabler, en utilisant `setReqestor()`. Cette fonction de rappel est appelée lorsque AccessEnabler a été téléchargé avec succès.

1. Appeler `getAuthorization()` lorsqu’un utilisateur demande l’accès à une ressource protégée. Utilisation `getAuthorization()`, transmettez un ID de ressource spécifiant la ressource demandée (par exemple , un canal, un épisode, etc.). AccessEnabler recherche un jeton d’authentification mis en cache à transmettre avec la demande d’autorisation. S’il est introuvable, le flux d’authentification est lancé.
1. Fournissez des fonctions de rappel pour gérer les résultats de l’autorisation :

   * `setToken()` - Si l’autorisation réussit ou si l’utilisateur a déjà été autorisé, Access Enabler poursuit le processus d’autorisation en appelant votre `setToken()` fonction de rappel, transmission du jeton d’autorisation de courte durée.

   * `tokenRequestFailed()` - Si l&#39;utilisateur n&#39;est pas autorisé pour la ressource demandée (ou si la requête échoue pour une autre raison), AccessEnabler appelle toutes les fonctions de reporting d&#39;erreur que vous avez enregistrées, plus la variable `tokenRequestFailed()` rappel, transmission des détails sur l’échec.

### Flux de connexion {#logout}

Efface les jetons et autres données associés au flux de droits de l’utilisateur actuel.

**AccessEnabler**

* `logout()`

**API sans client**

* `\<FQDN\>/.../logout`

## Comprendre le comportement d’AccessEnabler {#ae-behavior}

Tous les appels de l’API AccessEnabler sont asynchrones (à une exception près, indiqués dans les références de l’API). Vous pouvez appeler une API un nombre arbitraire de fois, mais il n’y a aucune garantie que les actions déclenchées par les appels seront terminées dans le même ordre que celui où les appels ont été effectués. (Une exception à cette règle est l’exécution du Flash Player en cours ; si vous n’êtes pas multi-thread, les appels seront assurés. *do* terminez dans l’ordre dans lequel ils sont appelés.)

Afin de faire la distinction entre les réponses et de pouvoir associer des réponses à des appels, tous les rappels reviennent sur leurs paramètres d’entrée. Cela inclut `setToken()` et`tokenRequestFailed()`, qui sont déclenchées ultimement par `checkAuthorization()`. (Pour `checkAuthorization()` callbacks, la ressource utilisée est renvoyée.) En tirant parti de cette fonctionnalité, vous pouvez distinguer la réponse qui correspond à l’appel . Pour utiliser cette fonctionnalité, vous pouvez coder quelque chose comme ceci :

```JavaScript
    for each (resource in ["TNT", "CNN", "TBS", "AdultSwim"] ) {
         ae.checkAuthorization(resource);
    }
    
    // Success callback
    function setToken(resource, token) {
         // Use "resource" to figure 
         // out which checkAuthorization
         // call triggered this response
    }
    
    // Old error callback
    function tokenRequestFailed(resource, error, details) {
         // use "resource" to figure
         // out in response to which
         // checkAuthorization call
         // this was triggered
    }
    
    // Error callback using new error api
    ae.bind("errorEvent',"errorHandler");
    
    function errorHandler(error) {
         if(error.resource) {        
              // Use error.resource to figure
              // which checkAuthorization call
              // triggered this response
         }
    }
```

### FAQ sur le comportement d’AEM {#ae-beh-faq}

**Question. Que se passe-t-il si je lance un deuxième appel AccessEnabler avant que le premier appel ne se termine ?**

Le premier appel continue de s’exécuter pendant l’exécution du second appel (communications asynchrones).

**Question. Y a-t-il un nombre maximal d’appels simultanés qu’AccessEnabler peut prendre en charge ?**

Aucune limite n’est explicitement définie dans le code AccessEnabler. Par conséquent, vous êtes limité uniquement par vos ressources système disponibles, ainsi que par la capacité du MVPD.

<!--

>[!MORELIKETHIS]
>
>*   [Programmer use cases](/help/authentication/programmer-use-cases.md)
>*   Error Reporting
>**Platform Cookbooks:**
>*   Clientless integration cookbook
>*   iOS Integration Cookbook
>*   Android Integration Cookbook
>*   JavaScript Integration Cookbook
>*   ActionScript Integration Cookbook
>*   Windows 8 Integration Cookbook
>*   AIR Native Extension Overview
-->
