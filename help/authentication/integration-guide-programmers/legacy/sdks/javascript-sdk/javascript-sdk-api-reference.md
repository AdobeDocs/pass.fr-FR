---
title: Référence de l’API JavaScript SDK
description: Référence de l’API JavaScript SDK
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2883'
ht-degree: 0%

---

# Référence de l’API SDK JavaScript (héritée) {#javascript-sdk-api-reference}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Référence d’API {#api-reference}

Ces fonctions lancent des demandes d’interaction avec un MVPD. Tous les appels sont asynchrones ; vous devez implémenter des [rappels](#callbacks) pour gérer les réponses :

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorizedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor (inRequestorID, points d’entrée, options){#setrequestor(inRequestorID,endpoints,options)}

**Description :** identifie le site d’où proviennent les requêtes.  Vous devez effectuer cet appel avant tout autre appel API dans une session de communication.

**Paramètres:**

- *inRequestorID* - Identifiant unique attribué par Adobe au site d’origine lors de l’enregistrement.

- *endpoints* - Ce paramètre est facultatif. Il peut s’agir de l’une des valeurs suivantes :

   - Un tableau qui vous permet de spécifier des points d’entrée pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Si plusieurs URL sont fournies, la liste MVPD est composée des points d’entrée de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD. Par défaut (si aucune valeur n’est spécifiée), le fournisseur Adobe est utilisé (<http://sp.auth.adobe.com/>).

  Exemple :
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *options* - Objet JSON contenant la valeur de l’ID d’application, la valeur de l’ID de visiteur sans actualisation (déconnexion en arrière-plan) et les paramètres MVPD (iFrame). Toutes les valeurs sont facultatives.
   1. S’il est spécifié, l’identifiant visiteur Experience Cloud est signalé sur tous les appels réseau effectués par la bibliothèque. La valeur peut être utilisée ultérieurement pour les rapports d’analyse avancée.
   2. Si l’identifiant unique de l’application est spécifié -`applicationId` - la valeur sera ajoutée à tous les appels suivants effectués par l’application dans le cadre de l’en-tête HTTP X-Device-Info. Cette valeur peut être récupérée ultérieurement à partir des rapports [ESM](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) à l’aide de la requête appropriée.

  **Remarque :** toutes les clés JSON sont sensibles à la casse.

  Exemple :

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- Le programmeur peut remplacer les paramètres MVPD configurés dans l’authentification Adobe Pass, en spécifiant si un iFrame est requis ou non pour la connexion (clé *iFrameRequired*) et les dimensions iFrame (clés *iFrameWidth* et *iFrameHeight*). L’objet JSON possède le modèle suivant :

```JSON
    {  
       "visitorID": <string>,
       "backgroundLogin": <boolean>,
       "backgroundLogout": <boolean>,
       "mvpdConfig":{  
          "MVPD_ID_1":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          },
          ...
          "MVPD_ID_N":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          }
       }
    }
```


Toutes les clés de niveau supérieur dans le modèle ci-dessus sont facultatives et ont des valeurs par défaut (*backgroundLogin*, *backgroundLogut* sont false par défaut et mvpdConfig est null, ce qui signifie qu’aucun paramètre MVPD n’est remplacé).


- **Remarque** : la spécification de valeurs/types non valides pour les paramètres ci-dessus entraîne un comportement non défini.



Voici un exemple de configuration pour le scénario suivant : activation de la connexion et de la déconnexion sans actualisation, changement de MVPD1 en connexion de redirection complète de page (hors iFrame) et MVPD2 en connexion iFrame avec une largeur=500 et une hauteur=300 :

```JSON
    {  
       "backgroundLogin": true,
       "backgroundLogout": true,
       "mvpdConfig":{  
          "MVPD1":{  
             "iFrameRequired": false
          },
          "MVPD2":{  
             "iFrameRequired": true,
             "iFrameWidth": 500,
             "iFrameHeight": 300
          }
       }
    }
```


**Rappels déclenchés :** [setConfig()](#setconfigconfigxml-setconfigconfigxml)
</br>

[Haut De La Page](#top)

</br>

## getAuthorization(inResourceID, redirect_url) {#getauthorization(inresourceid,redirect_url)}

**Description :** demande l’autorisation pour la ressource spécifiée. Chaque fois qu’un client ou une cliente tente d’accéder à une ressource autorisable, appelez cette fonction pour obtenir un jeton d’autorisation de courte durée auprès de l’activateur d’accès. Les identifiants de ressource sont convenus avec le MVPD qui fournit l’autorisation.

Utilise le jeton d’authentification mis en cache pour le client actuel. Si aucun jeton de ce type n’est trouvé, commence d’abord le processus d’authentification, puis continue avec l’autorisation.

**Paramètres:**

- `inResourceID` - Identifiant de la ressource pour laquelle l’utilisateur demande une autorisation.
- `redirect_url` - Fournissez éventuellement une URL de redirection, de sorte que le processus d’autorisation de MVPD renvoie l’utilisateur à cette page plutôt qu’à la page à partir de laquelle l’autorisation a été lancée.


**Rappels déclenchés :** [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) en cas de succès, [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) en cas d’échec

>[!CAUTION]
>
>Utilisez checkAuthorization() au lieu de getAuthorization() dans la mesure du possible. La méthode getAuthorization() lance un flux d&#39;authentification complet (si l&#39;utilisateur n&#39;est pas authentifié), ce qui peut entraîner une implémentation compliquée du côté du programmeur.

</b>

[Haut De La Page](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**Description :** demande l’authentification pour le client actuel. Généralement appelé en réponse à un clic sur un bouton de connexion. Recherche un jeton d’authentification mis en cache pour le client actuel. Si aucun jeton de ce type n’est trouvé, lance le processus d’authentification. Cela appelle la boîte de dialogue de sélection du fournisseur par défaut ou personnalisée, puis utilise le fournisseur sélectionné pour le rediriger vers l’interface de connexion de MVPD.

En cas de réussite, crée et stocke un jeton d’authentification pour l’utilisateur. Si l’authentification échoue, le fournisseur renvoie un message d’erreur approprié à votre rappel [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode).

**Paramètres:**

- redirect_url - Fournissez éventuellement une URL de redirection, de sorte que le processus d’authentification MVPD renvoie l’utilisateur à cette page plutôt qu’à la page à partir de laquelle l’authentification a été lancée.

**Rappels déclenchés :** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Haut De La Page](#top)

</br>

## checkAuthN {#checkauthn}

**Description :** vérifie le statut d’authentification actuel du client actuel.  Non associé à une interface utilisateur.

**Rappels déclenchés :** [setAuthentcationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[Haut De La Page](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**Description :** cette méthode est utilisée par l’application pour vérifier le statut d’autorisation du client actuel et de la ressource donnée. Il commence par vérifier d’abord le statut de l’authentification. Si elle n’est pas authentifiée, le rappel tokenRequestFailed() est déclenché et la méthode se ferme. Si l’utilisateur est authentifié, cela déclenche également le flux d’autorisation. Voir les détails sur la méthode [getAuthorization()]&#x200B;(#getAuthZ.

>[!TIP]
>
> **À l’aide des fonctions de vérification de statut** il n’est pas nécessaire de vérifier le statut de l’authentification ou de l’autorisation avant de demander l’autorisation. Vous pouvez appeler ces fonctions, par exemple, pour mettre à jour votre propre affichage d’état. Ne les utilisez pas lorsque vous avez besoin d’une interaction utilisateur.

**Paramètres:**

- `inResourceID` - Identifiant de la ressource pour laquelle l’utilisateur demande une autorisation.


**Rappels déclenchés :**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources(resources) {#checkPreauthorizedResources(resources)}

**Description :** demande le statut d’autorisation de « contrôle en amont » pour une liste de
ressources.

**Paramètres:**

- *resources* : le paramètre resources est un tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’identifiant de la ressource. L’ID de ressource est soumis aux mêmes limitations que l’ID de ressource dans l’appel `getAuthorization()`, c’est-à-dire qu’il s’agit d’une valeur convenue entre le programmeur et le MVPD, ou d’un fragment RSS de média.

</br>

## checkPreauthorizedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

Cette variante d’API est disponible à partir de la version 4.0 de JS SDK


**Paramètres:**

- *resources* : le paramètre resources est un tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’identifiant de la ressource. L’ID de ressource est soumis aux mêmes limitations que l’ID de ressource dans l’appel `getAuthorization()`, c’est-à-dire qu’il s’agit d’une valeur convenue entre le programmeur et le MVPD, ou d’un fragment RSS de média.

- *cache* : permet de spécifier si le cache interne doit être utilisé lors de la recherche de ressources préautorisées. Ce paramètre est facultatif, il est défini par défaut sur **true**. Si le paramètre est true, le comportement est identique à celui de l’API ci-dessus, ce qui signifie que les appels suivants à cette fonction utiliseront un cache interne pour résoudre la ressource préautorisée. La transmission de **false** pour ce paramètre désactive le cache interne, ce qui entraîne un appel au serveur chaque fois que l’API **checkPreauthorizedResources** est appelée.

**Rappels déclenchés :** [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[Haut De La Page](#top)
</br>

## getMetadata(Key) {#getMetadata}

**Description :** récupère les informations exposées en tant que métadonnées par la bibliothèque Access Enabler.

Il existe deux types de métadonnées :

- **Statique** (TTL de jeton d’authentification, TTL de jeton d’autorisation et ID d’appareil)
- **Métadonnées utilisateur** (cela inclut les informations spécifiques à l’utilisateur ou à l’utilisatrice transmises par le MVPD à l’appareil de l’utilisateur ou de l’utilisatrice pendant les flux d’authentification et/ou d’autorisation)

**Informations supplémentaires :** [Métadonnées utilisateur](#UserMetadata)

**Paramètres:**

- *key* : identifiant qui spécifie les métadonnées demandées :
   - Si la clé est `"TTL_AUTHN",`, la requête est effectuée pour obtenir le délai d’expiration du jeton d’authentification.

   - Si la clé est `"TTL_AUTHZ"` et que params est un tableau contenant l’ID de ressource sous la forme d’une chaîne, la requête est exécutée pour obtenir le délai d’expiration du jeton d’autorisation associé à la ressource spécifiée.

   - Si la clé est `"DEVICEID"`, la requête est effectuée pour obtenir l’identifiant d’appareil actuel. Notez que cette fonctionnalité est désactivée par défaut et les programmeurs doivent contacter Adobe pour plus d’informations sur l’activation et les frais.

   - Si la clé figure dans la liste suivante des types de métadonnées utilisateur, un objet JSON contenant les métadonnées utilisateur correspondantes est envoyé à la fonction de rappel [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) :

   - `"zip"` - Code postal

   - `"encryptedZip"` - Code Postal Chiffré

   - `"householdID"` - Identifiant du ménage. Dans le cas où un MVPD ne prend pas en charge les sous-comptes, celui-ci est identique à l’ID utilisateur.

   - `"maxRating"` - Évaluation parentale maximale pour l&#39;utilisateur

   - `"userID"` - Identifiant de l’utilisateur. Si un MVPD prend en charge les sous-comptes et que l’utilisateur n’est pas le compte principal, userID sera différent de householdID.

   - `"channelID"` - Liste des canaux que l’utilisateur est autorisé à afficher

   - `"is_hoh"` - Indicateur qui identifie si un utilisateur est le chef de ménage

   - `"encryptedZip"` - Code postal chiffré

   - `"typeID"` - Indicateur identifiant si le compte utilisateur est un compte principal/secondaire

   - `"primaryOID"` - Identifiant du ménage

   - `"postalCode"` - Similaire au code postal

   - `"acctID"` - ID de compte

   - `"acctParentID"` - ID parent du compte

  **Remarque** : les métadonnées d’utilisateur réellement disponibles pour un programmeur dépendent de ce qu’un MVPD rend disponible.  Consultez [Métadonnées utilisateur](#UserMetadata) pour obtenir la liste actuelle des métadonnées utilisateur disponibles.


Par exemple :

```JSON
    // Assume that a reference to the AccessEnabler has been previously 
    // obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
        if (status ==  1) {
            //user is authenticated, request metadata
            ae.getMetadata("zip");
            ae.getMetadata("maxRating");
        } else {
            ...
      }
    }
```


**Rappels déclenchés :** [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[Haut De La Page](#top)

</br>


## setSelectedProvider(providerid) {#setSelectedProvider}

**Description :** appelez cette fonction lorsque l’utilisateur a sélectionné un MVPD dans l’interface utilisateur de sélection de fournisseur afin d’envoyer la sélection de fournisseur à Access Enabler ou appelez cette fonction avec un paramètre null au cas où l’utilisateur aurait ignoré l’interface utilisateur de sélection de fournisseur sans sélectionner de fournisseur.

**Rappels
triggered:**[&#x200B; setAuthentcationStatus()](#setauthenticationstatusisauthenticated-errorcode), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Haut De La Page](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**Description :** récupère les résultats de la sélection du client dans la boîte de dialogue de sélection du fournisseur. Vous pouvez l’utiliser à tout moment après la vérification de l’authentification initiale.

Cette fonction est asynchrone et renvoie son résultat à votre fonction de rappel `selectedProvider()`.

- **MVPD** MVPD actuellement sélectionné ou valeur nulle si aucun MVPD n’a été sélectionné.
- **AE_State** résultat de l’authentification pour le client actuel : « Nouvel utilisateur », « Utilisateur non authentifié » ou « Utilisateur authentifié »

**Rappels déclenchés :** [selectedProvider()](#getselectedprovider-getselectedprovider)

</br>

[Haut De La Page](#top)

</br>

## déconnexion {#logout}

**Description :** déconnecte le client actuel, effaçant toutes les informations d’authentification et d’autorisation de cet utilisateur. Supprime tous les jetons authN et authZ du système du client.

**Rappels déclenchés :** [setAuthentcationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[Haut De La Page](#top)

</br>

## Définition du rappel {#calllback-definitions}

Vous devez implémenter ces rappels pour gérer les réponses à vos appels de requête asynchrones :

- [eligibilityLoaded()](#entitlementloaded-entitlementloaded)
- [setConfig()](#setconfigconfigxml-setconfigconfigxml)
- [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)
- [createIFrame()](#createiframe-createiframeinwidthminheight)
- [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
- [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)
- [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)
- [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)
- [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)
- [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)
- [selectedProvider()](#selectedproviderresult-selectedprovider)

</br>

## eligibilityLoaded() {#entitlementLoaded}

**Description :** déclenché lorsque l’activateur d’accès a terminé l’initialisation et est prêt à recevoir des requêtes. Mettez en œuvre ce rappel pour savoir quand vous pouvez commencer la communication avec l’API Access Enabler.
</br>

[Haut de la page](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**Description :** implémentez ce rappel pour recevoir les informations de configuration et la liste MVPD.

**Paramètres:**

- *configXML* : objet xml contenant la configuration du DEMANDEUR actif, y compris la liste MVPD.


**Déclenché par :** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[Haut de la page](#top)

</br>

## displayProviderDialog(providers) {#displayproviderdialog(providers)}

**Description :** implémentez ce rappel pour appeler votre propre interface utilisateur personnalisée de sélection de fournisseur. Votre boîte de dialogue doit utiliser le nom d’affichage (et le logo facultatif) pour indiquer les choix du client ou de la cliente. Lorsque le client a fait un choix et a ignoré la boîte de dialogue, envoyez l’identifiant associé au fournisseur choisi dans l’appel à *setSelectedProvider()*.

**Paramètres:**

- *providers* - Tableau d’objets représentant les fichiers MVPD demandés :

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**Déclenché par :** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[Haut de la page](#top)

</br>

## createIFrame(inWidth, inHeight) {#createIFrame(inWidth,inHeight)}

**Description :** implémentez ce rappel si l’utilisateur a sélectionné un MVPD qui nécessite un iFrame pour afficher l’interface utilisateur de sa page de connexion d’authentification.

**Déclenché par :**&#x200B;[&#x200B; setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [Haut de la page](#top)

</br>

## setAuthenticationStatus(isAuthenticated, errorCode) {#set-authn-status-isauthn-error}

**Description :** implémentez ce rappel pour recevoir le statut d’authentification (1=authentifié ou 0=non authentifié) et un message d’erreur descriptif si une erreur s’est produite lors de la tentative de détermination du statut d’authentification (chaîne vide à la fin de la vérification).

>[!NOTE]
> 
>Si vous utilisez le système actuel, [Rapports d’erreur avancés](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md), vous pouvez ignorer le paramètre errorCode envoyé à cette fonction.  Toutefois, l’indicateur isAuthenticated est toujours utile pour suivre le statut d’authentification d’un utilisateur dans le flux des droits


**Paramètres:**

- *isAuthenticated* - Fournit le statut d’authentification : 1 (authentifié) ou 0 (non authentifié).
- *errorCode* - Toute erreur survenue lors de la détermination du statut d’authentification. Chaîne vide si aucune.


**Déclenché par :** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[Haut de la page](#top)

</br>

## sendTrackingData(trackingEventType, trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>Le type d’appareil et le système d’exploitation sont dérivés à l’aide d’une bibliothèque Java publique (<http://java.net/projects/user-agent-utils>) et de la chaîne de l’agent utilisateur. Notez que ces informations ne sont fournies qu’à titre indicatif pour ventiler les mesures opérationnelles en catégories d’appareils, mais qu’Adobe ne peut assumer aucune responsabilité pour les résultats incorrects. Veuillez utiliser la nouvelle fonctionnalité en conséquence.

**Description :** implémentez ce rappel pour recevoir les données de suivi lorsque des événements spécifiques se produisent. Vous pouvez l’utiliser, par exemple, pour suivre le nombre d’utilisateurs qui se sont connectés avec les mêmes informations d’identification. Le tracking n’est actuellement pas configurable. Avec Adobe Pass Authentication 1.6, `sendTrackingData()` signale également les informations sur l’appareil, le client Access Enabler et le type de système d’exploitation. Le rappel `sendTrackingData()` reste rétrocompatible.

- Valeurs possibles pour le type d’appareil :
   - ordinateur
   - tablette
   - mobile
   - console de jeux
   - inconnu

- Valeurs possibles pour le type de client Access Enabler :
   - html5
   - ios
   - android


Transmet le type d’événement et un tableau d’informations associées. Les types d’événements sont les suivants :

| mvpdSelection | L’utilisateur a sélectionné un MVPD dans une boîte de dialogue de sélection de fournisseur. |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | Une vérification d’authentification est terminée. |
| authorizationDetection | Une demande d’autorisation est terminée. |

</br>
Les données sont spécifiques à chaque type d’événement :
</br>

| Type d&#39;événement (chaîne) | Données (tableau) |
|:--- | :--- |
| mvpdSelection | 0 : MVPD sélectionné |
|  | 1 : type d’appareil |
|  | 2 : type de client Access Enabler |
|  | 3 : SYSTÈME D’EXPLOITATION |
| authenticationDetection | 0 : si la demande de jeton a réussi (true/false) |
|  | 1 : MVPD ID |
|  | 2 : GUID |
|  | 3 : jeton déjà présent dans le cache (true/false) |
|  | 4 : type d’appareil |
|  | 5 : Type de client Access Enabler |
|  | 6 : SYSTÈME D’EXPLOITATION |
| authorizationDetection | 0 : si la demande de jeton a réussi (true/false) |
|  | 1 : MVPD ID |
|  | 2 : GUID |
|  | 3 : jeton déjà présent dans le cache (true/false) |
|  | 4 : erreur |
|  | 5 : Détails |
|  | 6 : type d’appareil |
|  | 7 : Type de client Access Enabler |
|  | 8 : SYSTÈME D’EXPLOITATION |


**Déclenché par :** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[Haut de la page](#top)

</br>

## setToken(inRequestedResourceID, inToken) {#setToken(inRequestedResourceID,inToken)}

**Description :** implémentez ce rappel pour recevoir le jeton de média de courte durée (inToken) et l’identifiant de la ressource (inRequestedResourceID) pour laquelle une demande d’autorisation ou une demande de vérification d’autorisation a été effectuée et s’est terminée avec succès.

**Déclenché par :** [checkAuthorization()](#checkAuthZ), [getAuthorization()](#getAuthZ)
</br>

[Haut de la page](#top)

</br>

## tokenRequestFailed(inRequestedResourceID, inRequestErrorCode, inRequestDetailedErrorMessage) {#token-request-failed-error-msg}

**Description :** implémentez ce rappel pour qu’il soit signalé lorsqu’une demande d’autorisation ou de vérification d’autorisation a échoué. Peut éventuellement être utilisé par un MVPD pour fournir un message personnalisé à afficher par le programmeur.

>[!IMPORTANT]
>
>Cette fonction de rappel fait partie de l’ancien système de rapports d’erreurs d’authentification d’Adobe Pass d’origine. Elle est conservée à des fins de rétrocompatibilité, mais il n’est pas nécessaire d’utiliser cette fonction si vous avez implémenté vos propres rappels à l’aide du système avancé de rapports d’erreurs actuel. Le nouveau système de rapport d’erreur fournit des informations plus détaillées sur les raisons de l’échec d’une autorisation (ou d’une autre opération), ainsi que des pistes d’action suggérées pour chaque type d’erreur ou d’avertissement.

**Paramètres:**

- *inRequestedResourceID* - Chaîne fournissant l’identifiant de ressource utilisé dans la demande d’autorisation.
- *inRequestErrorCode* - Chaîne qui affiche le code d’erreur d’authentification Adobe Pass, indiquant la raison de l’échec. Les valeurs possibles sont « Erreur d’utilisateur non authentifié » et « Erreur d’utilisateur non autorisé ». Pour plus d’informations, consultez la section « Codes d’erreur de rappel » ci-dessous.
- *inRequestDetailedErrorMessage* - Chaîne descriptive supplémentaire pouvant être affichée. Si cette chaîne descriptive n’est disponible pour aucune raison, l’authentification Adobe Pass envoie une chaîne vide **(«  »)**.  Il peut être utilisé par un MVPD pour transmettre des messages d’erreur personnalisés ou des messages liés aux ventes. Par exemple, si l’autorisation d’accès à une ressource est refusée à un abonné, le MVPD peut répondre avec une `*inRequestDetailedErrorMessage*` telle que : **« Vous n’avez actuellement pas accès à ce canal dans votre package. Si vous souhaitez mettre à niveau votre package, cliquez sur \*here\*. »** Le message est transmis par l’authentification Adobe Pass via ce rappel au site du programmeur. Le programmeur a alors la possibilité de l’afficher ou de l’ignorer. L’authentification Adobe Pass peut également utiliser `*inRequestDetailedErrorMessage*` pour informer le programmeur de la condition qui a pu entraîner une erreur. Par exemple, **« Une erreur réseau s’est produite lors de la communication avec le service d’autorisation du fournisseur ».**



**Déclenché par :** [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[Haut de la page](#top)

</br>


## preauthorizedResources(authorizedResources) {#preauthorizedResources(authorizedResources)}

**Description :** rappel déclenché par Access Enabler qui fournit la liste des ressources autorisées renvoyée après un appel à `checkPreauthorizedResources()`.

**Paramètres:**

- *authorizedResources* : liste des ressources autorisées.

**Déclenché par :** [checkPreauthorizedResources()](#checkPreauthRes)
</br>

[Haut de la page](#top)

</br>

## setMetadataStatus(key, encrypted, data) {#setMetadataStatus(key,encrypted,data)}

**Description :** rappel déclenché par l’activateur d’accès qui fournit les métadonnées demandées par le biais d’un appel `getMetadata()`.

**Informations supplémentaires :** [Métadonnées utilisateur](#userMetadata)

**Paramètres:**

- *key (chaîne)* : clé des métadonnées pour lesquelles la requête a été effectuée.
- *chiffré (booléen)* : indicateur qui signale si la « valeur » est chiffrée ou non. Si cette valeur est définie sur « true », « value » est une représentation chiffrée par le Web JSON de la valeur réelle.
- *data (objet JSON)* : objet JSON avec la représentation des métadonnées. Pour les requêtes simples (&#39;`TTL_AUTHN`&#39;, &#39;`TTL_AUTHZ`&#39;, &#39;`DEVICEID`&#39;), le résultat est une chaîne (représentant la TTL d’authentification, la TTL d’autorisation ou l’ID d’appareil). Dans le cas d’une requête de métadonnées utilisateur, le résultat peut être un objet primitif ou JSON représentant la payload des métadonnées. La structure réelle des objets de métadonnées d’utilisateur JSON est similaire à ce qui suit :

```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
            zip: ["12345", "34567"],
            maxrating: { 
                "MPAA": "PG-13",
                "VCHIP": "TV-Y", 
                "URL": "http://exam.pl/e/manage/ratings"
            },
            householdid: "3456",
            uid: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
            channelID: ["channel-1", "channel-2"]
    }
```


Par exemple :

```JSON
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
        if (encrypted) {
            //the metadata value is encrypted
            //needs to be decrypted by the programmer
            data = decrypt(data);
        }
        alert(key + "=" + data);
    }
```


**Déclenché par :** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[Haut de la page](#top)

</br>

## selectedProvider(result) {#selectedProvider(result)}

**Description :** implémentez ce rappel pour recevoir le MVPD actuellement sélectionné et le résultat de l’authentification de l’utilisateur actuel encapsulé dans le paramètre `result`. Le paramètre `result` est un objet avec les propriétés suivantes :

- **MVPD** MVPD actuellement sélectionné ou valeur nulle si aucun MVPD n’a été sélectionné.
- **AE\_State** résultat de l’authentification pour l’utilisateur actuel : « Nouvel utilisateur », « Utilisateur non authentifié » ou « Utilisateur authentifié »

**Déclenché par :** [getSelectedProvider()](#getSelProv)

</br>

[Haut de la page](#top)

</br>

### Codes d’erreur de rappel {#callback-error-codes}

| Erreurs génériques | |
|:--- | :--- | 
| Erreur interne | Une erreur système s’est produite lors de la tentative de traitement de la demande. |
| Erreur de fournisseur non sélectionné | Se produit lorsque le client annule dans la boîte de dialogue de sélection du fournisseur |
| Erreur de fournisseur non disponible | Se produit lorsqu&#39;aucun fournisseur n&#39;est disponible. |

| Erreurs d’authentification | |
|:--- | :--- | 
| Erreur d’authentification générique | Renvoyé lorsque la raison n’est pas connue ou ne peut pas être publiée. |
| Erreur d’authentification interne | Une erreur système s’est produite lors de la tentative d’authentification. |
| Erreur Utilisateur non authentifié | L’utilisateur n’est pas authentifié. |
| Erreur de plusieurs demandes d’authentification | D’autres demandes d’authentification ont été reçues avant la fin de la première. |

| Erreurs d’autorisation | |
|:--- | :--- | 
| Erreur d’autorisation générique | Renvoyé lorsque la raison n’est pas connue ou ne peut pas être publiée. |
| Erreur d’autorisation interne | Une erreur système s’est produite lors de la tentative d’autorisation. |
| Erreur Utilisateur non autorisé | Le client n’est pas autorisé à consulter le contenu demandé. |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[Haut De La Page](#top)
