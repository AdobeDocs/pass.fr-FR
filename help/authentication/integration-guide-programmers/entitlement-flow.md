---
title: Flux de droits du programmeur
description: Flux de droits du programmeur
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '1823'
ht-degree: 0%

---

# Flux de droits du programmeur {#prog-entitlement-flow}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

Ce document décrit le flux des droits de base du point de vue du programmeur.  Pour plus d’informations sur les fonctionnalités et les cas d’utilisation au-delà de l’intégration de base de TVE abordée ici, voir [Cas d’utilisation du programmeur](/help/authentication/integration-guide-programmers/programmer-use-cases.md).

L’authentification Adobe Pass assure la médiation du flux de droits entre les programmeurs et les MVPD en fournissant des interfaces sécurisées et cohérentes aux deux parties.  Du côté du programmeur, l’authentification Adobe Pass fournit deux types généraux d’interface de droits :

1. AccessEnabler : composant client qui fournit une bibliothèque d’API pour des applications sur des appareils pouvant effectuer le rendu de pages web (par exemple, des applications web, pour smartphones ou tablettes).
2. API sans client : services web RESTful pour les appareils qui ne peuvent pas effectuer le rendu des pages web (par exemple, les décodeurs, les consoles de jeux, les télévisions intelligentes). L’exigence relative au rendu des pages web provient de l’exigence de MVPD selon laquelle les utilisateurs doivent s’authentifier sur le site web de MVPD.

Outre la présentation neutre sur la plateforme présentée ici, vous trouverez ici une présentation spécifique aux API sans client : Documentation sur les API sans client . AccessEnabler s’exécute en mode natif sur les plateformes prises en charge (AS/JS sur le Web, Objective-C sur iOS et Java sur Android). Les API AccessEnabler sont cohérentes entre les plateformes prises en charge. Toutes les plateformes qui ne prennent pas en charge AccessEnabler utilisent la même API Clientless.

Pour les deux types d’interface, l’authentification Adobe Pass assure la médiation sécurisée du flux de droits entre l’application du programmeur et le MVPD de l’utilisateur :

![](../assets/prog-entitlement-flow.png)


*Image : écosystème d’authentification Adobe Pass*

>[!IMPORTANT]
>
>Notez dans le diagramme ci-dessus qu’une partie du flux de droits ne passe pas par les serveurs d’authentification d’Adobe Pass : la connexion à MVPD. Les utilisateurs doivent se connecter à leur page de connexion MVPD. En raison de cette exigence, sur les appareils qui ne peuvent pas générer de pages web, l’application du programmeur doit demander aux utilisateurs de passer à un appareil compatible web pour se connecter avec leur MVPD, après quoi ils reviennent à l’appareil d’origine pour le reste du flux des droits.

## Flux de droits {#entitlement-flow}

Quatre sous-flux distincts constituent le droit de base
flux :

1. [Flux de démarrage](/help/authentication/integration-guide-programmers/entitlement-flow.md#startup)
1. [Flux d’authentification](/help/authentication/integration-guide-programmers/entitlement-flow.md#authentication)
1. [Flux d’autorisation](/help/authentication/integration-guide-programmers/entitlement-flow.md#authorization)
1. [Flux de déconnexion](/help/authentication/integration-guide-programmers/entitlement-flow.md#logout)

Lors de la première visite d’un utilisateur ou d’une utilisatrice sur le site d’un programmeur, le flux des droits se poursuit dans l’ordre ci-dessus. Cependant, lors de visites ultérieures, selon que les jetons d’authentification et d’autorisation ont expiré ou non, ou selon les politiques d’affichage, un utilisateur peut uniquement passer par un ou deux des sous-flux.

### Flux de démarrage {#startup}

Etablit l’identité du programmeur et de l’appareil, effectue les tâches d’initialisation. Il s’agit d’un prérequis pour tous les appels de droits suivants.

**AccessEnabler**

* **`setRequestor()`** - Établit votre identité avec l’activateur d’accès et, par extension, les serveurs d’authentification Adobe Pass. Cet appel est un précurseur du reste du flux de droits. Par exemple, dans JavaScript :

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

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** - Selon la plateforme, il peut y avoir des tâches préalables à terminer avant que vos appels d&#39;application ne recodent. Pour plus d’informations, consultez la **Documentation de l’API sans client**. Par exemple, les plates-formes Xbox exigent que vous suiviez les étapes de sécurité prescrites avant d&#39;appeler regcode.

### Flux d’authentification {#authentication}

Une authentification réussie génère un jeton AuthN lié à l’appareil et au demandeur. Une authentification réussie est une condition préalable à l’autorisation.

**AccessEnabler**

* `checkAuthentication()` - Vérifie si un jeton d’authentification mis en cache valide existe dans le cache de jeton local, sans véritablement déclencher le flux d’authentification complet. Cela déclenche la fonction de rappel `setAuthenticationStatus()`.
* `getAuthentication()` - Lance le flux d’authentification complet. En cas de réussite, l’authentification Adobe Pass génère un jeton AuthN et le met en cache sur le client. L’utilisateur se connecte sur le site MVPD sélectionné, présenté dans un iFrame, une fenêtre contextuelle ou une vue web, selon la plateforme. Cela déclenche la méthode displayProviderDialog().

**API sans client**

* `<FQDN>/.../checkauthn` - Version du service web de `checkAuthentication()` ci-dessus.
* `<FQDN>/.../config` - Renvoie la liste des fichiers MVPD à l’application du deuxième écran.
* `<FQDN>/.../authenticate` - Lance le flux d’authentification à partir de l’application du deuxième écran, en redirigeant les utilisateurs vers le MVPD sélectionné pour la connexion. En cas de réussite, l’authentification Adobe Pass génère un jeton AuthN et le stocke sur le serveur, puis l’utilisateur revient à son appareil d’origine pour terminer le flux des droits.

Un jeton AuthN est considéré comme valide si les deux points suivants sont vrais :

* Le jeton AuthN n’a pas expiré
* Le MVPD associé au jeton AuthN figure dans la liste des MVPD autorisés pour l’ID de demandeur actuel

#### Workflow d’authentification initiale AccessEnabler générique {#generic-ae-initial-authn-flow}

1. Votre application lance le workflow d’authentification avec un appel à `getAuthentication()`, qui vérifie un jeton d’authentification en cache valide. Cette méthode comporte un paramètre de `redirectURL` facultatif. Si vous ne fournissez pas de valeur pour `redirectURL`, après une authentification réussie, l’utilisateur est renvoyé à l’URL à partir de laquelle l’authentification a été initialisée.
1. AccessEnabler détermine le statut d&#39;authentification actuel. Si l’utilisateur est actuellement authentifié, AccessEnabler appelle votre fonction de rappel `setAuthenticationStatus()`, transmettant un statut d’authentification indiquant la réussite de l’opération.
1. Si l’utilisateur n’est pas authentifié, AccessEnabler poursuit le flux d’authentification en déterminant si la dernière tentative d’authentification de l’utilisateur a réussi avec un MVPD donné. Si un ID MVPD est mis en cache ET que l’indicateur `canAuthenticate` est true OU si un MVPD a été sélectionné à l’aide de `setSelectedProvider()`, l’utilisateur n’est pas invité à ouvrir la boîte de dialogue de sélection de MVPD. Le flux d’authentification continue à utiliser la valeur mise en cache du MVPD (c’est-à-dire le même MVPD que celui utilisé lors de la dernière authentification réussie). Un appel réseau est effectué au serveur principal et l’utilisateur est redirigé vers la page de connexion de MVPD.

1. Si aucun ID MVPD n’est mis en cache ET qu’aucun MVPD n’a été sélectionné à l’aide de `setSelectedProvider()` OU que l’indicateur `canAuthenticate` est défini sur false, le rappel `displayProviderDialog()` est appelé. Ce rappel demande à votre application de créer l’interface utilisateur qui présente à l’utilisateur une liste de fichiers MVPD parmi lesquels choisir. Un tableau d’objets MVPD contenant les informations nécessaires à la création du sélecteur MVPD est fourni. Chaque objet MVPD décrit une entité MVPD et contient des informations telles que l’identifiant du MVPD et l’URL où se trouve le logo MVPD.

1. Une fois qu’un MVPD est sélectionné, votre application doit informer AccessEnabler du choix de l’utilisateur. Pour les clients hors Flash, une fois que l’utilisateur a sélectionné le MVPD souhaité, vous informez l’AccessEnabler de la sélection de l’utilisateur par un appel à la méthode `setSelectedProvider()`. Les clients de Flash distribuent plutôt un `MVPDEvent` partagé de type « `mvpdSelection` », en transmettant le fournisseur sélectionné.

1. Lorsque AccessEnabler est informé de la sélection MVPD de l’utilisateur, un appel réseau est effectué au serveur principal et l’utilisateur est redirigé vers la page de connexion de MVPD.

1. Dans le workflow d’authentification, AccessEnabler communique avec Adobe Pass Authentication et le MVPD sélectionné pour demander les informations d’identification de l’utilisateur (ID utilisateur et mot de passe) et vérifier son identité. Alors que certains MVPD redirigent vers leur propre site pour la connexion, d’autres exigent que vous affichiez leur page de connexion dans un iFrame. Votre page doit inclure le rappel qui crée un iFrame, au cas où le client choisirait l’un de ces MVPD.<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->.

1. Une fois la connexion effectuée, AccessEnabler récupère le jeton d’authentification et informe votre application que le flux d’authentification est terminé. AccessEnabler appelle le rappel `setAuthenticationStatus()` avec un code d&#39;état de 1, indiquant la réussite. En cas d’erreur lors de l’exécution de ces étapes, le rappel `setAuthenticationStatus()` est déclenché avec un code d’état de 0 indiquant l’échec de l’authentification, ainsi qu’un code d’erreur correspondant.

>[!IMPORTANT]
>Comcast est le seul MVPD pour le moment qui ne fournit pas d&#39;URL statique pour le logo. Les programmeurs doivent extraire les derniers logos à jour du portail [XFINITY Developer&#39;s portal](https://developers.xfinity.com/products/tv-everywhere).
>

### Flux d’autorisation {#authorization}

L’autorisation est une condition préalable à l’affichage de contenu protégé. Une autorisation réussie génère un jeton AuthZ, ainsi qu’un jeton de média de courte durée fourni à l’application du programmeur à des fins de sécurité. Notez que, pour prendre en charge le workflow d’autorisation, vous devez avoir préalablement effectué la configuration du demandeur nécessaire et intégré le [Vérificateur de jeton de média](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier). Une fois ces tâches terminées, vous pouvez lancer l’autorisation.

Votre application lance l’autorisation lorsqu’un utilisateur demande l’accès à une ressource protégée. Vous transmettez un identifiant de ressource spécifiant la ressource demandée (par exemple , un canal, un épisode, etc.). Votre application recherche d’abord un jeton d’authentification stocké. S’il n’en existe pas, vous lancez le processus d’authentification.

**AccessEnabler**

* `checkAuthorization()` - Vérifie l’autorisation sans initier le flux d’autorisation complet. Il est souvent utilisé pour mettre à jour les informations de statut affichées dans l’interface utilisateur de l’application de programmation.

* `getAuthorization()` - Lance le flux d’autorisation complet.

Vous fournissez les fonctions de rappel suivantes pour gérer les résultats de
l’appel d’autorisation :

* `setToken()` - Si l’authentification a réussi précédemment et que l’autorisation réussit, AccessEnabler appelle votre fonction de rappel `setToken()`, en transmettant le jeton de média de courte durée, indiquant une conclusion réussie du flux de droits d’authentification Adobe Pass. (Avant de permettre à l’utilisateur d’afficher du contenu protégé, l’application du programmeur vérifie la validité du jeton multimédia à l’aide du vérificateur de jeton multimédia.

* `tokenRequestFailed()` - Si l&#39;utilisateur n&#39;est pas autorisé pour la ressource demandée (ou si la requête échoue pour toute autre raison), AccessEnabler appelle cette fonction de rappel (ainsi que vos propres fonctions de reporting d&#39;erreurs), en transmettant les détails de l&#39;échec.

**API sans client**

* `\<FQDN\>/.../authorize` - Lance le flux d’autorisation complet.

#### Workflow d’autorisation AccessEnabler générique {#generic-ae-authr-wf}

1. Fournissez une fonction de rappel qui enregistre le GUID de programmeur affecté auprès de Access Enabler, à l&#39;aide de `setReqestor()`. Cette fonction de rappel est appelée lorsque l’AccessEnabler a été
téléchargement réussi.

1. Appelez `getAuthorization()` lorsqu’un utilisateur ou une utilisatrice demande l’accès à une ressource protégée. En utilisant `getAuthorization()`, transmettez un identifiant de ressource spécifiant la ressource demandée (par exemple, un canal, un épisode, etc.). AccessEnabler recherche un jeton d’authentification mis en cache à transmettre avec la demande d’autorisation. S’il n’en existe pas, il lance le flux d’authentification.
1. Fournissez des fonctions de rappel pour gérer les résultats de l’autorisation :

   * `setToken()` - Si l’autorisation réussit ou si l’utilisateur a déjà été autorisé, Access Enabler poursuit le processus d’autorisation en appelant votre fonction de rappel `setToken()` et en transmettant le jeton d’autorisation de courte durée.

   * `tokenRequestFailed()` - Si l&#39;utilisateur n&#39;est pas autorisé pour la ressource demandée (ou si la requête échoue pour toute autre raison), AccessEnabler appelle toutes les fonctions de rapport d&#39;erreurs que vous avez enregistrées, ainsi que le rappel `tokenRequestFailed()`, en transmettant les détails de l&#39;échec.

### Flux de déconnexion {#logout}

Efface les jetons et les autres données associées au flux de droits de l’utilisateur actuel.

**AccessEnabler**

* `logout()`

**API sans client**

* `\<FQDN\>/.../logout`

## Comprendre le comportement d’AccessEnabler {#ae-behavior}

Tous les appels API AccessEnabler sont asynchrones (à une exception près, notée dans les références d&#39;API). Vous pouvez appeler une API un nombre arbitraire de fois, mais il n’existe aucune garantie forte que les actions déclenchées
par les appels seront effectués dans le même ordre que celui des appels. (Sauf dans le cas de l’exécution du Flash Player actuel ; qui n’est pas multithread, il garantit que les appels *do* se terminent dans l’ordre
ils sont appelés .)

Afin de faire la distinction entre les réponses et de pouvoir associer les réponses aux appels, tous les rappels font écho à leurs paramètres d’entrée. Cela inclut les `setToken()` et `tokenRequestFailed()`, qui sont finalement déclenchés par `checkAuthorization()`. (Pour les rappels `checkAuthorization()`, la ressource utilisée est renvoyée.) Grâce à cette fonctionnalité, vous pouvez distinguer la réponse qui correspond à chaque appel. Pour utiliser cette fonctionnalité, vous pouvez coder quelque chose comme suit :

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

**Question. Que se passe-t-il si j&#39;effectue un deuxième appel AccessEnabler avant la fin du premier appel ?**

Le premier appel continue de s’exécuter tandis que le second s’exécute (communications asynchrones).

**Question. Existe-t-il un nombre maximal d’appels simultanés qu’AccessEnabler peut prendre en charge ?**

Aucune limite n’est définie explicitement dans le code AccessEnabler. Vous n’êtes donc limité que par les ressources système disponibles, ainsi que par la capacité de MVPD.

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
