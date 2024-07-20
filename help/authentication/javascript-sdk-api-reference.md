---
title: Référence de l’API du SDK JavaScript
description: Référence de l’API du SDK JavaScript
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 4eb5fc1eb1eea4c5e27bbee298db4b9b4ba2daef
workflow-type: tm+mt
source-wordcount: '2860'
ht-degree: 0%

---

# Référence de l’API du SDK JavaScript {#javascript-sdk-api-reference}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Référence d’API {#api-reference}

Ces fonctions déclenchent des demandes d’interaction avec un MVPD. Tous les appels sont asynchrones ; vous devez implémenter des [rappels](#callbacks) pour gérer les réponses :

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorizedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor (inRequestorID, endpoints, options){#setrequestor(inRequestorID,endpoints,options)}

**Description :** Identifie le site d’où proviennent les demandes.  Vous devez effectuer cet appel avant tout autre appel API dans une session de communication.

**Paramètres :**

- *inRequestorID* - Identifiant unique qui a été attribué à l’Adobe sur le site d’origine lors de l’enregistrement.

- *endpoints* - Ce paramètre est facultatif. Il peut s’agir de l’une des valeurs suivantes :

   - Tableau qui vous permet de spécifier des points de terminaison pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Si plusieurs URL sont fournies, la liste MVPD est composée des points de terminaison de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD. Par défaut (si aucune valeur n’est spécifiée), le fournisseur de services Adobe est utilisé (<http://sp.auth.adobe.com/>).

  Exemple :
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *options* - Objet JSON contenant la valeur de l’ID d’application, les paramètres sans actualisation de la valeur de l’identifiant visiteur (déconnexion de l’arrière-plan) et les paramètres MVPD (iFrame). Toutes les valeurs sont facultatives.
   1. S’il est spécifié, le visitorID Experience Cloud est reporté sur tous les appels réseau effectués par la bibliothèque. La valeur peut être utilisée ultérieurement pour les rapports d’analyse avancés.
   2. Si l’identifiant unique de l’application est spécifié -`applicationId` - la valeur sera ajoutée à tous les appels suivants effectués par l’application dans le cadre de l’en-tête HTTP X-Device-Info. Cette valeur peut être récupérée ultérieurement à partir des rapports [ESM](/help/authentication/entitlement-service-monitoring-overview.md) à l’aide de la requête appropriée.

  **Remarque :** Toutes les clés JSON sont sensibles à la casse.

  Exemple :

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- Le programmeur peut remplacer les paramètres MVPD configurés dans l’authentification Adobe Pass, en spécifiant si un iFrame est requis ou non pour la connexion (*clé iFrameRequired*) et les dimensions iFrame (*iFrameWidth* et *clés iFrameHeight*). L’objet JSON comporte le modèle suivant :

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


Toutes les clés de niveau supérieur du modèle ci-dessus sont facultatives et possèdent des valeurs par défaut (*backgroundLogin*, *backgroundLogut* sont false par défaut et mvpdConfig est null, ce qui signifie qu’aucun paramètre MVPD n’est remplacé).


- **Remarque** : La spécification de valeurs/types non valides pour les paramètres ci-dessus entraînera un comportement indéfini.



Voici un exemple de configuration pour le scénario suivant : activation de la connexion et de la déconnexion sans actualisation, changement de MVPD1 en connexion pleine page-redirection (non-iFrame) et MVPD2 en connexion iFrame avec width=500 et height=300 :

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

[Haut de page](#top)

</br>

## getAuthorization(inResourceID, redirect_url) {#getauthorization(inresourceid,redirect_url)}

**Description :** Demande l’autorisation pour la ressource spécifiée. Chaque fois qu’un client tente d’accéder à une ressource autorisable, appelez cette fonction pour obtenir un jeton d’autorisation de courte durée à partir de l’activateur d’accès. Les ID de ressource sont acceptés par le MVPD qui accorde l’autorisation.

Utilise le jeton d’authentification mis en cache pour le client actuel. Si aucun jeton de ce type n’est trouvé, lance d’abord le processus d’authentification, puis continue avec autorisation.

**Paramètres :**

- `inResourceID` - Identifiant de la ressource pour laquelle l’utilisateur demande l’autorisation.
- `redirect_url` - Vous pouvez éventuellement fournir une URL de redirection, de sorte que le processus d’autorisation du MVPD renvoie l’utilisateur sur cette page plutôt que sur la page à partir de laquelle l’autorisation a été lancée.


**Rappels déclenchés :** [ setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) en cas de succès, [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) en cas d’échec

>[!CAUTION]
>
>Dans la mesure du possible, utilisez checkAuthorization() au lieu de getAuthorization() . La méthode getAuthorization() démarre un flux d’authentification complet (si l’utilisateur n’est pas authentifié), ce qui peut entraîner une mise en oeuvre compliquée du côté du programmeur.

</b>

[Haut de page](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**Description :** Demande l’authentification pour le client actuel. Généralement appelé en réponse à un clic sur un bouton Connexion . Vérifie un jeton d’authentification mis en cache pour le client actuel. Si aucun jeton de ce type n’est trouvé, lance le processus d’authentification. Cela appelle la boîte de dialogue de sélection de fournisseur par défaut ou personnalisée, puis utilise le fournisseur sélectionné pour rediriger vers l’interface de connexion du MVPD.

En cas de succès, crée et stocke un jeton d’authentification pour l’utilisateur. Si l&#39;authentification échoue, le fournisseur renvoie un message d&#39;erreur approprié à votre rappel [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode).

**Paramètres :**

- redirect_url : éventuellement fournir une URL de redirection, de sorte que le processus d’authentification du MVPD renvoie l’utilisateur vers cette page plutôt que la page à partir de laquelle l’authentification a été lancée.

**Rappels déclenchés :** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Haut de page](#top)

</br>

## checkAuthN {#checkauthn}

**Description :** Vérifie l’état d’authentification actuel du client actuel.  Non associé à une interface utilisateur.

**Rappels déclenchés :** [setAuthentcationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[Haut de page](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**Description :** Cette méthode est utilisée par l’application pour vérifier l’état d’autorisation du client actuel et de la ressource donnée. Il commence par vérifier l’état d’authentification. Si elle n’est pas authentifiée, le rappel tokenRequestFailed() est déclenché et la méthode se ferme. Si l’utilisateur est authentifié, il déclenche également le flux d’autorisation. Voir les détails sur la méthode [getAuthorization()](#getAuthZ).

>[!TIP]
>
> **Utilisation des fonctions check-status** Vous n’avez pas besoin de vérifier l’état de l’authentification ou de l’autorisation avant de demander l’autorisation. Vous pouvez appeler ces fonctions, par exemple, pour mettre à jour votre propre affichage d’état. Ne les utilisez pas lorsque vous avez besoin d’une interaction utilisateur.

**Paramètres :**

- `inResourceID` - Identifiant de la ressource pour laquelle l’utilisateur demande l’autorisation.


**Rappels déclenchés :**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources(resources) {#checkPreauthorizedResources(resources)}

**Description :** Demande l’état d’autorisation &quot;preflight&quot; pour une liste
ressources.

**Paramètres :**

- *resources* : le paramètre resources est un tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’ID de ressource. L’ID de ressource est soumis aux mêmes limites que l’ID de ressource dans l’appel `getAuthorization()`, c’est-à-dire qu’il s’agit d’une valeur convenue établie entre le programmeur et le MVPD, ou un fragment RSS multimédia.

</br>

## checkPreauthorizedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

Cette variante d’API est disponible à partir de la version 4.0 du SDK JS


**Paramètres :**

- *resources* : le paramètre resources est un tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’ID de ressource. L’ID de ressource est soumis aux mêmes limites que l’ID de ressource dans l’appel `getAuthorization()`, c’est-à-dire qu’il s’agit d’une valeur convenue établie entre le programmeur et le MVPD, ou un fragment RSS multimédia.

- *cache* : si vous souhaitez utiliser le cache interne lors de la vérification des ressources préautorisées. Il s’agit d’un paramètre facultatif, défini par défaut sur **true**. Si la valeur est true, le comportement est identique à l’API ci-dessus, ce qui signifie que les appels suivants à cette fonction utiliseront un cache interne pour résoudre la ressource préautorisée. La transmission de **false** pour ce paramètre désactive le cache interne, ce qui entraîne un appel au serveur chaque fois que l’API **checkPreauthorizedResources** est appelée.

**Rappels déclenchés :** [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[Retour vers le haut](#top)
</br>

## getMetadata(Key) {#getMetadata}

**Description :** Récupère des informations exposées sous forme de métadonnées par la bibliothèque Access Enabler.

Il existe deux types de métadonnées :

- **Static** (jeton d’authentification TTL, jeton d’autorisation TTL et ID d’appareil)
- **Métadonnées utilisateur** (Cela inclut les informations spécifiques à l’utilisateur transmises du MVPD à l’appareil de l’utilisateur pendant les flux d’authentification et/ou d’autorisation)

**Plus d’informations :** [Métadonnées utilisateur](#UserMetadata)

**Paramètres :**

- *key* : identifiant qui spécifie les métadonnées demandées :
   - Si la clé est `"TTL_AUTHN",`, la requête est effectuée pour obtenir le délai d’expiration du jeton d’authentification.

   - Si la clé est `"TTL_AUTHZ"` et que params est un tableau contenant l’ID de ressource sous la forme d’une chaîne, la requête est effectuée pour obtenir le délai d’expiration du jeton d’autorisation associé à la ressource spécifiée.

   - Si la clé est `"DEVICEID"`, la requête est effectuée pour obtenir l’identifiant de l’appareil actuel. Notez que cette fonctionnalité est désactivée par défaut et que les programmeurs doivent contacter Adobe pour plus d’informations sur l’activation et les frais.

   - Si la clé figure dans la liste suivante des types de métadonnées utilisateur, un objet JSON contenant les métadonnées utilisateur correspondantes est envoyé à la fonction de rappel [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) :

   - `"zip"` - Code postal

   - `"encryptedZip"` - Code postal chiffré

   - `"householdID"` - Identifiant du foyer. Dans le cas où un MVPD ne prend pas en charge les sous-comptes, cela est identique à userID.

   - `"maxRating"` - Note parentale maximale pour l’utilisateur

   - `"userID"` - Identifiant de l’utilisateur. Dans le cas où un MVPD prend en charge des sous-comptes et que l’utilisateur n’est pas le compte principal, userID est différent de householdID.

   - `"channelID"` - La liste des canaux que l’utilisateur est autorisé à afficher

   - `"is_hoh"` - Indicateur qui identifie si un utilisateur est chef de famille

   - `"encryptedZip"` - Code postal chiffré

   - `"typeID"` - Indicateur qui identifie si le compte utilisateur est un compte principal/secondaire

   - `"primaryOID"` - Identifiant du foyer

   - `"postalCode"` - Similaire au code postal

   - `"acctID"` - ID de compte

   - `"acctParentID"` - ID parent du compte

  **Remarque** : les métadonnées utilisateur réelles disponibles pour un programmeur dépendent de ce qu’un MVPD rend disponible.  Voir [Métadonnées utilisateur](#UserMetadata) pour obtenir la liste actuelle des métadonnées utilisateur disponibles.


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

[Haut de page](#top)

</br>


## setSelectedProvider(providerid) {#setSelectedProvider}

**Description :** Appelez cette fonction lorsque l’utilisateur a sélectionné un MVPD depuis votre interface utilisateur de sélection du fournisseur afin d’envoyer la sélection du fournisseur à l’activateur d’accès ou appelez cette fonction avec un paramètre null si l’utilisateur a ignoré votre interface utilisateur de sélection du fournisseur sans sélectionner de fournisseur.

**Rappels
déclenchée :**[ setAuthentcationStatus()](#setauthenticationstatusisauthenticated-errorcode), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Haut de page](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**Description :** Récupère les résultats de la sélection du client dans la boîte de dialogue de sélection du fournisseur. Vous pouvez l’utiliser à tout moment après la vérification initiale de l’authentification.

Cette fonction est asynchrone et renvoie son résultat à votre fonction de rappel `selectedProvider()`.

- **MVPD** MVPD actuellement sélectionné ou valeur nulle si aucun MVPD n’a été sélectionné.
- **AE_State** Le résultat de l’authentification pour le client actuel est &quot;Nouvel utilisateur&quot;, &quot;Utilisateur non authentifié&quot; ou &quot;Utilisateur authentifié&quot;.

**Rappels déclenchés :** [selectedProvider()](#getselectedprovider-getselectedprovider)

</br>

[Haut de page](#top)

</br>

## déconnexion {#logout}

**Description :** Déconnecte le client actuel, en effaçant toutes les informations d’authentification et d’autorisation pour cet utilisateur. Supprime tous les jetons authN et authZ du système du client.

**Rappels déclenchés :** [setAuthentcationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[Haut de page](#top)

</br>

## Définition de rappel {#calllback-definitions}

Vous devez mettre en oeuvre ces rappels pour gérer les réponses à vos appels de requête asynchrones :

- [rightsLoaded()](#entitlementloaded-entitlementloaded)
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

## rightsLoaded() {#entitlementLoaded}

**Description :** déclenché lorsque l’activation d’accès a terminé l’initialisation et est prêt à recevoir des requêtes. Mettez en oeuvre ce rappel pour savoir quand commencer la communication avec l’API Access Enabler.
</br>

[Haut de page](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**Description :** Mettez en oeuvre ce rappel pour recevoir les informations de configuration et la liste MVPD.

**Paramètres :**

- *configXML* : objet xml contenant la configuration de la REQUESTOR actuelle, y compris la liste MVPD.


**Déclenché par :** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[Haut de page](#top)

</br>

## displayProviderDialog(provider) {#displayproviderdialog(providers)}

**Description :** Mettez en oeuvre ce rappel pour appeler votre propre interface utilisateur personnalisée de sélection de fournisseur. Votre boîte de dialogue doit utiliser le nom d’affichage (et le logo facultatif) pour fournir les choix du client. Lorsque le client a fait un choix et a ignoré la boîte de dialogue, envoyez l’identifiant associé du fournisseur choisi dans l’appel à *setSelectedProvider()*.

**Paramètres :**

- *provider* - Un tableau d’objets représentant les MVPD demandés :

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**Déclenchée par :** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[Retour vers le haut](#top)

</br>

## createIFrame(inWidth, inHeight) {#createIFrame(inWidth,inHeight)}

**Description :** Mettez en oeuvre ce rappel si l’utilisateur a sélectionné un MVPD qui nécessite un iFrame dans lequel afficher l’interface utilisateur de sa page de connexion d’authentification.

**Déclenchée par :**[ setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [Retour vers le haut](#top)

</br>

## setAuthenticationStatus(isAuthenticated, errorCode) {#set-authn-status-isauthn-error}

**Description :** Mettez en oeuvre ce rappel pour recevoir l’état d’authentification (1=authentifié ou 0=non authentifié) et un message d’erreur descriptif si une erreur s’est produite lors de la tentative de détermination de l’état d’authentification (chaîne vide une fois la vérification terminée).

>[!NOTE]
> 
>Si vous utilisez le système actuel [Rapport d’erreurs avancé](/help/authentication/error-reporting.md), vous pouvez ignorer le paramètre errorCode envoyé à cette fonction.  Toutefois, les indicateurs isAuthenticated restent à utiliser pour le suivi de l’état d’authentification d’un utilisateur dans le flux de droits.


**Paramètres :**

- *isAuthenticated* - Fournit l’état d’authentification : 1 (authentifié) ou 0 (non authentifié).
- *errorCode* - Toute erreur survenue lors de la détermination de l’état d’authentification. Chaîne vide, le cas échéant.


**Déclenché par :** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[Haut de page](#top)

</br>

## sendTrackingData(trackingEventType, trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>Le type d’appareil et le système d’exploitation sont dérivés de l’utilisation d’une bibliothèque Java publique (<http://java.net/projects/user-agent-utils>) et de la chaîne de l’agent utilisateur. Notez que ces informations ne sont fournies que pour ventiler les mesures opérationnelles en catégories d’appareils, mais que cet Adobe ne peut pas assumer la responsabilité de résultats incorrects. Utilisez la nouvelle fonctionnalité en conséquence.

**Description :** Mettez en oeuvre ce rappel pour recevoir des données de suivi lorsque des événements spécifiques se produisent. Vous pouvez l’utiliser, par exemple, pour effectuer le suivi du nombre d’utilisateurs qui se sont connectés avec les mêmes informations d’identification. Le suivi n’est actuellement pas configurable. Avec l’authentification Adobe Pass 1.6, `sendTrackingData()` consigne également des informations sur l’appareil, le client Access Enabler et le type de système d’exploitation. Le rappel `sendTrackingData()` reste rétrocompatible.

- Valeurs possibles pour le type d’appareil :
   - machine
   - tablette
   - mobile
   - gameconsole
   - unknown

- Valeurs possibles pour le type de client Access Enabler :
   - html5
   - ios
   - android


Transmet le type d’événement et un tableau d’informations associées. Les types d’événement sont les suivants :

| mvpdSelection | L’utilisateur a sélectionné un MVPD dans une boîte de dialogue de sélection de fournisseur. |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | Une vérification de l’authentification est terminée. |
| authorizationDetection | Une demande d’autorisation est terminée. |

</br>
Les données sont spécifiques à chaque type d’événement :
</br>

| Type d’événement (chaîne) | Données (tableau) |
|:--- | :--- |
| mvpdSelection | 0 : MVPD sélectionné |
|  | 1 : Type de périphérique |
|  | 2 : Accès au type de client d’activation |
|  | 3 : Système d’exploitation |
| authenticationDetection | 0 : si la requête de jeton a réussi (true/false) |
|  | 1 : identifiant MVPD |
|  | 2 : GUID |
|  | 3 : Jeton déjà en cache (true/false) |
|  | 4 : Type de périphérique |
|  | 5. Accès au type de client d’activation |
|  | 6 : système d’exploitation |
| authorizationDetection | 0 : si la requête de jeton a réussi (true/false) |
|  | 1 : identifiant MVPD |
|  | 2 : GUID |
|  | 3 : Jeton déjà en cache (true/false) |
|  | 4 : Erreur |
|  | 5 : Détails |
|  | 6 : Type de périphérique |
|  | 7 : Accès au type de client d’activation |
|  | 8 : Système d’exploitation |


**Déclenché par :** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[Haut de page](#top)

</br>

## setToken(inRequestedResourceID, inToken) {#setToken(inRequestedResourceID,inToken)}

**Description :** Mettez en oeuvre ce rappel pour recevoir le jeton multimédia de courte durée (inToken) et l’identifiant de la ressource (inRequestedResourceID) pour laquelle une demande d’autorisation ou de vérification d’autorisation a été effectuée et a abouti.

**Déclenchée par :** [checkAuthorization()](#checkAuthZ), [getAuthorization()](#getAuthZ)
</br>

[Haut de page](#top)

</br>

## tokenRequestFailed(inRequestedResourceID, inRequestErrorCode, inRequestDetailsErrorMessage) {#token-request-failed-error-msg}

**Description :** Mettez en oeuvre ce rappel pour qu’il soit signalé lorsqu’une demande d’autorisation ou de vérification d’autorisation a échoué. Peut éventuellement être utilisé par un MVPD pour fournir un message personnalisé à afficher par le programmeur.

>[!IMPORTANT]
>
>Cette fonction de rappel fait partie de l’ancien système de création de rapports d’erreurs d’authentification Adobe Pass. Elle est conservée à des fins de compatibilité descendante, mais il n’est pas du tout nécessaire d’utiliser cette fonction si vous avez implémenté vos propres rappels à l’aide du système de création de rapports d’erreurs avancé actuel. Le nouveau système de reporting des erreurs fournit des informations plus détaillées sur les raisons de l’échec d’une autorisation (ou d’une autre opération), ainsi que des suggestions d’actions pour chaque type d’erreur ou d’avertissement.

**Paramètres :**

- *inRequestedResourceID* - Chaîne fournissant l’ID de ressource utilisé dans la demande d’autorisation.
- *inRequestErrorCode* - Chaîne qui affiche le code d’erreur d’authentification Adobe Pass, indiquant la raison de l’échec ; les valeurs possibles sont &quot;Erreur de non-authentification de l’utilisateur&quot; et &quot;Erreur de non-autorisation de l’utilisateur&quot; ; pour plus d’informations, voir &quot;Codes d’erreur de rappel&quot; ci-dessous.
- *inRequestDetailsErrorMessage* - Chaîne descriptive supplémentaire adaptée à l’affichage. Si cette chaîne descriptive n’est disponible pour aucune raison, l’authentification Adobe Pass envoie une chaîne vide **(&quot;&quot;)**.  Il peut être utilisé par un MVPD pour transmettre des messages d’erreur personnalisés ou des messages liés aux ventes. Par exemple, si un abonné se voit refuser l’autorisation d’une ressource, le MVPD peut répondre avec un `*inRequestDetailedErrorMessage*` tel que : **&quot;Vous n’avez actuellement pas accès à ce canal dans votre package. Si vous souhaitez mettre à niveau votre package, cliquez sur \*ici\*.&quot;** Le message est transmis par l’authentification Adobe Pass via ce rappel au site du programmeur. Le programmeur a alors la possibilité de l&#39;afficher ou de l&#39;ignorer. L’authentification Adobe Pass peut également utiliser `*inRequestDetailedErrorMessage*` pour informer le programmeur de la condition qui a pu entraîner une erreur. Par exemple, **&quot;Une erreur réseau s&#39;est produite lors de la communication avec le service d&#39;autorisation du fournisseur&quot;.**



**Déclenchée par :** [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[Haut de page](#top)

</br>


## preauthorizedResources(authorizedResources) {#preauthorizedResources(authorizedResources)}

**Description :** Rappel déclenché par l’activateur d’accès qui fournit la liste des ressources autorisées renvoyée après un appel à `checkPreauthorizedResources()`.

**Paramètres :**

- *authorizedResources* : liste des ressources autorisées.

**Déclenchée par :** [checkPreauthorizedResources()](#checkPreauthRes)
</br>

[Haut de page](#top)

</br>

## setMetadataStatus(key, encrypted, data) {#setMetadataStatus(key,encrypted,data)}

**Description :** Rappel déclenché par l’activateur d’accès qui diffuse les métadonnées demandées via un appel `getMetadata()`.

**Plus d’informations :** [Métadonnées utilisateur](#userMetadata)

**Paramètres :**

- *key (String)* : clé des métadonnées pour lesquelles la demande a été effectuée.
- *encrypted (Boolean)* : indicateur signifiant si la &quot;valeur&quot; est chiffrée ou non. Si la valeur est &quot;true&quot;, alors &quot;value&quot; sera en fait une représentation JSON Web Encrypted de la valeur réelle.
- *data (objet JSON)* : un objet JSON avec la représentation des métadonnées. Pour les requêtes simples (&#39;`TTL_AUTHN`&#39;, &#39;`TTL_AUTHZ`&#39;, &#39;`DEVICEID`&#39;), le résultat est une chaîne (représentant le TTL d’authentification, le TTL d’autorisation ou l’ID de périphérique). Dans le cas d’une requête de métadonnées utilisateur, le résultat peut être un objet JSON ou primitif représentant la charge utile de métadonnées. La structure réelle des objets de métadonnées utilisateur JSON est similaire à celle-ci :

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
[Haut de page](#top)

</br>

## selectedProvider(result) {#selectedProvider(result)}

**Description :** Mettez en oeuvre ce rappel pour recevoir le MVPD actuellement sélectionné et le résultat de l’authentification de l’utilisateur actuel encapsulé dans le paramètre `result`. Le paramètre `result` est un objet avec les propriétés suivantes :

- **MVPD** MVPD actuellement sélectionné ou valeur nulle si aucun MVPD n’a été sélectionné.
- **AEM\_State** Résultat de l’authentification pour l’utilisateur actuel, un de &quot;Nouvel utilisateur&quot;, &quot;Utilisateur non authentifié&quot; ou &quot;Utilisateur authentifié&quot;

**Déclenché par :** [getSelectedProvider()](#getSelProv)

</br>

[Haut de page](#top)

</br>

### Codes d’erreur de rappel {#callback-error-codes}

| Erreurs génériques | |
|:--- | :--- | 
| Erreur interne | Une erreur système s’est produite lors de la tentative de traitement de la requête. |
| Erreur du fournisseur non sélectionné | Se produit lorsque le client annule dans la boîte de dialogue de sélection du fournisseur |
| Erreur du fournisseur non disponible | Se produit lorsqu’aucun fournisseur n’est disponible. |

| Erreurs d’authentification | |
|:--- | :--- | 
| Erreur d’authentification générique | Renvoyée lorsque la raison n’est pas connue ou ne peut pas être publiée. |
| Erreur d’authentification interne | Une erreur système s’est produite lors de la tentative d’authentification. |
| Erreur de non-authentification de l’utilisateur | L’utilisateur n’est pas authentifié. |
| Erreur de demandes d’authentification multiples | D’autres demandes d’authentification ont été reçues avant que la première ne soit terminée. |

| Erreurs d’autorisation | |
|:--- | :--- | 
| Erreur d’autorisation générique | Renvoyée lorsque la raison n’est pas connue ou ne peut pas être publiée. |
| Erreur d’autorisation interne | Une erreur système s’est produite lors de la tentative d’autorisation. |
| Erreur de l’utilisateur non autorisé | Le client n’est pas autorisé à afficher le contenu demandé. |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[Haut de page](#top)
