---
title: Référence de l’API iOS/tvOS
description: Référence de l’API iOS/tvOS
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '6933'
ht-degree: 0%

---

# Référence de l’API du SDK iOS/tvOS {#iostvos-sdk-api-reference}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

Cette page décrit les méthodes et les fonctions de rappel exposées par le client natif iOS/tvOS pour l’authentification Adobe Pass. Les méthodes et fonctions de rappel décrites ici sont définies dans les fichiers d’en-tête `AccessEnabler.h` et `EntitlementDelegate.h` ; vous pouvez les trouver dans le SDK iOS AccessEnabler ici : `[SDK directory]/AccessEnabler/headers/api/`


Documentation associée :

* Pour une description du flux de droits d’authentification Adobe Pass de base, voir [Flux de droits](/help/authentication/entitlement-flow.md).
* Présentation détaillée de la mise en oeuvre d’Adobe Pass
flux de droits d’authentification à l’aide de cette API, consultez le [guide de l’intégration iOS](/help/authentication/iostvos-sdk-cookbook.md).
* Pour obtenir le dernier SDK iOS AccessEnabler, voir la [bibliothèque iOS Native Access Enabler](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe vous encourage à utiliser uniquement les API *public* d’authentification Adobe Pass :
>
>* Les API publiques sont disponibles et entièrement testées sur tous les types de clients pris en charge. Pour toute fonction publique, nous nous assurons que chaque type de client possède une version correspondante de la ou des méthodes associées.
>* Les API publiques doivent être aussi stables que possible, pour prendre en charge la compatibilité descendante et garantir que les intégrations de partenaires ne se rompent pas. Cependant, pour les API non publiques, nous nous réservons le droit de modifier leur signature à tout moment futur. Si vous rencontrez un flux particulier qui ne peut pas être pris en charge par une combinaison des appels publics actuels de l’API d’authentification Adobe Pass, la meilleure approche consiste à nous le faire savoir. En tenant compte de vos besoins, nous pouvons modifier les API publiques et fournir une solution stable à l’avenir.

</br>

## Référence d’API {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - Instancie l’objet AccessEnabler.

* **[DEPRECATED]** [init](#init) - Instancie l’objet AccessEnabler.

* [`setOptions:options:`](#setOptions) - Configure les options du SDK global telles que profile ou visitorID.

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - Établit l’identité du programmeur.

* **[OBSOLÈTE]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - Établit l’identité du programmeur.

* **[OBSOLÈTE]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos)-Établit l’identité du programmeur.

* [`setRequestorComplete:`](#setReqComplete) - Informe votre application que la phase de configuration est terminée.

* [`checkAuthentication`](#checkAuthN) - Vérifie l’état d’authentification de l’utilisateur actuel.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN) - Démarre le workflow d’authentification complet.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN)[andFilter](#getAuthN_filter) - Démarre le workflow d’authentification complète.

* [`displayProviderDialog:`](#dispProvDialog) - Informe votre application d’instancier les éléments d’IU appropriés pour que l’utilisateur puisse sélectionner un MVPD.

* [`setSelectedProvider:`](#setSelProv) - Informe le AccessEnabler de la sélection MVPD de l’utilisateur.

* [`navigateToUrl:`](#nav2url) - Informe votre application que la page de connexion MVPD doit être présentée à l’utilisateur.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - Informe votre application que l’utilisateur doit se voir présenter la page de connexion MVPD, à l’aide de SFSafariViewController

* [`handleExternalURL:url`](#handleExternalURL) - Termine le flux d’authentification/déconnexion.

* **[OBSOLÈTE]** [`getAuthenticationToken`](#getAuthNToken) - Demande le jeton d’authentification auprès du serveur principal.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) - Informe votre application de l’état du flux d’authentification.

* [`checkPreauthorizedResources:`](#checkPreauth) - Détermine si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - Détermine si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques.

* [`preauthorizedResources:`](#preauthResources) - Fournit une liste des ressources que l’utilisateur est déjà autorisé à afficher.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - Vérifie l’état d’autorisation de l’utilisateur actuel.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - Lance le flux d’autorisation.

* [`setToken:forResource:`](#setToken) - Informe votre application que le flux d’autorisation a été terminé avec succès.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) - Indique à votre application que le flux d’autorisation a échoué.

* [`logout`](#logout) - Lance le flux de déconnexion.

* [`getSelectedProvider`](#getSelProv) - Détermine le fournisseur actuellement sélectionné.

* [`selectedProvider:`](#selProv) - Fournit des informations sur le MVPD actuellement sélectionné dans votre application.

* [`getMetadata:`](#getMeta) - Récupère des informations exposées en tant que métadonnées par la bibliothèque AccessEnabler.

* [`presentTvProviderDialog:`](#presentTvDialog) - Informe votre application d’afficher la boîte de dialogue SSO Apple.

* [`dismissTvProviderDialog:`](#dismissTvDialog) - Informe votre application de masquer la boîte de dialogue SSO Apple.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - Fournit les métadonnées demandées par un appel [`getMetadata:`](#getMeta).

* [`sendTrackingData:forEventType:`](#sendTracking) - Fournit des informations sur les données de suivi.

* [`MVPD`](#mvpd) - Classe MVPD. [Contient des informations sur le MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** instancie l’objet AccessEnabler. Il doit y avoir une seule instance AccessEnabler par instance d&#39;application.

| **appel API : constructeur iOS AccessEnabler** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Disponibilité :** v3.0+

**Paramètres :**

* **softwareStatement :** Chaîne qui identifie l’application dans le système de l’Adobe. Découvrez comment obtenir un relevé logiciel.

[Retour au sommet...](#apis)



### init - [OBSOLÈTE]{#init}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** instancie l’objet AccessEnabler. Il doit y avoir une seule instance AccessEnabler par instance d&#39;application.

| Appel API : constructeur iOS AccessEnabler |
| --- |
| ```- (id) init;``` |

**Disponibilité :** v1.0+ **Jusqu’à :** v3.0

**Paramètres :** Aucun

[Retour au sommet...](#apis)

</br>

### setOptions:options {#setOptions}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** configure les options du SDK global. Il accepte un NSDictionary comme argument. Les valeurs du dictionnaire seront transmises au serveur avec chaque appel réseau effectué par le SDK.

**Remarque :** Les valeurs seront transmises au serveur indépendamment du flux actuel (authentification/autorisation). Si vous souhaitez modifier les valeurs, vous pouvez appeler cette méthode à tout moment.

| Appel API : setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Disponibilité :** v2.3.0+

**Paramètres :**

* *options* : NSDictionary contenant les options du SDK global. Actuellement, les options suivantes sont disponibles :
   * **applicationProfile** - Il peut être utilisé pour créer des configurations de serveur en fonction de cette valeur.
   * **visitorID** - Service d’identification des Experience Cloud. Cette valeur peut être utilisée ultérieurement pour les rapports d’analyse avancés.
   * **handleSVC** - Valeur booléenne indiquant si le programmeur gérera les SFSafariViewContrôers. Pour plus d’informations, voir [Prise en charge de SFSafariViewController sur le SDK iOS 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) .
      * S’il est défini sur **false,**, le SDK présentera automatiquement à l’utilisateur final un SFSafariViewController. Le SDK accède ensuite à l’URL de la page de connexion des MVPD.
      * Si elle est définie sur **true,**, le SDK **NOT** présente automatiquement à l’utilisateur final un SFSafariViewController. Le SDK déclenche également **navigate(toUrl:{url}, useSVC:YES)**.
* **device\_info** - Informations sur le client comme décrit dans la section [Transfert d’informations sur le client](/help/authentication/passing-client-information-device-connection-and-application.md).

[Retour au sommet...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Établit l’identité du programmeur. Un identifiant unique est attribué à chaque programmeur lors de l’enregistrement auprès de l’Adobe pour le système d’authentification Adobe Pass. Lorsque vous traitez d’SSO et de jetons distants, l’état d’authentification peut changer lorsque l’application est en arrière-plan, setRequestor peut être appelé de nouveau lorsque l’application est mise en premier plan afin de se synchroniser avec l’état du système (récupération d’un jeton distant si SSO est activé ou suppression du jeton local si une déconnexion s’est produite entre-temps).

La réponse du serveur contient une liste de MVPD ainsi que certaines informations de configuration qui sont jointes à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code AccessEnabler. Seul l’état de l’opération (c’est-à-dire SUCCESS/FAIL) est présenté à votre application via le rappel `setRequestorComplete:`.

Si le paramètre `urls` n’est pas utilisé, l’appel réseau obtenu cible l’URL du fournisseur de services par défaut : l’environnement de LIBÉRATION/production de l’Adobe.


Si une valeur est fournie pour le paramètre `urls` , l’appel réseau obtenu cible toutes les URL fournies dans le paramètre `urls`. Toutes les requêtes de configuration sont déclenchées simultanément dans des threads distincts. Le premier participant a la priorité lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, AccessEnabler mémorise l&#39;URL du prestataire associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.

| Appel API : configuration du demandeur |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Disponibilité :** v3.0+

| Appel API : configuration du demandeur |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Disponibilité :** v3.0+

**Paramètres :**

* *requestorID* : identifiant unique associé au programmeur. Transmettez l’identifiant unique attribué par Adobe à votre site lorsque vous vous inscrivez pour la première fois auprès du service d’authentification Adobe Pass.
* *urls* : paramètre facultatif ; par défaut, le fournisseur de services Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points de terminaison pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances du fournisseur de services d’authentification Adobe Pass. Dans ce cas, la liste MVPD est composée des points de terminaison de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.

>[!NOTE]
>
>Si elle est appelée sans le paramètre `serviceProviders` , la bibliothèque récupère la configuration auprès du fournisseur de services par défaut (c’est-à-dire `https://sp.auth.adobe.com` pour le profil de production ou `https://sp.auth-staging.adobe.com` pour le profil d’évaluation). Si le paramètre `serviceProviders` est fourni, il doit s’agir d’un tableau d’URL. Les informations de configuration sont récupérées à partir de tous les points de terminaison spécifiés et sont fusionnées. Si des informations en double existent dans les réponses des différents fournisseurs de services, le conflit est résolu en faveur du serveur qui répond le plus rapidement (c’est-à-dire que le serveur avec le temps de réponse le plus court est prioritaire).

**Rappels déclenchés :** [`setRequestorComplete:`](#setReqComplete)

[Retour au sommet...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [OBSOLÈTE] {#setReq}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Établit l’identité du programmeur. Un identifiant unique est attribué à chaque programmeur lors de l’enregistrement auprès de l’Adobe pour le système d’authentification Adobe Pass. Lorsque vous traitez de SSO et de jetons distants, l’état d’authentification peut changer lorsque l’application est en arrière-plan, setRequestor peut être appelé de nouveau lorsque l’application est mise en premier plan afin de se synchroniser avec l’état du système (récupération d’un jeton distant si SSO est activé ou suppression du jeton local si une déconnexion s’est produite entre-temps).

La réponse du serveur contient une liste de MVPD ainsi que certaines informations de configuration qui sont jointes à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code AccessEnabler. Seul l’état de l’opération (c’est-à-dire SUCCESS/FAIL) est présenté à votre application via le rappel `setRequestorComplete:`.

Si le paramètre `urls` n’est pas utilisé, l’appel réseau obtenu cible l’URL du fournisseur de services par défaut : l’environnement de LIBÉRATION/production de l’Adobe.

Si une valeur est fournie pour le paramètre `urls` , l’appel réseau obtenu cible toutes les URL fournies dans le paramètre `urls`. Toutes les requêtes de configuration sont déclenchées simultanément dans des threads distincts. Le premier participant a la priorité lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, AccessEnabler mémorise l&#39;URL du prestataire associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.

| Appel API : configuration du demandeur |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Disponibilité :** v1.0+ **Jusqu’à :** v3.0

| Appel API : configuration du demandeur |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Disponibilité :** v1.0+ **Jusqu’à :** v3.0

**Paramètres :**

* *requestorID* : identifiant unique associé au programmeur. Transmettez l’identifiant unique attribué par Adobe à votre site lorsque vous vous êtes enregistré pour la première fois auprès du service d’authentification Adobe Pass.
* *signedRequestorID* : **Ce paramètre existe dans iOS AccessEnabler versions 1.2 et ultérieures.** Une copie de l’ID du demandeur signé numériquement avec votre clé privée. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls* : paramètre facultatif ; par défaut, le fournisseur de services Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points de terminaison pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances du fournisseur de services d’authentification Adobe Pass. Dans ce cas, la liste MVPD est composée des points de terminaison de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.

**Notes :** Si elle est appelée sans le paramètre `serviceProviders` , la bibliothèque récupère la configuration auprès du fournisseur de services par défaut (c’est-à-dire, `https://sp.auth.adobe.com` pour le profil de production ou `https://sp.auth-staging.adobe.com` pour le profil d’évaluation). Si le paramètre `serviceProviders` est fourni, il doit s’agir d’un tableau d’URL. Les informations de configuration sont récupérées à partir de tous les points de terminaison spécifiés et sont fusionnées. Si des informations en double existent dans les réponses des différents fournisseurs de services, le conflit est résolu en faveur du serveur qui répond le plus rapidement (c’est-à-dire que le serveur avec le temps de réponse le plus court est prioritaire).

**Rappels déclenchés :** [`setRequestorComplete:`](#setReqComplete)


[Retour au sommet...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [OBSOLÈTE] {#setReq_tvos}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Établit l’identité du programmeur. Un identifiant unique est attribué à chaque programmeur lors de l’enregistrement auprès de l’Adobe pour le système d’authentification Adobe Pass. Ce paramètre ne doit être exécuté qu’une seule fois pendant le cycle de vie de l’application.

La réponse du serveur contient une liste de MVPD ainsi que certaines informations de configuration qui sont jointes à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code AccessEnabler. Seul l’état de l’opération (c’est-à-dire SUCCESS/FAIL) est présenté à votre application via le rappel `setRequestorComplete:`.

Si le paramètre `urls` n’est pas utilisé, l’appel réseau obtenu cible l’URL du fournisseur de services par défaut : l’environnement de LIBÉRATION/production de l’Adobe.

Si une valeur est fournie pour le paramètre `urls` , l’appel réseau obtenu cible toutes les URL fournies dans le paramètre `urls`. Toutes les requêtes de configuration sont déclenchées simultanément dans des threads distincts. Le premier participant a la priorité lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, AccessEnabler mémorise l&#39;URL du prestataire associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.



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

* *requestorID* : identifiant unique associé au programmeur. Transmettez l’identifiant unique attribué par Adobe à votre site la première fois que vous   enregistré auprès du service d’authentification Adobe Pass.
* *signedRequestorID* : **Ce paramètre existe dans iOS AccessEnabler   versions 1.2 et ultérieures.** Une copie de l’ID du demandeur signé numériquement avec votre clé privée. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls* : paramètre facultatif ; par défaut, le fournisseur de services Adobe   est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points de terminaison pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances du fournisseur de services d’authentification Adobe Pass. Dans ce cas, la liste MVPD est composée des points de terminaison de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.
* secret et publicKey : clé secrète et publique utilisée pour signer le deuxième appel d’écran. Pour plus d’informations, consultez la [documentation sans client](#create_dev).

Si elle est appelée sans le paramètre `serviceProviders` , la bibliothèque récupère la configuration auprès du fournisseur de services par défaut (c’est-à-dire `https://sp.auth.adobe.com` pour le profil de production ou https://sp.auth-staging.adobe.com pour le profil d’évaluation). Si le paramètre `serviceProviders` est fourni, il doit s’agir d’un tableau d’URL. Les informations de configuration sont récupérées à partir de tous les points de terminaison spécifiés et sont fusionnées. Si des informations en double existent dans les réponses des différents fournisseurs de services, le conflit est résolu en faveur du serveur qui répond le plus rapidement (c’est-à-dire que le serveur avec le temps de réponse le plus court est prioritaire).

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

**Paramètres** :

* *status* : peut prendre l’une des valeurs suivantes :
   * `ACCESS_ENABLER_STATUS_SUCCESS` - La phase de configuration a été terminée avec succès
   * `ACCESS_ENABLER_STATUS_ERROR` - la phase de configuration a échoué

**Déclenché par :**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Retour au sommet...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Vérifie l’état d’authentification de l’utilisateur actuel.
Pour ce faire, il recherche un jeton d’authentification valide dans le fichier local
espace de stockage des jetons. Cette méthode n&#39;effectue aucun appel réseau et nous vous recommandons de l&#39;appeler sur le thread principal.
Il est utilisé par l’application pour interroger l’état d’authentification de l’utilisateur et
mettez à jour l’interface utilisateur en conséquence (c’est-à-dire mettez à jour l’interface utilisateur de connexion/déconnexion). La variable
l’état d’authentification est communiqué à l’application via
rappel [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).


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

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Démarre le processus d’authentification complet. Il commence par vérifier l’état d’authentification. Si elle n’est pas déjà authentifiée, la machine à états du flux d’authentification est démarrée :

* si la dernière tentative d’authentification a réussi, le MVPD   la phase de sélection est ignorée et   le rappel [`navigateToUrl:`](#nav2url) est déclenché. La variable   L’application utilise ce rappel pour instancier le contrôle WebView qui présente à l’utilisateur la page de connexion du MVPD. **[REMARQUE : À compter de la version 1.5 de l’activateur d’accès, cette fonctionnalité n’est pas disponible en raison d’une limitation du SDK].**
* si la dernière tentative d’authentification a échoué ou si l’utilisateur s’est explicitement déconnecté, le rappel [`displayProviderDialog:`](#dispProvDialog) est   déclenchée. Votre application utilise ce rappel pour afficher l’interface utilisateur de sélection MVPD. Votre application doit également reprendre le flux d’authentification en informant la bibliothèque AccessEnabler de la sélection du MVPD de l’utilisateur via la méthode [`setSelectedProvider:`](#setSelProv).

Comme les informations d’identification de l’utilisateur sont vérifiées sur la page de connexion MVPD, votre application est requise pour surveiller les multiples opérations de redirection qui ont lieu pendant que l’utilisateur s’authentifie sur la page de connexion du MVPD. Lorsque les informations d’identification correctes sont saisies, le contrôle WebView est redirigé vers une URL personnalisée définie par la constante `ADOBEPASS_REDIRECT_URL`. Cette URL n’est pas destinée à être chargée par le WebView. L’application doit intercepter cette URL et interpréter cet événement comme un signal indiquant que la phase de connexion est terminée. Il doit ensuite céder le contrôle à AccessEnabler pour terminer le flux d&#39;authentification (en appelant la méthode [handleExternalURL](#handleExternalURL) ).

Enfin, l’état d’authentification est communiqué à l’application via le rappel [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

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

* *forceAuthn* : indicateur spécifiant si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
* *data* : dictionnaire constitué de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Retour au sommet...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Démarre le processus d’authentification complet. Il commence par vérifier l’état d’authentification. Si elle n’est pas déjà authentifiée, la machine à états du flux d’authentification est démarrée :

* [presentTvProviderDialog()](#presentTvDialog) sera appelé si le demandeur actuel possède au moins un MVPD qui prend en charge la connexion unique. Si aucun MVPD ne prend en charge la fonction SSO, le flux d’authentification classique commence et le paramètre de filtre est ignoré.
* Une fois l’utilisateur terminé, le flux d’authentification unique Apple [`dismissTvProviderDialog()`](#dismissTvDialog) est déclenché et le processus d’authentification se termine.

Enfin, l’état d’authentification est communiqué à l’application via le rappel [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

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

* *forceAuthn* : indicateur spécifiant si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
* *data* : dictionnaire constitué de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.
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

* *forceAuthn* : indicateur spécifiant si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
* *data* : dictionnaire constitué de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.
* filter : liste des identifiants MVPD qui doivent apparaître dans la boîte de dialogue SSO d’Apple. Tout MVPD qui ne prend pas en charge SSO sera ignoré, mais l’ordre sera respecté.

**Rappels déclenchés :** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Retour au sommet...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler pour informer l’application que les éléments d’IU appropriés doivent être instanciés pour permettre à l’utilisateur de sélectionner le MVPD de votre choix. Le rappel fournit une liste d’objets MVPD avec des informations supplémentaires qui peuvent aider à créer correctement le panneau d’interface utilisateur de sélection (telles que l’URL pointant vers le logo du MVPD, le nom d’affichage convivial, etc.).

Une fois que l’utilisateur a sélectionné le MVPD souhaité, l’application de couche supérieure est requise pour reprendre le flux d’authentification en appelant `setSelectedProvider:` et en lui transmettant l’identifiant du MVPD correspondant à la sélection de l’utilisateur.

**Abandon du flux d’authentification** - C’est un point où l’utilisateur a la possibilité d’appuyer sur le bouton &quot;Retour&quot;, ce qui équivaut à interrompre le flux d’authentification. Dans ce scénario, votre application doit appeler la méthode [setSelectedProvider:](#setSelProv), en transmettant null en tant que paramètre, pour donner à AccessEnabler la possibilité de réinitialiser son ordinateur d’état d’authentification.

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

**Paramètres** :

* *mvpds* : liste des objets MVPD contenant des informations relatives au MVPD que l’application peut utiliser pour créer les éléments d’IU de sélection du MVPD.

**Déclenché par :** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN),`getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Retour au sommet...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est appelée par votre application pour informer l’Access Enabler de la sélection MVPD de l’utilisateur. L’application peut utiliser cette méthode pour sélectionner ou modifier le fournisseur de services utilisé pour l’authentification.

Si le MVPD sélectionné est un MVPD TempPass , il s’authentifiera automatiquement avec ce MVPD sans avoir à appeler getAuthentication() par la suite.

Veuillez noter que ceci n’est pas possible pour la transmission temporaire de conversion où des paramètres supplémentaires sont donnés sur la méthode getAuthentication() .

Lors de la transmission de *null* en tant que paramètre, Access Enabler suppose que l’utilisateur a annulé le flux d’authentification (c’est-à-dire qu’il a appuyé sur le bouton &quot;Précédent&quot;), et répond en réinitialisant la machine-état d’authentification et en appelant le rappel [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) avec le code d’erreur `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR`.

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

**Rappels déclenchés :** `setAuthenticationStatus:errorCode:`,`sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[Retour au sommet...](#apis)

</br>

#### navigateToUrl : {#nav2url}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description :** Rappel déclenché par AccessEnabler pour demander à votre application d’instancier un contrôleur UIWebView/WKWebView et de charger l’URL fournie dans le paramètre **`url`** du rappel. Le rappel transmet le paramètre **`url`** qui représente l’URL du point de terminaison d’authentification ou l’URL du point de terminaison de déconnexion.

Comme le contrôleur UIWebView/WKWebView` `passe par plusieurs redirections, votre application doit surveiller l’activité du contrôleur et détecter le moment où elle charge une URL personnalisée spécifique définie par la constante `ADOBEPASS_REDIRECT_URL `constante (c’est-à-dire `adobepass://ios.app`). Notez que cette URL personnalisée spécifique n’est en fait pas valide et qu’elle n’est pas destinée à être réellement chargée par le contrôleur. Elle ne doit être interprétée que par votre application comme un signal indiquant que le flux d’authentification ou de déconnexion est terminé et qu’il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer UIWebView/WKWebView et appeler la méthode d&#39;API `handleExternalURL:url ` d&#39;AccessEnabler.

**Remarque :** Veuillez noter qu’en cas de flux d’authentification, il s’agit d’un point où l’utilisateur peut appuyer sur le bouton &quot;Retour&quot;, ce qui équivaut à interrompre le flux d’authentification. Dans un tel scénario, votre application doit appeler la méthode [setSelectedProvider:](#setSelProv) en transmettant **`nil`** comme paramètre et en donnant la possibilité à AccessEnabler de réinitialiser son ordinateur d’état d’authentification.

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

**Paramètres** :

* *url* : URL pointant vers la page de connexion du MVPD

**Déclenchée par :** [setSelectedProvider:](#setSelProv)



[Retour au sommet...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description :** Rappel déclenché par AccessEnabler au lieu du rappel `navigateToUrl:` si votre application a activé la gestion manuelle du contrôleur de vue Safari (SVC) via l’appel [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions), et uniquement dans le cas de MVPD nécessitant le contrôleur de vue Safari (SVC). Pour tous les autres MVPD, le rappel `navigateToUrl:` sera appelé. Pour plus d’informations sur la gestion du contrôleur de vue Safari (SVC), voir [Prise en charge de SFSafariViewController sur le SDK iOS 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) .

Tout comme le rappel `navigateToUrl:`, `navigateToUrl:useSVC:` est déclenché par AccessEnabler pour demander à votre application d’instancier un contrôleur `SFSafariViewController` et de charger l’URL fournie dans le paramètre **`url`** du rappel. Le rappel transmet le paramètre **`url`** qui représente l’URL du point de terminaison d’authentification ou l’URL du point de terminaison de déconnexion, ainsi que le paramètre **`useSVC`** qui spécifie que l’application doit utiliser un `SFSafariViewController`.

Au fur et à mesure que le contrôleur `SFSafariViewController` passe par plusieurs redirections, votre application doit surveiller l’activité du contrôleur et détecter le moment où il charge une URL personnalisée spécifique définie par votre `application's custom scheme` (par ex. ** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Notez que cette URL personnalisée spécifique n’est en fait pas valide et qu’elle n’est pas destinée à être réellement chargée par le contrôleur. Elle ne doit être interprétée que par votre application comme un signal indiquant que le flux d’authentification ou de déconnexion est terminé et qu’il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer `SFSafariViewController` et appeler la méthode d&#39;API `handleExternalURL:url ` d&#39;AccessEnabler.

**Remarque :** Veuillez noter qu’en cas de flux d’authentification, il s’agit d’un point où l’utilisateur peut appuyer sur le bouton &quot;Retour&quot;, ce qui équivaut à interrompre le flux d’authentification. Dans un tel scénario, votre application doit appeler la méthode [setSelectedProvider:](#setSelProv) en transmettant **`nil`** comme paramètre et en donnant la possibilité à AccessEnabler de réinitialiser son ordinateur d’état d’authentification.

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

**Paramètres** :

* *url :* URL pointant vers la page de connexion du MVPD
* *useSVC:* si l’URL doit être chargée dans SFSafariViewController.

**Déclenchée par :**[ setOptions:](#setOptions) avant [setSelectedProvider:](#setSelProv)

[Retour au sommet...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est appelée par votre application pour terminer le flux d’authentification ou de déconnexion. Cette méthode doit être appelée juste après que votre application a détecté le moment où le contrôleur `UIWebView/WKWebView or SFSafariViewController` est redirigé vers une URL personnalisée spécifique. Si votre application est requise pour utiliser un contrôleur `SFSafariViewController `, l’URL personnalisée spécifique est définie par votre `application's custom scheme` (par exemple, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), sinon cette URL personnalisée spécifique est définie par la constante `ADOBEPASS_REDIRECT_URL `c’est-à-dire `adobepass://ios.app`).

Dans le cas du flux d’authentification, AccessEnabler complète le flux en récupérant le jeton d’authentification du serveur principal et en le stockant localement dans le stockage du jeton. AccessEnabler informe votre application que le flux d’authentification est terminé en appelant le rappel `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> avec un code d’état de 1, indiquant la réussite. En cas d’erreur lors de l’exécution de ces étapes, le rappel `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> est déclenché avec un code d’état 0, indiquant l’échec de l’authentification, ainsi qu’un code d’erreur correspondant.

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

* *url* : URL interceptée de la commande ` UIWebView/WKWebView or SFSafariViewController ` sous la forme d’une chaîne.


**Rappels déclenchés :** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Retour au sommet...](#apis)

</br>

#### getAuthenticationToken - [DEPRECATED] {#getAuthNToken}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** exécute le flux d’authentification en demandant le jeton d’authentification auprès du serveur principal. Cette méthode ne doit être appelée par votre application qu’en réponse à un événement où le contrôle WebView hébergeant la page de connexion MVPD est redirigé vers l’URL personnalisée définie par la constante `ADOBEPASS_REDIRECT_URL`.


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

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

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

**Paramètres** :

* *status* : peut prendre l’une des valeurs suivantes :
   * `ACCESS_ENABLER_STATUS_SUCCESS` - le flux d’authentification a été terminé avec succès
   * `ACCESS_ENABLER_STATUS_ERROR` - Échec du flux d’authentification
* *code* : raison de l’échec. Si *status* est `ACCESS_ENABLER_STATUS_SUCCESS`, alors *code* est une chaîne vide (c’est-à-dire définie par la constante `USER_AUTHENTICATED`). En cas d’échec, ce paramètre peut prendre l’une des valeurs suivantes :
   * `USER_NOT_AUTHENTICATED_ERROR` - L’utilisateur n’est pas authentifié. En réponse à l’appel de la méthode [checkAuthentication:](#checkAuthN) lorsqu’il n’existe pas de jeton d’authentification valide dans le cache de jeton local.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler a réinitialisé la variable       machine-état d’authentification après application de couche supérieure       a transmis *null* à [`setSelectedProvider:`](#setSelProv) pour interrompre le flux d’authentification.  L’utilisateur a probablement annulé le flux d’authentification (c’est-à-dire qu’il a appuyé sur le bouton &quot;Retour&quot;).
   * `GENERIC_AUTHENTICATION_ERROR` - Le flux d’authentification a échoué pour des raisons telles que l’indisponibilité du réseau ou l’utilisateur a explicitement annulé le flux d’authentification.

**Déclenché par :** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Retour au sommet...](#apis)

</br>

### checkPreauthorizedResources : {#checkPreauth}


**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est utilisée par l’application pour déterminer si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques. L’objectif principal de cette méthode est de récupérer les informations à utiliser pour décorer l’interface utilisateur **(par exemple, indiquer l’état d’accès avec les icônes de verrouillage et de déverrouillage).**

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

* *resources :* tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’ID de ressource. La variable     L’ID de ressource est soumis aux mêmes limites que l’ID de ressource dans l’appel , c’est-à-dire qu’il doit s’agir d’une valeur convenue établie entre le programmeur et le MVPD ou un fragment RSS multimédia.

**Rappel déclenché :** [`preauthorizedResources:`](#preauthResources)

[Retour au sommet...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est utilisée par l’application pour déterminer si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques. L’objectif principal de cette méthode est de récupérer des informations à utiliser dans la décoration de l’interface utilisateur (par exemple, en indiquant l’état d’accès avec les icônes de verrouillage et de déverrouillage). Le paramètre **cache** contrôle si le cache interne est utilisé pour résoudre les ressources.

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

* *resources :* tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’ID de ressource. L’ID de ressource est soumis aux mêmes limites que l’ID de ressource dans l’appel `getAuthorization:`, c’est-à-dire qu’il doit s’agir d’une valeur convenue établie entre le programmeur et le MVPD ou un fragment RSS multimédia.
* *cache :* Booléen spécifiant s’il faut utiliser le cache interne pour résoudre les ressources. Si la valeur est false, le cache est ignoré, ce qui entraîne des appels au serveur chaque fois que cette API est appelée.

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

* `resources` : tableau de ressources pour lequel l’utilisateur est déjà autorisé à afficher.

**Déclenché par :** [`checkPreauthorizedResources:`](#checkPreauth)



[Retour au sommet...](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est utilisée par l’application pour vérifier l’état de l’autorisation. Il commence par vérifier l’état d’authentification. Si elle n’est pas authentifiée, le rappel [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) est déclenché et la méthode se ferme. Si l’utilisateur est authentifié, il déclenche également le flux d’autorisation. Voir les détails sur la méthode [`getAuthorization:`](#getAuthZ).


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

* *resource* : identifiant de la ressource pour laquelle l’utilisateur demande l’autorisation.
* *data* : dictionnaire constitué de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Retour au sommet...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est utilisée par l’application pour lancer le flux d’autorisation. Si l’utilisateur n’est pas déjà authentifié, il lance également le flux d’authentification. Si l’utilisateur est authentifié, AccessEnabler émet des requêtes pour le jeton d’autorisation (si aucun jeton d’autorisation valide n’est présent dans le cache de jeton local) et pour le jeton multimédia de courte durée. Une fois le jeton multimédia court obtenu, le flux d’autorisation est considéré comme terminé. Le rappel [`setToken:forResource:`](#setToken) est déclenché et le jeton multimédia court est diffusé en tant que paramètre à l’application. Si, pour une raison quelconque, l’autorisation échoue, le rappel [`tokenRequestFailed:forEventType:`](#tokenReqFailed) est déclenché et le code d’erreur/les détails sont fournis.

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

* *resource* : identifiant de la ressource pour laquelle l’utilisateur demande l’autorisation.
* *data* : dictionnaire constitué de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Rappels supplémentaires déclenchés :**\
Cette méthode peut également déclencher les rappels suivants (si le flux d’authentification est également lancé) : `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Dans la mesure du possible, utilisez `checkAuthorization:` / `checkAuthorization:withData:` au lieu de `getAuthorization:` / `getAuthorization:withData:`. La méthode `getAuthorization:` / `getAuthorization:withData:` démarre un flux d’authentification complet (si l’utilisateur n’est pas authentifié), ce qui peut entraîner une mise en oeuvre compliquée du côté du programmeur.

[Retour au sommet...](#apis)

</br>

### `setToken:forResource:` {#setToken}

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

**Paramètres** :

* *token* : jeton multimédia de courte durée
* *resource* : ressource pour laquelle l’autorisation a été obtenue

**Déclenchée par :** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Retour au sommet...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

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

**Paramètres** :

* *resource* : ressource pour laquelle l’autorisation a été obtenue.
* *code* : code d’erreur associé au scénario d’échec. Valeurs possibles :
   * `USER_NOT_AUTHORIZED_ERROR` - L’utilisateur n’a pas pu autoriser
pour la ressource donnée
* *description* : détails supplémentaires sur le scénario d’échec. Si cette chaîne descriptive n’est disponible pour aucune raison, l’authentification Adobe Pass envoie une chaîne vide **(&quot;&quot;)**.\
  Cette chaîne peut être utilisée par un MVPD pour transmettre des messages d’erreur personnalisés ou des messages liés aux ventes. Par exemple, si l’autorisation d’une ressource est refusée à un abonné, le MVPD peut envoyer un message tel que : &quot;Vous n’avez actuellement pas accès à ce canal dans votre package. Si vous souhaitez mettre à niveau votre package, cliquez **ici**&quot;. Le message est transmis par l’authentification Adobe Pass via ce rappel au programmeur, qui a la possibilité de l’afficher ou de l’ignorer. L’authentification Adobe Pass peut également utiliser ce paramètre pour fournir une notification de la condition qui a pu entraîner une erreur. Par exemple, &quot;Une erreur de réseau s’est produite lors de la communication avec le service d’autorisation du fournisseur&quot;.

**Déclenché par :** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Retour au sommet...](#apis)

</br>

### déconnexion {#logout}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Cette méthode est appelée par votre application pour lancer le flux de déconnexion. La déconnexion est le résultat d’une série d’opérations de redirection HTTP en raison du fait que l’utilisateur doit être déconnecté des serveurs d’authentification Adobe Pass et des serveurs du MVPD. Comme ce flux ne peut pas être terminé avec une requête HTTP simple émise par la bibliothèque AccessEnabler, un contrôleur `UIWebView/WKWebView or SFSafariViewController` doit être instancié pour pouvoir suivre les opérations de redirection HTTP.

Le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir de quelque manière que ce soit avec le contrôleur `UIWebView/WKWebView or SFSafariViewController`. Par conséquent, Adobe recommande de rendre le contrôle invisible (c’est-à-dire masqué) pendant le processus de déconnexion.

Un modèle similaire au flux d’authentification est utilisé. IOS AccessEnabler déclenche le rappel `navigateToUrl:` ou `navigateToUrl:useSVC:` pour créer un contrôleur `UIWebView/WKWebView or SFSafariViewController` et charger l’URL fournie dans le paramètre `url` du rappel. Il s’agit de l’URL du point de terminaison de la déconnexion sur le serveur principal. Pour tvOS AccessEnabler, ni le rappel `navigateToUrl:` ni le rappel `navigateToUrl:useSVC:` ne sont appelés.

Au fur et à mesure qu&#39;elle passe par plusieurs redirections, votre application doit surveiller l&#39;activité du contrôleur `UIWebView/WKWebView or SFSafariViewController `et détecter le moment où elle charge une URL personnalisée spécifique. Notez que cette URL personnalisée spécifique n’est en fait pas valide et qu’elle n’est pas destinée à être réellement chargée par le contrôleur. Elle ne doit être interprétée que par votre application comme un signal indiquant que le flux de déconnexion est terminé et qu’il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer le contrôleur et appeler la méthode d’API `handleExternalURL:url ` d’AccessEnabler. Si votre application est requise pour utiliser un contrôleur `SFSafariViewController `, l’URL personnalisée spécifique est définie par votre `application's custom scheme` (par exemple, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), sinon cette URL personnalisée spécifique est définie par la constante `ADOBEPASS_REDIRECT_URL `c’est-à-dire `adobepass://ios.app`).

En fin de compte, AccessEnabler appelle le rappel [`setAuthenticationStatus()`](#setAuthNStatus) avec un code d’état de 0, ce qui indique la réussite du flux de déconnexion.

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

**Rappels déclenchés :** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



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

**Paramètres** :

* *mvpd* : objet contenant des informations sur le MVPD actuellement sélectionné

**Déclenché par :** [`getSelectedProvider`](#getSelProv)

[Retour au sommet...](#apis)

</br>

### getMetadata : {#getMeta}

**Fichier :** AccessEnabler/headers/AccessEnabler.h

**Description :** Utilisez cette méthode pour récupérer des informations exposées en tant que métadonnées par la bibliothèque AccessEnabler. L’application peut accéder à ces données en fournissant un paramètre d’entrée *key* basé sur un dictionnaire.

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

* *keyDictionary* : une structure de données de dictionnaire, avec ce qui suit
format :
   * Si la clé est `METADATA_OPCODE_KEY` et la valeur est `METADATA_AUTHENTICATION`, la requête est effectuée pour obtenir le délai d’expiration du jeton d’authentification.
   * Si la clé est `METADATA_OPCODE_KEY` et que la valeur est `METADATA_AUTHORIZATION` **et**\
     La clé est `METADATA_RESOURCE_ID_KEY` et la valeur est un identifiant de ressource particulier, puis la requête est effectuée pour obtenir le délai d’expiration du jeton d’autorisation associé à la ressource spécifiée.
   * Si la clé est `METADATA_OPCODE_KEY` et la valeur est `METADATA_DEVICE_ID`, la requête est effectuée pour obtenir l’identifiant de l’appareil actuel. Notez que cette fonctionnalité est désactivée par défaut et que les programmeurs doivent contacter Adobe pour plus d’informations sur l’activation et les frais.
   * Si la clé est `METADATA_OPCODE_KEY` et que la valeur est `METADATA_USER_META` **et que la clé** est `METADATA_USER_META_KEY` et que la valeur est le nom des métadonnées, la requête est alors faite pour les métadonnées utilisateur. Liste des types de métadonnées utilisateur disponibles :
      * `zip` - Liste des codes postaux
      * `householdID` - Identifiant du foyer. Dans le cas où un MVPD ne prend pas en charge les sous-comptes, cela sera identique à `userID`.
      * `maxRating` - Collection de notations parentales maximales pour l’utilisateur
      * `userID` - Identifiant de l’utilisateur. Si un MVPD prend en charge des sous-comptes et que l’utilisateur n’est pas le compte principal, `userID` sera différent de `householdID.`
      * `channelID` - Liste des canaux qu’un utilisateur est autorisé à afficher.

  >[!NOTE]
  >
  >Les métadonnées utilisateur réelles disponibles pour un programmeur dépendent de ce qu’un MVPD rend disponible. Cette liste sera étendue à mesure que de nouvelles métadonnées seront disponibles et ajoutées au système d’authentification Adobe Pass.

**Rappels déclenchés :** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Plus d’informations :** [Métadonnées utilisateur](/help/authentication/user-metadata.md)

[Retour au sommet...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler après avoir appelé[getAuthentication()](#getAuthN) si le demandeur actuel prend en charge au moins un MVPD avec la prise en charge de l’authentification unique.

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

**Paramètres** :

* viewController : représente la boîte de dialogue Apple SSO. Ce viewController doit s’afficher à l’écran.

**Déclenché par :** [`getAuthentication`](#getAuthN)

**Plus d’informations :** [Connexion unique iOS/tvOS](#presentTvDialog)

[Retour au sommet...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler après la fermeture par l’utilisateur de la boîte de dialogue SSO Apple.

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

**Paramètres** :

* viewController : représente la boîte de dialogue Apple SSO. Ce viewController doit être supprimé de l’écran.

**Déclenché par :** Action de l’utilisateur

**Plus d’informations :** [Connexion unique iOS/tvOS](#presentTvDialog)

[Retour au sommet...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler qui diffuse les métadonnées demandées via un appel [`getMetadata:`](#getMeta).

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

**Paramètres** :

* *metadata* : métadonnées demandées. Cette valeur est un `NSString` dans le cas de métadonnées statiques (TTL d’authentification, TTL d’autorisation, ID de périphérique).  Il s’agit d’un objet complexe lors de la demande de métadonnées spécifiques à l’utilisateur. Cet objet complexe est généralement la représentation Objective-C d’une charge utile JSON (par exemple &#39;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;: [&quot;150&quot;, &quot;320&quot;]&quot; est traduit en Objective-C en NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;building&quot; -> NSArray(&quot;150&quot;) , &quot;320&quot;)).   Exemple d’objet JSON de métadonnées :

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

* *encrypted* : valeur booléenne qui spécifie si les métadonnées récupérées sont chiffrées ou non. Ce paramètre n’est significatif que pour les requêtes de métadonnées utilisateur. Il n’a aucune signification pour les métadonnées statiques (par exemple, Authentification TTL) qui sont toujours reçues sans chiffrement. Si ce paramètre est défini sur true, c’est au programmeur d’obtenir la valeur de métadonnées utilisateur non chiffrée en effectuant un décryptage RSA à l’aide de la clé privée de mise en whiteliste (la même clé privée utilisée pour la signature de l’identifiant du demandeur dans les appels [`setRequestor:setSignedRequestorId:`](#setReq) et `setRequestor:setSignedRequestorId:serviceProviders: `).

* *key* : clé utilisée pour formuler la requête de récupération de métadonnées.

* *arguments* : même dictionnaire qui a été transmis à l’appel [`getMetadata:`](#getMeta). Elle permet à l’application de faire correspondre les requêtes aux réponses.

**Déclenché par :** [`getMetadata:`](#getMeta)

**Plus d’informations :** [Métadonnées utilisateur](/help/authentication/user-metadata.md)


[Retour au sommet...](#apis)

</br>

### MVPD {#mvpd}

**Fichier :** AccessEnabler/headers/model/MVPD.h

**Description** Décrit l’objet MVPD. Peut être utilisé pour obtenir des informations sur les propriétés du MVPD.

**Disponibilité :** v1.0+ [ La propriété boardingStatus est disponible à partir de la version 2.2]

**Propriétés** :

* ID (NSString) - Identifiant MVPD.
* (NSString) displayName - nom MVPD. [Cela doit être utilisé pour l’affichage dans le sélecteur]
* LogoURL (NSString) - Adresse du logo MVPD.
* (BOOL) enablePlatformServices - Si la valeur est true, le MVPD prend en charge des services d’authentification unique tels que [Apple SSO](#presentTvDialog).
* (NSString) boardingStatus - Peut comporter 3 valeurs :
   * nil - Le MVPD ne prend pas en charge la fonction SSO d’Apple.
   * SÉLECTEUR : le MVPD peut apparaître dans le sélecteur Apple, mais le flux d’authentification est effectué par Adobe.
   * PRIS EN CHARGE : le MVPD est entièrement pris en charge par Apple et utilisera le jeton d’authentification unique Apple.

[Retour au sommet...](#apis)

</br>

## Suivi des événements {#tracking}

AccessEnabler déclenche un rappel supplémentaire qui n’est pas nécessairement lié aux flux de droits. La mise en oeuvre de la fonction de rappel [`sendTrackingData()`](#sendTracking) est facultative, mais elle permet à l’application de suivre des événements spécifiques et de compiler des statistiques telles que le nombre de tentatives d’authentification/d’autorisation réussies/échouées.

### sendTrackingData:forEventType: {#sendTracking}

**Fichier :** AccessEnabler/headers/EntitlementDelegate.h

**Description** Rappel déclenché par AccessEnabler signalant à l’application l’occurrence de divers événements tels que l’achèvement/l’échec des flux d’authentification/d’autorisation. Avec l’authentification Adobe Pass 1.6, le type d’appareil, le type de client AccessEnabler et le système d’exploitation sont signalés par [`sendTrackingData()`](#sendTracking). Le rappel [`sendTrackingData()`](#sendTracking) reste rétrocompatible.

**Rappel : suivi des événements**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Disponibilité :** v1.0+

**Remarque :** Le type d’appareil et le système d’exploitation sont dérivés de l’utilisation d’une bibliothèque Java publique (<http://java.net/projects/user-agent-utils>) et de la chaîne de l’agent utilisateur. Notez que ces informations ne sont fournies que pour ventiler les mesures opérationnelles en catégories d’appareils, mais que cet Adobe ne peut pas assumer la responsabilité de résultats incorrects. Utilisez la nouvelle fonctionnalité en conséquence.

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

* *event* : code de l’événement suivi. Il existe trois types d’événements de suivi possibles :
   * **authorizationDetection :** chaque fois qu’une requête de jeton d’autorisation est renvoyée (l’événement est `TRACKING_AUTHORIZATION`)
   * **authenticationDetection :** chaque fois qu’une vérification de l’authentification se produit (l’événement est `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** lorsque l’utilisateur sélectionne un MVPD dans le formulaire de sélection MVPD (l’événement est `TRACKING_GET_SELECTED_PROVIDER`)
* *data* : données supplémentaires associées à l’événement signalé. Ces données sont présentées sous la forme d&#39;une liste de valeurs.

**Déclenché par :** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Instructions pour interpréter les valeurs du tableau *data* :

* Pour trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Indique si la requête de jeton a réussi (true/false) et si elle a réussi :
   * **1** - Chaîne d’identifiant MVPD
   * **2** - GUID (hachage md5)
   * **3** - Jeton déjà en cache (true/false)
   * **4** - Type de périphérique
   * **5** - Type de client AccessEnabler
   * **6** - Type de système d’exploitation

* Pour trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Indique si la requête de jeton a réussi (true/false) et si elle a réussi :
   * **1** - ID MVPD
   * **2** - GUID (hachage md5)
   * **3** - Jeton déjà en cache (true/false)
   * **4** - Erreur
   * **5** - Détails
   * **6** - Type de périphérique
   * **7** - Type de client AccessEnabler
   * **8** - Type de système d’exploitation
* Pour trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID du MVPD actuellement sélectionné
   * **1** - Type de périphérique
   * **2** - Type de client AccessEnabler
   * **3** - Type de système d’exploitation

</br>
