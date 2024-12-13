---
title: Référence de l’API iOS/tvOS
description: Référence de l’API iOS/tvOS
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '6956'
ht-degree: 0%

---

# Référence de l’API SDK iOS/tvOS (héritée) {#iostvos-sdk-api-reference}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Introduction {#intro}

Cette page décrit les méthodes et les fonctions de rappel exposées par le client natif iOS/tvOS pour l’authentification Adobe Pass. Les méthodes et fonctions de rappel décrites ici sont définies dans les fichiers d’en-tête `AccessEnabler.h` et `EntitlementDelegate.h` ; vous pouvez les retrouver dans le SDK AccessEnabler d’iOS à l’adresse suivante : `[SDK directory]/AccessEnabler/headers/api/`


Documentation associée :

* Pour obtenir une description du flux de droits de base de l’authentification Adobe Pass, voir [Flux de droits](/help/authentication/integration-guide-programmers/entitlement-flow.md).
* Pour une présentation détaillée de la mise en œuvre d’Adobe Pass
Flux de droits d’authentification utilisant cette API, consultez le [guide pas à pas d’intégration d’iOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md).
* Pour la dernière version du SDK AccessEnabler iOS, consultez [Bibliothèque iOS Native Access Enabler](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe vous incite à utiliser uniquement les API d’authentification Adobe Pass *publiques* :
>
>* Les API publiques sont disponibles et entièrement testées sur tous les types de clients pris en charge. Pour toute fonctionnalité publique, nous nous assurons que chaque type de client dispose d’une version correspondante des méthodes associées.
>* Les API publiques doivent être aussi stables que possible, pour prendre en charge la rétrocompatibilité et s’assurer que les intégrations des partenaires ne sont pas rompues. Toutefois, pour les API non publiques, nous nous réservons le droit de modifier leur signature à tout moment ultérieur. Si vous rencontrez un flux particulier qui ne peut pas être pris en charge par une combinaison des appels de l’API d’authentification Adobe Pass publics actuels, la meilleure approche consiste à nous le faire savoir. En tenant compte de vos besoins, nous pouvons modifier les API publiques et fournir une solution stable à l’avenir.

</br>

## Référence d’API {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - Instancie l’objet AccessEnabler.

* **[DEPRECATED]** [init](#init) - Instancie l&#39;objet AccessEnabler.

* [`setOptions:options:`](#setOptions) - Configure les options SDK globales telles que le profil ou l’identifiant visiteur.

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - Établit l’identité du programmeur.

* **[OBSOLÈTE]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - Établit l’identité du programmeur.

* **[OBSOLÈTE]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos)-Établit l’identité du programmeur.

* [`setRequestorComplete:`](#setReqComplete) - Informe votre application que la phase de configuration est terminée.

* [`checkAuthentication`](#checkAuthN) - Vérifie le statut d’authentification de l’utilisateur actuel.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN) - Démarre le workflow d’authentification complet.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN)[andFilter](#getAuthN_filter) - Démarre le processus d’authentification complet.

* [`displayProviderDialog:`](#dispProvDialog) - Indique à votre application d’instancier les éléments d’interface utilisateur appropriés pour que l’utilisateur puisse sélectionner un MVPD.

* [`setSelectedProvider:`](#setSelProv) - Informe AccessEnabler de la sélection MVPD de l’utilisateur.

* [`navigateToUrl:`](#nav2url) - Informe votre application que la page de connexion de MVPD doit être présentée à l’utilisateur.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - Informe votre application que la page de connexion de MVPD doit être présentée à l’utilisateur à l’aide de SFSafariViewController

* [`handleExternalURL:url`](#handleExternalURL) - Termine le flux d’authentification/de déconnexion.

* **[OBSOLÈTE]** [`getAuthenticationToken`](#getAuthNToken) - Demande le jeton d’authentification au serveur principal.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) - Informe votre application du statut du flux d’authentification.

* [`checkPreauthorizedResources:`](#checkPreauth) - Détermine si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - Détermine si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques.

* [`preauthorizedResources:`](#preauthResources) - Fournit une liste des ressources que l’utilisateur est déjà autorisé à afficher.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - Vérifie le statut d’autorisation de l’utilisateur actuel.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - Lance le flux d’autorisation.

* [`setToken:forResource:`](#setToken) - Informe votre demande que le flux d’autorisation a été terminé avec succès.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) - Informe votre application que le flux d’autorisation a échoué.

* [`logout`](#logout) - Lance le flux de déconnexion.

* [`getSelectedProvider`](#getSelProv) - Détermine le fournisseur actuellement sélectionné.

* [`selectedProvider:`](#selProv) - Fournit des informations sur le MVPD actuellement sélectionné à votre application.

* [`getMetadata:`](#getMeta) - Récupère les informations exposées en tant que métadonnées par la bibliothèque AccessEnabler.

* [`presentTvProviderDialog:`](#presentTvDialog) - Informe votre application d’afficher la boîte de dialogue de l’authentification unique Apple.

* [`dismissTvProviderDialog:`](#dismissTvDialog) : indique à votre application de masquer la boîte de dialogue de l’authentification unique Apple.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - Diffuse les métadonnées demandées par un appel [`getMetadata:`](#getMeta).

* [`sendTrackingData:forEventType:`](#sendTracking) - Fournit des informations sur les données de suivi.

* [`MVPD`](#mvpd) - Classe MVPD. [Contient des informations sur le MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** instancie l&#39;objet AccessEnabler. Il doit y avoir une seule instance AccessEnabler par instance d&#39;application.

| **Appel API : constructeur iOS AccessEnabler** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Disponibilité :** v3.0+

**Paramètres:**

* **softwareStatement :** chaîne qui identifie l’application dans le système d’Adobe. Découvrez comment obtenir une déclaration logicielle.

[Haut de la page...](#apis)



### init - [OBSOLÈTE]{#init}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** instancie l&#39;objet AccessEnabler. Il doit y avoir une seule instance AccessEnabler par instance d&#39;application.

| Appel API : constructeur iOS AccessEnabler |
| --- |
| ```- (id) init;``` |

**Disponibilité :** v1.0+ **Jusqu’au :** v3.0

**Paramètres:** Aucun

[Haut de la page...](#apis)

</br>

### setOptions:options {#setOptions}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** configure les options globales de SDK. Il accepte un NSDictionary comme argument. Les valeurs du dictionnaire seront transmises au serveur avec chaque appel réseau effectué par le SDK.

**Remarque :** les valeurs seront transmises au serveur indépendamment du flux actuel (authentification/autorisation). Si vous souhaitez modifier les valeurs, vous pouvez appeler cette méthode à tout moment.

| Appel API : setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Disponibilité :** v2.3.0+

**Paramètres:**

* *options* : un NSDictionary contenant des options SDK globales. Actuellement, les options suivantes sont disponibles :
   * **applicationProfile** - Peut être utilisé pour effectuer des configurations de serveur en fonction de cette valeur.
   * **visitorID** - Service d’identification des Experience Cloud. Cette valeur peut être utilisée ultérieurement pour les rapports d’analyse avancée.
   * **handleSVC** - Valeur booléenne indiquant si le programmeur gérera SFSafariViewControllers. Pour plus d’informations, consultez la section Prise en charge de [SFSafariViewController sur iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md).
      * Si cette valeur est définie sur **false** le SDK présente automatiquement un SFSafariViewController à l’utilisateur final. Le SDK accède ensuite à l’URL de la page de connexion des MVPD.
      * Si la valeur est définie sur **true** le SDK **NOT** présente automatiquement un SFSafariViewController à l&#39;utilisateur final. Le SDK se déclenchera en outre **navigate(toUrl:{url}, useSVC:YES)**.
* **device\_info** - Informations du client comme décrit dans [Transmission des informations du client](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

[Haut de la page...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** établit l’identité du programmeur. Chaque programmeur se voit attribuer un ID unique lors de l’enregistrement avec l’Adobe pour le système d’authentification Adobe Pass. Lorsque vous traitez des jetons SSO et distants, l&#39;état d&#39;authentification peut changer lorsque l&#39;application est en arrière-plan, setRequestor peut être appelé à nouveau lorsque l&#39;application est amenée au premier plan afin de se synchroniser avec l&#39;état du système (récupérer un jeton distant si SSO est activé ou supprimer le jeton local si une déconnexion s&#39;est produite en attendant).

La réponse du serveur contient une liste de fichiers MVPD ainsi que des informations de configuration associées à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code AccessEnabler. Seul le statut de l’opération (c’est-à-dire SUCCÈS/ÉCHEC) est présenté à votre application via le rappel `setRequestorComplete:`.

Si le paramètre `urls` n’est pas utilisé, l’appel réseau résultant cible l’URL du fournisseur de services par défaut : l’environnement RELEASE/production d’Adobe.


Si une valeur est fournie pour le paramètre `urls`, l’appel réseau résultant cible toutes les URL fournies dans le paramètre `urls`. Toutes les demandes de configuration sont déclenchées simultanément dans des threads distincts. Le premier répondant est prioritaire lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, AccessEnabler mémorise l’URL du fournisseur d’accès associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.

| Appel API : configuration du demandeur |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Disponibilité :** v3.0+

| Appel API : configuration du demandeur |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Disponibilité :** v3.0+

**Paramètres:**

* *requestorID* : ID unique associé au programmeur. Transmettez l’ID unique attribué par Adobe à votre site lors de votre premier enregistrement auprès du service d’authentification d’Adobe Pass.
* *urls* : paramètre facultatif ; par défaut, le fournisseur d’accès Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points d’entrée pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances de fournisseur de services d’authentification Adobe Pass. Ce faisant, la liste MVPD est composée des points d’entrée de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.

>[!NOTE]
>
>Si elle est appelée sans le paramètre `serviceProviders`, la bibliothèque récupère la configuration du fournisseur de services par défaut (c’est-à-dire `https://sp.auth.adobe.com` pour le profil de production ou `https://sp.auth-staging.adobe.com` pour le profil d’évaluation). Si le paramètre `serviceProviders` est fourni, il doit s’agir d’un tableau d’URL. Les informations de configuration sont récupérées à partir de tous les points d’entrée spécifiés et sont fusionnées. S&#39;il existe des informations en double dans les différentes réponses du fournisseur de services, le conflit est résolu en faveur du serveur qui répond le plus rapidement (c&#39;est-à-dire que le serveur qui a le temps de réponse le plus court est prioritaire).

**Rappels déclenchés :** [`setRequestorComplete:`](#setReqComplete)

[Haut de la page...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [ OBSOLÈTE ] {#setReq}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** établit l’identité du programmeur. Chaque programmeur se voit attribuer un ID unique lors de l’enregistrement avec l’Adobe pour le système d’authentification Adobe Pass. Lorsque vous traitez avec des jetons SSO et distants, l&#39;état d&#39;authentification peut changer lorsque l&#39;application est en arrière-plan, setRequestor peut être appelé à nouveau lorsque l&#39;application est mise en premier plan afin de se synchroniser avec l&#39;état du système (récupérer un jeton distant si SSO est activé ou supprimer le jeton local si une déconnexion s&#39;est produite en attendant).

La réponse du serveur contient une liste de fichiers MVPD ainsi que des informations de configuration associées à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code AccessEnabler. Seul le statut de l’opération (c’est-à-dire SUCCÈS/ÉCHEC) est présenté à votre application via le rappel `setRequestorComplete:`.

Si le paramètre `urls` n’est pas utilisé, l’appel réseau résultant cible l’URL du fournisseur de services par défaut : l’environnement RELEASE/production d’Adobe.

Si une valeur est fournie pour le paramètre `urls`, l’appel réseau résultant cible toutes les URL fournies dans le paramètre `urls`. Toutes les demandes de configuration sont déclenchées simultanément dans des threads distincts. Le premier répondant est prioritaire lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, AccessEnabler mémorise l’URL du fournisseur d’accès associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.

| Appel API : configuration du demandeur |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Disponibilité :** v1.0+ **Jusqu’au :** v3.0

| Appel API : configuration du demandeur |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Disponibilité :** v1.0+ **Jusqu’au :** v3.0

**Paramètres:**

* *requestorID* : ID unique associé au programmeur. Transmettez l’ID unique attribué par Adobe à votre site lors de votre premier enregistrement auprès du service d’authentification Adobe Pass.
* *signedRequestorID* : **ce paramètre existe dans iOS AccessEnabler versions 1.2 et ultérieures.** Une copie de l’ID du demandeur qui est signée numériquement avec votre clé privée. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls* : paramètre facultatif ; par défaut, le fournisseur d’accès Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points d’entrée pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances de fournisseur de services d’authentification Adobe Pass. Ce faisant, la liste MVPD est composée des points d’entrée de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.

**Notes :** si elle est appelée sans le paramètre `serviceProviders`, la bibliothèque récupère la configuration du fournisseur de services par défaut (c’est-à-dire `https://sp.auth.adobe.com` pour le profil de production ou `https://sp.auth-staging.adobe.com` pour le profil d’évaluation). Si le paramètre `serviceProviders` est fourni, il doit s’agir d’un tableau d’URL. Les informations de configuration sont récupérées à partir de tous les points d’entrée spécifiés et sont fusionnées. S&#39;il existe des informations en double dans les différentes réponses du fournisseur de services, le conflit est résolu en faveur du serveur qui répond le plus rapidement (c&#39;est-à-dire que le serveur qui a le temps de réponse le plus court est prioritaire).

**Rappels déclenchés :** [`setRequestorComplete:`](#setReqComplete)


[Haut de la page...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [ OBSOLÈTE ] {#setReq_tvos}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** établit l’identité du programmeur. Chaque programmeur se voit attribuer un ID unique lors de l’enregistrement avec l’Adobe pour le système d’authentification Adobe Pass. Ce réglage ne doit être effectué qu&#39;une seule fois pendant le cycle de vie de l&#39;application.

La réponse du serveur contient une liste de fichiers MVPD ainsi que des informations de configuration associées à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code AccessEnabler. Seul le statut de l’opération (c’est-à-dire SUCCÈS/ÉCHEC) est présenté à votre application via le rappel `setRequestorComplete:`.

Si le paramètre `urls` n’est pas utilisé, l’appel réseau résultant cible l’URL du fournisseur de services par défaut : l’environnement RELEASE/production d’Adobe.

Si une valeur est fournie pour le paramètre `urls`, l’appel réseau résultant cible toutes les URL fournies dans le paramètre `urls`. Toutes les demandes de configuration sont déclenchées simultanément dans des threads distincts. Le premier répondant est prioritaire lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, AccessEnabler mémorise l’URL du fournisseur d’accès associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.



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


**Disponibilité :** v2.0+ **Jusqu’au :** v3.0

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

**Disponibilité :** v2.0+ **Jusqu’au :** v3.0

**Paramètres:**

* *requestorID* : ID unique associé au programmeur. Transmettez l’ID unique attribué par Adobe à votre site la première fois   enregistré auprès du service d’authentification Adobe Pass.
* *signedRequestorID* : **Ce paramètre existe dans iOS AccessEnabler   versions 1.2 et ultérieures.** Une copie de l’ID du demandeur qui est signée numériquement avec votre clé privée. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls* : paramètre facultatif ; par défaut, le fournisseur d’accès d’Adobe   est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points d’entrée pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances de fournisseur de services d’authentification Adobe Pass. Ce faisant, la liste MVPD est composée des points d’entrée de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.
* secret et publicKey : clé secrète et publique utilisée pour signer les deuxièmes appels à l’écran. Pour plus d’informations, consultez la [documentation relative à Clienteless](#create_dev).

Si elle est appelée sans le paramètre `serviceProviders`, la bibliothèque récupère la configuration auprès du fournisseur de services par défaut (à savoir, `https://sp.auth.adobe.com` pour le profil de production ou https://sp.auth-staging.adobe.com pour le profil d’évaluation). Si le paramètre `serviceProviders` est fourni, il doit s’agir d’un tableau d’URL. Les informations de configuration sont récupérées à partir de tous les points d’entrée spécifiés et sont fusionnées. S&#39;il existe des informations en double dans les différentes réponses du fournisseur de services, le conflit est résolu en faveur du serveur qui répond le plus rapidement (c&#39;est-à-dire que le serveur qui a le temps de réponse le plus court est prioritaire).

**Rappels déclenchés :** [`setRequestorComplete:`](#setReqComplete)

[Haut de la page...](#apis)

</br>

### setRequestorComplete : {#setReqComplete}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui informe votre application que la phase de configuration est terminée. Il s’agit d’un signal indiquant que l’application peut commencer à émettre des demandes de droits. Aucune demande de droits ne peut être émise par l’application tant que la phase de configuration n’est pas terminée.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : configuration du demandeur terminée</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilité :** v1.0+

**Paramètres** :

* *status* : peut prendre l&#39;une des valeurs suivantes :
   * `ACCESS_ENABLER_STATUS_SUCCESS` - la phase de configuration s’est terminée avec succès
   * `ACCESS_ENABLER_STATUS_ERROR` - échec de la phase de configuration

**Déclenché par :**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Haut de la page...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** vérifie le statut d’authentification de l’utilisateur actuel.
Pour ce faire, il recherche un jeton d’authentification valide dans le fichier local
espace de stockage des jetons. Cette méthode n’effectue aucun appel réseau et nous vous recommandons de l’appeler sur le thread principal.
Il est utilisé par l’application pour interroger le statut d’authentification de l’utilisateur et
mettez à jour l’interface utilisateur en conséquence (c’est-à-dire, mettez à jour l’interface utilisateur de connexion/déconnexion). Le
le statut d’authentification est communiqué à l’application via .
le rappel [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : vérification du statut d'authentification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres:** Aucun

**Rappels déclenchés :**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Haut de la page...](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** démarre le workflow d’authentification complet. Il commence par vérifier le statut de l’authentification. S’il n’est pas déjà authentifié, la machine d’état du flux d’authentification est démarrée :

* si la dernière tentative d’authentification a réussi, le MVPD   la phase de sélection est ignorée et   le rappel [`navigateToUrl:`](#nav2url) est déclenché. Le   L’application utilise ce rappel pour instancier le contrôle WebView qui présente à l’utilisateur la page de connexion MVPD. **[REMARQUE : à compter de la version 1.5 de l’activateur d’accès, cette fonctionnalité n’est plus disponible en raison d’une limitation dans SDK].**
* si la dernière tentative d’authentification a échoué ou si l’utilisateur s’est explicitement déconnecté, le rappel [`displayProviderDialog:`](#dispProvDialog) est   déclenché. Votre application utilise ce rappel pour afficher l’interface utilisateur de sélection de MVPD. Votre application doit également reprendre le flux d’authentification en informant la bibliothèque AccessEnabler de la sélection MVPD de l’utilisateur via la méthode [`setSelectedProvider:`](#setSelProv).

Comme les informations d’identification de l’utilisateur sont vérifiées sur la page de connexion de MVPD, votre application doit surveiller les multiples opérations de redirection qui ont lieu lorsque l’utilisateur s’authentifie sur la page de connexion de MVPD. Lorsque les informations d&#39;identification correctes sont saisies, le contrôle WebView est redirigé vers une URL personnalisée définie par la constante `ADOBEPASS_REDIRECT_URL`. Cette URL ne doit pas être chargée par le WebView. L’application doit intercepter cette URL et interpréter cet événement comme un signal indiquant que la phase de connexion est terminée. Il doit ensuite transmettre le contrôle à AccessEnabler pour terminer le flux d’authentification (en appelant la méthode [handleExternalURL](#handleExternalURL)).

Enfin, le statut d&#39;authentification est communiqué à l&#39;application via le rappel [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : lance le flux d'authentification</th>
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
<th>Appel API : lance le flux d'authentification</th>
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

**Paramètres:**

* *forceAuthn* : indicateur qui spécifie si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
* *data* : dictionnaire composé de paires clé-valeur à envoyer au service de pass de télévision à péage. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Haut de la page...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** démarre le workflow d’authentification complet. Il commence par vérifier le statut de l’authentification. S’il n’est pas déjà authentifié, la machine d’état du flux d’authentification est démarrée :

* [presentTvProviderDialog()](#presentTvDialog) sera appelé si le demandeur actuel dispose d’au moins un MVPD prenant en charge la connexion unique. Si aucun MVPD ne prend en charge la connexion unique, le flux d’authentification classique commence et le paramètre de filtre est ignoré.
* Une fois que l’utilisateur a terminé, le flux SSO d’Apple [`dismissTvProviderDialog()`](#dismissTvDialog) est déclenché et le processus d’authentification se termine.

Enfin, le statut d&#39;authentification est communiqué à l&#39;application via le rappel [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

**Disponibilité :** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : lance le flux d'authentification</th>
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
<th>Appel API : lance le flux d'authentification</th>
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



**Paramètres:**

* *forceAuthn* : indicateur qui spécifie si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
* *data* : dictionnaire composé de paires clé-valeur à envoyer au service de pass de télévision à péage. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.
* filter : un dictionnaire contenant deux listes d’identifiants MVPD qui doivent apparaître dans la boîte de dialogue SSO d’Apple. Tout MVPD qui ne prend pas en charge la connexion unique sera ignoré, mais l’ordre sera respecté. Le dictionnaire doit comporter deux clés :
   * TV\_PROVIDERS : liste de tous les MVPD qui doivent apparaître dans le sélecteur
   * FEATURED\_TV\_PROVIDERS : liste de tous les MVPD qui doivent être marqués comme figurant dans le sélecteur. Les fichiers MVPD de cette liste doivent également être spécifiés dans la liste TV\_PROVIDERS.

**Disponibilité :** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : lance le flux d'authentification</th>
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
<th>Appel API : lance le flux d'authentification</th>
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



**Paramètres:**

* *forceAuthn* : indicateur qui spécifie si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
* *data* : dictionnaire composé de paires clé-valeur à envoyer au service de pass de télévision à péage. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.
* filter : liste des identifiants MVPD qui doivent apparaître dans la boîte de dialogue SSO Apple. Tout MVPD qui ne prend pas en charge la connexion unique sera ignoré, mais l’ordre sera respecté.

**Rappels déclenchés :** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Haut de la page...](#apis)

</br>

#### displayProviderDialog : {#dispProvDialog}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler pour informer l’application que les éléments d’IU appropriés doivent être instanciés pour permettre à l’utilisateur de sélectionner le MVPD souhaité. Le rappel fournit une liste d’objets MVPD avec des informations supplémentaires qui peuvent aider à créer correctement le panneau de l’interface utilisateur de sélection (comme l’URL pointant vers le logo MVPD, le nom d’affichage convivial, etc.)

Une fois que l’utilisateur a sélectionné le MVPD souhaité, l’application de couche supérieure doit reprendre le flux d’authentification en appelant `setSelectedProvider:` et en lui transmettant l’identifiant du MVPD correspondant à la sélection de l’utilisateur.

**Abandon du flux d’authentification** - À ce stade, l’utilisateur peut appuyer sur le bouton « Précédent », ce qui équivaut à abandonner le flux d’authentification. Dans ce scénario, votre application doit appeler la méthode [setSelectedProvider:](#setSelProv), en transmettant null comme paramètre, pour donner à AccessEnabler la possibilité de réinitialiser son ordinateur d&#39;état d&#39;authentification.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : affichage de l’interface utilisateur de sélection de MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilité :** v1.0+

**Paramètres** :

* *mvpds* : liste d’objets MVPD contenant des informations liées à MVPD que l’application peut utiliser pour créer les éléments de l’interface utilisateur de sélection de MVPD.

**Déclenché par :** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Haut de la page...](#apis)

</br>

#### setSelectedProvider : {#setSelProv}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** cette méthode est appelée par votre application pour informer Access Enabler de la sélection MVPD de l’utilisateur. L’application peut utiliser cette méthode pour sélectionner ou modifier le fournisseur de services utilisé pour l’authentification.

Si le MVPD sélectionné est un MVPD TempPass, il s’authentifiera automatiquement avec ce MVPD sans avoir à appeler getAuthentication() par la suite.

Notez que cela n’est pas possible pour le transfert temporaire promotionnel où des paramètres supplémentaires sont donnés sur la méthode getAuthentication().

Lors de la transmission du paramètre *null*, Access Enabler suppose que l’utilisateur a annulé le flux d’authentification (c’est-à-dire qu’il a appuyé sur le bouton « Précédent ») et répond en réinitialisant la machine d’état d’authentification et en appelant le rappel [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) avec le code d’erreur `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR`.

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

**Paramètres:** Aucun

**Rappels déclenchés :** `setAuthenticationStatus:errorCode:`,`sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[Haut de la page...](#apis)

</br>

#### navigateToUrl : {#nav2url}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description :** rappel déclenché par AccessEnabler pour demander à votre application d&#39;instancier un contrôleur UIWebView/WKWebView et de charger l&#39;URL fournie dans le paramètre **`url`** du rappel. Le rappel transmet le paramètre **`url`** qui représente l’URL du point d’entrée d’authentification ou l’URL du point d’entrée de déconnexion.

Comme le contrôleur UIWebView/WKWebView` `Controller passe par plusieurs redirections, votre application doit surveiller l&#39;activité du contrôleur et détecter le moment où il charge une URL personnalisée spécifique définie par la `ADOBEPASS_REDIRECT_URL `constante (c&#39;est-à-dire `adobepass://ios.app`). Notez que cette URL personnalisée spécifique n&#39;est pas valide et qu&#39;elle n&#39;est pas destinée à être chargée par le contrôleur. Il doit être interprété par votre application uniquement comme un signal indiquant que l’authentification ou le flux de déconnexion est terminé et qu’il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer UIWebView/WKWebView et appeler la méthode `handleExternalURL:url `API d&#39;AccessEnabler.

**Remarque :** Notez que dans le cas du flux d’authentification, il s’agit d’un point où l’utilisateur a la possibilité d’appuyer sur le bouton « Précédent », ce qui équivaut à l’abandon du flux d’authentification. Dans un tel scénario, votre application doit appeler la méthode [setSelectedProvider:](#setSelProv) en transmettant **`nil`** comme paramètre et en donnant une chance à AccessEnabler de réinitialiser son ordinateur d&#39;état d&#39;authentification.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : affichage de la page de connexion à MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres** :

* *url* : URL pointant vers la page de connexion de MVPD

**Déclenché par :** [setSelectedProvider:](#setSelProv)



[Haut de la page...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description :** rappel déclenché par AccessEnabler au lieu du rappel `navigateToUrl:` au cas où votre application aurait activé la gestion manuelle du contrôleur de vue Safari (SVC) via l’appel [setOptions(\[« handleSVC »:true »\])](#setOptions) et uniquement dans le cas de MVPD nécessitant le contrôleur de vue Safari (SVC). Pour tous les autres MVPD, le rappel `navigateToUrl:` est appelé. Veuillez consulter la prise en charge [ SFSafariViewController sur iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md) pour plus d’informations sur la manière dont Safari View Controller (SVC) doit être géré.

Tout comme le rappel `navigateToUrl:`, le `navigateToUrl:useSVC:` est déclenché par AccessEnabler pour demander à votre application d’instancier un contrôleur de `SFSafariViewController` et de charger l’URL fournie dans le paramètre **`url`** du rappel. Le rappel transmet le paramètre **`url`** qui représente l’URL du point d’entrée d’authentification ou l’URL du point d’entrée de déconnexion, ainsi que le paramètre **`useSVC`** qui spécifie que l’application doit utiliser un `SFSafariViewController`.

Comme le contrôleur de `SFSafariViewController` passe par plusieurs redirections, votre application doit surveiller l&#39;activité du contrôleur et détecter le moment où il charge une URL personnalisée spécifique définie par votre `application's custom scheme` (par exemple ** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Notez que cette URL personnalisée spécifique n&#39;est pas valide et qu&#39;elle n&#39;est pas destinée à être chargée par le contrôleur. Il doit être interprété par votre application uniquement comme un signal indiquant que l’authentification ou le flux de déconnexion est terminé et qu’il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer le `SFSafariViewController` et appeler la méthode `handleExternalURL:url `API d&#39;AccessEnabler.

**Remarque :** Notez que dans le cas du flux d’authentification, il s’agit d’un point où l’utilisateur a la possibilité d’appuyer sur le bouton « Précédent », ce qui équivaut à l’abandon du flux d’authentification. Dans un tel scénario, votre application doit appeler la méthode [setSelectedProvider:](#setSelProv) en transmettant **`nil`** comme paramètre et en donnant une chance à AccessEnabler de réinitialiser son ordinateur d&#39;état d&#39;authentification.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : affichage de la page de connexion MVPD dans SFSafariViewController</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :**v 3.2+

**Paramètres** :

* *url :* URL pointant vers la page de connexion de MVPD
* *useSVC :* si l’URL doit être chargée dans SFSafariViewController.

**Déclenché par:**[ setOptions:](#setOptions) avant [setSelectedProvider:](#setSelProv)

[Haut de la page...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** cette méthode est appelée par votre application pour terminer le flux d’authentification ou de déconnexion. Cette méthode doit être appelée juste après que votre application détecte le moment où le contrôleur de `UIWebView/WKWebView or SFSafariViewController` est redirigé vers une URL personnalisée spécifique. Si votre application doit utiliser un `SFSafariViewController `contrôleur, l’URL personnalisée spécifique est définie par votre `application's custom scheme` (par exemple, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Dans le cas contraire, cette URL personnalisée spécifique est définie par la `ADOBEPASS_REDIRECT_URL `constante (par exemple, `adobepass://ios.app`).

Dans le cas du flux d’authentification, AccessEnabler termine le flux en récupérant le jeton d’authentification du serveur principal et en le stockant localement dans le stockage des jetons. AccessEnabler informe votre application que le flux d’authentification est terminé en appelant le rappel `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> avec un code d’état de 1, indiquant la réussite. En cas d’erreur lors de l’exécution de ces étapes, le rappel `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> est déclenché avec un code d’état de 0 indiquant l’échec de l’authentification, ainsi qu’un code d’erreur correspondant.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : terminer l'authentification ou le flux de déconnexion</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v3.0+

**Paramètres:**

* *url* : URL interceptée à partir du contrôle ` UIWebView/WKWebView or SFSafariViewController ` en tant que chaîne.


**Rappels déclenchés :** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Haut de la page...](#apis)

</br>

#### getAuthenticationToken - [OBSOLÈTE] {#getAuthNToken}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** termine le flux d’authentification en demandant le jeton d’authentification au serveur principal. Cette méthode ne doit être appelée par votre application qu’en réponse à un événement où le contrôle WebView hébergeant la page de connexion MVPD est redirigé vers l’URL personnalisée définie par la constante `ADOBEPASS_REDIRECT_URL`.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : récupération du jeton d'authentification</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+ **Jusqu’au :** v3.0

**Paramètres:** Aucun

**Rappels déclenchés :** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Haut de la page...](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui informe l’application du statut du flux d’authentification. Il existe de nombreux cas où ce flux peut échouer, soit en raison de l’interaction de l’utilisateur ou d’autres scénarios imprévus (par exemple, des problèmes de connectivité réseau, etc.). Ce rappel informe l’application de l’état de succès/échec du flux d’authentification, tout en fournissant des informations supplémentaires sur la raison de l’échec, le cas échéant.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : rapport du statut du flux d’authentification</th>
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

**Paramètres** :

* *status* : peut prendre l&#39;une des valeurs suivantes :
   * `ACCESS_ENABLER_STATUS_SUCCESS` - flux d’authentification terminé avec succès
   * `ACCESS_ENABLER_STATUS_ERROR` - échec du flux d’authentification
* *code* : raison de l’échec. Si *status* est `ACCESS_ENABLER_STATUS_SUCCESS`, alors *code* est une chaîne vide (c’est-à-dire définie par la constante `USER_AUTHENTICATED`). En cas d’échec, ce paramètre peut prendre l’une des valeurs suivantes :
   * `USER_NOT_AUTHENTICATED_ERROR` - L’utilisateur n’est pas authentifié. En réponse à l’appel de la méthode [checkAuthentication:](#checkAuthN) lorsqu’il n’existe aucun jeton d’authentification valide dans le cache de jetons local.
   * `PROVIDER_NOT_SELECTED_ERROR` - L&#39;AccessEnabler a réinitialisé       machine d’état d’authentification après l’application de la couche supérieure       a transmis *null* à [`setSelectedProvider:`](#setSelProv) pour abandonner le flux d’authentification.  L’utilisateur a probablement annulé le flux d’authentification (c’est-à-dire qu’il a appuyé sur le bouton « Précédent »).
   * `GENERIC_AUTHENTICATION_ERROR` - Le flux d’authentification a échoué pour des raisons telles que l’indisponibilité du réseau ou l’annulation explicite du flux d’authentification.

**Déclenché par :** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Haut de la page...](#apis)

</br>

### checkPreauthorizedResources : {#checkPreauth}


**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** cette méthode est utilisée par l’application pour déterminer si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques. L’objectif principal de cette méthode est de récupérer des informations à utiliser pour décorer la **de l’interface utilisateur (par exemple, en indiquant le statut d’accès avec des icônes de verrouillage et de déverrouillage).**

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

**Paramètres:**

* *resources:* tableau de ressources pour lesquelles l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’identifiant de la ressource. Le     L’ID de ressource est soumis aux mêmes limitations que l’ID de ressource dans l’appel à , c’est-à-dire qu’il doit s’agir d’une valeur convenue entre le programmeur et le MVPD ou d’un fragment RSS de média.

**Rappel déclenché :** [`preauthorizedResources:`](#preauthResources)

[Haut de la page...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** cette méthode est utilisée par l’application pour déterminer si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques. L’objectif principal de cette méthode est de récupérer des informations à utiliser pour décorer l’interface utilisateur (par exemple, en indiquant le statut d’accès avec des icônes de verrouillage et de déverrouillage). Le paramètre **cache** contrôle si le cache interne est utilisé pour résoudre les ressources.

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



**Paramètres:**

* *resources:* tableau de ressources pour lesquelles l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’identifiant de la ressource. L’ID de ressource est soumis aux mêmes limitations que l’ID de ressource dans l’appel `getAuthorization:`. En d’autres termes, il doit s’agir d’une valeur convenue entre le programmeur et le MVPD ou d’un fragment RSS de média.
* *cache :* booléen spécifiant s’il faut utiliser le cache interne pour résoudre les ressources. Si la valeur est false, le cache est contourné, ce qui entraîne des appels au serveur chaque fois que cette API est appelée.

**Rappel déclenché :** [`preauthorizedResources:`](#preauthResources)

[Haut de la page...](#apis)

</br>

### preauthorizedResources : {#preauthResources}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description : rappel** déclenché par `checkPreauthorizedResources:`. Fournit une liste des ressources que l’utilisateur est déjà autorisé à afficher.

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

**Paramètres:**

* `resources` : tableau de ressources que l’utilisateur est déjà autorisé à afficher.

**Déclenché par :** [`checkPreauthorizedResources:`](#checkPreauth)



[Haut de la page...](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** cette méthode est utilisée par l’application pour vérifier le statut de l’autorisation. Il commence par vérifier d’abord le statut de l’authentification. S’il n’est pas authentifié, le rappel [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) est déclenché et la méthode se ferme. Si l’utilisateur est authentifié, cela déclenche également le flux d’autorisation. Voir les détails sur la méthode [`getAuthorization:`](#getAuthZ).


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : vérification du statut d'autorisation</th>
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
<th>Appel API : vérification du statut d'autorisation</th>
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

**Paramètres:**

* *resource* : identifiant de la ressource pour laquelle l’utilisateur demande une autorisation.
* *data* : dictionnaire composé de paires clé-valeur à envoyer au service de pass de télévision à péage. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Haut de la page...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** cette méthode est utilisée par l’application pour lancer le flux d’autorisation. Si l’utilisateur n’est pas déjà authentifié, il lance également le flux d’authentification. Si l’utilisateur est authentifié, AccessEnabler émet des demandes pour le jeton d’autorisation (si aucun jeton d’autorisation valide n’est présent dans le cache de jetons local) et pour le jeton de média de courte durée. Une fois le jeton de média court obtenu, le flux d’autorisation est considéré comme terminé. Le rappel [`setToken:forResource:`](#setToken) est déclenché et le jeton de média court est fourni en tant que paramètre à l’application. Si, pour une raison quelconque, l’autorisation échoue, le rappel [`tokenRequestFailed:forEventType:`](#tokenReqFailed) est déclenché et le code/les détails de l’erreur sont fournis.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : lancement du flux d’autorisation</th>
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
<th>Appel API : lancement du flux d’autorisation</th>
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

**Paramètres:**

* *resource* : identifiant de la ressource pour laquelle l’utilisateur demande une autorisation.
* *data* : dictionnaire composé de paires clé-valeur à envoyer au service de pass de télévision à péage. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Rappels supplémentaires déclenchés :**\
Cette méthode peut également déclencher les rappels suivants (si le flux d’authentification est également initié) : `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Utilisez `checkAuthorization:` / `checkAuthorization:withData:` au lieu de `getAuthorization:` / `getAuthorization:withData:` dans la mesure du possible. La méthode `getAuthorization:` / `getAuthorization:withData:` démarre un flux d’authentification complet (si l’utilisateur n’est pas authentifié), ce qui peut entraîner une implémentation complexe du côté du programmeur.

[Haut de la page...](#apis)

</br>

### `setToken:forResource:` {#setToken}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui informe votre application que le flux d’autorisation a été terminé avec succès. Le jeton de média de courte durée est également diffusé en tant que paramètre.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : flux d’autorisation terminé avec succès</th>
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

**Paramètres** :

* *token* : jeton de média de courte durée
* *resource* : ressource pour laquelle l’autorisation a été obtenue

**Déclenché par :** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Haut de la page...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui informe l’application de couche supérieure de l’échec du flux d’autorisation.

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

**Paramètres** :

* *resource* : ressource pour laquelle l’autorisation a été obtenue.
* *code* : code d’erreur associé au scénario d’échec. Valeurs possibles :
   * `USER_NOT_AUTHORIZED_ERROR` - l&#39;utilisateur n&#39;a pas pu autoriser
pour la ressource donnée
* *description* : informations supplémentaires sur le scénario d’échec. Si cette chaîne descriptive n’est disponible pour aucune raison, l’authentification Adobe Pass envoie une chaîne vide **(«  »)**.\
  Cette chaîne peut être utilisée par un MVPD pour transmettre des messages d’erreur personnalisés ou des messages liés aux ventes. Par exemple, si l’autorisation d’accès à une ressource est refusée à un abonné, le MVPD peut envoyer un message du type : « Vous n’avez pas accès à ce canal dans votre package. Si vous souhaitez mettre à niveau votre package, cliquez **ici**. » Le message est transmis par l’authentification Adobe Pass via ce rappel au programmeur, qui a la possibilité de l’afficher ou de l’ignorer. L’authentification Adobe Pass peut également utiliser ce paramètre pour fournir une notification de la condition qui a pu entraîner une erreur. Par exemple, « Une erreur réseau s’est produite lors de la communication avec le service d’autorisation du fournisseur ».

**Déclenché par :** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Haut de la page...](#apis)

</br>

### déconnexion {#logout}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** cette méthode est appelée par votre application pour lancer le flux de déconnexion. La déconnexion est le résultat d’une série d’opérations de redirection HTTP en raison du fait que l’utilisateur doit être déconnecté des serveurs d’authentification Adobe Pass et des serveurs de MVPD. Comme ce flux ne peut pas être exécuté avec une simple requête HTTP émise par la bibliothèque AccessEnabler, un contrôleur `UIWebView/WKWebView or SFSafariViewController` doit être instancié pour pouvoir suivre les opérations de redirection HTTP.

Le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir de quelque manière que ce soit avec le contrôleur de `UIWebView/WKWebView or SFSafariViewController`. Par conséquent, Adobe recommande de rendre le contrôle invisible (c’est-à-dire masqué) pendant le processus de déconnexion.

Un motif similaire au flux d’authentification est utilisé. IOS AccessEnabler déclenche le rappel `navigateToUrl:` ou le `navigateToUrl:useSVC:` pour créer un contrôleur de `UIWebView/WKWebView or SFSafariViewController` et charger l’URL fournie dans le paramètre `url` du rappel. Il s’agit de l’URL du point d’entrée de déconnexion sur le serveur principal. Pour tvOS AccessEnabler, ni le rappel `navigateToUrl:` ni le rappel `navigateToUrl:useSVC:` n’est appelé.

Comme il passe par plusieurs redirections, votre application doit surveiller l&#39;activité du contrôleur `UIWebView/WKWebView or SFSafariViewController ` et détecter le moment où il charge une URL personnalisée spécifique. Notez que cette URL personnalisée spécifique n&#39;est pas valide et qu&#39;elle n&#39;est pas destinée à être chargée par le contrôleur. Il doit être interprété par votre application uniquement comme un signal indiquant que le flux de déconnexion est terminé et qu&#39;il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer le contrôleur et appeler la méthode `handleExternalURL:url `API AccessEnabler. Si votre application doit utiliser un `SFSafariViewController `contrôleur, l’URL personnalisée spécifique est définie par votre `application's custom scheme` (par exemple, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Dans le cas contraire, cette URL personnalisée spécifique est définie par la `ADOBEPASS_REDIRECT_URL `constante (par exemple, `adobepass://ios.app`).

À la fin, AccessEnabler appelle le rappel [`setAuthenticationStatus()`](#setAuthNStatus) avec un code d&#39;état de 0, indiquant le succès du flux de déconnexion.

**Remarque :** si l’utilisateur est connecté à l’aide de l’authentification unique Apple, le statut de VSA203 est déclenché. Si c’est le cas, l’utilisateur doit être invité à se déconnecter également des paramètres système. Si vous ne le faites pas, une nouvelle authentification sera effectuée lors du relancement de l’application.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : lancement du flux de déconnexion</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres:** Aucun

**Rappels déclenchés :** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Haut de la page...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** utilisez cette méthode pour déterminer le fournisseur actuellement sélectionné.

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

**Paramètres:** Aucun

**Rappels déclenchés :** [`selectedProvider:`](#selProv)

[Haut de la page...](#apis)

</br>

### selectedProvider {#selProv}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par l’AccessEnabler qui fournit des informations sur le MVPD actuellement sélectionné à l’application.

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

**Paramètres** :

* *mvpd* : objet contenant des informations sur le MVPD actuellement sélectionné

**Déclenché par :** [`getSelectedProvider`](#getSelProv)

[Haut de la page...](#apis)

</br>

### getMetadata: {#getMeta}

**Fichier:** AccessEnabler/headers/AccessEnabler.h

**Description :** utilisez cette méthode pour récupérer des informations exposées en tant que métadonnées par la bibliothèque AccessEnabler. L’application peut accéder à ces données en fournissant un paramètre d’entrée *clé* basé sur un dictionnaire.

Les programmeurs ont accès à deux types de métadonnées :

* Métadonnées statiques (TTL de jeton d’authentification, TTL de jeton d’autorisation et ID d’appareil)
* Métadonnées utilisateur (informations spécifiques à l’utilisateur, telles que l’identifiant utilisateur ou le code postal ; peuvent être transmises d’un MVPD à l’appareil d’un utilisateur pendant les flux d’authentification et d’autorisation)

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Appel API : requête de métadonnées à AccessEnabler</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v1.0+

**Paramètres:**

* *keyDictionary* : structure de données de dictionnaire avec les éléments suivants.
format :
   * Si la clé est `METADATA_OPCODE_KEY` et que la valeur est `METADATA_AUTHENTICATION`, la requête est exécutée pour obtenir le délai d’expiration du jeton d’authentification.
   * Si la clé est `METADATA_OPCODE_KEY` et que la valeur est `METADATA_AUTHORIZATION` **et**\
     La clé est `METADATA_RESOURCE_ID_KEY` et la valeur est un identifiant de ressource particulier, puis la requête est effectuée pour obtenir le délai d’expiration du jeton d’autorisation associé à la ressource spécifiée.
   * Si la clé est `METADATA_OPCODE_KEY` et que la valeur est `METADATA_DEVICE_ID`, la requête est exécutée pour obtenir l’identifiant d’appareil actuel. Notez que cette fonctionnalité est désactivée par défaut et les programmeurs doivent contacter l’Adobe pour plus d’informations sur l’activation et les frais.
   * Si la clé est `METADATA_OPCODE_KEY` et que la valeur est `METADATA_USER_META` **et** la clé est `METADATA_USER_META_KEY` et que la valeur est le nom des métadonnées, la requête porte sur les métadonnées de l’utilisateur. Liste des types de métadonnées utilisateur disponibles :
      * `zip` - Liste des codes postaux
      * `householdID` - Identifiant du ménage. Dans le cas où un MVPD ne prend pas en charge les sous-comptes, celui-ci est identique à `userID`.
      * `maxRating` - Ensemble de notes parentales maximales attribuées à l’utilisateur
      * `userID` - Identifiant de l’utilisateur. Si un MVPD prend en charge les sous-comptes et que l’utilisateur n’est pas le compte principal, `userID` sera différent de `householdID.`
      * `channelID` - Liste des canaux qu’un utilisateur est autorisé à afficher.

  >[!NOTE]
  >
  >Les métadonnées d’utilisateur réellement disponibles pour un programmeur dépendent de ce qu’un MVPD rend disponible. Cette liste sera étendue à mesure que de nouvelles métadonnées seront disponibles et ajoutées au système d’authentification Adobe Pass.

**Rappels déclenchés :** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Informations supplémentaires :** [Métadonnées utilisateur](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

[Haut de la page...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler après avoir appelé [getAuthentication()](#getAuthN) si le demandeur actuel prend en charge au moins un MVPD avec prise en charge de l’authentification unique.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : résultat des flux SSO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v2.0+

**Paramètres** :

* viewController : représente la boîte de dialogue de l’authentification unique Apple. Ce viewController doit être affiché à l&#39;écran.

**Déclenché par :** [`getAuthentication`](#getAuthN)

**Informations supplémentaires :** [Authentification unique iOS/tvOS](#presentTvDialog)

[Haut de la page...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler après la fermeture de la boîte de dialogue SSO d’Apple par l’utilisateur.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : résultat des flux SSO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilité :** v2.0+

**Paramètres** :

* viewController : représente la boîte de dialogue de l’authentification unique Apple. Ce viewController doit être supprimé de l&#39;écran.

**Déclenché par :** action de l’utilisateur

**Informations supplémentaires :** [Authentification unique iOS/tvOS](#presentTvDialog)

[Haut de la page...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par l’AccessEnabler qui fournit les métadonnées demandées par le biais d’un appel [`getMetadata:`](#getMeta).

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rappel : résultat de la requête de récupération des métadonnées</th>
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

**Paramètres** :

* *metadata* : les métadonnées demandées. Cette valeur est une `NSString` dans le cas de métadonnées statiques (TTL d’authentification, TTL d’autorisation, ID d’appareil).  Il s’agit d’un objet complexe lors de la demande de métadonnées spécifiques à l’utilisateur. Cet objet complexe est généralement la représentation Objective-C d’une payload JSON (par exemple, &#39;{« street »: « Main Avenue », « bâtiments »: [« 150 », « 320 »]}&#39; est traduit dans Objective-C par NSDictionary(« street » -> « Main Avenue », « bâtiments » -> NSArray(« 150 », « 320 »))).   Exemple d’objet JSON de métadonnées :

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

* *encrypted* : valeur booléenne qui spécifie si les métadonnées récupérées sont chiffrées ou non. Ce paramètre est important uniquement pour les requêtes de métadonnées d’utilisateur. Il n’a aucune signification pour les métadonnées statiques (par exemple, la TTL d’authentification) qui sont toujours reçues non chiffrées. Si ce paramètre est défini sur true, il appartient au programmeur d’obtenir la valeur non chiffrée des métadonnées de l’utilisateur en effectuant un déchiffrement RSA à l’aide de la clé privée placée sur la liste blanche (la même clé privée que celle utilisée pour la signature de l’ID du demandeur dans les appels [`setRequestor:setSignedRequestorId:`](#setReq) et `setRequestor:setSignedRequestorId:serviceProviders: `).

* *key* : clé utilisée pour formuler la requête de récupération des métadonnées.

* *arguments* : dictionnaire transmis à l’appel [`getMetadata:`](#getMeta). Cette opération permet à l’application de faire correspondre les requêtes aux réponses.

**Déclenché par :** [`getMetadata:`](#getMeta)

**Informations supplémentaires :** [Métadonnées utilisateur](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)


[Haut de la page...](#apis)

</br>

### MVPD {#mvpd}

**Fichier:** AccessEnabler/headers/model/MVPD.h

**Description** décrit l’objet MVPD. Peut être utilisé pour obtenir des informations sur les propriétés de MVPD.

**Disponibilité :** version 1.0+ [la propriété boardingStatus est disponible dans la version 2.2]

**Propriétés** :

* (NSString) ID : ID MVPD.
* (NSString) displayName : nom du MVPD. [À utiliser pour l’affichage dans le sélecteur]
* (NSString) logoURL : adresse du logo MVPD.
* (BOOL) enablePlatformServices : si la valeur est true, le MVPD prend en charge les services SSO tels que [Apple SSO](#presentTvDialog).
* (NSString) boardingStatus - Peut avoir 3 valeurs :
   * néant - Le MVPD ne prend pas en charge l’authentification unique (SSO) Apple.
   * SÉLECTEUR - Le MVPD peut apparaître dans le sélecteur Apple, mais le flux d’authentification est effectué par Adobe.
   * PRIS EN CHARGE - Le MVPD est entièrement pris en charge par Apple et utilisera le jeton SSO d’Apple.

[Haut de la page...](#apis)

</br>

## Suivi des événements {#tracking}

AccessEnabler déclenche un rappel supplémentaire qui n’est pas nécessairement lié aux flux de droits. L’implémentation de la fonction de rappel [`sendTrackingData()`](#sendTracking) est facultative, mais elle permet à l’application de suivre des événements spécifiques et de compiler des statistiques telles que le nombre de tentatives d’authentification/d’autorisation réussies/ayant échoué.

### sendTrackingData:forEventType: {#sendTracking}

**Fichier:** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par l’AccessEnabler signalant à l’application l’occurrence de divers événements tels que l’achèvement/l’échec des flux d’authentification/d’autorisation. Avec Adobe Pass Authentication 1.6, le type d’appareil, le type de client AccessEnabler et le système d’exploitation sont signalés par [`sendTrackingData()`](#sendTracking). Le rappel [`sendTrackingData()`](#sendTracking) reste rétrocompatible.

**Rappel : suivi des événements**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Disponibilité :** v1.0+

**Remarque :** le type d’appareil et le système d’exploitation sont dérivés à l’aide d’une bibliothèque Java publique (<http://java.net/projects/user-agent-utils>) et de la chaîne de l’agent utilisateur. Notez que ces informations ne sont fournies qu’à titre approximatif pour ventiler les mesures opérationnelles en catégories d’appareils, mais que l’Adobe ne peut être tenu responsable de résultats incorrects. Veuillez utiliser la nouvelle fonctionnalité en conséquence.

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


**Paramètres** :

* *event* : code de l’événement qui fait l’objet d’un suivi. Il existe trois types d&#39;événements de tracking possibles :
   * **authorizationDetection :** chaque fois qu’une demande de jeton d’autorisation est renvoyée (événement `TRACKING_AUTHORIZATION`)
   * **authenticationDetection :** chaque fois qu’une vérification d’authentification se produit (l’événement est `TRACKING_AUTHENTICATION`)
   * **mvpdSelection :** lorsque l’utilisateur sélectionne un MVPD dans le formulaire de sélection MVPD (l’événement est `TRACKING_GET_SELECTED_PROVIDER`)
* *data* : données supplémentaires associées à l’événement signalé. Ces données sont présentées sous la forme d’une liste de valeurs.

**Déclenché par :** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Instructions pour interpréter les valeurs du tableau *data* :

* Pour trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Indique si la demande de jeton a réussi (true/false) et, si elle a réussi :
   * **1** - Chaîne d’identifiant MVPD
   * **2** - GUID (md5 haché)
   * **3** - Jeton déjà présent dans le cache (true/false)
   * **4** - Type d’appareil
   * **5** - Type de client AccessEnabler
   * **6** - Type de système d’exploitation

* Pour trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Indique si la demande de jeton a réussi (true/false) et, si elle a réussi :
   * **1** - MVPD ID
   * **2** - GUID (md5 haché)
   * **3** - Jeton déjà présent dans le cache (true/false)
   * **4** - Erreur
   * **5** - Détails
   * **6** - Type d’appareil
   * **7** - Type de client AccessEnabler
   * **8** - Type de système d’exploitation
* Pour trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID du MVPD actuellement sélectionné
   * **1** - Type d’appareil
   * **2** - Type de client AccessEnabler
   * **3** - Type de système d’exploitation

</br>
