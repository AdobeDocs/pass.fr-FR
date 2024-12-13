---
title: Manuel d’intégration d’Amazon FireOS
description: Manuel d’intégration d’Amazon FireOS
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1447'
ht-degree: 0%

---

# Manuel d’intégration d’Amazon FireOS (hérité) {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>


## Introduction {#intro}

Ce document décrit les workflows de droits qu’une application de niveau supérieur d’un programmeur peut implémenter via les API exposées par la bibliothèque `AccessEnabler` FireOS d’Amazon.

La solution Droits d’authentification Adobe Pass pour Amazon FireOS est finalement divisée en deux domaines :

- Le domaine de l’interface utilisateur : il s’agit de la couche d’application de niveau supérieur qui implémente l’interface utilisateur et utilise les services fournis par la bibliothèque `AccessEnabler` pour fournir l’accès au contenu limité.
- Le domaine `AccessEnabler` où les workflows de droits sont implémentés sous la forme de :
   - Appels réseau effectués aux serveurs principaux d’Adobe
   - Règles de logique commerciale liées aux workflows d’authentification et d’autorisation
   - Gestion de diverses ressources et traitement de l’état des workflows (tel que le cache de jetons)

L’objectif du domaine `AccessEnabler` est de masquer toutes les complexités des workflows de droits et de fournir à l’application de couche supérieure (par le biais de la bibliothèque de `AccessEnabler`) un ensemble de primitives de droits simples. Ce processus permet de mettre en œuvre les workflows de droits :

1. Définissez l’identité du demandeur.
1. Vérifiez et obtenez une authentification auprès d’un fournisseur d’identité spécifique.
1. Vérifiez et obtenez l’autorisation pour une ressource spécifique.
1. Déconnexion.

L’activité réseau de l’`AccessEnabler` se déroule dans un autre thread, de sorte que le thread de l’interface utilisateur n’est jamais bloqué. Par conséquent, le canal de communication bidirectionnel entre les deux domaines d’application doit suivre un modèle entièrement asynchrone :

- La couche d’application de l’interface utilisateur envoie des messages au domaine `AccessEnabler` via les appels d’API exposés par la bibliothèque `AccessEnabler`.
- Le `AccessEnabler` répond à la couche d’interface utilisateur par le biais des méthodes de rappel incluses dans le protocole `AccessEnabler` que la couche d’interface utilisateur enregistre auprès de la bibliothèque `AccessEnabler`.

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

      - Déclenché par `setRequestor()`, renvoie un succès ou un échec.     La réussite indique que vous pouvez poursuivre les appels de droits.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - Déclenché par `getAuthentication()` uniquement si l’utilisateur n’a pas sélectionné de fournisseur (MVPD) et n’est pas encore authentifié. Le paramètre `mvpds` est un tableau de fournisseurs disponibles pour l’utilisateur.

   - [`setAuthenticationStatus(status, reason)`](#$setAuthNStatus)

      - Déclenché par `checkAuthentication()` à chaque fois. Déclenché par `getAuthentication()` uniquement si l’utilisateur est déjà authentifié et a sélectionné un fournisseur.

      - Le statut renvoyé est authentifié ou non authentifié. La raison décrit un échec d’authentification ou une action de déconnexion.

   - [navigateToUrl(url)](#$navigateToUrl)

      - Ignorée dans AmazonFireOS SDK, la méthode est utilisée sur les plateformes Android où est déclenchée par `getAuthentication()` une fois que l’utilisateur a sélectionné un MVPD.  Le paramètre `url` indique l’emplacement de la page de connexion de MVPD.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

      - Déclenché par `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
Le paramètre `event` indique quel événement de droit s’est produit ; le paramètre `data` est une liste de valeurs relatives à l’événement.

   - [`setToken(token, resource)`](#$setToken)

      - Déclenché par `checkAuthorization()` et `getAuthorization()` après une autorisation réussie d’affichage d’une ressource.
      - Le paramètre `token` est le jeton de média de courte durée ; le paramètre `resource` est le contenu que l’utilisateur est autorisé à afficher.

   - [`tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

      - Déclenché par `checkAuthorization()` et `getAuthorization()` après une autorisation infructueuse.
      - Le paramètre `resource` correspond au contenu que l’utilisateur tentait d’afficher ; le paramètre `code` correspond au code d’erreur indiquant le type d’échec ; le paramètre `description` décrit l’erreur associée au code d’erreur.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

      - Déclenché par `getSelectedProvider()`.
      - Le paramètre `mvpd` fournit des informations sur le fournisseur sélectionné par l’utilisateur ou l’utilisatrice.

   - [`setMetadataStatus(metadata, key, arguments)`](#$setMetadataStatus)

      - Déclenché par `getMetadata().`
      - Le paramètre `metadata` fournit les données spécifiques que vous avez demandées ; le paramètre `key` est la clé utilisée dans la requête `getMetadata()` ; et le paramètre `arguments` est le même dictionnaire que celui transmis à `getMetadata()`.

   - [`preauthorizedResources(resources)`](#$preauthResources)

      - Déclenché par `checkPreauthorizedResources()`.
      - Le paramètre `authorizedResources` présente les ressources que l’utilisateur est autorisé à afficher.


![](../../../../assets/android-entitlement-flows.png)


### B. Flux de démarrage {#startup_flow}

1. Démarrez l’application de niveau supérieur.
1. Lancer l’authentification Adobe Pass.

   1. Appelez [`getInstance`](#$getInstance) pour créer une instance unique de l’`AccessEnabler` d’authentification Adobe Pass.

      - Adobe Pass **Dépendance :** Bibliothèque FireOS native d’authentification Amazon (`AccessEnabler`)

   1. Appelez ` setRequestor()` pour établir l’identité du programmeur ; transmettez le `requestorID` du programmeur et (éventuellement) un tableau de points d’entrée d’authentification Adobe Pass.

      - **Dépendance :** ID de demandeur d’authentification Adobe Pass valide (demandez l’aide de votre gestionnaire de compte d’authentification Adobe Pass).

      - Rappel **Triggers:** setRequestorComplete()

   Aucune demande de droit ne peut être traitée tant que l’identité du demandeur n’est pas entièrement établie. Cela signifie effectivement que, pendant que setRequestor() est toujours en cours d’exécution, toutes les demandes de droits suivantes (par exemple, `checkAuthentication()`) sont bloquées.

   Vous disposez de deux options de mise en œuvre : une fois les informations d’identification du demandeur envoyées au serveur principal, la couche d’application de l’interface utilisateur peut choisir l’une des deux approches suivantes :</p>

   1. Attendez le déclenchement du rappel `setRequestorComplete()` (partie du délégué `AccessEnabler`).  Cette option offre la plus grande certitude quant à la réalisation de l`setRequestor()`opération. Elle est donc recommandée pour la plupart des implémentations.
   1. Continuez sans attendre le déclenchement du rappel `setRequestorComplete()` et commencez à émettre des demandes de droits. Ces appels (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) sont mis en file d&#39;attente par la bibliothèque `AccessEnabler`, qui effectuera les appels réseau réels après la `setRequestor()`. Cette option peut parfois être interrompue si, par exemple, la connexion réseau est instable.

1. Appelez [checkAuthentication()](#$checkAuthN) pour vérifier une authentification existante sans initier le flux d’authentification complet.  Si cet appel réussit, vous pouvez passer directement au flux d’autorisation.  Dans le cas contraire, passez au flux d’authentification.

- **Dépendance :** un appel réussi à `setRequestor()` (cette dépendance s’applique également à tous les appels suivants).

- Rappel **Triggers:** setAuthenticationStatus()

### C. Flux d’authentification {#authn_flow}

1. Appelez [`getAuthentication()`](#$getAuthN) pour lancer le flux d’authentification ou pour obtenir la confirmation que l’utilisateur est déjà authentifié.

   **Triggers:**

   - Le rappel setAuthenticationStatus() , si l’utilisateur est déjà authentifié.  Dans ce cas, passez directement au [flux d’autorisation](#authz_flow).
   - Le rappel displayProviderDialog() , si l’utilisateur n’est pas encore authentifié.

1. Présentez à l&#39;utilisateur la liste des fournisseurs envoyés à `displayProviderDialog()`.
1. Une fois que l&#39;utilisateur a sélectionné un fournisseur, une vue Web ouvre la page du fournisseur pour permettre à l&#39;utilisateur de se connecter

   >[!NOTE]
   >
   >À ce stade, l’utilisateur a la possibilité d’annuler le flux d’authentification. Si cela se produit, le `AccessEnabler` nettoie son état interne et réinitialise le flux d’authentification.

1. Une fois la connexion établie par l&#39;utilisateur, WebView se ferme.
1. appelez `getAuthenticationToken(),` qui demande au `AccessEnabler` de récupérer le jeton d’authentification du serveur principal.
1. [Facultatif] Appelez [`checkPreauthorizedResources(resources)`](#$checkPreauth) pour vérifier quelles ressources l’utilisateur est autorisé à afficher. Le paramètre `resources` est un tableau de ressources protégées associé au jeton d’authentification de l’utilisateur.

   **Triggers:** rappel `preAuthorizedResources()`\
   **Point d’exécution :** après le flux d’authentification terminé

1. Si l’authentification a réussi, passez au flux d’autorisation.


### D. Flux d’autorisation {#authz_flow}

1. Appelez [`getAuthorization()`](#$getAuthZ) pour lancer le flux d’autorisation.

   Dépendance : identifiant(s) de ressource valide(s) convenu(s) avec le ou les MVPD.

   **Remarque :** ResourceID doivent être identiques à ceux utilisés sur d’autres appareils ou plateformes et seront identiques sur tous les MVPD.

1. Validez l’authentification et l’autorisation.

   - Si l’appel `getAuthorization()` réussit : l’utilisateur dispose de jetons AuthN et AuthZ valides (l’utilisateur est authentifié et autorisé à regarder le média demandé).
   - Si `getAuthorization()` échoue : examinez l’exception renvoyée pour déterminer son type (AuthN, AuthZ ou autre chose) :
      - S’il s’agissait d’une erreur d’authentification (AuthN), redémarrez le flux d’authentification.
      - S’il s’agissait d’une erreur d’autorisation (AuthZ), l’utilisateur n’est pas autorisé à regarder le média demandé et un message d’erreur doit s’afficher à l’intention de l’utilisateur.
      - S’il y a eu un autre type d’erreur (erreur de connexion, erreur réseau, etc.), affichez un message d’erreur approprié à l’intention de l’utilisateur.

1. Validez le jeton de média court.

   Utilisez la bibliothèque Vérificateur de jeton de média d’authentification Adobe Pass pour vérifier le jeton de média de courte durée renvoyé par l’appel `getAuthorization()` ci-dessus :

   - Si la validation réussit : lisez le média demandé pour l’utilisateur.
   - Si la validation échoue : le jeton AuthZ n’était pas valide, la requête de média doit être refusée et un message d’erreur doit s’afficher à l’intention de l’utilisateur.

1. Retournez à votre flux d’application normal.

### E. Afficher le flux multimédia {#media_flow}

1. L’utilisateur sélectionne le média à afficher.
1. Les médias sont-ils protégés ?  Votre application vérifie si le média sélectionné est protégé :
   - Si le média sélectionné est protégé, votre application lance le [flux d’autorisation](#authz_flow) ci-dessus.
   - Si le média sélectionné n’est pas protégé, lisez le média pour l’utilisateur.

### F. Flux de déconnexion {#logout_flow}

1. Appelez [`logout()`](#$logout) pour déconnecter l’utilisateur. Le `AccessEnabler` efface toutes les valeurs mises en cache et les jetons obtenus par l’utilisateur pour le MVPD actuel pour tous les demandeurs partageant la connexion via l’authentification unique. Après avoir effacé le cache, le `AccessEnabler` effectue un appel au serveur pour nettoyer les sessions côté serveur.  Notez que puisque l’appel au serveur peut entraîner une redirection SAML vers l’IdP (cela permet le nettoyage de la session du côté IdP), cet appel doit suivre toutes les redirections. Pour cette raison, cet appel sera géré à l&#39;intérieur d&#39;un contrôle WebView, invisible pour l&#39;utilisateur.

   **Remarque :** le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir avec le WebView de quelque manière que ce soit. Il est donc possible (et recommandé) de rendre le contrôle WebView invisible (c&#39;est-à-dire masqué) pendant le processus de déconnexion.
