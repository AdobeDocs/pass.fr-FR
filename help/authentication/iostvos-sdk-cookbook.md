---
title: Guide pas à pas iOS/tvOS
description: Guide pas à pas iOS/tvOS
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '2402'
ht-degree: 0%

---

# Guide pas à pas du SDK iOS/tvOS {#iostvos-sdk-cookbook}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

Ce document décrit les processus de droits que l’application de niveau supérieur d’un programmeur peut mettre en oeuvre via les API exposées par la bibliothèque iOS/tvOS AccessEnabler.

La solution de droit d’authentification Adobe Pass pour iOS/tvOS est finalement divisée en deux domaines :

* Domaine de l’interface utilisateur : il s’agit de la couche supérieure de l’application qui implémente l’interface utilisateur et qui utilise les services fournis par la bibliothèque AccessEnabler pour fournir l’accès au contenu restreint.

* Domaine AccessEnabler : c&#39;est là que les workflows de droits sont implémentés sous la forme :

   * Appels réseau effectués sur les serveurs principaux d’Adobe
   * Règles de logique métier liées aux workflows d’authentification et d’autorisation
   * Gestion de diverses ressources et traitement de l’état du workflow (comme le cache de jeton)

L’objectif du domaine AccessEnabler est de masquer toutes les complexités des workflows de droits et de fournir à l’application de couche supérieure (via la bibliothèque AccessEnabler) un ensemble de primitives de droits simples avec lesquelles vous implémentez les workflows de droits :

1. Définition de l’identité du demandeur
1. Vérifier et obtenir une authentification auprès d’un fournisseur d’identité particulier
1. Obtention de l’autorisation d’une ressource spécifique
1. Déconnexion
1. Flux d’authentification unique Apple par proxy de la structure Apple VSA

L’activité réseau d’AccessEnabler a lieu dans son propre thread, de sorte que le thread de l’interface utilisateur n’est jamais bloqué. Par conséquent, le canal de communication bidirectionnel entre les deux domaines d’application doit suivre un modèle entièrement asynchrone :

* La couche d’application de l’interface utilisateur envoie des messages au domaine AccessEnabler via les appels d’API exposés par la bibliothèque AccessEnabler.
* AccessEnabler répond à la couche de l’interface utilisateur par le biais des méthodes de rappel incluses dans le protocole AccessEnabler que la couche de l’interface utilisateur enregistre auprès de la bibliothèque AccessEnabler.

## Configuration du service d’identification des Experience Cloud (identifiant visiteur) {#visitorIDSetup}

La configuration de la valeur [ID Experience Cloud](https://experienceleague.adobe.com/docs/id-service/using/home.html) est importante du point de vue [!DNL Analytics]. Une fois qu’une valeur `visitorID` est définie, le SDK envoie ces informations avec chaque appel réseau et le serveur d’authentification [!DNL Adobe Pass] collecte ces informations. Vous pouvez mettre en relation les analyses du service d’authentification Adobe Pass avec tout autre rapport d’analyse que vous pouvez avoir à partir d’autres applications ou sites web. Vous trouverez des informations sur la configuration de visitorID [ici](#setOptions).

## Flux de droits {#entitlement}

A. [Conditions préalables](#prereqs) </br>
B. [Flux de démarrage ](#startup_flow) </br>
C. [Flux d’authentification avec Apple SSO ](#authn_flow_wo_applesso) </br>
D. [Flux d’authentification avec SSO Apple sur iOS](#authn_flow_with_applesso) </br>
E. [Flux d’authentification avec SSO Apple sur tvOS](#authn_flow_with_applesso_tvOS) </br>
F. [Flux d’autorisation](#authz_flow) </br>
G. [Afficher le flux multimédia](#media_flow) </br>
H. [Flux de déconnexion sans SSO Apple](#logout_flow_wo_AppleSSO) </br>
I. [Flux de déconnexion avec Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Conditions préalables {#prereqs}

1. Créez vos fonctions de rappel :
   * `setRequestorComplete()` </br>
   * Déclenché par [setRequestor()](#$setReq), renvoie succès ou échec. </br>
   * La réussite indique que vous pouvez procéder à des appels de droits.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Déclenché par [`getAuthentication()`](#$getAuthN) uniquement si l’utilisateur n’a pas sélectionné de fournisseur (MVPD) et n’est pas encore authentifié. </br>
      * Le paramètre `mvpds` est un tableau de fournisseurs mis à la disposition de l’utilisateur.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Déclenché par `checkAuthentication()` à chaque fois. </br>
      * Déclenché par [`getAuthentication()`](#$getAuthN) uniquement si l’utilisateur est déjà authentifié et a sélectionné un fournisseur. </br>
      * L’état renvoyé est succès ou échec, le code d’erreur décrit le type de l’échec.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Déclenché par [`getAuthentication()`](#$getAuthN) après que l’utilisateur a sélectionné un MVPD. Le paramètre `url` fournit l’emplacement de la page de connexion du MVPD.

   * `sendTrackingData(event, data)` </br>
      * Déclenché par `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * Le paramètre `event` indique quel événement de droit s’est produit ; le paramètre `data` est une liste de valeurs relatives à l’événement.

   * `setToken(token, resource)`

      * Déclenché par [checkAuthorization()](#checkAuthZ) et [getAuthorization()](#$getAuthZ) après une autorisation réussie d’afficher une ressource.
      * Le paramètre `token` est le jeton multimédia de courte durée ; le paramètre `resource` est le contenu que l’utilisateur est autorisé à afficher.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Déclenché par [checkAuthorization()](#checkAuthZ) et [getAuthorization()](#$getAuthZ) après une autorisation manquée.
      * Le paramètre `resource` est le contenu que l’utilisateur tentait de consulter ; le paramètre `code` est le code d’erreur indiquant le type d’échec survenu ; le paramètre `description` décrit l’erreur associée au code d’erreur.

   * `selectedProvider(mvpd)` </br>
      * Déclenché par [`getSelectedProvider()`](#getSelProv).
      * Le paramètre `mvpd` fournit des informations sur le fournisseur sélectionné par l’utilisateur.

   * `setMetadataStatus(metadata, key, arguments)`
      * Déclenché par `getMetadata().`
      * Le paramètre `metadata` fournit les données spécifiques que vous avez demandées ; le paramètre `key` est la clé utilisée dans la requête [getMetadata()](#getMeta) ; et le paramètre `arguments` est le même dictionnaire qui a été transmis à [getMetadata()](#getMeta).

   * [`preauthorizedResources(authorizedResources)`](#preauthResources)

      * Déclenché par [`checkPreauthorizedResources()`](#checkPreauth).

      * Le paramètre `authorizedResources` présente les ressources que l’utilisateur
est autorisé à afficher.

   * [`presentTvProviderDialog(viewController)`](#presentTvDialog)

      * Déclenché par [getAuthentication()](#getAuthN) lorsque le demandeur actuel prend en charge au moins sur MVPD qui prend en charge la SSO.
      * Le paramètre viewController est la boîte de dialogue Apple SSO et doit être présenté sur le contrôleur de vue principal.

   * [`dismissTvProviderDialog(viewController)`](#dismissTvDialog)

      * Déclenché par une action de l’utilisateur (en sélectionnant &quot;Annuler&quot; ou &quot;Autres fournisseurs de télévision&quot; dans la boîte de dialogue SSO Apple).
      * Le paramètre viewController est la boîte de dialogue Apple SSO et doit être ignoré du contrôleur de vue principal.

![](assets/iOS-flows.png)

### B. Flux de démarrage {#startup_flow}

1. Démarrez l&#39;application de niveau supérieur.</br>
1. Lancement de l’authentification Adobe Pass </br>

   a. Appelez [`init`](#$init) pour créer une instance unique de Adobe Pass Authentication AccessEnabler.
   * **Dépendance :** Authentification Adobe Pass natif iOS/bibliothèque tvOS (AccessEnabler)

   b. Appelez `setRequestor()` pour établir l’identité du programmeur ; transmettez dans le `requestorID` du programmeur et (éventuellement) un tableau de points de terminaison d’authentification Adobe Pass. Pour tvOS, vous devrez également fournir la clé publique et le secret. Pour plus d’informations, consultez la [documentation sans client](#create_dev) .

   * **Dépendance :** Demandeur d’authentification Adobe Pass valide (utiliser votre compte d’authentification Adobe Pass)
Gestionnaire pour arranger cela).

   * **Triggers :**
     Rappel [setRequestorComplete()](#$setReqComplete).

   >[!NOTE]
   >
   >Aucune demande de droit ne peut être effectuée tant que l’identité du demandeur n’a pas été entièrement établie. Cela signifie que, pendant que [`setRequestor()`](#$setReq) est toujours en cours d’exécution, toutes les demandes de droits suivantes sont exécutées. Par exemple, [`checkAuthentication()`](#checkAuthN) sont bloqués.

   Vous disposez de deux options d’implémentation : une fois que les informations d’identification du demandeur sont envoyées au serveur principal, la couche d’application de l’interface utilisateur peut choisir l’une des deux approches suivantes : </br>

   1. Attendez le déclenchement du rappel [`setRequestorComplete()`](#setReqComplete) (partie du délégué AccessEnabler). Cette option garantit la plus grande certitude que [`setRequestor()`](#$setReq) a été effectuée. Elle est donc recommandée pour la plupart des mises en oeuvre.

   1. Continuez sans attendre le déclenchement du rappel [`setRequestorComplete()`](#setReqComplete) et commencez à émettre des demandes de droits. Ces appels (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sont placés en file d’attente par la bibliothèque AccessEnabler qui effectuera les appels réseau réels après le [`setRequestor()`](#$setReq). Cette option peut parfois être perturbée si, par exemple, la connexion réseau est instable.

1. Appelez `checkAuthentication()` pour rechercher une authentification existante sans initialiser le flux d’authentification complet.  Si cet appel réussit, vous pouvez passer directement au flux d’autorisation. Si ce n’est pas le cas, passez au flux d’authentification.

   * **Dépendance :** Appel réussi à [setRequestor()](#$setReq) (cette dépendance s’applique également à tous les appels suivants).

   * **Triggers:** [setAuthenticationStatus()](#$setAuthNStatus) callback.


### C. Flux d’authentification sans authentification unique Apple {#authn_flow_wo_applesso}

1. Appelez [`getAuthentication()`](#$getAuthN) pour lancer le flux d’authentification ou pour obtenir la confirmation que l’utilisateur est déjà
authentifié.

   **Triggers :**

   * Rappel [setAuthenticationStatus()](#$setAuthNStatus) si l’utilisateur est déjà authentifié. Dans ce cas, accédez directement au [flux d’autorisation](#authz_flow).

   * Rappel [displayProviderDialog()](#$dispProvDialog) si l’utilisateur n’est pas encore authentifié.

1. Présenter à l’utilisateur la liste des fournisseurs envoyés à
   [`displayProviderDialog()`](#dispProvDialog).

1. Une fois que l’utilisateur a sélectionné un fournisseur, obtenez l’URL du MVPD de l’utilisateur à partir du rappel `navigateToUrl:` ou `navigateToUrl:useSVC:` et ouvrez un contrôleur `UIWebView/WKWebView` ou `SFSafariViewController` et redirigez ce contrôleur vers l’URL.

1. Par le biais de l’instance `UIWebView/WKWebView` ou `SFSafariViewController` instanciée à l’étape précédente, l’utilisateur accède à la page de connexion du MVPD et saisit les informations de connexion. Plusieurs opérations de redirection ont lieu au sein du contrôleur.</br>

>[!NOTE]
>
>À ce stade, l’utilisateur a la possibilité d’annuler le flux d’authentification. Si cela se produit, la couche de l’interface utilisateur est chargée d’informer AccessEnabler de cet événement en appelant [setSelectedProvider()](#setSelProv) avec `null` comme paramètre. Cela permet à AccessEnabler de nettoyer son état interne et de réinitialiser le flux d’authentification.

1. Lors d’une connexion réussie de l’utilisateur, votre couche d’application détecte le chargement d’une URL personnalisée spécifique. Notez que cette URL personnalisée spécifique n’est en fait pas valide et qu’elle n’est pas destinée à être réellement chargée par le contrôleur. Elle ne doit être interprétée que par votre application comme un signal que le flux d&#39;authentification est terminé et qu&#39;il est sûr de fermer le contrôleur `UIWebView/WKWebView` ou `SFSafariViewController`. Dans le cas où un `SFSafariViewController`contrôleur est nécessaire pour utiliser l’URL personnalisée spécifique est définie par **`application's custom scheme`** (par exemple, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), sinon cette URL personnalisée spécifique est définie par la constante **`ADOBEPASS_REDIRECT_URL`** (c’est-à-dire `adobepass://ios.app`).

1. Fermez le contrôleur UIWebView/WKWebView ou SFSafariViewController et appelez la méthode d’API `handleExternalURL:url` d’AccessEnabler, qui demande à AccessEnabler de récupérer le jeton d’authentification du serveur principal.

1. (Facultatif) Appelez [`checkPreauthorizedResources(resources)`](#$checkPreauth) pour vérifier les ressources que l’utilisateur est autorisé à afficher. Le paramètre `resources` est un tableau de ressources protégées associées au jeton d’authentification de l’utilisateur. Une utilisation pour les informations d’autorisation obtenues à partir du MVPD de l’utilisateur consiste à décorer votre interface utilisateur (par exemple, des symboles verrouillés/déverrouillés en regard du contenu protégé).

   * **Triggers:** [`preauthorizedResources()`](#preauthResources) callback
   * **Point d’exécution :** après le flux d’authentification terminé

1. Si l’authentification a réussi, passez au flux d’autorisation.

### D. Flux d’authentification avec SSO Apple sur iOS {#authn_flow_with_applesso}

1. Appelez [`getAuthentication()`](#$getAuthN) pour lancer le flux d’authentification ou pour obtenir la confirmation que l’utilisateur est déjà authentifié.
   **Triggers :**

   * Rappel [presentTvProviderDialog()](#presentTvDialog) si l’utilisateur n’est pas authentifié et que le demandeur actuel a au moins sur MVPD qui prend en charge la connexion unique. Si aucun MVPD ne prend en charge l’authentification unique, le flux d’authentification classique est utilisé.

1. Une fois que l’utilisateur a sélectionné un fournisseur, la bibliothèque AccessEnabler obtient un jeton d’authentification avec les informations fournies par la structure VSA Apple.

1. Le rappel [setAuthenticatiosStatus()](#setAuthNStatus) sera déclenché. À ce stade, l’utilisateur doit être authentifié avec la connexion unique Apple.

1. [Facultatif] Appelez [`checkPreauthorizedResources(resources)`](#$checkPreauth) pour vérifier les ressources que l’utilisateur est autorisé à afficher. Le paramètre `resources` est un tableau de ressources protégées associées au jeton d’authentification de l’utilisateur. Une utilisation pour les informations d’autorisation obtenues à partir du MVPD de l’utilisateur consiste à décorer votre interface utilisateur (par exemple, les symboles verrouillés/déverrouillés en regard du contenu protégé).

   * **Triggers:** [`preauthorizedResources()`](#preauthResources) callback
   * **Point d’exécution :** après le flux d’authentification terminé

1. Si l’authentification a réussi, passez au flux d’autorisation.

### E. Flux d’authentification avec SSO Apple sur tvOS {#authn_flow_with_applesso_tvOS}

1. Appelez [`getAuthentication()`](#$getAuthN) pour lancer la
flux d’authentification ou pour obtenir la confirmation que l’utilisateur est déjà
authentifié.
   **Triggers :**
   * Rappel [`presentTvProviderDialog()`](#presentTvDialog), si l’utilisateur n’est pas authentifié et que le demandeur actuel a au moins un MVPD qui prend en charge la connexion unique. Si aucun MVPD ne prend en charge l’authentification unique, le flux d’authentification classique est utilisé.

1. Une fois que l’utilisateur sélectionne un fournisseur, le rappel [`status()`](#status_callback_implementation) est appelé. Un code d’enregistrement est fourni et la bibliothèque AccessEnabler commence à interroger le serveur pour une authentification du second écran réussie.

1. Si le code d’enregistrement fourni a été utilisé pour s’authentifier correctement sur le deuxième écran, le rappel [`setAuthenticatiosStatus()`](#setAuthNStatus) est déclenché. À ce stade, l’utilisateur doit être authentifié avec la connexion unique Apple.
1. [Facultatif] Appelez [`checkPreauthorizedResources(resources)`](#$checkPreauth) pour vérifier les ressources que l’utilisateur est autorisé à afficher. Le paramètre `resources` est un tableau de ressources protégées associées au jeton d’authentification de l’utilisateur. Une utilisation pour les informations d’autorisation obtenues à partir du MVPD de l’utilisateur consiste à décorer votre interface utilisateur (par exemple, les symboles verrouillés/déverrouillés en regard du contenu protégé).

   * **Triggers:** [`preauthorizedResources()`](#preauthResources) callback

   * **Point d’exécution :** après le flux d’authentification terminé
1. Si l’authentification a réussi, passez au flux d’autorisation.

### F. Flux d’autorisation {#authz_flow}

1. Appelez [getAuthorization()](#$getAuthZ) pour lancer le flux d’autorisation.

   * **Dépendance :** ID de ressource valide accepté avec le ou les MVPD.
   * Les identifiants de ressource doivent être identiques à ceux utilisés sur d’autres appareils ou plateformes et seront identiques pour tous les MVPD. Pour plus d’informations sur les ID de ressource, voir [Identification des ressources protégées](/help/authentication/identify-protected-resources.md)

1. Validez l’authentification et l’autorisation.

   * Si l’appel [getAuthorization()](#$getAuthZ) réussit : l’utilisateur dispose de jetons AuthN et AuthZ valides (l’utilisateur est authentifié et autorisé à regarder le média demandé).

   * Si [getAuthorization()](#$getAuthZ) échoue : examinez l’exception générée pour déterminer son type (AuthN, AuthZ, etc.) :
      * S’il s’agissait d’une erreur d’authentification (AuthN), redémarrez le flux d’authentification.
      * S’il s’agissait d’une erreur d’autorisation (AuthZ), l’utilisateur n’est pas autorisé à regarder le média demandé et un message d’erreur de ce type doit s’afficher à l’utilisateur.
      * En cas d&#39;erreur d&#39;un autre type (erreur de connexion, erreur réseau, etc.) puis afficher un message d’erreur approprié à l’utilisateur.

1. Validez le jeton de média court.\
   Utilisez la bibliothèque du vérificateur de jeton multimédia d’authentification Adobe Pass pour vérifier le jeton multimédia de courte durée renvoyé par l’appel [getAuthorization()](#$getAuthZ) ci-dessus :

   * Si la validation réussit : lisez le média demandé pour l’utilisateur.
   * Si la validation échoue : le jeton AuthZ n’était pas valide, la demande de média doit être refusée et un message d’erreur doit s’afficher à l’utilisateur.


1. Revenez à votre flux d’applications normal.

### G. Afficher le flux multimédia {#media_flow}

1. L’utilisateur sélectionne le média à afficher.
1. Les médias sont-ils protégés ? Votre application vérifie si le média sélectionné est protégé :

   * Si le média sélectionné est protégé, votre application démarre le [flux d’autorisation](#authz_flow) ci-dessus.

   * Si le média sélectionné n’est pas protégé, lisez le média pour
l’utilisateur.

### H. Flux de déconnexion sans SSO Apple {#logout_flow_wo_AppleSSO}

1. Appelez [`logout()`](#$logout) pour déconnecter l’utilisateur. AccessEnabler efface toutes les valeurs et tous les jetons mis en cache. Après avoir vidé le cache, AccessEnabler effectue un appel au serveur pour nettoyer les sessions côté serveur. Puisque l’appel au serveur peut entraîner une redirection SAML vers l’IdP (cela permet le nettoyage de session du côté IdP), cet appel doit suivre toutes les redirections. Pour cette raison, cet appel doit être traité dans un contrôleur UIWebView/WKWebView ou SFSafariViewController .

   a. Suivant le même modèle que le workflow d’authentification, le domaine AccessEnabler émet une requête à la couche d’application de l’interface utilisateur, via le rappel `navigateToUrl:` ou `navigateToUrl:useSVC:`, pour créer un UIWebView/WKWebView ou SFSafariViewController et lui demander de charger l’URL fournie dans le paramètre `url` du rappel. Il s’agit de l’URL du point de terminaison de la déconnexion sur le serveur principal.

   b. Votre application doit surveiller l’activité du contrôleur `UIWebView/WKWebView or SFSafariViewController` et détecter le moment où elle charge une URL personnalisée spécifique lorsqu’elle passe par plusieurs redirections. Notez que cette URL personnalisée spécifique n’est en fait pas valide et qu’elle n’est pas destinée à être réellement chargée par le contrôleur. Elle ne doit être interprétée que par votre application comme un signal que le flux de déconnexion est terminé et qu’il est sûr de fermer le contrôleur `UIWebView/WKWebView` ou `SFSafariViewController`. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer le contrôleur `UIWebView/WKWebView or SFSafariViewController` et appeler la méthode d&#39;API `handleExternalURL:url` d&#39;AccessEnabler. Dans le cas où un `SFSafariViewController`contrôleur est nécessaire pour utiliser l’URL personnalisée spécifique est définie par **`application's custom scheme`** (par exemple, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), sinon cette URL personnalisée spécifique est définie par la constante **`ADOBEPASS_REDIRECT_URL`** (c’est-à-dire `adobepass://ios.app`).

   >[!NOTE]
   >
   >Le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir de quelque manière que ce soit avec UIWebView/WKWebView ou SFSafariViewController. La couche de l’application de l’interface utilisateur utilise UIWebView/WKWebView ou SFSafariViewController pour s’assurer que toutes les redirections sont suivies. Par conséquent, il est possible (et recommandé) de rendre le contrôleur invisible (c’est-à-dire masqué) pendant le processus de déconnexion.


### I. Flux de déconnexion avec Apple SSO {#logout_flow_with_AppleSSO}

1. Appelez [`logout()`](#$logout) pour déconnecter l’utilisateur.
1. Le rappel [status()](#status_callback_implementation) sera appelé avec l’ID VSA203.
1. L’utilisateur doit également être invité à se connecter à partir des paramètres système. Si vous ne le faites pas, une nouvelle authentification est effectuée lorsque l’application est relancée.



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
