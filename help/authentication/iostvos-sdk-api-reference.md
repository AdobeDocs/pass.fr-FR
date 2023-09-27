---
title: Référence de l’API iOS/tvOS
description: Référence de l’API iOS/tvOS
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '7010'
ht-degree: 0%

---

# Référence de l’API du SDK iOS/tvOS {#iostvos-sdk-api-reference}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

Cette page décrit les méthodes et les fonctions de rappel exposées par le client natif iOS/tvOS pour l’authentification Adobe Pass. Les méthodes et fonctions de rappel décrites ici sont définies dans la section `AccessEnabler.h` et `EntitlementDelegate.h` fichiers d’en-tête ; vous pouvez les trouver dans le SDK iOS AccessEnabler ici : `[SDK directory]/AccessEnabler/headers/api/`


Documentation associée :

* Pour une description du flux de droits d’authentification Adobe Pass de base, voir [Flux de droits](/help/authentication/entitlement-flow.md).
* Pour une présentation détaillée de l’implémentation du flux de droits d’authentification Adobe Pass à l’aide de cette API, voir la section [Guide d’intégration iOS](/help/authentication/iostvos-sdk-cookbook.md).
* Pour obtenir le dernier SDK iOS AccessEnabler, voir [Bibliothèque iOS Native Access Enabler](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe vous encourage à utiliser uniquement l’authentification Adobe Pass *public* API :
>
>* Les API publiques sont disponibles et entièrement testées sur tous les types de clients pris en charge. Pour toute fonction publique, nous nous assurons que chaque type de client possède une version correspondante de la ou des méthodes associées.
>* Les API publiques doivent être aussi stables que possible, pour prendre en charge la compatibilité descendante et garantir que les intégrations de partenaires ne se rompent pas. Cependant, pour les API non publiques, nous nous réservons le droit de modifier leur signature à tout moment futur. Si vous rencontrez un flux particulier qui ne peut pas être pris en charge par une combinaison des appels publics actuels de l’API d’authentification Adobe Pass, la meilleure approche consiste à nous le faire savoir. En tenant compte de vos besoins, nous pouvons modifier les API publiques et fournir une solution stable à l’avenir.

</br>

## Référence d’API {#apis}

* [init](#initWithSoftwareStatement):softwareStatement : instancie l’objet AccessEnabler.

* **[OBSOLÈTE]** [init](#init) - Instancie l’objet AccessEnabler.

* [setOptions:options:](#setOptions) - Configure les options du SDK global telles que profile ou visitorID.

* [setRequestor:](#setReqV3)[requestorID](#setReqV3),[setRequestor:requestorID:serviceProviders :](#setReqV3) - Établit l’identité du programmeur.

* **[OBSOLÈTE]** [setRequestor:signedRequestorId:](#setReq),[setRequestor:signedRequestorId:serviceProviders :](#setReq) - Établit l’identité du programmeur.

* **[OBSOLÈTE]** [setRequestor:signedRequestorId:secret:publicKey](#setReq_tvos), [setRequestor:signedRequestorId:serviceProviders:secret:publicKey](#setReq_tvos)- Établit l’identité du programmeur.

* [setRequestorComplete :](#setReqComplete) - Informe votre application que la phase de configuration est terminée.

* [checkAuthentication](#checkAuthN) - Vérifie l’état d’authentification de l’utilisateur actuel.

* [getAuthentication](#getAuthN), [getAuthentication:withData:](#getAuthN) - Démarre le workflow d’authentification complète.

* [getAuthentication:filter](#getAuthN_filter),[getAuthentication:withData:](#getAuthN)[andFilter](#getAuthN_filter) - Démarre le workflow d’authentification complète.

* [displayProviderDialog:](#dispProvDialog) - Informe votre application d’instancier les éléments d’IU appropriés pour que l’utilisateur puisse sélectionner un MVPD.

* [setSelectedProvider:](#setSelProv) - Informe le AccessEnabler de la sélection MVPD de l’utilisateur.

* [navigateToUrl :](#nav2url) - Informe votre application que l’utilisateur doit se voir présenter la page de connexion MVPD.

* [navigateToUrl:useSVC:](#nav2urlSVC) - Informe votre application que l’utilisateur doit se voir présenter la page de connexion MVPD, à l’aide de SFSafariViewController

* [handleExternalURL:url](#handleExternalURL) - Termine le flux d’authentification/de déconnexion.

* **[OBSOLÈTE]** [getAuthenticationToken](#getAuthNToken) - Demande le jeton d’authentification auprès du serveur principal.

* [setAuthenticationStatus:errorCode:](#setAuthNStatus) - Informe votre application de l’état du flux d’authentification.

* [checkPreauthorizedResources :](#checkPreauth) - Détermine si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques.

* [checkPreauthorizedResources:cache:](#checkPreauthCache) - Détermine si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques.

* [preauthorizedResources:](#preauthResources) - Fournit une liste des ressources que l’utilisateur est déjà autorisé à afficher.

* [checkAuthorization:](#checkAuthZ), [checkAuthorization:withData:](#checkAuthZ) - Vérifie l’état d’autorisation de l’utilisateur actuel.

* [getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ) - Lance le flux d’autorisation.

* [setToken:forResource:](#setToken) - Informe votre application que le flux d’autorisation a bien été terminé.

* [tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed) - Indique à votre application que le flux d’autorisation a échoué.

* [déconnexion](#logout) - Lance le flux de déconnexion.

* [getSelectedProvider](#getSelProv) - Détermine le fournisseur actuellement sélectionné.

* [selectedProvider :](#selProv) - Fournit des informations sur le MVPD actuellement sélectionné à votre application.

* [getMetadata :](#getMeta) - Récupère les informations exposées en tant que métadonnées par la bibliothèque AccessEnabler.

* [presentTvProviderDialog:](#presentTvDialog) - Informe votre application d’afficher la boîte de dialogue SSO Apple.

* [dismissTvProviderDialog:](#dismissTvDialog) - Informe votre application de masquer la boîte de dialogue SSO Apple.

* [setMetadataStatus:encrypted:forKey:andArguments:](#setMetaStatus) - Fournit les métadonnées demandées par un [getMetadata :](#getMeta) appelez .

* [sendTrackingData:forEventType:](#sendTracking) - Fournit des informations de suivi sur les données.

* [MVPD](#mvpd) - Classe MVPD. [Contient des informations sur le MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Instancie l’objet AccessEnabler. Il doit y avoir une seule instance AccessEnabler par instance d&#39;application.

| **Appel API : constructeur iOS AccessEnabler** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Disponibilité :** v3.0+

**Paramètres :**

* **softwareStatement :** Chaîne qui identifie l’application dans le système de l’Adobe. Découvrez comment obtenir un relevé logiciel.

[Retour au sommet...](#apis)



### init - [OBSOLÈTE]{#init}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Instancie l’objet AccessEnabler. Il doit y avoir une seule instance AccessEnabler par instance d&#39;application.

| Appel API : constructeur iOS AccessEnabler |
| --- |
| ```- (id) init;``` |

**Disponibilité :** v1.0+ **Jusqu’à :** v3.0

**Paramètres :** Aucun

[Retour au sommet...](#apis)

</br>

### setOptions:options {#setOptions}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Configure les options du SDK global. Il accepte un NSDictionary comme argument. Les valeurs du dictionnaire seront transmises au serveur avec chaque appel réseau effectué par le SDK.

**Remarque :** Les valeurs seront transmises au serveur indépendamment du flux actuel (authentification/autorisation). Si vous souhaitez modifier les valeurs, vous pouvez appeler cette méthode à tout moment.

| Appel API : setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Disponibilité :** v2.3.0+

**Paramètres :**

* *options*: NSDictionary contenant les options du SDK global. Actuellement, les options suivantes sont disponibles :
   * **applicationProfile** - Il peut être utilisé pour créer des configurations de serveur en fonction de cette valeur.
   * **visitorID** - Service d’ID d’Experience Cloud. Cette valeur peut être utilisée ultérieurement pour les rapports d’analyse avancés.
   * **handleSVC** - Valeur booléenne indiquant si le programmeur va gérer les SFSafariViewContrôers. Veuillez consulter [Prise en charge de SFSafariViewController sur le SDK iOS 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) pour plus d’informations.
      * Si la variable est définie sur **false,** Le SDK présente automatiquement à l’utilisateur final un SFSafariViewController. Le SDK accède ensuite à l’URL de la page de connexion des MVPD.
      * Si la variable est définie sur **true,** Le SDK **NOT** présente automatiquement à l’utilisateur final un SFSafariViewController. Le SDK se déclenche de manière plus détaillée. **navigate(toUrl:{url}, useSVC:YES)**.
* **device\_info** - Informations sur le client, comme décrit dans la section [Transmission des informations client](/help/authentication/passing-client-information-device-connection-and-application.md).

[Retour au sommet...](#apis)


### setRequestor:requestorID, setRequestor:requestorID:serviceProviders : {#setReqV3}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Établit l’identité du programmeur. Un identifiant unique est attribué à chaque programmeur lors de l’enregistrement auprès de l’Adobe pour le système d’authentification Adobe Pass. Lorsque vous traitez d’SSO et de jetons distants, l’état d’authentification peut changer lorsque l’application est en arrière-plan, setRequestor peut être appelé de nouveau lorsque l’application est mise en premier plan afin de se synchroniser avec l’état du système (récupération d’un jeton distant si SSO est activé ou suppression du jeton local si une déconnexion s’est produite entre-temps).

La réponse du serveur contient une liste de MVPD ainsi que certaines informations de configuration qui sont jointes à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code AccessEnabler. Seul l’état de l’opération (c.-à-d., SUCCESS/FAIL) est présenté à votre application via le `setRequestorComplete:` rappel.

Si la variable `urls` n’est pas utilisé, l’appel réseau obtenu cible l’URL du fournisseur de services par défaut : l’environnement Adobe RELEASE/production.


Si une valeur est fournie pour la variable `urls` , l’appel réseau obtenu cible toutes les URL fournies dans la variable `urls` . Toutes les requêtes de configuration sont déclenchées simultanément dans des threads distincts. Le premier participant a la priorité lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, AccessEnabler mémorise l&#39;URL du prestataire associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.

| Appel API : configuration du demandeur |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Disponibilité :** v3.0+

| Appel API : configuration du demandeur |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Disponibilité :** v3.0+

**Paramètres :**

* *requestorID*: identifiant unique associé au programmeur. Transmettez l’identifiant unique attribué par Adobe à votre site lorsque vous vous inscrivez pour la première fois auprès du service d’authentification Adobe Pass.
* *url*: paramètre facultatif ; par défaut, le fournisseur de services Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points de terminaison pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances du fournisseur de services d’authentification Adobe Pass. Dans ce cas, la liste MVPD est composée des points de terminaison de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.

>[!NOTE]
>
>Si appelé sans la variable `serviceProviders` , la bibliothèque récupérera la configuration auprès du fournisseur de services par défaut (c’est-à-dire, `https://sp.auth.adobe.com` pour le profil de production ou `https://sp.auth-staging.adobe.com` pour le profil d’évaluation). Si la variable `serviceProviders` est fourni, il doit s’agir d’un tableau d’URL. Les informations de configuration sont récupérées à partir de tous les points de terminaison spécifiés et sont fusionnées. Si des informations en double existent dans les réponses des différents fournisseurs de services, le conflit est résolu en faveur du serveur qui répond le plus rapidement (c’est-à-dire que le serveur avec le temps de réponse le plus court est prioritaire).

**Rappels déclenchés :** [`setRequestorComplete:`](#setReqComplete)

[Retour au sommet...](#apis)

</br>

### setRequestor:setSignedRequestorId:, setRequestor:setSignedRequestorId:serviceProviders : - [OBSOLÈTE] {#setReq}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Établit l’identité du programmeur. Un identifiant unique est attribué à chaque programmeur lors de l’enregistrement auprès de l’Adobe pour le système d’authentification Adobe Pass. Lorsque vous traitez de SSO et de jetons distants, l’état d’authentification peut changer lorsque l’application est en arrière-plan, setRequestor peut être appelé de nouveau lorsque l’application est mise en premier plan afin de se synchroniser avec l’état du système (récupération d’un jeton distant si SSO est activé ou suppression du jeton local si une déconnexion s’est produite entre-temps).

La réponse du serveur contient une liste de MVPD ainsi que certaines informations de configuration qui sont jointes à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code AccessEnabler. Seul l’état de l’opération (c.-à-d., SUCCESS/FAIL) est présenté à votre application via le `setRequestorComplete:` rappel.

Si la variable `urls` n’est pas utilisé, l’appel réseau obtenu cible l’URL du fournisseur de services par défaut : l’environnement Adobe RELEASE/production.

Si une valeur est fournie pour la variable `urls` , l’appel réseau obtenu cible toutes les URL fournies dans la variable `urls` . Toutes les requêtes de configuration sont déclenchées simultanément dans des threads distincts. Le premier participant a la priorité lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, AccessEnabler mémorise l&#39;URL du prestataire associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.

| Appel API : configuration du demandeur |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Disponibilité :** v1.0+ **Jusqu’à :** v3.0

| Appel API : configuration du demandeur |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Disponibilité :** v1.0+ **Jusqu’à :** v3.0

**Paramètres :**

* *requestorID*: identifiant unique associé au programmeur. Transmettez l’identifiant unique attribué par Adobe à votre site lorsque vous vous êtes enregistré pour la première fois auprès du service d’authentification Adobe Pass.
* *signedRequestorID*: **Ce paramètre existe dans iOS AccessEnabler versions 1.2 et ultérieures.** Une copie de l’ID du demandeur signé numériquement avec votre clé privée. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: paramètre facultatif ; par défaut, le fournisseur de services Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points de terminaison pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances du fournisseur de services d’authentification Adobe Pass. Dans ce cas, la liste MVPD est composée des points de terminaison de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.

**Remarques :** Si appelé sans la variable `serviceProviders` , la bibliothèque récupérera la configuration auprès du fournisseur de services par défaut (c’est-à-dire,`https://sp.auth.adobe.com` pour le profil de production ou `https://sp.auth-staging.adobe.com` pour le profil d’évaluation). Si la variable `serviceProviders` est fourni, il doit s’agir d’un tableau d’URL. Les informations de configuration sont récupérées à partir de tous les points de terminaison spécifiés et sont fusionnées. Si des informations en double existent dans les réponses des différents fournisseurs de services, le conflit est résolu en faveur du serveur qui répond le plus rapidement (c’est-à-dire que le serveur avec le temps de réponse le plus court est prioritaire).

**Rappels déclenchés :** [`setRequestorComplete:`](#setReqComplete)


[Retour au sommet...](#apis)

### setRequestor:setSignedRequestorId:secret:publicKey, setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey - [OBSOLÈTE] {#setReq_tvos}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Établit l’identité du programmeur. Un identifiant unique est attribué à chaque programmeur lors de l’enregistrement auprès de l’Adobe pour le système d’authentification Adobe Pass. Ce paramètre ne doit être exécuté qu’une seule fois pendant le cycle de vie de l’application.

La réponse du serveur contient une liste de MVPD ainsi que certaines informations de configuration qui sont jointes à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code AccessEnabler. Seul l’état de l’opération (c.-à-d., SUCCESS/FAIL) est présenté à votre application via le `setRequestorComplete:` rappel.

Si la variable `urls` n’est pas utilisé, l’appel réseau obtenu cible l’URL du fournisseur de services par défaut : l’environnement Adobe RELEASE/production.

Si une valeur est fournie pour la variable `urls` , l’appel réseau obtenu cible toutes les URL fournies dans la variable `urls` . Toutes les requêtes de configuration sont déclenchées simultanément dans des threads distincts. Le premier participant a la priorité lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, AccessEnabler mémorise l&#39;URL du prestataire associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : configuration du demandeur</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID
               secret:(NSString *)secret
            publicKey:(NSString *)publicKey;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilité :** v2.0+ **Jusqu’à :** v3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : configuration du demandeur</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID 
     serviceProviders:(NSArray *)urls</code></pre>
<p><code class="sourceCode objectivec">               secret:(NSString *)secret</code></p>
<p><code class="sourceCode objectivec">            publicKey:(NSString *)publicKey;</code></p></td>
</tr>
</tbody>
</table>

**Disponibilité :** v2.0+ **Jusqu’à :** v3.0

**Paramètres :**

* *requestorID*: identifiant unique associé au programmeur. Transmettez l’identifiant unique attribué par Adobe à votre site lorsque vous vous êtes enregistré pour la première fois auprès du service d’authentification Adobe Pass.
* *signedRequestorID*: **Ce paramètre existe dans iOS AccessEnabler versions 1.2 et ultérieures.** Une copie de l’ID du demandeur signé numériquement avec votre clé privée. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: paramètre facultatif ; par défaut, le fournisseur de services Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points de terminaison pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances du fournisseur de services d’authentification Adobe Pass. Dans ce cas, la liste MVPD est composée des points de terminaison de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.
* secret et publicKey : clé secrète et publique utilisée pour signer le deuxième appel d’écran. Pour plus d’informations, voir [Documentation sans client](#create_dev).

Si appelé sans la variable `serviceProviders` , la bibliothèque récupère la configuration auprès du fournisseur de services par défaut (c’est-à-dire, `https://sp.auth.adobe.com` pour le profil de production ou https://sp.auth-staging.adobe.com pour le profil intermédiaire). Si la variable `serviceProviders` est fourni, il doit s’agir d’un tableau d’URL. Les informations de configuration sont récupérées à partir de tous les points de terminaison spécifiés et sont fusionnées. Si des informations en double existent dans les réponses des différents fournisseurs de services, le conflit est résolu en faveur du serveur qui répond le plus rapidement (c’est-à-dire que le serveur avec le temps de réponse le plus court est prioritaire).

**Rappels déclenchés :** [`setRequestorComplete:`](#setReqComplete)

[Retour au sommet...](#apis)

</br>

### setRequestorComplete : {#setReqComplete}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui informe votre application que la phase de configuration est terminée. Il s’agit d’un signal indiquant que l’application peut commencer à émettre des demandes de droits. Aucune demande de droit ne peut être émise par l’application tant que la phase de configuration n’est pas terminée.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : fin de la configuration du demandeur</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilité :** v1.0+

**Paramètres**:

* *status*: peut prendre l’une des valeurs suivantes :
   * `ACCESS_ENABLER_STATUS_SUCCESS` - la phase de configuration a été terminée
   * `ACCESS_ENABLER_STATUS_ERROR` - échec de la phase de configuration

**Déclenché par :**
`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Retour au sommet...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Vérifie l’état d’authentification de l’utilisateur actuel.
Pour ce faire, il recherche un jeton d’authentification valide dans l’espace de stockage du jeton local. L’appel de cette méthode n’effectue aucun appel réseau. Il est utilisé par l’application pour interroger l’état d’authentification de l’utilisateur et mettre à jour l’interface utilisateur en conséquence (c’est-à-dire mettre à jour l’interface utilisateur de connexion/déconnexion). L&#39;état d&#39;authentification est communiqué à l&#39;application via le [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) rappel.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : vérification de l’état d’authentification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres :** Aucun

**Rappels déclenchés :**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Retour au sommet...](#apis)

</br>

### getAuthentication, getAuthentication:withData: {#getAuthN}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Démarre le workflow d’authentification complète. Il commence par vérifier l’état d’authentification. Si elle n’est pas déjà authentifiée, la machine à états du flux d’authentification est démarrée :

* si la dernière tentative d’authentification a réussi, la phase de sélection du MVPD est ignorée et la variable [`navigateToUrl:`](#nav2url) callback est déclenché. L’application utilise ce rappel pour instancier le contrôle WebView qui présente à l’utilisateur la page de connexion du MVPD. **[REMARQUE : À compter de la version 1.5 d’Access Enabler, cette fonctionnalité n’est plus disponible en raison d’une limitation du SDK.].**
* si la dernière tentative d’authentification a échoué ou si l’utilisateur s’est explicitement déconnecté, la variable [`displayProviderDialog:`](#dispProvDialog) callback est déclenché. Votre application utilise ce rappel pour afficher l’interface utilisateur de sélection MVPD. Votre application doit également reprendre le flux d’authentification en informant la bibliothèque AccessEnabler de la sélection du MVPD de l’utilisateur via l’ [`setSelectedProvider:`](#setSelProv) .

Comme les informations d’identification de l’utilisateur sont vérifiées sur la page de connexion MVPD, votre application est requise pour surveiller les multiples opérations de redirection qui ont lieu pendant que l’utilisateur s’authentifie sur la page de connexion du MVPD. Lorsque les informations d’identification correctes sont saisies, le contrôle WebView est redirigé vers une URL personnalisée définie par la variable `ADOBEPASS_REDIRECT_URL` constante. Cette URL n’est pas destinée à être chargée par le WebView. L’application doit intercepter cette URL et interpréter cet événement comme un signal indiquant que la phase de connexion est terminée. Il doit ensuite céder le contrôle à AccessEnabler pour terminer le flux d’authentification (en appelant la fonction [handleExternalURL](#handleExternalURL) ).

Enfin, l&#39;état d&#39;authentification est communiqué à l&#39;application via le [setAuthenticationStatus:errorCode:](#setAuthNStatus) rappel.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : initie le flux d’authentification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : initie le flux d’authentification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Disponibilité :** v1.9+

**Paramètres :**

* *forceAuthn*: indicateur qui spécifie si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
* *data*: dictionnaire composé de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** ` setAuthenticationStatus:errorCode:, `[`displayProviderDialog:`](#dispProvDialog)`,`` sendTrackingData:forEventType:`


[Retour au sommet...](#apis)

</br>

### getAuthentication:filter, getAuthentication:withData:andFilter {#getAuthN_filter}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Démarre le workflow d’authentification complète. Il commence par vérifier l’état d’authentification. Si elle n’est pas déjà authentifiée, la machine à états du flux d’authentification est démarrée :

* [presentTvProviderDialog()](#presentTvDialog) sera appelé si le demandeur actuel possède au moins un MVPD qui prend en charge l’authentification unique. Si aucun MVPD ne prend en charge la fonction SSO, le flux d’authentification classique commence et le paramètre de filtre est ignoré.
* Une fois que l’utilisateur a terminé le flux d’authentification unique Apple [dismissTvProviderDialog()](#dismissTvDialog) sera déclenché et le processus d’authentification sera terminé.

Enfin, l&#39;état d&#39;authentification est communiqué à l&#39;application via le [setAuthenticationStatus:errorCode:](#setAuthNStatus) rappel.

**Disponibilité :** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : initie le flux d’authentification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSDictionary *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : initie le flux d’authentification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSDictionary *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Paramètres :**

* *forceAuthn*: indicateur qui spécifie si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
* *data*: dictionnaire composé de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.
* filter : dictionnaire contenant deux listes d’identifiants MVPD qui doivent apparaître dans la boîte de dialogue SSO Apple. Tout MVPD qui ne prend pas en charge SSO sera ignoré, mais l’ordre sera respecté. Le dictionnaire doit comporter deux clés :
   * TV\_PROVIDERS : liste de tous les distributeurs multicanaux de programmes audiovisuels qui doivent apparaître dans le sélecteur
   * FEATURED\_TV\_PROVIDERS : liste de tous les MVPD qui doivent être marqués comme présentés dans le sélecteur. Les MVPD de cette liste doivent également être spécifiés dans la liste TV\_PROVIDERS .

**Disponibilité :** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : initie le flux d’authentification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSArray *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : initie le flux d’authentification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSArray *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Paramètres :**

* *forceAuthn*: indicateur qui spécifie si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
* *data*: dictionnaire composé de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.
* filter : liste des identifiants MVPD qui doivent apparaître dans la boîte de dialogue SSO d’Apple. Tout MVPD qui ne prend pas en charge SSO sera ignoré, mais l’ordre sera respecté.

**Rappels déclenchés :** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Retour au sommet...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler pour informer l’application que les éléments d’IU appropriés doivent être instanciés pour permettre à l’utilisateur de sélectionner le MVPD de votre choix. Le rappel fournit une liste d’objets MVPD avec des informations supplémentaires qui peuvent aider à créer correctement le panneau d’interface utilisateur de sélection (telles que l’URL pointant vers le logo du MVPD, le nom d’affichage convivial, etc.).

Une fois que l’utilisateur a sélectionné le MVPD souhaité, l’application de couche supérieure est requise pour reprendre le flux d’authentification en appelant `setSelectedProvider:` et lui transmettre l’identifiant du MVPD correspondant à la sélection de l’utilisateur.

**Abandon du flux d’authentification** - Il s’agit d’un point où l’utilisateur peut appuyer sur le bouton &quot;Retour&quot;, ce qui équivaut à interrompre le flux d’authentification. Dans ce scénario, votre application doit appeler la fonction [setSelectedProvider:](#setSelProv) , en transmettant null en tant que paramètre, pour donner à AccessEnabler la possibilité de réinitialiser son ordinateur d’état d’authentification.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : affichage de l’interface utilisateur de sélection MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilité :** v1.0+

**Paramètres**:

* *mvpds*: liste des objets MVPD contenant des informations relatives au MVPD que l’application peut utiliser pour créer les éléments d’IU de sélection du MVPD.

**Déclenché par :** ` getAuthentication, `[getAuthentication:withData:](#getAuthN),` getAuthorization:, `[getAuthorization:withData:](#getAuthZ)


[Retour au sommet...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est appelée par votre application pour informer le gestionnaire d’accès de la sélection MVPD de l’utilisateur. L’application peut utiliser cette méthode pour sélectionner ou modifier le fournisseur de services utilisé pour l’authentification.

Si le MVPD sélectionné est un MVPD TempPass , il s’authentifiera automatiquement avec ce MVPD sans avoir à appeler getAuthentication() par la suite.

Veuillez noter que ceci n’est pas possible pour la transmission temporaire de conversion où des paramètres supplémentaires sont donnés sur la méthode getAuthentication() .

Lorsque la variable *null* en tant que paramètre, Access Enabler suppose que l’utilisateur a annulé le flux d’authentification (c’est-à-dire qu’il a appuyé sur le bouton &quot;Précédent&quot;) et répond en réinitialisant la machine-état d’authentification et en appelant la fonction [setAuthenticationStatus:errorCode:](#setAuthNStatus) avec le rappel `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` code d’erreur.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : définir le fournisseur actuellement sélectionné</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres :** Aucun

**Rappels déclenchés :** ` setAuthenticationStatus:errorCode:,sendTrackingData:forEventType:,  `[`navigateToUrl:`](#nav2url)

[Retour au sommet...](#apis)

</br>

#### navigateToUrl : {#nav2url}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description :** Rappel déclenché par AccessEnabler pour demander à votre application d’instancier un contrôleur UIWebView/WKWebView et de charger l’URL fournie dans le **`url`** . Le rappel transmet la variable **`url`** qui représente l’URL du point de terminaison d’authentification ou celle du point de terminaison de la déconnexion.

Comme UIWebView/WKWebView` `Le contrôleur traverse plusieurs redirections. Votre application doit surveiller l’activité du contrôleur et détecter le moment où il charge une URL personnalisée spécifique définie par la fonction `ADOBEPASS_REDIRECT_URL `constante (c’est-à-dire `adobepass://ios.app`). Notez que cette URL personnalisée spécifique n’est en fait pas valide et qu’elle n’est pas destinée à être réellement chargée par le contrôleur. Elle ne doit être interprétée que par votre application comme un signal indiquant que le flux d’authentification ou de déconnexion est terminé et qu’il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer UIWebView/WKWebView et appeler AccessEnabler `handleExternalURL:url `méthode API.

**Remarque :** Dans le cas du flux d’authentification, il s’agit d’un point où l’utilisateur a la possibilité d’appuyer sur le bouton &quot;Retour&quot;, ce qui équivaut à interrompre le flux d’authentification. Dans ce cas, votre application doit appeler la fonction [setSelectedProvider:](#setSelProv) transmission de méthode **`nil`** comme paramètre et permettant à AccessEnabler de réinitialiser son ordinateur-état d’authentification.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : afficher la page de connexion MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres**:

* *url*: URL pointant vers la page de connexion du MVPD.

**Déclenché par :** [setSelectedProvider:](#setSelProv)



[Retour au sommet...](#apis)

</br>

#### navigateToUrl:useSVC: {#nav2urlSVC}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description :** Rappel déclenché par AccessEnabler au lieu de `navigateToUrl:` rappel au cas où votre application aurait auparavant activé la gestion manuelle du contrôleur de vue Safari (SVC) via la fonction [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) et uniquement dans le cas de MVPD nécessitant un contrôleur de vue Safari (SVC). Pour tous les autres MVPD, la variable `navigateToUrl:` callback sera appelé. Veuillez consulter[Prise en charge de SFSafariViewController sur le SDK iOS 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) pour plus d’informations sur la manière dont le contrôleur de vue Safari (SVC) doit être géré.

Semblable au `navigateToUrl:` rappel de `navigateToUrl:useSVC:` est déclenché par AccessEnabler pour demander à votre application d’instancier une `SFSafariViewController` et de charger l’URL fournie dans le rappel **`url`** . Le rappel transmet la variable **`url`** qui représente l’URL du point de terminaison d’authentification ou l’URL du point de terminaison de la déconnexion, et la variable **`useSVC`** qui spécifie que l’application doit utiliser une `SFSafariViewController`.

Comme la variable `SFSafariViewController` Le contrôleur passe par plusieurs redirections. Votre application doit surveiller l’activité du contrôleur et détecter le moment où il charge une URL personnalisée spécifique définie par votre `application's custom scheme` (par exemple ** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Notez que cette URL personnalisée spécifique n’est en fait pas valide et qu’elle n’est pas destinée à être réellement chargée par le contrôleur. Elle ne doit être interprétée que par votre application comme un signal indiquant que le flux d’authentification ou de déconnexion est terminé et qu’il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer la variable `SFSafariViewController` et appelez AccessEnabler&#39;s `handleExternalURL:url `méthode API.

**Remarque :** Dans le cas du flux d’authentification, il s’agit d’un point où l’utilisateur a la possibilité d’appuyer sur le bouton &quot;Retour&quot;, ce qui équivaut à interrompre le flux d’authentification. Dans ce cas, votre application doit appeler la fonction [setSelectedProvider:](#setSelProv) transmission de méthode **`nil`** comme paramètre et permettant à AccessEnabler de réinitialiser son ordinateur-état d’authentification.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : afficher la page de connexion MVPD dans SFSafariViewController</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité : **v 3.2+

**Paramètres**:

* *url :* URL pointant vers la page de connexion du MVPD
* *useSVC:* si l’URL doit être chargée dans SFSafariViewController.

**Déclenché par :**[ setOptions:](#setOptions) before [setSelectedProvider:](#setSelProv)

[Retour au sommet...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est appelée par votre application pour terminer le flux d’authentification ou de déconnexion. Cette méthode doit être appelée juste après que votre application a détecté le moment où la variable `UIWebView/WKWebView or SFSafariViewController` Le contrôleur est redirigé vers une URL personnalisée spécifique. Si votre application doit utiliser une `SFSafariViewController `Le contrôleur de l’URL personnalisée spécifique est défini par votre `application's custom scheme` (par exemple,`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), sinon cette URL personnalisée spécifique est définie par la variable `ADOBEPASS_REDIRECT_URL `constante (c’est-à-dire `adobepass://ios.app`).

Dans le cas du flux d’authentification, AccessEnabler complète le flux en récupérant le jeton d’authentification du serveur principal et en le stockant localement dans le stockage du jeton. AccessEnabler informe votre application que le flux d&#39;authentification est terminé en appelant la fonction `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> rappel avec un code d’état de 1, indiquant la réussite. En cas d’erreur lors de l’exécution de ces étapes, la variable `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> Le rappel est déclenché avec un code d’état de 0, indiquant l’échec de l’authentification, ainsi qu’un code d’erreur correspondant.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : fin du flux d’authentification ou de déconnexion</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v3.0+

**Paramètres :**

* *url*: l’URL interceptée de la fonction ` UIWebView/WKWebView or SFSafariViewController ` contrôle en tant que chaîne.


**Rappels déclenchés :** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Retour au sommet...](#apis)

</br>

#### getAuthenticationToken - [OBSOLÈTE] {#getAuthNToken}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Termine le flux d’authentification en demandant le jeton d’authentification auprès du serveur principal. Cette méthode ne doit être appelée par votre application qu’en réponse à un événement où le contrôle WebView hébergeant la page de connexion MVPD est redirigé vers l’URL personnalisée définie par la variable `ADOBEPASS_REDIRECT_URL` constante.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : récupération du jeton d’authentification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+ **Jusqu’à :** v3.0

**Paramètres :** Aucun

**Rappels déclenchés :** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Retour au sommet...](#apis)

&lt;/br

#### setAuthenticationStatus:errorCode: {#setAuthNStatus}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui informe l’application de l’état du flux d’authentification. Il existe de nombreux endroits où ce flux peut échouer, soit en raison de l’interaction de l’utilisateur, soit en raison d’autres scénarios imprévus (c’est-à-dire des problèmes de connectivité réseau, etc.). Ce rappel informe l’application de l’état de réussite/échec du flux d’authentification, tout en fournissant des informations supplémentaires sur la raison de l’échec, le cas échéant.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : signalez l’état du flux d’authentification.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**Disponibilité :** v1.0+

**Paramètres**:

* *status*: peut prendre l’une des valeurs suivantes :
   * `ACCESS_ENABLER_STATUS_SUCCESS` - le flux d’authentification a été terminé
   * `ACCESS_ENABLER_STATUS_ERROR` - échec du flux d’authentification
* *code*: raison de l’échec. If *status* is `ACCESS_ENABLER_STATUS_SUCCESS`, puis *code* est une chaîne vide (c’est-à-dire définie par la variable `USER_AUTHENTICATED` constante). En cas d’échec, ce paramètre peut prendre l’une des valeurs suivantes :
   * `USER_NOT_AUTHENTICATED_ERROR` - L’utilisateur n’est pas authentifié. En réponse à la variable [checkAuthentication:](#checkAuthN) appel de méthode lorsqu’il n’existe pas de jeton d’authentification valide dans le cache de jeton local.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler a réinitialisé l’ordinateur-état d’authentification une fois l’application de couche supérieure transmise. *null* to [`setSelectedProvider:`](#setSelProv) pour interrompre le flux d’authentification.  L’utilisateur a probablement annulé le flux d’authentification (c’est-à-dire qu’il a appuyé sur le bouton &quot;Retour&quot;).
   * `GENERIC_AUTHENTICATION_ERROR` - Le flux d’authentification a échoué pour des raisons telles que l’indisponibilité du réseau ou l’utilisateur a explicitement annulé le flux d’authentification.

**Déclenché par :** ` checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN),` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ)

[Retour au sommet...](#apis)

</br>

### checkPreauthorizedResources : {#checkPreauth}


**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est utilisée par l’application pour déterminer si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques. L’objectif principal de cette méthode est de récupérer des informations à utiliser dans la décoration de l’interface utilisateur. **(par exemple, en indiquant l’état d’accès avec les icônes de verrouillage et de déverrouillage).**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : définir le fournisseur actuellement sélectionné</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.3+

**Paramètres :**

* *ressources :* tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’ID de ressource. L’ID de ressource est soumis aux mêmes limites que l’ID de ressource dans l’appel , c’est-à-dire qu’il doit s’agir d’une valeur convenue établie entre le programmeur et le MVPD ou un fragment RSS multimédia.

**Rappel déclenché :** [`preauthorizedResources:`](#preauthResources)

[Retour au sommet...](#apis)

</br>

### checkPreauthorizedResources:cache: {#checkPreauthCache}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est utilisée par l’application pour déterminer si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques. L’objectif principal de cette méthode est de récupérer des informations à utiliser dans la décoration de l’interface utilisateur (par exemple, en indiquant l’état d’accès avec les icônes de verrouillage et de déverrouillage). La variable **cache** contrôle si le cache interne est utilisé pour résoudre les ressources.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : définir le fournisseur actuellement sélectionné</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**Disponibilité :** v3.1+



**Paramètres :**

* *ressources :* tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’ID de ressource. L’ID de ressource est soumis aux mêmes limites que l’ID de ressource dans la variable `getAuthorization:` , c’est-à-dire qu’il doit s’agir d’une valeur convenue établie entre le programmeur et le MVPD ou un fragment RSS multimédia.
* *cache :* Valeur booléenne indiquant s’il faut utiliser le cache interne pour résoudre les ressources. Si la valeur est false, le cache est ignoré, ce qui entraîne des appels au serveur chaque fois que cette API est appelée.

**Rappel déclenché :** [`preauthorizedResources:`](#preauthResources)

[Retour au sommet...](#apis)

</br>

### preauthorizedResources: {#preauthResources}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description :** Rappel déclenché par `checkPreauthorizedResources:`. Fournit une liste des ressources que l’utilisateur est déjà autorisé à afficher.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : définir le fournisseur actuellement sélectionné</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.3+

**Paramètres :**

* `resources`: tableau de ressources pour lequel l’utilisateur est déjà autorisé à afficher.

**Déclenché par :** [`checkPreauthorizedResources:`](#checkPreauth)



[Retour au sommet...](#apis)

</br>

### checkAuthorization:, checkAuthorization:withData: {#checkAuthZ}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est utilisée par l’application pour vérifier l’état de l’autorisation. Il commence par vérifier l’état d’authentification. Si elle n’est pas authentifiée, la variable [tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed) callback est déclenché et la méthode se ferme. Si l’utilisateur est authentifié, il déclenche également le flux d’autorisation. Voir les détails de la variable [`getAuthorization:`](#getAuthZ) .


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : vérification de l’état de l’autorisation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : vérification de l’état de l’autorisation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.9+

**Paramètres :**

* *resource*: identifiant de la ressource pour laquelle l’utilisateur demande l’autorisation.
* *data*: dictionnaire composé de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :**
[tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed)`,setToken:forResource:, sendTrackingData:forEventType:, setAuthenticationStatus:errorCode:`

[Retour au sommet...](#apis)

</br>

### getAuthorization:, getAuthorization:withData: {#getAuthZ}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est utilisée par l’application pour lancer le flux d’autorisation. Si l’utilisateur n’est pas déjà authentifié, il lance également le flux d’authentification. Si l’utilisateur est authentifié, AccessEnabler émet des requêtes pour le jeton d’autorisation (si aucun jeton d’autorisation valide n’est présent dans le cache de jeton local) et pour le jeton multimédia de courte durée. Une fois le jeton multimédia court obtenu, le flux d’autorisation est considéré comme terminé. La variable [setToken:forResource:](#setToken) callback est déclenché et le jeton multimédia court est diffusé en tant que paramètre à l’application. Si, pour une raison quelconque, l’autorisation échoue, la variable [tokenRequestFailed:forEventType:](#tokenReqFailed) est déclenché et le code d’erreur/les détails sont fournis.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : lancer le flux d’autorisation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : lancer le flux d’autorisation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**Disponibilité :** v1.9+

**Paramètres :**

* *resource*: identifiant de la ressource pour laquelle l’utilisateur demande l’autorisation.
* *data*: dictionnaire composé de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Rappels supplémentaires déclenchés :**\
Cette méthode peut également déclencher les rappels suivants (si le flux d’authentification est également lancé) : `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

**REMARQUE : Veuillez utiliser checkAuthorization: / checkAuthorization:withData: au lieu de getAuthorization: / getAuthorization:withData: dans la mesure du possible. getAuthorization: / getAuthorization:withData: démarre un flux d’authentification complet (si l’utilisateur n’est pas authentifié), ce qui peut entraîner une mise en oeuvre compliquée du côté du programmeur.**

[Retour au sommet...](#apis)

</br>

### setToken:forResource: {#setToken}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui informe votre application que le flux d’autorisation a été terminé avec succès. Le jeton multimédia de courte durée est également fourni en tant que paramètre.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : le flux d’autorisation a réussi</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres**:

* *token*: jeton multimédia de courte durée
* *resource*: la ressource pour laquelle l’autorisation a été obtenue

**Déclenché par :** [checkAuthorization:](#checkAuthZ)` , `[checkAuthorization:withData:](#checkAuthZ),` `[getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ)

[Retour au sommet...](#apis)

</br>

### tokenRequestFailed:errorCode:errorDescription: {#tokenReqFailed}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui informe l’application de couche supérieure que le flux d’autorisation a échoué.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : échec du flux d’autorisation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) tokenRequestFailed:(NSString *)resource 
                  errorCode:(NSString *)code 
           errorDescription:(NSString *)description; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres**:

* *resource*: ressource pour laquelle l’autorisation a été obtenue.
* *code*: code d’erreur associé au scénario d’échec. Valeurs possibles :
   * `USER_NOT_AUTHORIZED_ERROR` - l’utilisateur n’a pas pu autoriser pour la ressource donnée
* *description*: détails supplémentaires sur le scénario d’échec. Si cette chaîne descriptive n’est disponible pour aucune raison, l’authentification Adobe Pass envoie une chaîne vide. **(&quot;&quot;)**.\
  Cette chaîne peut être utilisée par un MVPD pour transmettre des messages d’erreur personnalisés ou des messages liés aux ventes. Par exemple, si l’autorisation d’une ressource est refusée à un abonné, le MVPD peut envoyer un message tel que : &quot;Vous n’avez actuellement pas accès à ce canal dans votre package. Si vous souhaitez mettre à niveau votre package, cliquez sur **here**.&quot; Le message est transmis par l’authentification Adobe Pass via ce rappel au programmeur, qui a la possibilité de l’afficher ou de l’ignorer. L’authentification Adobe Pass peut également utiliser ce paramètre pour fournir une notification de la condition qui a pu entraîner une erreur. Par exemple, &quot;Une erreur de réseau s’est produite lors de la communication avec le service d’autorisation du fournisseur&quot;.

**Déclenché par :** ` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ)

[Retour au sommet...](#apis)

</br>

### déconnexion {#logout}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est appelée par votre application pour lancer le flux de déconnexion. La déconnexion est le résultat d’une série d’opérations de redirection HTTP en raison du fait que l’utilisateur doit être déconnecté des serveurs d’authentification Adobe Pass et des serveurs du MVPD. Comme ce flux ne peut pas être complété avec une requête HTTP simple émise par la bibliothèque AccessEnabler, une `UIWebView/WKWebView or SFSafariViewController` Le contrôleur doit être instancié pour pouvoir suivre les opérations de redirection HTTP.

Le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir avec la variable `UIWebView/WKWebView or SFSafariViewController`  du contrôleur, de quelque manière que ce soit. Par conséquent, Adobe recommande de rendre le contrôle invisible (c’est-à-dire masqué) pendant le processus de déconnexion.

Un modèle similaire au flux d’authentification est utilisé. IOS AccessEnabler déclenche l’événement `navigateToUrl:` callback ou `navigateToUrl:useSVC:` pour créer un `UIWebView/WKWebView or SFSafariViewController` et de charger l’URL fournie dans le rappel `url` . Il s’agit de l’URL du point de terminaison de la déconnexion sur le serveur principal. Pour tvOS AccessEnabler, ni la variable `navigateToUrl:` callback ou `navigateToUrl:useSVC:` callback est appelé.

Au fur et à mesure de plusieurs redirections, votre application doit surveiller l’activité de la variable `UIWebView/WKWebView or SFSafariViewController `et détecter le moment où il charge une URL personnalisée spécifique. Notez que cette URL personnalisée spécifique n’est en fait pas valide et qu’elle n’est pas destinée à être réellement chargée par le contrôleur. Elle ne doit être interprétée que par votre application comme un signal indiquant que le flux de déconnexion est terminé et qu’il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer le contrôleur et appeler le `handleExternalURL:url `méthode API. Si votre application doit utiliser une `SFSafariViewController `Le contrôleur de l’URL personnalisée spécifique est défini par votre `application's custom scheme` (par exemple,`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), sinon cette URL personnalisée spécifique est définie par la variable `ADOBEPASS_REDIRECT_URL `constante (c’est-à-dire `adobepass://ios.app`).

À la fin, AccessEnabler appelle la fonction [`setAuthenticationStatus()`](#setAuthNStatus) rappel avec un code d’état de 0, indiquant la réussite du flux de déconnexion.

**Remarque :** Si l’utilisateur est connecté à l’aide de l’authentification unique Apple, l’état VSA203 est déclenché. Si c’est le cas, l’utilisateur doit être invité à se déconnecter des paramètres du système. Si vous ne le faites pas, une nouvelle authentification est effectuée lorsque l’application est relancée.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : lancer le flux de déconnexion</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres :** Aucun

**Rappels déclenchés :** `navigateToUrl:, `[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Retour au sommet...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Utilisez cette méthode pour déterminer le fournisseur actuellement sélectionné.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : déterminer le MVPD actuellement sélectionné</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres :** Aucun

**Rappels déclenchés :** [`selectedProvider:`](#selProv)

[Retour au sommet...](#apis)

</br>

### selectedProvider {#selProv}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui fournit des informations sur le MVPD actuellement sélectionné à l’application.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : informations sur le MVPD actuellement sélectionné</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres**:

* *mvpd*: objet contenant des informations sur le MVPD actuellement sélectionné

**Déclenché par :** [`getSelectedProvider`](#getSelProv)

[Retour au sommet...](#apis)

</br>

### getMetadata : {#getMeta}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Utilisez cette méthode pour récupérer des informations exposées en tant que métadonnées par la bibliothèque AccessEnabler. L’application peut accéder à ces données en fournissant un dictionnaire *key* paramètre d’entrée.

Deux types de métadonnées sont disponibles pour les programmeurs :

* Métadonnées statiques (jeton d’authentification TTL, jeton d’autorisation TTL et ID de périphérique)
* Métadonnées utilisateur (informations spécifiques à l’utilisateur, telles que l’ID utilisateur, le code postal ; peuvent être transmises d’un MVPD à l’appareil d’un utilisateur au cours des flux d’authentification et d’autorisation)

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : requête de AccessEnabler pour les métadonnées</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres :**

* *keyDictionary*: une structure de données du dictionnaire, au format suivant :
   * Si la clé est `METADATA_OPCODE_KEY` et la valeur est `METADATA_AUTHENTICATION`, la requête est alors effectuée pour obtenir le délai d’expiration du jeton d’authentification.
   * Si la clé est `METADATA_OPCODE_KEY` et la valeur est `METADATA_AUTHORIZATION` **et**\
     key is `METADATA_RESOURCE_ID_KEY` et est un identifiant de ressource spécifique, puis la requête est faite pour obtenir le délai d’expiration du jeton d’autorisation associé à la ressource spécifiée.
   * Si la clé est `METADATA_OPCODE_KEY` et la valeur est `METADATA_DEVICE_ID`, la requête est alors effectuée pour obtenir l’identifiant de l’appareil actuel. Notez que cette fonctionnalité est désactivée par défaut et que les programmeurs doivent contacter Adobe pour plus d’informations sur l’activation et les frais.
   * Si la clé est `METADATA_OPCODE_KEY` et la valeur est `METADATA_USER_META` **et** key is `METADATA_USER_META_KEY` et est le nom des métadonnées, puis la requête est faite pour les métadonnées utilisateur. Liste des types de métadonnées utilisateur disponibles :
      * `zip` - Liste des codes postaux
      * `householdID` - Identifiant du foyer. Dans le cas où un MVPD ne prend pas en charge les sous-comptes, cela est identique à `userID`.
      * `maxRating` - Collection d’évaluations parentales maximales pour l’utilisateur
      * `userID` - Identifiant de l’utilisateur. Si un MVPD prend en charge des sous-comptes et que l’utilisateur n’est pas le compte principal, `userID` sera différent de `householdID.`
      * `channelID` - Liste des canaux qu’un utilisateur est autorisé à voir.

  >[!NOTE]
  >
  >Les métadonnées utilisateur réelles disponibles pour un programmeur dépendent de ce qu’un MVPD rend disponible. Cette liste sera étendue à mesure que de nouvelles métadonnées seront disponibles et ajoutées au système d’authentification Adobe Pass.

**Rappels déclenchés :** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**En savoir plus :** [Métadonnées utilisateur](/help/authentication/user-metadata.md)

[Retour au sommet...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler après appel[getAuthentication()](#getAuthN) si le demandeur actuel prend en charge au moins un MVPD avec la prise en charge de l’authentification unique.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : résultat des flux d’authentification unique</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v2.0+

**Paramètres**:

* viewController : représente la boîte de dialogue Apple SSO. Ce viewController doit s’afficher à l’écran.

**Déclenché par :** [`getAuthentication`](#getAuthN)

**En savoir plus :** [Connexion unique iOS/tvOS](#presentTvDialog)

[Retour au sommet...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler après la fermeture de la boîte de dialogue SSO Apple par l’utilisateur.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : résultat des flux d’authentification unique</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v2.0+

**Paramètres**:

* viewController : représente la boîte de dialogue Apple SSO. Ce viewController doit être supprimé de l’écran.

**Déclenché par :** Action de l’utilisateur

**En savoir plus :** [Connexion unique iOS/tvOS](#presentTvDialog)

[Retour au sommet...](#apis)

</br>

### setMetadataStatus:encrypted:forKey:andArguments: {#setMetaStatus}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui diffuse les métadonnées demandées via un [`getMetadata:`](#getMeta) appelez .

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : résultat de la requête de récupération de métadonnées</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setMetadataStatus:(id)metadata
                 encrypted:(bool)encrypted 
                    forKey:(int)key 
              andArguments:(NSDictionary *)arguments; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres**:

* *metadata*: métadonnées demandées. Cette valeur est une `NSString` dans le cas de métadonnées statiques (TTL d’authentification, TTL d’autorisation, ID de périphérique).  Il s’agit d’un objet complexe lors de la demande de métadonnées spécifiques à l’utilisateur. Cet objet complexe est généralement la représentation Objective-C d’une charge utile JSON (par exemple &quot;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;) : [&quot;150&quot;, &quot;320&quot;]}&#39; est traduit en Objective-C comme NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;building&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;)).   Exemple d’objet JSON de métadonnées :

```JSON
        {
            updated: 1334243471,
            encrypted: ["encryptedProp"],
            data: {
                zip: ["12345", "34567"],
                maxRating: { 
                    "MPAA": "PG-13",
                    "VCHIP": "TV-Y", 
                    "URL": "http://exam.pl/e/manage/ratings"
                },
                householdID: "3456",
                userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
                channelID: ["channel-1", "channel-2"]
            }
```

* *encrypted*: valeur booléenne qui spécifie si les métadonnées récupérées sont chiffrées ou non. Ce paramètre n’est significatif que pour les requêtes de métadonnées utilisateur. Il n’a aucune signification pour les métadonnées statiques (par exemple, Authentification TTL) qui sont toujours reçues sans chiffrement. Si ce paramètre est défini sur true, c’est au programmeur d’obtenir la valeur des métadonnées utilisateur non chiffrées en effectuant un décryptage RSA à l’aide de la clé privée de mise en whiteliste (la même clé privée utilisée pour la signature de l’ID de demandeur dans la variable [`setRequestor:setSignedRequestorId:`](#setReq) et `setRequestor:setSignedRequestorId:serviceProviders: `appels).

* *key*: clé utilisée pour formuler la requête de récupération de métadonnées.

* *arguments*: même dictionnaire qui a été transmis à la variable [`getMetadata:`](#getMeta) appelez . Elle permet à l’application de faire correspondre les requêtes aux réponses.

**Déclenché par :** [`getMetadata:`](#getMeta)

**En savoir plus :** [Métadonnées utilisateur](/help/authentication/user-metadata.md)


[Retour au sommet...](#apis)

</br>

### MVPD {#mvpd}

**Fichier :** AccessEnabler/headers/model/MVPD.h

**Description** Décrit l’objet MVPD. Peut être utilisé pour obtenir des informations sur les propriétés du MVPD.

**Disponibilité :** v1.0+ [La propriété boardingStatus est disponible à partir de la version 2.2]

**Propriétés**:

* ID (NSString) - Identifiant MVPD.
* (NSString) displayName - nom MVPD. [Il doit être utilisé pour l’affichage dans le sélecteur]
* LogoURL (NSString) - Adresse du logo MVPD.
* (BOOL) enablePlatformServices - Si la valeur est true, le MVPD prend en charge les services d’authentification unique tels que [APPLE SSO](#presentTvDialog).
* (NSString) boardingStatus - Peut comporter 3 valeurs :
   * nil - Le MVPD ne prend pas en charge la fonction SSO d’Apple.
   * SÉLECTEUR : le MVPD peut apparaître dans le sélecteur Apple, mais le flux d’authentification est effectué par Adobe.
   * PRIS EN CHARGE : le MVPD est entièrement pris en charge par Apple et utilisera le jeton d’authentification unique Apple.

[Retour au sommet...](#apis)

</br>

## Suivi des événements {#tracking}

AccessEnabler déclenche un rappel supplémentaire qui n’est pas nécessairement lié aux flux de droits. Implémentation de [`sendTrackingData()`](#sendTracking) La fonction de rappel est facultative, mais elle permet à l’application de suivre des événements spécifiques et de compiler des statistiques telles que le nombre de tentatives d’authentification/d’autorisation réussies/ayant échoué.

### sendTrackingData:forEventType: {#sendTracking}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler signalant à l’application l’occurrence de divers événements tels que l’achèvement/l’échec des flux d’authentification/d’autorisation. Avec l’authentification Adobe Pass 1.6, le type de périphérique, le type de client AccessEnabler et le système d’exploitation sont signalés par [`sendTrackingData()`](#sendTracking). La variable [`sendTrackingData()`](#sendTracking) callback reste rétrocompatible.

**Rappel : suivi des événements**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Disponibilité :** v1.0+

**Remarque :** Le type d’appareil et le système d’exploitation sont dérivés de l’utilisation d’une bibliothèque Java publique (<http://java.net/projects/user-agent-utils>) et la chaîne de l’agent utilisateur. Notez que ces informations ne sont fournies que pour ventiler les mesures opérationnelles en catégories d’appareils, mais que cet Adobe ne peut pas assumer la responsabilité de résultats incorrects. Utilisez la nouvelle fonctionnalité en conséquence.

* Valeurs possibles pour le type d’appareil :
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* Valeurs possibles pour le type de client AccessEnabler :
   * `flash`
   * `html5`
   * `ios`
   * `android`


**Paramètres**:

* *event*: code de l’événement suivi. Il existe trois types d’événements de suivi possibles :
   * **authorizationDetection :** chaque fois qu’une requête de jeton d’autorisation est renvoyée (l’événement est `TRACKING_AUTHORIZATION`)
   * **authenticationDetection :** chaque fois qu’une vérification d’authentification a lieu (événement `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** lorsque l’utilisateur sélectionne un MVPD dans le formulaire de sélection du MVPD (l’événement est `TRACKING_GET_SELECTED_PROVIDER`)
* *data*: données supplémentaires associées à l’événement signalé. Ces données sont présentées sous la forme d&#39;une liste de valeurs.

**Déclenché par :** `checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN), `checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ), `setSelectedProvider:`

Instructions pour l’interprétation des valeurs dans la variable *data* tableau :

* Pour trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Indique si la requête de jeton a réussi (true/false) et si elle a réussi :
   * **1** - Chaîne d’identifiant MVPD
   * **2** - GUID (haché md5)
   * **3** - Jeton déjà dans le cache (true/false)
   * **4** - Type de périphérique
   * **5** - Type de client AccessEnabler
   * **6** - Type de système d’exploitation

* Pour trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Indique si la requête de jeton a réussi (true/false) et si elle a réussi :
   * **1** - Identifiant MVPD
   * **2** - GUID (haché md5)
   * **3** - Jeton déjà dans le cache (true/false)
   * **4** - Erreur
   * **5** - Détails
   * **6** - Type de périphérique
   * **7** - Type de client AccessEnabler
   * **8** - Type de système d’exploitation
* Pour trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - Identifiant du MVPD actuellement sélectionné
   * **1** - Type de périphérique
   * **2** - Type de client AccessEnabler
   * **3** - Type de système d’exploitation

</br>
