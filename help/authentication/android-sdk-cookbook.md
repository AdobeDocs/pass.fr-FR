---
title: Guide pas à pas du SDK Android
description: Guide pas à pas du SDK Android
exl-id: 7f66ab92-f52c-4dae-8016-c93464dd5254
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 0%

---

# Guide pas à pas du SDK Android {#android-sdk-cookbook}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Introduction {#intro}

Ce document décrit les processus de droits que l’application de niveau supérieur d’un programmeur peut implémenter via les API exposées par la bibliothèque Android AccessEnabler.


La solution de droit d’authentification Adobe Pass pour Android est finalement divisée en deux domaines :

- Domaine de l’interface utilisateur : il s’agit de la couche supérieure de l’application qui implémente l’interface utilisateur et qui utilise les services fournis par la bibliothèque AccessEnabler pour fournir l’accès au contenu restreint.
- Domaine AccessEnabler : c&#39;est là que les workflows de droits sont implémentés sous la forme :
   - Appels réseau effectués sur les serveurs principaux d’Adobe
   - Règles de logique métier liées aux workflows d’authentification et d’autorisation
   - Gestion de diverses ressources et traitement de l’état du workflow (comme le cache de jeton)

L’objectif du domaine AccessEnabler est de masquer toutes les complexités des workflows de droits et de fournir à l’application de couche supérieure (via la bibliothèque AccessEnabler) un ensemble de primitives de droits simples avec lesquelles vous implémentez les workflows de droits :

1. Définissez l’identité du demandeur.

1. Vérifiez et obtenez une authentification auprès d’un fournisseur d’identité particulier.

1. Vérifiez et obtenez l’autorisation d’une ressource particulière.

1. Déconnectez-vous.

L’activité réseau d’AccessEnabler a lieu dans un autre thread, de sorte que le thread d’interface utilisateur n’est jamais bloqué. Par conséquent, le canal de communication bidirectionnel entre les deux domaines d’application doit suivre un modèle entièrement asynchrone :

- La couche d’application de l’interface utilisateur envoie des messages au domaine AccessEnabler via les appels d’API exposés par la bibliothèque AccessEnabler.
- AccessEnabler répond à la couche de l’interface utilisateur par le biais des méthodes de rappel incluses dans le protocole AccessEnabler que la couche de l’interface utilisateur enregistre auprès de la bibliothèque AccessEnabler.

## Flux de droits {#entitlement}

1. [Conditions préalables](#prereqs)
1. [Flux de démarrage](#startup_flow)
1. [Flux d’authentification](#authn_flow)
1. [Flux d’autorisation](#authz_flow)
1. [Afficher le flux du média](#media_flow)
1. [Flux de connexion](#logout_flow)

### A. Conditions préalables {#prereqs}

1. Créez vos fonctions de rappel :
   - [`setRequestorComplete()`](#$setRequestorComplete)

     Déclenché par `setRequestor()`, renvoie succès ou échec.\
     La réussite indique que vous pouvez procéder à des appels de droits.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

     Déclenché par `getAuthentication()` uniquement si l’utilisateur n’a pas sélectionné de fournisseur (MVPD) et n’est pas encore authentifié.\
     Le paramètre `mvpds` est un tableau de fournisseurs mis à la disposition de l’utilisateur.

   - [`setAuthenticationStatus(status, errorcode)`](#$setAuthNStatus)

     Déclenché par `checkAuthentication()` à chaque fois.\
     Déclenché par `getAuthentication()` uniquement si l’utilisateur est déjà authentifié et a sélectionné un fournisseur.

     L’état renvoyé est succès ou échec, le code d’erreur décrit le type de l’échec.

   - [navigateToUrl(url)](#$navigateToUrl)

     Déclenché par `getAuthentication()` après que l’utilisateur a sélectionné un MVPD. Le paramètre `url` fournit l’emplacement de la page de connexion du MVPD.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

     Déclenché par `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.\
     Le paramètre `event` indique quel événement de droit s’est produit ; le paramètre `data` est une liste de valeurs relatives à l’événement.

   - [`setToken(jeton, ressource)`](#$setToken)

     Déclenché par `checkAuthorization()` et `getAuthorization()` après une autorisation réussie d’affichage d’une ressource.\
     Le paramètre `token` est le jeton multimédia de courte durée ; le paramètre `resource` est le contenu que l’utilisateur est autorisé à afficher.

   - [`tokenRequestFailed(ressource, code, description)`](#$tokenRequestFailed)

     Déclenché par `checkAuthorization()` et `getAuthorization()` après une autorisation manquée.\
     Le paramètre `resource` est le contenu que l’utilisateur tentait de consulter ; le paramètre `code` est le code d’erreur indiquant le type d’échec survenu ; le paramètre `description` décrit l’erreur associée au code d’erreur.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

     Déclenché par `getSelectedProvider()`.\
     Le paramètre `mvpd` fournit des informations sur le fournisseur sélectionné par l’utilisateur.

   - [`setMetadataStatus(metadata, key, arguments)`](#$setMetadataStatus)

     Déclenché par `getMetadata().`\
     Le paramètre `metadata` fournit les données spécifiques que vous avez demandées ; le paramètre `key` est la clé utilisée dans la requête `getMetadata()` ; et le paramètre `arguments` est le même dictionnaire qui a été transmis à `getMetadata()`.

   - [`preauthorizedResources(resources)`](#$preauthResources)

     Déclenché par `checkPreauthorizedResources()`.\
     Le paramètre `authorizedResources` présente les ressources que l’utilisateur est autorisé à afficher.


![](assets/android-entitlement-flows.png)


### B. Flux de démarrage {#startup_flow}

1. Démarrez l’application de niveau supérieur.
1. Lancement de l’authentification Adobe Pass

   a. Appelez [`getInstance`](#$getInstance) pour créer une instance unique de Adobe Pass Authentication AccessEnabler.

   - **Dépendance :** Authentification native Adobe Pass
Bibliothèque Android (AccessEnabler)

   b. Appelez` setRequestor()` pour établir l’identification du programmeur ; transmettez le `requestorID` du programmeur et (éventuellement) un tableau de points de terminaison d’authentification Adobe Pass.

   - **Dépendance :** Demandeur d’authentification Adobe Pass valide\
     (Consultez votre gestionnaire de compte d’authentification Adobe Pass pour organiser cela.)

   - **Triggers:** rappel setRequestorComplete()

   | REMARQUE |     |
   | --- | --- |  
   | ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/icons/1313859077_lightbulb.png) | Aucune demande de droit ne peut être effectuée tant que l’identité du demandeur n’a pas été entièrement établie. Cela signifie que, pendant que setRequestor() est toujours en cours d’exécution, toutes les demandes de droits suivantes (par exemple, `checkAuthentication()`) sont bloquées.<br><br>Vous avez deux options de mise en oeuvre : Une fois les informations d’identification du demandeur envoyées au serveur principal, la couche d’application de l’interface utilisateur peut choisir l’une des deux approches suivantes :<br><br>1.  Attendez le déclenchement du rappel `setRequestorComplete()` (partie du délégué AccessEnabler).  Cette option garantit la plus grande certitude que `setRequestor()` a été effectuée. Elle est donc recommandée pour la plupart des mises en oeuvre.<br>2.  Continuez sans attendre le déclenchement du rappel `setRequestorComplete()` et commencez à émettre des demandes de droits. Ces appels (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sont placés en file d’attente par la bibliothèque AccessEnabler, qui effectuera les appels réseau réels après l’ `setRequestor(). `Cette option peut occasionnellement être interrompue si, par exemple, la connexion réseau est instable. |

1. Appelez [checkAuthentication()](#$checkAuthN) pour rechercher une authentification existante sans initialiser le flux d’authentification complet.   Si cet appel réussit, vous pouvez passer directement au flux d’autorisation.  Si ce n’est pas le cas, passez au flux d’authentification.

   - **Dépendance :** Appel réussi à `setRequestor()` (cette dépendance s’applique également à tous les appels suivants).

   - **Triggers:** rappel setAuthenticationStatus()

### C. Flux d’authentification {#authn_flow}

1. Appelez [`getAuthentication()`](#$getAuthN) pour lancer le flux d’authentification ou pour obtenir la confirmation que l’utilisateur est déjà authentifié.\
   **Triggers :**
   - Rappel setAuthenticationStatus() , si l’utilisateur est déjà authentifié.  Dans ce cas, accédez directement au [flux d’autorisation](#authz_flow).
   - Rappel displayProviderDialog() , si l’utilisateur n’est pas encore authentifié.

1. Présenter à l’utilisateur la liste des fournisseurs envoyés à `displayProviderDialog()`.

1. Une fois que l’utilisateur a sélectionné un fournisseur, obtenez l’URL du MVPD de l’utilisateur à partir du rappel `navigateToUrl()`.  Ouvrez un WebView et redirigez ce contrôle WebView vers l’URL.

1. Par le biais de WebView instancié à l’étape précédente, l’utilisateur accède à la page de connexion du MVPD et saisit les informations de connexion. Plusieurs opérations de redirection ont lieu dans WebView.

   **Remarque :** À ce stade, l’utilisateur a la possibilité d’annuler le flux d’authentification. Si cela se produit, la couche de l’interface utilisateur est chargée d’informer AccessEnabler de cet événement en appelant `setSelectedProvider()` avec `null` comme paramètre. Cela permet à AccessEnabler de nettoyer son état interne et de réinitialiser le flux d’authentification.

1. Lors d’une connexion réussie de l’utilisateur, votre couche d’application détecte le chargement d’une &quot;URL de redirection personnalisée&quot; (c’est-à-dire : `http://adobepass.android.app`). Cette URL personnalisée est en fait une URL non valide qui n’est pas destinée au chargement de WebView. Il s’agit d’un signal indiquant que le flux d’authentification est terminé et que WebView doit être fermé.

1. Fermez le contrôle WebView et appelez `getAuthenticationToken()`, ce qui demande à AccessEnabler de récupérer le jeton d&#39;authentification du serveur principal.

1. [Facultatif] Appelez [`checkPreauthorizedResources(resources)`](#$checkPreauth) pour vérifier les ressources que l’utilisateur est autorisé à afficher. Le paramètre `resources` est un tableau de ressources protégées associées au jeton d’authentification de l’utilisateur.\
   **Triggers:** `preAuthorizedResources()` callback\
   **Point d’exécution :** après le flux d’authentification terminé

1. Si l’authentification a réussi, passez au flux d’autorisation.



### D. Flux d’autorisation {#authz_flow}

1. Appelez [getAuthorization()](#$getAuthZ) pour lancer l’autorisation
flux.

   Dépendance : ID de ressource valide convenu avec le ou les MVPD.

   **Remarque :** Les ResourceID doivent être identiques à ceux utilisés sur d’autres appareils ou plateformes et seront identiques sur plusieurs MVPD.

1. Validez l’authentification et l’autorisation.

   - Si l’appel `getAuthorization()` réussit : l’utilisateur dispose de jetons AuthN et AuthZ valides (l’utilisateur est authentifié et autorisé à regarder le média demandé).
   - Si `getAuthorization()` échoue : examinez l’exception générée pour déterminer son type (AuthN, AuthZ, etc.) :
      - S’il s’agissait d’une erreur d’authentification (AuthN), redémarrez le flux d’authentification.
      - S’il s’agissait d’une erreur d’autorisation (AuthZ), l’utilisateur n’est pas autorisé à regarder le média demandé et un message d’erreur de ce type doit s’afficher à l’utilisateur.
      - En cas d&#39;erreur d&#39;un autre type (erreur de connexion, erreur réseau, etc.) puis afficher un message d’erreur approprié à l’utilisateur.

1. Validez le jeton de média court.\
   Utilisez la bibliothèque du vérificateur de jeton multimédia d’authentification Adobe Pass pour vérifier le jeton multimédia de courte durée renvoyé par l’appel `getAuthorization()` ci-dessus :

   - Si la validation réussit : lisez le média demandé pour l’utilisateur.
   - Si la validation échoue : le jeton AuthZ n’était pas valide, la demande de média doit être refusée et un message d’erreur doit s’afficher à l’utilisateur.

1. Revenez à votre flux d’applications normal.

### E. Afficher le flux multimédia {#media_flow}

1. L’utilisateur sélectionne le média à afficher.
2. Les médias sont-ils protégés ?  Votre application vérifie si le média sélectionné est protégé :
- Si le média sélectionné est protégé, votre application démarre le [flux d’autorisation](#authz_flow) ci-dessus.
- Si le média sélectionné n’est pas protégé, lisez-le pour l’utilisateur.



### F. Flux de déconnexion {#logout_flow}

1. Appelez [`logout()`](#$logout) pour déconnecter l’utilisateur.\
   AccessEnabler efface toutes les valeurs et tous les jetons mis en cache du MVPD actuel pour le demandeur actuel, ainsi que pour les demandeurs avec authentification unique. Après avoir vidé le cache, AccessEnabler effectue un appel au serveur pour nettoyer les sessions côté serveur.  Puisque l’appel au serveur peut entraîner une redirection SAML vers l’IdP (cela permet le nettoyage de session du côté IdP), cet appel doit suivre toutes les redirections. C’est pourquoi cet appel doit être traité dans un contrôle WebView.

   a. Suivant le même modèle que le workflow d’authentification, le domaine AccessEnabler envoie une requête à la couche de l’application de l’interface utilisateur (via le rappel `navigateToUrl()`) pour créer un contrôle WebView et demander à ce contrôle de charger l’URL du point de terminaison de déconnexion sur le serveur principal.

   b. Encore une fois, l’interface utilisateur doit surveiller l’activité du contrôle WebView et détecter le moment où le contrôle, lorsqu’il passe par plusieurs redirections, charge l’URL personnalisée de l’application (c’est-à-dire : `http://adobepass.android.app/`). Une fois cet événement survenu, la couche de l’application de l’interface utilisateur ferme le WebView et le processus de déconnexion est terminé.

   **Remarque :** Le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir de quelque manière que ce soit avec WebView. La couche de l’application de l’interface utilisateur utilise un WebView pour s’assurer que toutes les redirections sont suivies. Il est donc possible (et recommandé) de rendre le contrôle WebView invisible (c’est-à-dire masqué) pendant le processus de déconnexion.



### Flux utilisateur pour la connexion avec plusieurs MVPD et la déconnexion {#user_flows}

[Ici](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AndroidSSOUserFlows.pdf) vous avez un document décrivant le comportement lors de l’utilisation de plusieurs MVPD et ce qui se passe lorsque l’utilisateur se déconnecte d’une application.

Le comportement décrit est disponible lors de l’utilisation du SDK Android version >= 2.0.0.
