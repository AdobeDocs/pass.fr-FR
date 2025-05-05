---
title: Manuel d’iOS/tvOS
description: Manuel d’iOS/tvOS
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '2424'
ht-degree: 0%

---

# Manuel SDK d’iOS/tvOS (hérité) {#iostvos-sdk-cookbook}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Introduction {#intro}

Ce document décrit les workflows de droits qu’une application de niveau supérieur d’un programmeur peut implémenter via les API exposées par la bibliothèque AccessEnabler d’iOS/tvOS.

La solution Droits d’authentification Adobe Pass pour iOS/tvOS est divisée en deux domaines :

* Le domaine de l’interface utilisateur : il s’agit de la couche d’application de niveau supérieur qui implémente l’interface utilisateur et utilise les services fournis par la bibliothèque AccessEnabler pour fournir l’accès au contenu limité.

* Le domaine AccessEnabler - c’est là que les workflows de droits sont implémentés sous la forme de :

   * Appels réseau effectués aux serveurs principaux d’Adobe
   * Règles de logique commerciale liées aux workflows d’authentification et d’autorisation
   * Gestion de diverses ressources et traitement de l’état des workflows (tel que le cache de jetons)

L’objectif du domaine AccessEnabler est de masquer toutes les complexités des workflows de droits et de fournir à l’application de couche supérieure (par le biais de la bibliothèque AccessEnabler) un ensemble de primitives de droits simples avec lesquelles vous implémentez les workflows de droits :

1. Définition de l’identité du demandeur
1. Vérifier et obtenir une authentification auprès d’un fournisseur d’identité spécifique
1. Vérifier et obtenir l’autorisation pour une ressource particulière
1. Déconnexion
1. Flux SSO d’Apple par proxy du framework Apple VSA

L’activité réseau d’AccessEnabler a lieu dans son propre thread, de sorte que le thread de l’interface utilisateur n’est jamais bloqué. Par conséquent, le canal de communication bidirectionnel entre les deux domaines d’application doit suivre un modèle entièrement asynchrone :

* La couche d’application de l’interface utilisateur envoie des messages au domaine AccessEnabler via les appels d’API exposés par la bibliothèque AccessEnabler.
* AccessEnabler répond à la couche d’interface utilisateur par le biais des méthodes de rappel incluses dans le protocole AccessEnabler que la couche d’interface utilisateur enregistre auprès de la bibliothèque AccessEnabler.

## Configuration du service d’ID Experience Cloud (identifiant visiteur) {#visitorIDSetup}

La configuration de la valeur [ID Experience Cloud ](https://experienceleague.adobe.com/docs/id-service/using/home.html) est importante du point de vue [!DNL Analytics]. Une fois qu’une valeur de `visitorID` est définie, le SDK envoie ces informations avec chaque appel réseau et le serveur d’authentification [!DNL Adobe Pass] collecte ces informations. Vous pouvez mettre en corrélation les analyses du service d’authentification Adobe Pass avec tout autre rapport d’analyse que vous pouvez avoir à partir d’autres applications ou sites web. Vous trouverez des informations sur la configuration de l’identifiant visiteur [ici](#setOptions).

## Flux de droits {#entitlement}

A. [Conditions préalables](#prereqs) </br>
B. [Flux de démarrage](#startup_flow) </br>
C. [ Flux d’authentification avec le SSO d’Apple ](#authn_flow_wo_applesso) </br>
D. [ Flux d’authentification avec le SSO Apple sur iOS ](#authn_flow_with_applesso) </br>
E. [Flux d’authentification avec l’authentification unique Apple sur tvOS](#authn_flow_with_applesso_tvOS) </br>
F. [ Flux d’autorisation ](#authz_flow) </br>
G. [Afficher le flux des médias](#media_flow) </br>
H. [ Flux de déconnexion sans SSO Apple ](#logout_flow_wo_AppleSSO) </br>
I. [Flux de déconnexion avec l’authentification unique Apple](#logout_flow_with_AppleSSO) </br>


### A. Prérequis {#prereqs}

1. Créez vos fonctions de rappel :
   * `setRequestorComplete()` </br>
   * Déclenché par [setRequestor()](#$setReq), renvoie un succès ou un échec. </br>
   * La réussite indique que vous pouvez poursuivre les appels de droits.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Déclenché par [`getAuthentication()`](#$getAuthN) uniquement si l’utilisateur n’a pas sélectionné de fournisseur (MVPD) et n’est pas encore authentifié. </br>
      * Le paramètre `mvpds` est un tableau de fournisseurs disponibles pour l’utilisateur.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Déclenché par `checkAuthentication()` à chaque fois. </br>
      * Déclenché par [`getAuthentication()`](#$getAuthN) uniquement si l’utilisateur est déjà authentifié et a sélectionné un fournisseur. </br>
      * Le statut renvoyé est succès ou échec. Le code d’erreur décrit le type de l’échec.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Déclenché par [`getAuthentication()`](#$getAuthN) après la sélection d’un MVPD par l’utilisateur. Le paramètre `url` indique l’emplacement de la page de connexion de MVPD.

   * `sendTrackingData(event, data)` </br>
      * Déclenché par `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * Le paramètre `event` indique quel événement de droit s’est produit ; le paramètre `data` est une liste de valeurs relatives à l’événement.

   * `setToken(token, resource)`

      * Déclenché par [checkAuthorization()](#checkAuthZ) et [getAuthorization()](#$getAuthZ) après une autorisation réussie d’affichage d’une ressource.
      * Le paramètre `token` est le jeton de média de courte durée ; le paramètre `resource` est le contenu que l’utilisateur est autorisé à afficher.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Déclenché par [checkAuthorization()](#checkAuthZ) et [getAuthorization()](#$getAuthZ) après une autorisation infructueuse.
      * Le paramètre `resource` correspond au contenu que l’utilisateur tentait d’afficher ; le paramètre `code` correspond au code d’erreur indiquant le type d’échec ; le paramètre `description` décrit l’erreur associée au code d’erreur.

   * `selectedProvider(mvpd)` </br>
      * Déclenché par [`getSelectedProvider()`](#getSelProv).
      * Le paramètre `mvpd` fournit des informations sur le fournisseur sélectionné par l’utilisateur ou l’utilisatrice.

   * `setMetadataStatus(metadata, key, arguments)`
      * Déclenché par `getMetadata().`
      * Le paramètre `metadata` fournit les données spécifiques que vous avez demandées ; le paramètre `key` est la clé utilisée dans la requête [getMetadata()](#getMeta) et le paramètre `arguments` est le même dictionnaire que celui transmis à [getMetadata()](#getMeta).

   * [`preauthorizedResources(authorizedResources)`](#preauthResources)

      * Déclenché par [`checkPreauthorizedResources()`](#checkPreauth).

      * Le paramètre `authorizedResources` présente les ressources que l’utilisateur
est autorisé à afficher.

   * [`presentTvProviderDialog(viewController)`](#presentTvDialog)

      * Déclenché par [getAuthentication()](#getAuthN) lorsque le demandeur actuel prend en charge au moins un MVPD prenant en charge la connexion unique.
      * Le paramètre viewController correspond à la boîte de dialogue SSO d’Apple et doit être présenté sur le contrôleur de vue principal.

   * [`dismissTvProviderDialog(viewController)`](#dismissTvDialog)

      * Déclenché par une action de l’utilisateur (en sélectionnant « Annuler » ou « Autres fournisseurs de télévision » dans la boîte de dialogue SSO d’Apple).
      * Le paramètre viewController correspond à la boîte de dialogue SSO d’Apple et doit être ignoré du contrôleur de vue principal.

![](../../../../assets/iOS-flows.png)

### B. Flux de démarrage {#startup_flow}

1. Démarrez l’application de niveau supérieur.</br>
1. Lancement du </br> d’authentification Adobe Pass

   a. Appelez [`init`](#$init) pour créer une instance unique d’Adobe Pass Authentication AccessEnabler.
   * **Dépendance :** Bibliothèque iOS/tvOS native d’authentification Adobe Pass (AccessEnabler)

   b. Appelez `setRequestor()` pour établir l’identité du programmeur ; transmettez le `requestorID` du programmeur et (éventuellement) un tableau de points d’entrée d’authentification Adobe Pass. Pour tvOS, vous devrez également fournir la clé publique et le secret. Pour plus d’informations[&#128279;](#create_dev) consultez la documentation relative à Clientless .

   * **Dépendance :** ID de demandeur d’authentification Adobe Pass valide (utilisez votre compte d’authentification Adobe Pass)
Manager pour organiser cela).

   * **Triggers:**

     Rappel [setRequestorComplete()](#$setReqComplete).

   >[!NOTE]
   >
   >Aucune demande de droit ne peut être traitée tant que l’identité du demandeur n’est pas entièrement établie. Cela signifie effectivement que pendant que [`setRequestor()`](#$setReq) est toujours en cours d’exécution, toutes les demandes de droits ultérieures. Par exemple, les [`checkAuthentication()`](#checkAuthN) sont bloquées.

   Vous disposez de deux options de mise en œuvre : une fois les informations d’identification du demandeur envoyées au serveur principal, la couche d’application de l’interface utilisateur peut choisir l’une des deux approches suivantes : </br>

   1. Attendez le déclenchement du rappel [`setRequestorComplete()`](#setReqComplete) (partie du délégué AccessEnabler). Cette option offre la plus grande certitude quant à la réalisation de l[`setRequestor()`](#$setReq)opération. Elle est donc recommandée pour la plupart des implémentations.

   1. Continuez sans attendre le déclenchement du rappel [`setRequestorComplete()`](#setReqComplete) et commencez à émettre des demandes de droits. Ces appels (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sont mis en file d&#39;attente par la bibliothèque AccessEnabler, qui effectuera les appels réseau réels après la [`setRequestor()`](#$setReq). Cette option peut parfois être interrompue si, par exemple, la connexion réseau est instable.

1. Appelez `checkAuthentication()` pour vérifier une authentification existante sans lancer le flux d’authentification complet.  Si cet appel réussit, vous pouvez passer directement au flux d’autorisation. Dans le cas contraire, passez au flux d’authentification.

   * **Dependency:** un appel réussi à [setRequestor()](#$setReq) (cette dépendance s’applique également à tous les appels suivants).

   * Rappel **Triggers:** [setAuthenticationStatus()](#$setAuthNStatus).


### C. Flux d’authentification sans SSO Apple {#authn_flow_wo_applesso}

1. Appelez [`getAuthentication()`](#$getAuthN) pour lancer le flux d’authentification ou pour obtenir la confirmation que l’utilisateur est déjà
authentifié.

   **Triggers:**

   * Le rappel [setAuthenticationStatus()](#$setAuthNStatus) si l’utilisateur est déjà authentifié. Dans ce cas, passez directement au [flux d’autorisation](#authz_flow).

   * Le rappel [displayProviderDialog()](#$dispProvDialog) si l’utilisateur n’est pas encore authentifié.

1. Présenter à l&#39;utilisateur la liste des fournisseurs envoyés à
   [`displayProviderDialog()`](#dispProvDialog).

1. Une fois que l’utilisateur a sélectionné un fournisseur, récupérez l’URL du MVPD de l’utilisateur à partir du rappel `navigateToUrl:` ou `navigateToUrl:useSVC:`, ouvrez un contrôleur `UIWebView/WKWebView` ou `SFSafariViewController` et dirigez-le vers l’URL.

1. Par le biais du `UIWebView/WKWebView` ou de l’`SFSafariViewController` instancié à l’étape précédente, l’utilisateur accède à la page de connexion de MVPD et saisit ses informations d’identification. Plusieurs opérations de redirection ont lieu au sein du contrôleur.</br>

>[!NOTE]
>
>À ce stade, l’utilisateur a la possibilité d’annuler le flux d’authentification. Si cela se produit, votre calque d’interface utilisateur est chargé d’informer l’AccessEnabler de cet événement en appelant [setSelectedProvider()](#setSelProv) avec `null` comme paramètre. Cela permet à AccessEnabler de nettoyer son état interne et de réinitialiser le flux d’authentification.

1. Une fois la connexion établie par l’utilisateur, votre couche d’application détecte le chargement d’une URL personnalisée spécifique. Notez que cette URL personnalisée spécifique n&#39;est pas valide et qu&#39;elle n&#39;est pas destinée à être chargée par le contrôleur. Il doit être interprété par votre application uniquement comme un signal indiquant que le flux d’authentification est terminé et qu’il est sûr de fermer le contrôleur `UIWebView/WKWebView` ou `SFSafariViewController`. Si l’utilisation d’un `SFSafariViewController` contrôleur est requise, l’URL personnalisée spécifique est définie par le **`application's custom scheme`** (par exemple, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Dans le cas contraire, cette URL personnalisée spécifique est définie par la constante **`ADOBEPASS_REDIRECT_URL`** (c’est-à-dire `adobepass://ios.app`).

1. Fermez le contrôleur UIWebView/WKWebView ou SFSafariViewController et appelez la méthode API `handleExternalURL:url` d&#39;AccessEnabler, qui demande à AccessEnabler de récupérer le jeton d&#39;authentification du serveur principal.

1. (Facultatif) Appelez [`checkPreauthorizedResources(resources)`](#$checkPreauth) pour vérifier quelles ressources l’utilisateur est autorisé à afficher. Le paramètre `resources` est un tableau de ressources protégées associé au jeton d’authentification de l’utilisateur. Les informations d’autorisation obtenues à partir du MVPD de l’utilisateur peuvent être utilisées pour décorer l’interface utilisateur (par exemple, des symboles verrouillés/déverrouillés en regard du contenu protégé).

   * **Triggers:** rappel [`preauthorizedResources()`](#preauthResources)
   * **Point d’exécution :** après le flux d’authentification terminé

1. Si l’authentification a réussi, passez au flux d’autorisation.

### D. Flux d’authentification avec l’authentification unique Apple sur iOS {#authn_flow_with_applesso}

1. Appelez [`getAuthentication()`](#$getAuthN) pour lancer le flux d’authentification ou pour obtenir la confirmation que l’utilisateur est déjà authentifié.
   **Triggers:**

   * Le rappel [presentTvProviderDialog()](#presentTvDialog) si l’utilisateur n’est pas authentifié et que le demandeur actuel dispose au moins d’un MVPD prenant en charge la connexion unique. Si aucun MVPD ne prend en charge la connexion unique, le flux d’authentification classique est utilisé.

1. Une fois que l’utilisateur a sélectionné un fournisseur, la bibliothèque AccessEnabler obtient un jeton d’authentification contenant les informations fournies par le framework VSA d’Apple.

1. Le rappel [setAuthenticationsStatus()](#setAuthNStatus) sera déclenché. À ce stade, l’utilisateur doit être authentifié avec l’authentification unique Apple.

1. [Facultatif] Appelez [`checkPreauthorizedResources(resources)`](#$checkPreauth) pour vérifier quelles ressources l’utilisateur est autorisé à afficher. Le paramètre `resources` est un tableau de ressources protégées associé au jeton d’authentification de l’utilisateur. Les informations d’autorisation obtenues à partir du MVPD de l’utilisateur peuvent être utilisées pour décorer l’interface utilisateur (par exemple, des symboles verrouillés/déverrouillés en regard du contenu protégé).

   * **Triggers:** rappel [`preauthorizedResources()`](#preauthResources)
   * **Point d’exécution :** après le flux d’authentification terminé

1. Si l’authentification a réussi, passez au flux d’autorisation.

### E. Flux d’authentification avec le SSO d’Apple sur tvOS {#authn_flow_with_applesso_tvOS}

1. Appelez [`getAuthentication()`](#$getAuthN) pour lancer l’opération .
flux d’authentification ou pour obtenir la confirmation que l’utilisateur est déjà
authentifié.
   **Triggers:**
   * Le rappel [`presentTvProviderDialog()`](#presentTvDialog), si l’utilisateur n’est pas authentifié et que le demandeur actuel dispose au moins d’un MVPD prenant en charge la connexion unique. Si aucun MVPD ne prend en charge la connexion unique, le flux d’authentification classique est utilisé.

1. Une fois que l’utilisateur a sélectionné un fournisseur, le rappel [`status()`](#status_callback_implementation) est appelé. Un code d’enregistrement est fourni et la bibliothèque AccessEnabler commence à interroger le serveur pour une authentification réussie sur le deuxième écran.

1. Si le code d’enregistrement fourni a été utilisé pour s’authentifier correctement sur le deuxième écran, le rappel [`setAuthenticatiosStatus()`](#setAuthNStatus) est déclenché. À ce stade, l’utilisateur doit être authentifié avec l’authentification unique Apple.
1. [Facultatif] Appelez [`checkPreauthorizedResources(resources)`](#$checkPreauth) pour vérifier quelles ressources l’utilisateur est autorisé à afficher. Le paramètre `resources` est un tableau de ressources protégées associé au jeton d’authentification de l’utilisateur. Les informations d’autorisation obtenues à partir du MVPD de l’utilisateur peuvent être utilisées pour décorer l’interface utilisateur (par exemple, des symboles verrouillés/déverrouillés en regard du contenu protégé).

   * **Triggers:** rappel [`preauthorizedResources()`](#preauthResources)

   * **Point d’exécution :** après le flux d’authentification terminé
1. Si l’authentification a réussi, passez au flux d’autorisation.

### F. Flux d’autorisation {#authz_flow}

1. Appelez [getAuthorization()](#$getAuthZ) pour lancer le flux d’autorisation.

   * **Dépendance :** ResourceID valides convenus avec le ou les MVPD.
   * Les identifiants de ressource doivent être identiques à ceux utilisés sur d’autres appareils ou plateformes et seront identiques sur tous les MVPD. Pour plus d’informations sur les identifiants de ressource, voir [Identifiant de ressource](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)

1. Validez l’authentification et l’autorisation.

   * Si l’appel [getAuthorization()](#$getAuthZ) réussit : l’utilisateur possède des jetons AuthN et AuthZ valides (l’utilisateur est authentifié et autorisé à regarder le média demandé).

   * Si [getAuthorization()](#$getAuthZ) échoue : examinez l’exception renvoyée pour déterminer son type (AuthN, AuthZ ou autre) :
      * S’il s’agissait d’une erreur d’authentification (AuthN), redémarrez le flux d’authentification.
      * S’il s’agissait d’une erreur d’autorisation (AuthZ), l’utilisateur n’est pas autorisé à regarder le média demandé et un message d’erreur doit s’afficher à l’intention de l’utilisateur.
      * S’il y a eu un autre type d’erreur (erreur de connexion, erreur réseau, etc.), affichez un message d’erreur approprié à l’intention de l’utilisateur.

1. Validez le jeton de média court.\
   Utilisez la bibliothèque Vérificateur de jeton de média d’authentification Adobe Pass pour vérifier le jeton de média de courte durée renvoyé par l’appel [getAuthorization()](#$getAuthZ) ci-dessus :

   * Si la validation réussit : lisez le média demandé pour l’utilisateur.
   * Si la validation échoue : le jeton AuthZ n’était pas valide, la requête de média doit être refusée et un message d’erreur doit s’afficher à l’intention de l’utilisateur.


1. Retournez à votre flux d’application normal.

### G. Afficher le flux multimédia {#media_flow}

1. L’utilisateur sélectionne le média à afficher.
1. Les médias sont-ils protégés ? Votre application vérifie si le média sélectionné est protégé :

   * Si le média sélectionné est protégé, votre application lance le [flux d’autorisation](#authz_flow) ci-dessus.

   * Si le média sélectionné n’est pas protégé, lisez le média pendant
l’utilisateur .

### H. Flux de déconnexion sans SSO Apple {#logout_flow_wo_AppleSSO}

1. Appelez [`logout()`](#$logout) pour déconnecter l’utilisateur. AccessEnabler efface toutes les valeurs et tous les jetons mis en cache. Après avoir effacé le cache, AccessEnabler effectue un appel au serveur pour nettoyer les sessions côté serveur. Notez que puisque l’appel au serveur peut entraîner une redirection SAML vers l’IdP (cela permet le nettoyage de la session du côté IdP), cet appel doit suivre toutes les redirections. Pour cette raison, cet appel doit être géré à l&#39;intérieur d&#39;un contrôleur UIWebView/WKWebView ou SFSafariViewController.

   a. En suivant le même modèle que le workflow d’authentification, le domaine AccessEnabler effectue une requête à la couche d’application de l’interface utilisateur, via le rappel `navigateToUrl:` ou `navigateToUrl:useSVC:`, pour créer un contrôleur UIWebView/WKWebView ou SFSafariViewController et lui demander de charger l’URL fournie dans le paramètre `url` du rappel. Il s’agit de l’URL du point d’entrée de déconnexion sur le serveur principal.

   b. Votre application doit surveiller l’activité du contrôleur de `UIWebView/WKWebView or SFSafariViewController` et détecter le moment où il charge une URL personnalisée spécifique, car il passe par plusieurs redirections. Notez que cette URL personnalisée spécifique n&#39;est pas valide et qu&#39;elle n&#39;est pas destinée à être chargée par le contrôleur. Votre application doit l&#39;interpréter uniquement comme un signal indiquant que le flux de déconnexion est terminé et qu&#39;il est sûr de fermer le contrôleur `UIWebView/WKWebView` ou `SFSafariViewController`. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer le contrôleur `UIWebView/WKWebView or SFSafariViewController` et appeler la méthode `handleExternalURL:url`API AccessEnabler. Si l’utilisation d’un `SFSafariViewController`contrôleur est requise, l’URL personnalisée spécifique est définie par le **`application's custom scheme`** (par exemple, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), sinon cette URL personnalisée spécifique est définie par la constante **`ADOBEPASS_REDIRECT_URL`** (c’est-à-dire `adobepass://ios.app`).

   >[!NOTE]
   >
   >Le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir de quelque manière que ce soit avec l’UIWebView/WKWebView ou SFSafariViewController. La couche d’application de l’interface utilisateur utilise un UIWebView/WKWebView ou SFSafariViewController pour s’assurer que toutes les redirections sont suivies. Par conséquent, il est possible (et recommandé) de rendre le contrôleur invisible (c&#39;est-à-dire masqué) pendant le processus de déconnexion.


### I. Flux de déconnexion avec l’authentification unique Apple {#logout_flow_with_AppleSSO}

1. Appelez [`logout()`](#$logout) pour déconnecter l’utilisateur.
1. Le rappel [status()](#status_callback_implementation) sera appelé avec l’ID VSA203.
1. L’utilisateur doit également être invité à se connecter à partir des paramètres système. Si vous ne le faites pas, une nouvelle authentification sera effectuée lorsque l’application sera relancée.



<!--
### Related Information {#related}


- [iOS API Reference](#)

- [iOS Technical Overview](#)

- [Generating Digital Certificates](#)

- [Identifying Protected Resources](#)

- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)

- [iOS Authentication error - adobepass.ios.app cannot be found (Tech
  Note)](#)
-->
