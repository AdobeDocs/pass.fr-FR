---
title: Manuel d’Android SDK
description: Manuel d’Android SDK
exl-id: 7f66ab92-f52c-4dae-8016-c93464dd5254
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1682'
ht-degree: 0%

---

# Manuel Android SDK (hérité) {#android-sdk-cookbook}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Introduction {#intro}

Ce document décrit les workflows de droits qu’une application de niveau supérieur d’un programmeur peut implémenter via les API exposées par la bibliothèque Android AccessEnabler.


La solution Droits d’authentification Adobe Pass pour Android est finalement divisée en deux domaines :

- Le domaine de l’interface utilisateur : il s’agit de la couche d’application de niveau supérieur qui implémente l’interface utilisateur et utilise les services fournis par la bibliothèque AccessEnabler pour fournir l’accès au contenu limité.
- Le domaine AccessEnabler - c’est là que les workflows de droits sont implémentés sous la forme de :
   - Appels réseau effectués aux serveurs principaux d’Adobe
   - Règles de logique commerciale liées aux workflows d’authentification et d’autorisation
   - Gestion de diverses ressources et traitement de l’état des workflows (tel que le cache de jetons)

L’objectif du domaine AccessEnabler est de masquer toutes les complexités des workflows de droits et de fournir à l’application de couche supérieure (par le biais de la bibliothèque AccessEnabler) un ensemble de primitives de droits simples avec lesquelles vous implémentez les workflows de droits :

1. Définissez l’identité du demandeur.

1. Vérifiez et obtenez une authentification auprès d’un fournisseur d’identité spécifique.

1. Vérifiez et obtenez l’autorisation pour une ressource spécifique.

1. Déconnexion.

L’activité réseau d’AccessEnabler a lieu dans un thread différent, de sorte que le thread de l’interface utilisateur n’est jamais bloqué. Par conséquent, le canal de communication bidirectionnel entre les deux domaines d’application doit suivre un modèle entièrement asynchrone :

- La couche d’application de l’interface utilisateur envoie des messages au domaine AccessEnabler via les appels d’API exposés par la bibliothèque AccessEnabler.
- AccessEnabler répond à la couche d’interface utilisateur par le biais des méthodes de rappel incluses dans le protocole AccessEnabler que la couche d’interface utilisateur enregistre auprès de la bibliothèque AccessEnabler.

## Flux de droits {#entitlement}

1. [Conditions préalables](#prereqs)
1. [Flux de démarrage](#startup_flow)
1. [Flux d’authentification](#authn_flow)
1. [Flux d’autorisation](#authz_flow)
1. [Afficher le flux multimédia](#media_flow)
1. [Flux de déconnexion](#logout_flow)

### A. Prérequis {#prereqs}

1. Créez vos fonctions de rappel :
   - [`setRequestorComplete()`](#$setRequestorComplete)

     Déclenché par `setRequestor()`, renvoie un succès ou un échec.\
     La réussite indique que vous pouvez poursuivre les appels de droits.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

     Déclenché par `getAuthentication()` uniquement si l’utilisateur n’a pas sélectionné de fournisseur (MVPD) et n’est pas encore authentifié.\
     Le paramètre `mvpds` est un tableau de fournisseurs disponibles pour l’utilisateur.

   - [`setAuthenticationStatus(status, errorcode)`](#$setAuthNStatus)

     Déclenché par `checkAuthentication()` à chaque fois.\
     Déclenché par `getAuthentication()` uniquement si l’utilisateur est déjà authentifié et a sélectionné un fournisseur.

     Le statut renvoyé est succès ou échec. Le code d’erreur décrit le type de l’échec.

   - [navigateToUrl(url)](#$navigateToUrl)

     Déclenché par `getAuthentication()` après la sélection d’un MVPD par l’utilisateur. Le paramètre `url` indique l’emplacement de la page de connexion de MVPD.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

     Déclenché par `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.\
     Le paramètre `event` indique quel événement de droit s’est produit ; le paramètre `data` est une liste de valeurs relatives à l’événement.

   - [`setToken(token, resource)`](#$setToken)

     Déclenché par `checkAuthorization()` et `getAuthorization()` après une autorisation réussie d’affichage d’une ressource.\
     Le paramètre `token` est le jeton de média de courte durée ; le paramètre `resource` est le contenu que l’utilisateur est autorisé à afficher.

   - [`tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

     Déclenché par `checkAuthorization()` et `getAuthorization()` après une autorisation infructueuse.\
     Le paramètre `resource` correspond au contenu que l’utilisateur tentait d’afficher ; le paramètre `code` correspond au code d’erreur indiquant le type d’échec ; le paramètre `description` décrit l’erreur associée au code d’erreur.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

     Déclenché par `getSelectedProvider()`.\
     Le paramètre `mvpd` fournit des informations sur le fournisseur sélectionné par l’utilisateur ou l’utilisatrice.

   - [`setMetadataStatus(metadata, key, arguments)`](#$setMetadataStatus)

     Déclenché par `getMetadata().`\
     Le paramètre `metadata` fournit les données spécifiques que vous avez demandées ; le paramètre `key` est la clé utilisée dans la requête `getMetadata()` ; et le paramètre `arguments` est le même dictionnaire que celui transmis à `getMetadata()`.

   - [`preauthorizedResources(resources)`](#$preauthResources)

     Déclenché par `checkPreauthorizedResources()`.\
     Le paramètre `authorizedResources` présente les ressources que l’utilisateur est autorisé à afficher.


![](../../../../assets/android-entitlement-flows.png)


### B. Flux de démarrage {#startup_flow}

1. Démarrez l’application de niveau supérieur.
1. Lancement de l’authentification Adobe Pass

   a. Appelez [`getInstance`](#$getInstance) pour créer une instance unique d’Adobe Pass Authentication AccessEnabler.

   - **Dépendance :** Authentification Adobe Pass Native
Bibliothèque Android (AccessEnabler)

   b. Appelez ` setRequestor()` pour établir l’identité du programmeur ; transmettez le `requestorID` du programmeur et (éventuellement) un tableau de points d’entrée d’authentification Adobe Pass.

   - **Dépendance : ID** demandeur d’authentification Adobe Pass valide\
     (Contactez votre gestionnaire de compte d’authentification Adobe Pass pour organiser cette opération.)

   - Rappel **Triggers:** setRequestorComplete()

   | REMARQUE |     |
   | --- | --- |  
   | ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/icons/1313859077_lightbulb.png) | Aucune demande de droit ne peut être traitée tant que l’identité du demandeur n’est pas entièrement établie. Cela signifie effectivement que, pendant que setRequestor() est toujours en cours d’exécution, toutes les demandes de droits suivantes (par exemple, `checkAuthentication()`) sont bloquées.<br><br>Vous disposez de deux options d’implémentation : une fois les informations d’identification du demandeur envoyées au serveur principal, la couche d’application de l’interface utilisateur peut choisir l’une des deux approches suivantes :<br><br>1.  Attendez le déclenchement du rappel `setRequestorComplete()` (partie du délégué AccessEnabler).  Cette option offre la plus grande certitude quant à la réalisation de l`setRequestor()`opération. Elle est donc recommandée pour la plupart des implémentations.<br>2.  Continuez sans attendre le déclenchement du rappel `setRequestorComplete()` et commencez à émettre des demandes de droits. Ces appels (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sont mis en file d&#39;attente par la bibliothèque AccessEnabler, qui effectuera les appels réseau réels après l&#39;`setRequestor(). `Cette option peut parfois être interrompue si, par exemple, la connexion réseau est instable. |

1. Appelez [checkAuthentication()](#$checkAuthN) pour vérifier une authentification existante sans initier le flux d’authentification complet.   Si cet appel réussit, vous pouvez passer directement au flux d’autorisation.  Dans le cas contraire, passez au flux d’authentification.

   - **Dépendance :** un appel réussi à `setRequestor()` (cette dépendance s’applique également à tous les appels suivants).

   - Rappel **Triggers:** setAuthenticationStatus()

### C. Flux d’authentification {#authn_flow}

1. Appelez [`getAuthentication()`](#$getAuthN) pour lancer le flux d’authentification ou pour obtenir la confirmation que l’utilisateur est déjà authentifié.\
   **Triggers:**
   - Le rappel setAuthenticationStatus() , si l’utilisateur est déjà authentifié.  Dans ce cas, passez directement au [flux d’autorisation](#authz_flow).
   - Le rappel displayProviderDialog() , si l’utilisateur n’est pas encore authentifié.

1. Présentez à l&#39;utilisateur la liste des fournisseurs envoyés à `displayProviderDialog()`.

1. Une fois que l’utilisateur a sélectionné un fournisseur, obtenez l’URL du MVPD de l’utilisateur à partir du rappel `navigateToUrl()`.  Ouvrez un WebView et dirigez-le vers l&#39;URL.

1. Par le biais du WebView instancié à l’étape précédente, l’utilisateur accède à la page de connexion de MVPD et saisit les informations d’identification de connexion. Plusieurs opérations de redirection ont lieu dans WebView.

   **Remarque :** à ce stade, l’utilisateur a la possibilité d’annuler le flux d’authentification. Si cela se produit, votre calque d’interface utilisateur est chargé d’informer l’AccessEnabler de cet événement en appelant la `setSelectedProvider()` avec `null` comme paramètre. Cela permet à AccessEnabler de nettoyer son état interne et de réinitialiser le flux d’authentification.

1. Une fois la connexion établie par l’utilisateur, votre couche d’application détecte le chargement d’une « URL de redirection personnalisée » (par exemple : `http://adobepass.android.app`). Cette URL personnalisée est en fait une URL non valide qui n&#39;est pas destinée au chargement de WebView. Il s’agit d’un signal indiquant que le flux d’authentification est terminé et que la vue Web doit être fermée.

1. Fermez le contrôle WebView et appelez `getAuthenticationToken()`, qui demande à AccessEnabler de récupérer le jeton d&#39;authentification du serveur principal.

1. [Facultatif] Appelez [`checkPreauthorizedResources(resources)`](#$checkPreauth) pour vérifier quelles ressources l’utilisateur est autorisé à afficher. Le paramètre `resources` est un tableau de ressources protégées associé au jeton d’authentification de l’utilisateur.\
   **Triggers:** rappel `preAuthorizedResources()`\
   **Point d’exécution :** après le flux d’authentification terminé

1. Si l’authentification a réussi, passez au flux d’autorisation.



### D. Flux d’autorisation {#authz_flow}

1. Appelez [getAuthorization()](#$getAuthZ) pour lancer l’autorisation
flux.

   Dépendance : identifiant(s) de ressource valide(s) convenu(s) avec le ou les MVPD.

   **Remarque :** ResourceID doivent être identiques à ceux utilisés sur d’autres appareils ou plateformes et seront identiques sur tous les MVPD.

1. Validez l’authentification et l’autorisation.

   - Si l’appel `getAuthorization()` réussit : l’utilisateur dispose de jetons AuthN et AuthZ valides (l’utilisateur est authentifié et autorisé à regarder le média demandé).
   - Si `getAuthorization()` échoue : examinez l’exception renvoyée pour déterminer son type (AuthN, AuthZ ou autre chose) :
      - S’il s’agissait d’une erreur d’authentification (AuthN), redémarrez le flux d’authentification.
      - S’il s’agissait d’une erreur d’autorisation (AuthZ), l’utilisateur n’est pas autorisé à regarder le média demandé et un message d’erreur doit s’afficher à l’intention de l’utilisateur.
      - S’il y a eu un autre type d’erreur (erreur de connexion, erreur réseau, etc.), affichez un message d’erreur approprié à l’intention de l’utilisateur.

1. Validez le jeton de média court.\
   Utilisez la bibliothèque Vérificateur de jeton de média d’authentification Adobe Pass pour vérifier le jeton de média de courte durée renvoyé par l’appel `getAuthorization()` ci-dessus :

   - Si la validation réussit : lisez le média demandé pour l’utilisateur.
   - Si la validation échoue : le jeton AuthZ n’était pas valide, la requête de média doit être refusée et un message d’erreur doit s’afficher à l’intention de l’utilisateur.

1. Retournez à votre flux d’application normal.

### E. Afficher le flux multimédia {#media_flow}

1. L’utilisateur sélectionne le média à afficher.
2. Les médias sont-ils protégés ?  Votre application vérifie si le média sélectionné est protégé :
- Si le média sélectionné est protégé, votre application lance le [flux d’autorisation](#authz_flow) ci-dessus.
- Si le média sélectionné n’est pas protégé, lisez le média pour l’utilisateur.



### F. Flux de déconnexion {#logout_flow}

1. Appelez [`logout()`](#$logout) pour déconnecter l’utilisateur.\
   AccessEnabler efface toutes les valeurs et tous les jetons mis en cache pour le MVPD actuel pour le demandeur actuel et également pour les demandeurs avec authentification SSO. Après avoir effacé le cache, AccessEnabler effectue un appel au serveur pour nettoyer les sessions côté serveur.  Notez que puisque l’appel au serveur peut entraîner une redirection SAML vers l’IdP (cela permet le nettoyage de la session du côté IdP), cet appel doit suivre toutes les redirections. Pour cette raison, cet appel doit être géré dans un contrôle WebView.

   a. Suivant le même modèle que le workflow d’authentification, le domaine AccessEnabler effectue une requête à la couche d’application de l’interface utilisateur (via le rappel `navigateToUrl()`) pour créer un contrôle WebView et demander à ce contrôle de charger l’URL du point d’entrée de déconnexion sur le serveur principal.

   b. Là encore, l&#39;interface utilisateur doit surveiller l&#39;activité du contrôle WebView et détecter le moment où le contrôle, lorsqu&#39;il passe par plusieurs redirections, charge l&#39;URL personnalisée de l&#39;application (c&#39;est-à-dire : `http://adobepass.android.app/`). Une fois cet événement effectué, la couche d’application de l’interface utilisateur ferme le WebView et le processus de déconnexion est terminé.

   **Remarque :** le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir avec le WebView de quelque manière que ce soit. La couche d’application de l’interface utilisateur utilise un WebView pour s’assurer que toutes les redirections sont suivies. Il est donc possible (et recommandé) de rendre le contrôle WebView invisible (c&#39;est-à-dire masqué) pendant le processus de déconnexion.



### Flux utilisateur pour la connexion avec plusieurs MVPD et la déconnexion {#user_flows}

[Vous avez ici ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AndroidSSOUserFlows.pdf) document décrivant le comportement lors de l’utilisation de plusieurs fichiers MVPD et ce qui se passe lorsque l’utilisateur se déconnecte d’une application.

Le comportement décrit est disponible lors de l’utilisation d’Android SDK version >= 2.0.0.
