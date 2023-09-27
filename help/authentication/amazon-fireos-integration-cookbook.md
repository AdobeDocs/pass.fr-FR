---
title: Guide d’intégration Amazon FireOS
description: Guide d’intégration Amazon FireOS
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1432'
ht-degree: 0%

---

# Guide d’intégration Amazon FireOS {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>


## Introduction {#intro}

Ce document décrit les processus de droits que l’application de niveau supérieur d’un programmeur peut mettre en oeuvre via les API exposées par la bibliothèque Amazon FireOS AccessEnabler.

La solution de droit d’authentification Adobe Pass pour Amazon FireOS est finalement divisée en deux domaines :

- Domaine de l’interface utilisateur : il s’agit de la couche supérieure de l’application qui implémente l’interface utilisateur et qui utilise les services fournis par la bibliothèque AccessEnabler pour fournir l’accès au contenu restreint.
- Domaine AccessEnabler : c&#39;est là que les workflows de droits sont implémentés sous la forme :
   - Appels réseau effectués sur les serveurs principaux d’Adobe
   - Règles de logique métier liées aux workflows d’authentification et d’autorisation
   - Gestion de diverses ressources et traitement de l’état du workflow (comme le cache de jeton)

L’objectif du domaine AccessEnabler est de masquer toutes les complexités des workflows de droits et de fournir à l’application de couche supérieure (via la bibliothèque AccessEnabler) un ensemble de primitives de droits simples avec lesquelles vous implémentez les workflows de droits :

1. Définition de l’identité du demandeur
1. Vérifier et obtenir une authentification auprès d’un fournisseur d’identité particulier
1. Obtention de l’autorisation d’une ressource spécifique
1. Déconnexion

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

      - Déclenché par `setRequestor()`, renvoie succès ou échec.     La réussite indique que vous pouvez procéder à des appels de droits.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - Déclenché par `getAuthentication()` uniquement si l’utilisateur n’a pas sélectionné de fournisseur (MVPD) et n’est pas encore authentifié. La variable `mvpds` est un tableau de fournisseurs mis à la disposition de l’utilisateur.

   - [`setAuthenticationStatus(status, reason)`](#$setAuthNStatus)

      - Déclenché par `checkAuthentication()` à chaque fois. Déclenché par `getAuthentication()` uniquement si l’utilisateur est déjà authentifié et a sélectionné un fournisseur.

      - L’état renvoyé est authentifié ou n’est pas authentifié. La raison décrit un échec de l’authentification ou une action de déconnexion.

   - [navigateToUrl(url)](#$navigateToUrl)

      - Ignorée dans le SDK AmazonFireOS, la méthode est utilisée sur les plateformes Android où est déclenché par `getAuthentication()` une fois que l’utilisateur a sélectionné un MVPD.  La variable `url` fournit l’emplacement de la page de connexion du MVPD.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

      - Déclenché par `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
La variable `event` indique quel événement de droit s’est produit ; `data` est une liste de valeurs relatives à l’événement.

   - [`setToken(jeton, ressource)`](#$setToken)

      - Déclenché par `checkAuthorization()` et `getAuthorization()` après une autorisation réussie d’affichage d’une ressource.
      - La variable `token` est le jeton multimédia de courte durée ; le paramètre `resource` est le contenu que l’utilisateur est autorisé à afficher.

   - [`tokenRequestFailed(ressource, code, description)`](#$tokenRequestFailed)

      - Déclenché par `checkAuthorization()` et `getAuthorization()` après une autorisation manquée.
      - La variable `resource` est le contenu que l’utilisateur tentait d’afficher ; le paramètre `code` est le code d’erreur indiquant le type d’échec qui s’est produit ; le paramètre `description` décrit l’erreur associée au code d’erreur.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

      - Déclenché par `getSelectedProvider()`.
      - La variable `mvpd` fournit des informations sur le fournisseur sélectionné par l’utilisateur.

   - [`setMetadataStatus(metadata, key, arguments)`](#$setMetadataStatus)

      - Déclenché par `getMetadata().`
      - La variable `metadata` fournit les données spécifiques demandées ; le paramètre `key` est la clé utilisée dans la variable `getMetadata()` et la demande `arguments` est le même dictionnaire que celui qui a été transmis à `getMetadata()`.

   - [`preauthorizedResources(resources)`](#$preauthResources)

      - Déclenché par `checkPreauthorizedResources()`.
      - La variable `authorizedResources` présente les ressources que l’utilisateur est autorisé à afficher.


![](assets/android-entitlement-flows.png)


### B. Flux de démarrage {#startup_flow}

1. Démarrez l’application de niveau supérieur.
1. Lancer l’authentification Adobe Pass.

   1. Appeler [`getInstance`](#$getInstance) pour créer une instance unique de l’Adobe Pass Authentication AccessEnabler.

      - **Dépendance :** Bibliothèque Adobe Pass Authentication Native Amazon FireOS (AccessEnabler)

   1. Appeler` setRequestor()` pour établir l&#39;identification du programmeur ; transmettez la variable `requestorID` et (éventuellement) un tableau de points de terminaison d’authentification Adobe Pass.

      - **Dépendance :** Identifiant de demandeur d’authentification Adobe Pass valide (demandez à votre gestionnaire de compte d’authentification Adobe Pass d’organiser cela.)

      - **Triggers :** rappel setRequestorComplete()

   Aucune demande de droit ne peut être effectuée tant que l’identité du demandeur n’a pas été entièrement établie. Cela signifie effectivement que, pendant que setRequestor() est toujours en cours d’exécution, toutes les demandes de droits suivantes (par exemple,`checkAuthentication()`) sont bloquées.

   Vous disposez de deux options d’implémentation : une fois que les informations d’identification du demandeur sont envoyées au serveur principal, la couche d’application de l’interface utilisateur peut choisir l’une des deux approches suivantes :</p>

   1. Attendez le déclenchement de la fonction `setRequestorComplete()` callback (fait partie du délégué AccessEnabler).  Cette option permet de déterminer avec la plus grande certitude que `setRequestor()` terminé. Il est donc recommandé pour la plupart des mises en oeuvre.
   1. Continuez sans attendre le déclenchement de la fonction `setRequestorComplete()` et commencez à émettre des demandes de droits. Ces appels (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sont placés en file d’attente par la bibliothèque AccessEnabler, qui effectuera les appels réseau réels après l’expiration de la variable `setRequestor()`. Cette option peut parfois être perturbée si, par exemple, la connexion réseau est instable.

1. Appeler [checkAuthentication()](#$checkAuthN) pour rechercher une authentification existante sans initialiser le flux d’authentification complet.  Si cet appel réussit, vous pouvez passer directement au flux d’autorisation.  Si ce n’est pas le cas, passez au flux d’authentification.

- **Dépendance :** Un appel réussi à `setRequestor()` (cette dépendance s’applique également à tous les appels suivants).

- **Triggers :** rappel setAuthenticationStatus()

### C. Flux d’authentification {#authn_flow}

1. Appeler [`getAuthentication()`](#$getAuthN) pour lancer le flux d’authentification ou pour obtenir la confirmation que l’utilisateur est déjà authentifié.

   **Triggers :**

   - Rappel setAuthenticationStatus() , si l’utilisateur est déjà authentifié.  Dans ce cas, accédez directement à la [Flux d’autorisation](#authz_flow).
   - Rappel displayProviderDialog() , si l’utilisateur n’est pas encore authentifié.

1. Présenter à l’utilisateur la liste des fournisseurs envoyés à `displayProviderDialog()`.
1. Une fois que l’utilisateur sélectionne un fournisseur, WebView ouvre la page du fournisseur pour permettre à l’utilisateur de se connecter.

   >[!NOTE]
   >
   >À ce stade, l’utilisateur a la possibilité d’annuler le flux d’authentification. Si cela se produit, AccessEnabler nettoie son état interne et réinitialise le flux d’authentification.

1. Une fois la connexion établie de l’utilisateur, WebView se ferme.
1. appel `getAuthenticationToken(),` qui demande à AccessEnabler de récupérer le jeton d’authentification du serveur principal.
1. [Facultatif] Appeler [`checkPreauthorizedResources(resources)`](#$checkPreauth) pour vérifier les ressources que l’utilisateur est autorisé à afficher. La variable `resources` est un tableau de ressources protégées associées au jeton d’authentification de l’utilisateur.

   **Triggers :** `preAuthorizedResources()` callback\
   **Point d&#39;exécution :** Une fois le flux d’authentification terminé

1. Si l’authentification a réussi, passez au flux d’autorisation.


### D. Flux d’autorisation {#authz_flow}

1. Appeler [`getAuthorization()`](#$getAuthZ) pour lancer le flux d’autorisation.

   Dépendance : ID de ressource valide convenu avec le ou les MVPD.

   **Remarque :** Les ResourceID doivent être identiques à ceux utilisés sur d’autres appareils ou plateformes et seront identiques pour tous les MVPD.

1. Validez l’authentification et l’autorisation.

   - Si la variable `getAuthorization()` L’appel réussit : l’utilisateur dispose de jetons AuthN et AuthZ valides (l’utilisateur est authentifié et autorisé à regarder le média demandé).
   - If `getAuthorization()` échec : examinez l’exception générée pour déterminer son type (AuthN, AuthZ, etc.) :
      - S’il s’agissait d’une erreur d’authentification (AuthN), redémarrez le flux d’authentification.
      - S’il s’agissait d’une erreur d’autorisation (AuthZ), l’utilisateur n’est pas autorisé à regarder le média demandé et un message d’erreur de ce type doit s’afficher à l’utilisateur.
      - En cas d&#39;erreur d&#39;un autre type (erreur de connexion, erreur réseau, etc.) puis afficher un message d’erreur approprié à l’utilisateur.

1. Validez le jeton de média court.

   Utilisez la bibliothèque du vérificateur de jeton multimédia d’authentification Adobe Pass pour vérifier le jeton multimédia de courte durée renvoyé par le `getAuthorization()` Appel ci-dessus :

   - Si la validation réussit : lisez le média demandé pour l’utilisateur.
   - Si la validation échoue : le jeton AuthZ n’était pas valide, la demande de média doit être refusée et un message d’erreur doit s’afficher à l’utilisateur.

1. Revenez à votre flux d’applications normal.

### E. Afficher le flux multimédia {#media_flow}

1. L’utilisateur sélectionne le média à afficher.
1. Les médias sont-ils protégés ?  Votre application vérifie si le média sélectionné est protégé :
   - Si le média sélectionné est protégé, votre application démarre la fonction [Flux d’autorisation](#authz_flow) ci-dessus.
   - Si le média sélectionné n’est pas protégé, lisez-le pour l’utilisateur.

### F. Flux de déconnexion {#logout_flow}

1. Appeler [`logout()`](#$logout) pour déconnecter l’utilisateur. AccessEnabler efface toutes les valeurs et tous les jetons mis en cache obtenus par l’utilisateur pour le MVPD actuel sur tous les demandeurs partageant la connexion par l’authentification unique. Après avoir vidé le cache, AccessEnabler effectue un appel au serveur pour nettoyer les sessions côté serveur.  Puisque l’appel au serveur peut entraîner une redirection SAML vers l’IdP (cela permet le nettoyage de session du côté IdP), cet appel doit suivre toutes les redirections. C’est pourquoi cet appel est géré dans un contrôle WebView, invisible pour l’utilisateur.

   **Remarque :** Le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir de quelque manière que ce soit avec WebView. Il est donc possible (et recommandé) de rendre le contrôle WebView invisible (c’est-à-dire masqué) pendant le processus de déconnexion.
