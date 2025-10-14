---
title: Référence de l’API Android SDK
description: Référence de l’API Android SDK
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '4560'
ht-degree: 0%

---

# Référence de l’API SDK Android (héritée) {#android-sdk-api-reference}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Introduction {#intro}

Ce document présente les méthodes et rappels exposés par Android SDK pour l’authentification Adobe Pass, prise en charge avec Adobe Pass Authentication versions 1.7 et ultérieures. Les méthodes et fonctions de rappel décrites ici sont définies dans les fichiers d’en-tête AccessEnabler.h et EntitlementDelegate.h .

Reportez-vous à [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) pour la dernière version du SDK AccessEnabler Android.


**Remarque :** l’équipe Authentification Adobe Pass vous incite à utiliser uniquement les API d’authentification Adobe Pass *publiques* :

- Les API publiques sont disponibles *et entièrement testées* sur tous les types de clients pris en charge. Pour toute fonctionnalité publique, nous nous assurons que chaque type de client dispose d’une version correspondante de la ou des méthodes associées.</span>
- Les API publiques doivent être aussi stables que possible, pour prendre en charge la rétrocompatibilité et s’assurer que les intégrations des partenaires ne sont pas rompues. Toutefois, pour les API *non publiques* nous nous réservons le droit de modifier leur signature à tout moment dans le futur. Si vous rencontrez un flux particulier qui ne peut pas être pris en charge par une combinaison des appels de l’API d’authentification Adobe Pass publics actuels, la meilleure approche consiste à nous le faire savoir. En tenant compte de vos besoins, nous pouvons modifier les API publiques et fournir une solution stable à l’avenir.

## API ANDROID {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- autoriser à l&#39;avance
- [checkAuthorization](#checkAuthZ)
- [getAuthorization](#getAuthZ)
- [setToken](#setToken)
- [tokenRequestFailed](#tokenRequestFailed)
- [déconnexion](#logout)
- [getSelectedProvider](#getSelectedProvider)
- [selectedProvider](#selectedProvider)
- [getMetadata](#getMetadata)
- [setMetadataStatus](#setMetadaStatus)
- [getVersion](#getVersion)

### Factory.getInstance {#getInstance}

**Description :** instancie l&#39;objet Access Enabler. Il doit y avoir une seule instance Access Enabler par instance d&#39;application.

| Appel API : constructeur |
| --- |
| **public static** AccessEnabler **getInstance**(Context appContext, String softwareStatement, String redirectUrl)<br>        **throws** AccessEnablerException <br><br>**public static** AccessEnabler getInstance(ContextAppContext, String env_url, String softwareStatement, String redirectUrl)<br>**throws** AccessEnablerException |

**Disponibilité :** v3.1.2+

**Paramètres:**

- *appContext* : contexte de l’application Android.
- env\_url : pour les tests à l’aide de l’environnement d’évaluation Adobe, env\_url peut être défini sur « sp.auth-staging.adobe.com »

**Obsolète :**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**Description :** établit l’identité du programmeur. Chaque programmeur se voit attribuer un ID unique lors de l’enregistrement avec l’Adobe pour le système d’authentification Adobe Pass. Lorsque vous traitez avec des jetons SSO et distants, l&#39;état d&#39;authentification peut changer lorsque l&#39;application est en arrière-plan, setRequestor peut être appelé à nouveau lorsque l&#39;application est mise en premier plan afin de se synchroniser avec l&#39;état du système (récupérer un jeton distant si SSO est activé ou supprimer le jeton local si une déconnexion s&#39;est produite en attendant).

La réponse du serveur contient une liste de fichiers MVPD ainsi que des informations de configuration associées à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code d’activation d’Access. Seul le statut de l’opération (c’est-à-dire SUCCÈS/ÉCHEC) est présenté à votre application via le rappel setRequestorComplete().

Si le paramètre *urls* n’est pas utilisé, l’appel réseau résultant cible l’URL du fournisseur de services par défaut : l’environnement de publication/production d’Adobe.

Si une valeur est fournie pour le paramètre *urls*, l’appel réseau résultant cible toutes les URL fournies dans le paramètre *urls*. Toutes les demandes de configuration sont déclenchées simultanément dans des threads distincts. Le premier répondant est prioritaire lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, Access Enabler mémorise l’URL du fournisseur d’accès associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.

| Appel API : configuration du demandeur |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Disponibilité :** v3.0+

| Appel API : configuration du demandeur |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Disponibilité :** v3.0+


**Paramètres:**

- *requestorID* : ID unique associé au programmeur. Transmettez l’ID unique attribué par Adobe à votre site lors de votre premier enregistrement auprès du service d’authentification Adobe Pass.

- *signedRequestorID* : copie de l’ID du demandeur signé numériquement avec votre clé privée. <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *urls* : paramètre facultatif ; par défaut, le fournisseur d’accès Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points d’entrée pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances de fournisseur de services d’authentification Adobe Pass. Ce faisant, la liste MVPD est composée des points d’entrée de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.

**Rappels déclenchés :** `setRequestorComplete()`

Obsolète :

    public void setRequestor(String requestorId, String signedRequestorId)
    
    public void setRequestor (String requestorId, String signedRequestorId, ArrayList&lt;String> url)

[Retour à l’API Android...](#api)

### setRequestorComplete {#setRequestorComplete}

**Description :** rappel déclenché par Access Enabler qui informe votre application que la phase de configuration est terminée. Il s’agit d’un signal indiquant que l’application peut commencer à émettre des demandes de droits. Aucune demande de droits ne peut être émise par l’application tant que la phase de configuration n’est pas terminée.

| Rappel : configuration du demandeur terminée |
| --- |
| java public void setRequestorComplete(int status) |

**Disponibilité :** v1.0+

**Paramètres:**

- *status* : peut prendre l&#39;une des valeurs suivantes :
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - la phase de configuration s’est terminée avec succès
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - échec de la phase de configuration
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - la phase de configuration s’est terminée avec succès
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - échec de la phase de configuration

**Déclenché par :** `setRequestor()`

[Retour à l’API Android...](#api)

### setOptions {#setOptions}

**Description :** configure les options globales de SDK. Elle accepte un **Map\&lt;String, String\>** comme argument. Les valeurs du mappage seront transmises au serveur avec chaque appel réseau effectué par le SDK.

Les valeurs seront transmises au serveur indépendamment du flux actuel (authentification/autorisation). Si vous souhaitez modifier les valeurs, vous pouvez appeler cette méthode à tout moment.

| Appel API : setOptions |
| --- |
| public void setOptions(HashMap&lt;String,String> options) |

**Disponibilité :** v1.9.2+

**Paramètres:**

- *options* : Map&lt;String, String> contenant des options SDK globales. Actuellement, les options suivantes sont disponibles :
   - **applicationProfile** - Peut être utilisé pour effectuer des configurations de serveur en fonction de cette valeur.
   - **ap_vi** - Identifiant Experience Cloud (visitorID). Cette valeur peut être utilisée ultérieurement pour les rapports d’analyse avancée.
   - **ap_ai** - Advertising ID
   - **device_info** - Informations du client comme décrit ici : [Transmission des informations du client, connexion du périphérique et application](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

[Haut de la page...](#apis)


### checkAuthentication {#checkAuthN}

**Description :** vérifie le statut d’authentification. Pour ce faire, il recherche un jeton d’authentification valide dans l’espace de stockage du jeton local. Cette méthode n’effectue aucun appel réseau et nous vous recommandons de l’appeler sur le thread principal. Il est utilisé par l’application pour interroger le statut d’authentification de l’utilisateur et mettre à jour l’interface utilisateur en conséquence (c’est-à-dire mettre à jour l’interface utilisateur de connexion/déconnexion). Le statut de l&#39;authentification est communiqué à l&#39;application via le rappel [*setAuthenticationStatus()*](#setAuthNStatus).

Si un MVPD prend en charge la fonction « Authentification par demandeur », plusieurs jetons d’authentification peuvent être stockés sur un appareil.  Pour plus d’informations sur cette fonctionnalité, consultez la section [&#x200B; Instructions de mise en cache &#x200B;](#$caching) de la présentation technique d’Android.

| Appel API : vérification du statut d&#39;authentification |
| --- |
| public void checkAuthentication() |

**Disponibilité :** v1.0+

**Paramètres:** Aucun

**Rappels déclenchés :** `setAuthenticationStatus()`

[Retour à l’API Android...](#api)


### getAuthentication {#getAuthN}

**Description :** démarre le workflow d’authentification complet. Il commence par vérifier le statut de l’authentification. S’il n’est pas déjà authentifié, la machine d’état du flux d’authentification est démarrée :

- Si la dernière tentative d’authentification a réussi, la phase de sélection du MVPD est ignorée et le rappel [*navigateToUrl()*](#navigagteToUrl) est déclenché. L&#39;application utilise ce rappel pour instancier le contrôle WebView qui présente à l&#39;utilisateur la page de connexion MVPD.
- Si la dernière tentative d’authentification a échoué ou si l’utilisateur s’est explicitement déconnecté, le rappel [*displayProviderDialog()*](#displayProviderDialog) est déclenché. Votre application utilise ce rappel pour afficher l’interface utilisateur de sélection de MVPD. Votre application doit également reprendre le flux d’authentification en informant la bibliothèque Access Enabler de la sélection MVPD de l’utilisateur par le biais de la méthode [setSelectedProvider()](#setSelectedProvider).

Comme les informations d’identification de l’utilisateur sont vérifiées sur la page de connexion de MVPD, votre application doit surveiller les multiples opérations de redirection qui ont lieu lorsque l’utilisateur s’authentifie sur la page de connexion de MVPD. Lorsque les informations d&#39;identification correctes sont saisies, le contrôle WebView est redirigé vers une URL personnalisée définie par la constante *AccessEnabler.ADOBEPASS\_REDIRECT\_URL*. Cette URL ne doit pas être chargée par le WebView. L’application doit intercepter cette URL et interpréter cet événement comme un signal indiquant que la phase de connexion est terminée. Il doit ensuite transmettre le contrôle à Access Enabler pour terminer le flux d’authentification (en appelant la méthode *getAuthenticationToken()*).

Si un MVPD prend en charge la fonction « Authentification par demandeur », plusieurs jetons d’authentification peuvent être stockés sur un appareil (un par programmeur).  Pour plus d’informations sur cette fonctionnalité, consultez la section [&#x200B; Instructions de mise en cache &#x200B;](#$caching) de la présentation technique d’Android.

Enfin, le statut de l&#39;authentification est communiqué à l&#39;application via le rappel *setAuthenticationStatus()*.



| Appel API : lance le flux d&#39;authentification |
| --- |
| public void getAuthentication() |

**Disponibilité :** v1.0+

| Appel API : lance le flux d&#39;authentification |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;String, Object> genericData) |

**Disponibilité :** v1.8+

**Paramètres:**

- *forceAuthn* : indicateur qui spécifie si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
- *data* : carte composée de paires clé-valeur à envoyer au service de pass de télévision à péage. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Retour à l’API Android...](#api)

### displayProviderDialog {#displayProviderDialog}

**Description** rappel déclenché par Access Enabler pour informer l’application que les éléments appropriés de l’interface utilisateur doivent être instanciés pour permettre à l’utilisateur de sélectionner le MVPD souhaité. Le rappel fournit une liste d’objets MVPD avec des informations supplémentaires qui peuvent aider à créer correctement le panneau de l’interface utilisateur de sélection (comme l’URL pointant vers le logo MVPD, le nom d’affichage convivial, etc.)

Une fois que l’utilisateur a sélectionné le MVPD souhaité, l’application de couche supérieure doit reprendre le flux d’authentification en appelant *setSelectedProvider()* et en lui transmettant l’identifiant du MVPD correspondant à la sélection de l’utilisateur.

>[!NOTE]
>
> Abandon du flux d’authentification
> </br></br>
> Notez qu’il s’agit d’un point où l’utilisateur ou l’utilisatrice peut appuyer sur le bouton « Précédent », ce qui équivaut à l’abandon du flux d’authentification. Dans un tel scénario, votre application doit appeler la méthode `setSelectedProvider()`, en transmettant *null* comme paramètre, pour donner à Access Enabler la possibilité de réinitialiser son ordinateur d’état d’authentification.

| Rappel : affichage de l’interface utilisateur de sélection de MVPD |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**Disponibilité :** v1.0+

**Paramètres** :

- *mvpds* : liste des objets MVPD contenant des informations liées à MVPD que l’application peut utiliser pour créer les éléments de l’interface utilisateur de sélection de MVPD.

**Déclenché par :** `getAuthentication(), getAuthorization()`

[Retour à l’API Android...](#api)


### setSelectedProvider {#setSelectedProvider}

**Description :** cette méthode est appelée par votre application pour informer Access Enabler de la sélection MVPD de l’utilisateur. L’application peut utiliser cette méthode pour sélectionner ou modifier le fournisseur de services utilisé pour l’authentification.

Si le MVPD sélectionné est un MVPD TempPass, il s’authentifiera automatiquement avec ce MVPD sans avoir à appeler getAuthentication() par la suite.

Notez que cela n’est pas possible pour le transfert temporaire promotionnel où des paramètres supplémentaires sont donnés sur la méthode getAuthentication().

Lors de la transmission du paramètre *null*, Access Enabler suppose que l’utilisateur a annulé le flux d’authentification (c’est-à-dire qu’il a appuyé sur le bouton « Retour ») et répond en réinitialisant l’état-machine d’authentification et en appelant le rappel *setAuthenticationStatus()* avec le code d’erreur `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR`.

| Appel API : définir le fournisseur actuellement sélectionné |
| --- |
| public void setSelectedProvider(String mvpdId) |

**Disponibilité :** v1.0+

**Paramètres:** Aucun

**Rappels déclenchés :** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Retour à l’API Android...](#api)


### navigateToUrl {#navigagteToUrl}

**Obsolète :** à partir d’Android SDK 3.0, navigateToUrl est utilisé uniquement si l’onglet personnalisé Chrome n’est pas présent sur l’appareil

**Description :** rappel déclenché par Access Enabler qui informe l’application que l’utilisateur doit recevoir la page de connexion de MVPD pour saisir ses informations d’identification. Access Enabler transmet en tant que paramètre l’URL de la page de connexion de MVPD. Votre application est nécessaire pour instancier un contrôle WebView et le diriger vers cette URL. En outre, l&#39;application est nécessaire pour surveiller les URL chargées par le contrôle WebView et intercepter l&#39;opération de redirection ciblant l&#39;URL personnalisée définie par la constante `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)`. Lors de cet événement, l&#39;application doit fermer ou masquer le contrôle WebView et le rendre à la bibliothèque Access Enabler en appelant la méthode *getAuthenticationToken()*. Access Enabler termine le flux d’authentification en récupérant le jeton d’authentification du serveur principal et en le stockant localement dans le stockage des jetons.

>[!WARNING]
>
> **Abandon du flux d’authentification** <br>Notez qu’il s’agit d’un point où l’utilisateur peut appuyer sur le bouton « Précédent », ce qui équivaut à un abandon du flux d’authentification. Dans un tel scénario, votre application doit appeler la méthode _setSelectedProvider()_ en transmettant _null_ en tant que paramètre et en donnant une chance à Access Enabler de réinitialiser son état d&#39;authentification.

| Rappel : affichage de la page de connexion à MVPD |
| --- |
| public void navigateToUrl(URL de chaîne) |

**Disponibilité :** v1.0+

**Paramètres:**

- *url* : URL pointant vers la page de connexion de MVPD

**Déclenché par :** `getAuthentication(), setSelectedProvider()`

[Retour à l’API Android...](#api)


### getAuthenticationToken {#getAuthNToken}

**Obsolète :** à partir d’Android SDK 3.0, étant donné que l’onglet personnalisé Chrome est utilisé pour l’authentification, cette méthode n’est plus utilisée à partir de l’application.

**Description :** termine le flux d’authentification en demandant le jeton d’authentification au serveur principal. Cette méthode ne doit être appelée par votre application qu’en réponse à un événement où le contrôle WebView hébergeant la page de connexion MVPD est redirigé vers l’URL personnalisée définie par la constante `AccessEnabler.ADOBEPASS_REDIRECT_URL`.

| Appel API : récupération du jeton d&#39;authentification |
| --- |
| public void getAuthenticationToken() |

**Disponibilité :** v1.0+

**Paramètres:**

- *cookies* : cookies définis sur le domaine cible (voir l’application de démonstration dans le SDK pour une implémentation de référence).

**Rappels déclenchés :** `setAuthenticationStatus()`, `sendTrackingData()`

[Retour à l’API Android...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**Description :** rappel déclenché par Access Enabler qui informe
application du statut du flux d’authentification. Il y en a beaucoup
les endroits où ce flux peut échouer, soit en raison de
interaction ou en raison d’autres scénarios imprévus (par ex. réseau)
problèmes de connectivité, etc.). Ce rappel informe l’application de
le statut de réussite/échec du flux d’authentification, ainsi que
fournir des informations supplémentaires sur le motif de l’échec, le cas échéant.

| Rappel : rapport du statut du flux d’authentification |
| --- |
| public void setAuthenticationStatus(int status, String errorCode) |

**Disponibilité :** v1.0+

**Paramètres:**

- *status* : peut prendre l&#39;une des valeurs suivantes :
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - flux d’authentification terminé avec succès
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - échec du flux d’authentification
- *code* : motif de l’échec. Si *status* est `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`, alors *code* est une chaîne vide (c’est-à-dire définie par la constante `AccessEnablerConstants.USER_AUTHENTICATED`). En cas d’échec, ce paramètre peut prendre l’une des valeurs suivantes :
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - L’utilisateur n’est pas authentifié. En réponse à l’appel de la méthode *checkAuthentication()* lorsqu’il n’existe aucun jeton d’authentification valide dans le cache de jetons local.
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - L’AccessEnabler a réinitialisé l’ordinateur d’état d’authentification après que l’application de couche supérieure a transmis *null* à `setSelectedProvider()` pour abandonner le flux d’authentification.  L’utilisateur a probablement annulé le flux d’authentification (c’est-à-dire qu’il a appuyé sur le bouton « Précédent »).
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - Le flux d’authentification a échoué pour des raisons telles que l’indisponibilité du réseau ou l’annulation explicite du flux d’authentification.

**Déclenché par :** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Retour à l’API Android...](#api)


### checkPreauthorizedResources {#checkPreauth}

>**Obsolète :** à partir d’Android SDK 3.6, l’API preauthorize remplace checkResources en fournissant des codes d’erreur étendus.

**Description :** cette méthode est utilisée par l’application pour déterminer si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques. L’objectif principal de cette méthode est de récupérer des informations à utiliser pour décorer l’interface utilisateur (par exemple, en indiquant le statut d’accès avec des icônes de verrouillage et de déverrouillage).

| Appel API : définir le fournisseur actuellement sélectionné |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Disponibilité :** v1.3+

**Paramètres :** le paramètre `resources` est un tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’identifiant de la ressource. L’ID de ressource est soumis aux mêmes limitations que l’ID de ressource dans l’appel `getAuthorization()`. En d’autres termes, il doit s’agir d’une valeur convenue entre le programmeur et le MVPD ou d’un fragment RSS de média.

**Rappel déclenché :** `preauthorizedResources()`

[Retour à l’API Android...](#api)


### checkPreauthorizedResources {#checkPreauth2}

**Obsolète :** à partir d’Android SDK 3.6, l’API preauthorize remplace checkResources en fournissant des codes d’erreur étendus.

**Description :** cette méthode est utilisée par l’application pour déterminer si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques. L’objectif principal de cette méthode est de récupérer des informations à utiliser pour décorer l’interface utilisateur (par exemple, en indiquant le statut d’accès avec des icônes de verrouillage et de déverrouillage).

| Appel API : définir le fournisseur actuellement sélectionné |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**Disponibilité :** v3.1+

**Paramètres :** le paramètre `resources` est un tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’identifiant de la ressource. L’ID de ressource est soumis aux mêmes limitations que l’ID de ressource dans l’appel `getAuthorization()`. En d’autres termes, il doit s’agir d’une valeur convenue entre le programmeur et le MVPD ou d’un fragment RSS de média.

Le paramètre `cache` indique si la réponse de préautorisation mise en cache peut être utilisée ou non. Par défaut, le cache est défini sur true et le SDK renvoie une réponse précédemment mise en cache si elle est disponible.

**Rappel déclenché :** `preauthorizedResources()`

[Retour à l’API Android...](#api)

### preauthorizedResources {#preauthResources}

**Obsolète :** à partir d’Android SDK 3.6, l’API preauthorize remplace checkResources en fournissant des codes d’erreur étendus. Le rappel preauthorizedResources ne sera pas appelé sur la nouvelle API.


**Description : rappel** déclenché par checkPreauthorizedResources(). Fournit une liste des ressources que l’utilisateur est déjà autorisé à afficher.

| Appel API : définir le fournisseur actuellement sélectionné |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Disponibilité :** v1.3+

**Paramètres :** le paramètre `resources` est un tableau de ressources que l’utilisateur est déjà autorisé à afficher.

**Déclenché par :** `checkPreauthorizedResources()`

[Retour à l’API Android...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**Description :** cette méthode est utilisée par l’application pour vérifier le statut de l’autorisation. Il commence par vérifier d’abord le statut de l’authentification. S’il n’est pas authentifié, le rappel *setTokenRequestFailed()* est déclenché et la méthode se ferme. Si l’utilisateur est authentifié, cela déclenche également le flux d’autorisation. Voir les détails sur la méthode *getAuthorization()*.

| Appel API : vérification du statut d&#39;autorisation |
| --- |
| public void checkAuthorization(String resourceId) |

**Disponibilité :** v1.0+

| Appel API : vérification du statut d&#39;autorisation |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Disponibilité :** v1.8+

**Paramètres:**

- *resourceId* : ID de la ressource pour laquelle l’utilisateur demande une autorisation.
- *data* : carte composée de paires clé-valeur à envoyer au service de pass de télévision à péage. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Retour à l’API Android...](#api)


### <span id="getAuthZ"></span>getAuthorization

**Description :** cette méthode est utilisée par l’application pour lancer le flux d’autorisation. Si l’utilisateur n’est pas déjà authentifié, il lance également le flux d’authentification. Si l’utilisateur est authentifié, Access Enabler émet des demandes pour le jeton d’autorisation (si aucun jeton d’autorisation valide n’est présent dans le cache de jetons local) et pour le jeton de média de courte durée. Une fois le jeton de média court obtenu, le flux d’autorisation est considéré comme terminé. Le rappel *setToken()* est déclenché et le jeton de média court est fourni en tant que paramètre à l’application. Si, pour une raison quelconque, l’autorisation échoue, le rappel *tokenRequestFailed()* est déclenché et le code d’erreur et les détails sont fournis.

| Appel API : lancement du flux d’autorisation |
| --- |
| public void getAuthorization(String resourceId) |

**Disponibilité :** v1.0+

| Appel API : lancement du flux d’autorisation |
| --- |
| public void getAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Disponibilité :** v1.8+

**Paramètres:**

- *resourceId* : ID de la ressource pour laquelle l’utilisateur demande une autorisation.
- *data* : carte composée de paires clé-valeur à envoyer au service de pass de télévision à péage. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **Rappels supplémentaires déclenchés** <br> Cette méthode peut également déclencher les rappels suivants (si le flux d’authentification est également initié) : *setAuthenticationStatus()*, *displayProviderDialog()*, *navigateToUrl()*

**REMARQUE : utilisez checkAuthorization() au lieu de getAuthorization() dans la mesure du possible. La méthode getAuthorization() lance un flux d&#39;authentification complet (si l&#39;utilisateur n&#39;est pas authentifié), ce qui peut entraîner une implémentation compliquée du côté du programmeur.**

[Retour à l’API Android...](#api)


### setToken {#setToken}

**Description :** rappel déclenché par Access Enabler qui informe votre application que le flux d’autorisation a été terminé avec succès. Le jeton de média de courte durée est également diffusé en tant que paramètre.

| Rappel : flux d’autorisation terminé avec succès |
| --- |
| public void setToken(Jeton de chaîne, ResourceId de chaîne) |

**Disponibilité :** v1.0+

**Paramètres:**

- *token* : jeton de média de courte durée.
- *resourceId* : ressource pour laquelle l’autorisation a été obtenue

**Déclenché par :** `checkAuthorization()`, `getAuthorization()`


[Retour à l’API Android...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**Description :** rappel déclenché par Access Enabler qui informe l’application de couche supérieure de l’échec du flux d’autorisation.

| Rappel : échec du flux d’autorisation |
| --- |
| public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription) |

**Disponibilité :** v1.0+

**Paramètres:**

- *resourceId* : ressource pour laquelle l’autorisation a été obtenue
- *errorCode* : code d’erreur associé au scénario d’échec. Valeurs possibles :
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` - L’utilisateur n’a pas pu autoriser pour la ressource donnée.
- *errorDescription* : informations supplémentaires sur le scénario d’échec. Si cette chaîne descriptive n’est disponible pour aucune raison, l’authentification Adobe Pass envoie une chaîne vide **(«  »)**.

  Cette chaîne peut être utilisée par un MVPD pour transmettre des messages d’erreur personnalisés ou des messages liés aux ventes. Par exemple, si l’autorisation d’accès à une ressource est refusée à un abonné, le MVPD peut envoyer un message du type : « Vous n’avez pas accès à ce canal dans votre package. Si vous souhaitez mettre à niveau votre package, cliquez ici. » Le message est transmis par l’authentification Adobe Pass via ce rappel au programmeur, qui a la possibilité de l’afficher ou de l’ignorer. L’authentification Adobe Pass peut également utiliser ce paramètre pour fournir une notification de la condition qui a pu entraîner une erreur. Par exemple, « Une erreur réseau s’est produite lors de la communication avec le service d’autorisation du fournisseur ».

**Déclenché par :** `checkAuthorization(), getAuthorization()`

[Retour à l’API Android...](#api)

### déconnexion {#logout}

**Description :** utilisez cette méthode pour lancer le flux de déconnexion. La déconnexion est le résultat d’une série d’opérations de redirection HTTP en raison du fait que l’utilisateur doit être déconnecté des serveurs d’authentification Adobe Pass et des serveurs de MVPD. Par conséquent, ce flux ne peut pas être exécuté avec une simple requête HTTP émise par la bibliothèque Access Enabler. Un onglet personnalisé Chrome est utilisé par le SDK pour exécuter les opérations de redirection HTTP. Ce flux sera visible pour l’utilisateur et fermé une fois terminé

| Appel API : lancement du flux de déconnexion |
| --- |
| public void logout() |

**Disponibilité :** v1.0+

**Paramètres:** Aucun

**Rappels déclenchés :**

- `navigateToUrl()` pour les versions de SDK antérieures à la version 3.0
- `setAuthenticationStatus()` pour SDK version > 3.0


[Retour à l’API Android...](#api)


### getSelectedProvider {#getSelectedProvider}

**Description :** utilisez cette méthode pour déterminer le fournisseur actuellement sélectionné.

| Appel API : déterminer le MVPD actuellement sélectionné |
| --- |
| public void getSelectedProvider() |

**Disponibilité :** v1.0+

**Paramètres:** Aucun

**Rappels déclenchés :** `selectedProvider()`

[Retour à l’API Android...](#api)


### <span id="selectedProvider"></span>selectedProvider

**Description :** rappel déclenché par Access Enabler qui fournit des informations sur le MVPD actuellement sélectionné à l’application.

| Rappel : informations sur le MVPD actuellement sélectionné |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**Disponibilité :** v1.0+

**Paramètres:**

- *mvpd* : objet contenant des informations sur le MVPD actuellement sélectionné

**Déclenché par :** `getSelectedProvider()`

[Retour à l’API Android...](#api)


### getMetadata {#getMetadata}

**Description :** utilisez cette méthode pour récupérer des informations exposées en tant que métadonnées par la bibliothèque Access Enabler. L’application peut accéder à ces informations en fournissant un objet MetadataKey composite.

| Appel API : requête de métadonnées à AccessEnabler |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**Disponibilité :** v1.0+

Les programmeurs ont accès à deux types de métadonnées :

- Métadonnées statiques (TTL de jeton d’authentification, TTL de jeton d’autorisation et ID d’appareil)
- Métadonnées utilisateur (informations spécifiques à l’utilisateur, telles que l’identifiant utilisateur et le code postal ; transmises d’un MVPD à l’appareil d’un utilisateur lors des flux d’authentification et/ou d’autorisation)

**Paramètres:**

- *metadataKey* : structure de données qui encapsule une clé et une variable args, avec la signification suivante :
   - Si la clé est `METADATA_KEY_USER_META` et que les arguments contiennent un objet SerializableNameValuePair avec le nom = `METADATA_ARG_USER_META` et la valeur = `[metadata_name]`, la requête porte sur les métadonnées de l’utilisateur. Liste actuelle des types de métadonnées utilisateur disponibles :
      - `zip` - Code postal

      - `householdID` - Identifiant du ménage. Si un MVPD ne prend pas en charge les sous-comptes, celui-ci est identique à `userID`.

      - `maxRating` - Évaluation parentale maximale pour l&#39;utilisateur

      - `userID` - Identifiant de l’utilisateur. Si un MVPD prend en charge les sous-comptes et que l’utilisateur n’est pas le compte principal, `userID` sera différent de `householdID`.

      - `channelID` - Liste des canaux que l’utilisateur est autorisé à afficher
   - Si la clé est `METADATA_KEY_DEVICE_ID`, la requête est effectuée pour obtenir l’identifiant d’appareil actuel. Notez que cette fonctionnalité est désactivée par défaut et les programmeurs doivent contacter l’Adobe pour plus d’informations sur l’activation et les frais.
   - Si la clé est `METADATA_KEY_TTL_AUTHZ` et que les arguments contiennent un objet SerializableNameValuePair avec le nom = `METADATA_ARG_RESOURCE_ID` et la valeur = `[resource_id]`, la requête est exécutée pour obtenir le délai d’expiration du jeton d’autorisation associé à la ressource spécifiée.
   - Si la clé est `METADATA_KEY_TTL_AUTHN`, la requête est effectuée pour obtenir le délai d’expiration du jeton d’authentification.



>[!NOTE]
>
>Pour SDK 3.4.0, les constantes : `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` sont disponibles à partir de com.adobe.adobepass.accessenabler.api.profile.UserProfileService.



>[!NOTE]
>
>Les métadonnées d’utilisateur réellement disponibles pour un programmeur dépendent de ce qu’un MVPD rend disponible.  Cette liste sera complétée à mesure que de nouvelles métadonnées seront disponibles et ajoutées au système d’authentification Adobe Pass.

**Rappels déclenchés :** [`setMetadataStatus()`](#setMetadaStatus)

**Informations supplémentaires :** [Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

[Retour à l’API Android...](#api)

### setMetadataStatus {#setMetadaStatus}

**Description :** rappel déclenché par Access Enabler qui fournit les métadonnées demandées via un appel *getMetadata()*.

| Rappel : résultat de la requête de récupération des métadonnées |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Disponibilité :** v1.0+

**Paramètres:**

- *key* : objet MetadataKey contenant la clé pour laquelle la valeur de métadonnées est demandée et les paramètres associés (voir l’application de démonstration pour une implémentation de référence).
- *result* : objet composite contenant les métadonnées demandées. L’objet comporte les champs suivants :
   - *simpleResult* : une chaîne qui représente la valeur des métadonnées au moment où la demande a été faite pour la TTL d’authentification, la TTL d’autorisation ou l’ID d’appareil. Cette valeur est nulle si la demande porte sur les métadonnées de l’utilisateur.

   - *userMetadataResult* : objet contenant la représentation Java d’une payload de métadonnées d’utilisateur JSON.\
     Par exemple :

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
```

est traduit en Java comme suit :

```java
          Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
```

**La structure réelle des objets de métadonnées utilisateur est similaire à ce qui suit :**

```json
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
          }
```

Cette valeur est nulle lorsque la demande porte sur des métadonnées simples (TTL d’authentification, TTL d’autorisation ou ID d’appareil).

- *encrypted* : valeur booléenne qui spécifie si les métadonnées récupérées sont chiffrées ou non. Ce paramètre est important uniquement pour les requêtes de métadonnées d’utilisateur. Il n’a aucune signification pour les métadonnées statiques (par exemple : TTL d’authentification) qui sont toujours reçues non chiffrées. Si ce paramètre est défini sur True, il revient au programmeur d&#39;obtenir la valeur non chiffrée des métadonnées de l&#39;utilisateur en effectuant un déchiffrement RSA à l&#39;aide de la clé privée placée sur la liste blanche (la même clé privée que celle utilisée pour la signature de l&#39;ID du demandeur dans l&#39;appel [`setRequestor`](#setRequestor)).

**Déclenché par :** [`getMetadata()`](#getMetadata)

**Informations supplémentaires :** [Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)


[Retour à l’API Android...](#api)


### getVersion {#getVersion}

**Description :** cette méthode peut être utilisée pour récupérer la version de la bibliothèque AccessEnabler.

| Appel API : obtenir la version AccessEnabler |
| --- |
| ```public static String getVersion()``` |


[Retour à l’API Android...](#api)

</br>

## Suivi des événements {#tracking}

Access Enabler déclenche un rappel supplémentaire qui n’est pas nécessairement lié aux flux de droits. L’implémentation de la fonction de rappel de suivi des événements nommée *sendTrackingData()* est facultative, mais elle permet à l’application de suivre des événements spécifiques et de compiler des statistiques, telles que le nombre de tentatives d’authentification/d’autorisation réussies/ayant échoué. Vous trouverez ci-dessous la spécification du rappel *sendTrackingData()* :


### sendTrackingData {#sendTrackingData}

**Description :** rappel déclenché par le Gestionnaire d’accès signalant à l’application l’occurrence de divers événements tels que l’achèvement/l’échec des flux d’authentification/d’autorisation. Le type d’appareil, le type de client Access Enabler et le système d’exploitation sont également signalés par sendTrackingData().

>[!WARNING]
>
> Le type d’appareil et le système d’exploitation sont dérivés à l’aide d’une bibliothèque Java publique ([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)) et de la chaîne de l’agent utilisateur. Notez que ces informations ne sont fournies qu’à titre approximatif pour ventiler les mesures opérationnelles en catégories d’appareils, mais que l’Adobe ne peut être tenu responsable de résultats incorrects. Veuillez utiliser la nouvelle fonctionnalité en conséquence.


- Valeurs possibles pour le type d’appareil :
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`


- Valeurs possibles pour le type de client Access Enabler :
   - `flash`
   - `html5`
   - `ios`
   - `android`

</br>

| Rappel : suivi des événements |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**Disponibilité :** v1.0+

**Paramètres:**

- *event* : l’événement qui fait l’objet d’un suivi. Il existe trois types d&#39;événements de tracking possibles :
   - **authorizationDetection :** chaque fois qu’une demande de jeton d’autorisation est renvoyée (type d’événement `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection :** à chaque vérification de l’authentification (le type d’événement est `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection :** lorsque l’utilisateur sélectionne un MVPD dans le formulaire de sélection MVPD (le type d’événement est `EVENT_MVPD_SELECTION`)
- *data* : données supplémentaires associées à l’événement signalé. Ces données sont présentées sous la forme d’une liste de valeurs.

Vous trouverez ci-dessous des instructions pour interpréter les valeurs dans le *data*
tableau :

- Pour le type d’événement *`EVENT_AUTHN_DETECTION`:*
   - **0** - Indique si la demande de jeton a réussi (true/false) et si ce qui précède est vrai :
   - **1** - Chaîne d’identifiant MVPD
   - **2** - GUID (md5 haché)
   - **3** - Jeton déjà présent dans le cache (true/false)
   - **4** - Type d’appareil
   - **5** - Type de client Access Enabler
   - **6** - Type de système d’exploitation

- Pour le type d’événement `EVENT_AUTHZ_DETECTION`
   - **0** - Indique si la demande de jeton a réussi (true/false) et, si elle a réussi :
   - **1** - MVPD ID
   - **2** - GUID (md5 haché)
   - **3** - Jeton déjà présent dans le cache (true/false)
   - **4** - Erreur
   - **5** - Détails
   - **6** - Type d’appareil
   - **7** - Type de client Access Enabler
   - **8** - Type de système d’exploitation

- Pour le type d’événement `EVENT_MVPD_SELECTION`
   - **0** - ID du MVPD actuellement sélectionné
   - **1** - Type d’appareil
   - **2** - Type de client Access Enabler
   - **3** - Type de système d’exploitation

**Déclenché par :** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Retour à l’API Android...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
