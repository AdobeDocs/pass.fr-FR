---
title: Référence de l’API du SDK Android
description: Référence de l’API du SDK Android
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '4537'
ht-degree: 0%

---

# Référence de l’API du SDK Android {#android-sdk-api-reference}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

Ce document présente les méthodes et les rappels exposés par le SDK Android pour l’authentification Adobe Pass, pris en charge avec l’authentification Adobe Pass versions 1.7 et ultérieures. Les méthodes et fonctions de rappel décrites ici sont définies dans les fichiers d’en-tête AccessEnabler.h et EntitlementDelegate.h .

Reportez-vous à [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) pour connaître le dernier SDK Android AccessEnabler.


**Remarque :** L’équipe d’authentification Adobe Pass vous encourage à utiliser uniquement les API d’authentification Adobe Pass *publiques* :

- Les API publiques sont disponibles *et entièrement testées* sur tous les types de clients pris en charge. Pour toute fonction publique, nous nous assurons que chaque type de client possède une version correspondante de la ou des méthodes associées.</span>
- Les API publiques doivent être aussi stables que possible, pour prendre en charge la compatibilité descendante et garantir que les intégrations de partenaires ne se rompent pas. Cependant, pour les API *non* publiques, nous nous réservons le droit de modifier leur signature à tout moment. Si vous rencontrez un flux particulier qui ne peut pas être pris en charge par une combinaison des appels publics actuels de l’API d’authentification Adobe Pass, la meilleure approche consiste à nous le faire savoir. En tenant compte de vos besoins, nous pouvons modifier les API publiques et fournir une solution stable à l’avenir.

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
- preautoriser
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

**Description :** instancie l’objet Access Enabler. Il doit y avoir une instance Access Enabler unique par instance d’application.

| Appel API : constructeur |
| --- |
| **public static** AccessEnabler **getInstance**(Context appContext, String softwareStatement, String redirectUrl)<br>        **renvoie** AccessEnablerException <br><br>**public static** AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl)<br>**renvoie** AccessEnablerException |

**Disponibilité :** v3.1.2+

**Paramètres :**

- *appContext* : contexte de l’application Android.
- env\_url : pour les tests à l’aide de l’environnement d’évaluation d’Adobe, env\_url peut être défini sur &quot;sp.auth-staging.adobe.com&quot;

**Obsolète :**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**Description :** Établit l’identité du programmeur. Un identifiant unique est attribué à chaque programmeur lors de l’enregistrement auprès de l’Adobe pour le système d’authentification Adobe Pass. Lorsque vous traitez de SSO et de jetons distants, l’état d’authentification peut changer lorsque l’application est en arrière-plan, setRequestor peut être appelé de nouveau lorsque l’application est mise en premier plan afin de se synchroniser avec l’état du système (récupération d’un jeton distant si SSO est activé ou suppression du jeton local si une déconnexion s’est produite entre-temps).

La réponse du serveur contient une liste de MVPD ainsi que certaines informations de configuration qui sont jointes à l’identité du programmeur. La réponse du serveur est utilisée en interne par le code Access Enabler. Seul l’état de l’opération (c.-à-d. SUCCESS/FAIL) est présenté à votre application via le rappel setRequestorComplete() .

Si le paramètre *urls* n’est pas utilisé, l’appel réseau obtenu cible l’URL du fournisseur de services par défaut : l’environnement de production/version d’Adobe.

Si une valeur est fournie pour le paramètre *urls* , l’appel réseau obtenu cible toutes les URL fournies dans le paramètre *urls* . Toutes les requêtes de configuration sont déclenchées simultanément dans des threads distincts. Le premier participant a la priorité lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, Access Enabler mémorise l&#39;URL du prestataire associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.

| Appel API : configuration du demandeur |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Disponibilité :** v3.0+

| Appel API : configuration du demandeur |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Disponibilité :** v3.0+


**Paramètres :**

- *requestorID* : identifiant unique associé au programmeur. Transmettez l’identifiant unique attribué par Adobe à votre site lorsque vous vous êtes enregistré pour la première fois auprès du service d’authentification Adobe Pass.

- *signedRequestorID* : copie de l’ID du demandeur signé numériquement avec votre clé privée. <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *urls* : paramètre facultatif ; par défaut, le fournisseur de services Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points de terminaison pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances du fournisseur de services d’authentification Adobe Pass. Dans ce cas, la liste MVPD est composée des points de terminaison de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.

**Rappels déclenchés :** `setRequestorComplete()`

Obsolète :

    public void setRequestor(String requestorId, String signedRequestorId)
    
    public void setRequestor (String requestorId, String signedRequestorId, ArrayList&lt;String> urls)

[Revenez à l’API Android...](#api)

### setRequestorComplete {#setRequestorComplete}

**Description :** Rappel déclenché par l’activateur d’accès qui informe votre application que la phase de configuration est terminée. Il s’agit d’un signal indiquant que l’application peut commencer à émettre des demandes de droits. Aucune demande de droit ne peut être émise par l’application tant que la phase de configuration n’est pas terminée.

| Rappel : fin de la configuration du demandeur |
| --- |
| java public void setRequestorComplete(int status) |

**Disponibilité :** v1.0+

**Paramètres :**

- *status* : peut prendre l’une des valeurs suivantes :
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - La phase de configuration a été terminée avec succès
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - la phase de configuration a échoué
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - La phase de configuration a été terminée avec succès
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - la phase de configuration a échoué

**Déclenché par :** `setRequestor()`

[Revenez à l’API Android...](#api)

### setOptions {#setOptions}

**Description :** configure les options du SDK global. Il accepte un argument **Map\&lt;String, String\>** en tant qu’argument. Les valeurs de la carte seront transmises au serveur avec chaque appel réseau effectué par le SDK.

Les valeurs seront transmises au serveur indépendamment du flux actuel (authentification/autorisation). Si vous souhaitez modifier les valeurs, vous pouvez appeler cette méthode à tout moment.

| Appel API : setOptions |
| --- |
| public void setOptions(HashMap&lt;String,String> options) |

**Disponibilité :** v1.9.2+

**Paramètres :**

- *options* : Carte&lt;Chaîne, Chaîne> contenant des options de SDK global. Actuellement, les options suivantes sont disponibles :
   - **applicationProfile** - Il peut être utilisé pour créer des configurations de serveur en fonction de cette valeur.
   - **ap_vi** - Identifiant Experience Cloud (visitorID). Cette valeur peut être utilisée ultérieurement pour les rapports d’analyse avancés.
   - **ap_ai** - Advertising ID
   - **device_info** - Informations sur le client comme décrit ici : [Transfert de la connexion et de l’application de l’appareil d’informations sur le client](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md).

[Retour au sommet...](#apis)


### checkAuthentication {#checkAuthN}

**Description :** Vérifie l’état d’authentification. Pour ce faire, il recherche un jeton d’authentification valide dans l’espace de stockage du jeton local. Cette méthode n&#39;effectue aucun appel réseau et nous vous recommandons de l&#39;appeler sur le thread principal. Il est utilisé par l’application pour interroger l’état d’authentification de l’utilisateur et mettre à jour l’interface utilisateur en conséquence (c’est-à-dire mettre à jour l’interface utilisateur de connexion/déconnexion). L’état d’authentification est communiqué à l’application via le rappel [*setAuthenticationStatus()*](#setAuthNStatus) .

Si un MVPD prend en charge la fonction &quot;Authentification par demandeur&quot;, plusieurs jetons d’authentification peuvent être stockés sur un appareil.  Pour plus d’informations sur cette fonctionnalité, voir la section [Instructions de mise en cache](#$caching) dans l’aperçu technique d’Android.

| Appel API : vérification de l’état d’authentification |
| --- |
| public void checkAuthentication() |

**Disponibilité :** v1.0+

**Paramètres :** Aucun

**Rappels déclenchés :** `setAuthenticationStatus()`

[Revenez à l’API Android...](#api)


### getAuthentication {#getAuthN}

**Description :** Démarre le processus d’authentification complet. Il commence par vérifier l’état d’authentification. Si elle n’est pas déjà authentifiée, la machine à états du flux d’authentification est démarrée :

- Si la dernière tentative d’authentification a réussi, la phase de sélection MVPD est ignorée et le rappel [*navigateToUrl()*](#navigagteToUrl) est déclenché. L’application utilise ce rappel pour instancier le contrôle WebView qui présente à l’utilisateur la page de connexion du MVPD.
- Si la dernière tentative d’authentification a échoué ou si l’utilisateur s’est explicitement déconnecté, le rappel [*displayProviderDialog()*](#displayProviderDialog) est déclenché. Votre application utilise ce rappel pour afficher l’interface utilisateur de sélection MVPD. Votre application doit également reprendre le flux d’authentification en informant la bibliothèque Access Enabler de la sélection du MVPD de l’utilisateur via la méthode [setSelectedProvider()](#setSelectedProvider) .

Comme les informations d’identification de l’utilisateur sont vérifiées sur la page de connexion MVPD, votre application est requise pour surveiller les multiples opérations de redirection qui ont lieu pendant que l’utilisateur s’authentifie sur la page de connexion du MVPD. Lorsque les informations d’identification correctes sont saisies, le contrôle WebView est redirigé vers une URL personnalisée définie par la constante *AccessEnabler.ADOBEPASS\_REDIRECT\_URL*. Cette URL n’est pas destinée à être chargée par le WebView. L’application doit intercepter cette URL et interpréter cet événement comme un signal indiquant que la phase de connexion est terminée. Il doit ensuite céder le contrôle à Access Enabler pour terminer le flux d&#39;authentification (en appelant la méthode *getAuthenticationToken()* ).

Si un MVPD prend en charge la fonction &quot;Authentification par demandeur&quot;, plusieurs jetons d’authentification peuvent être stockés sur un appareil (un par programmeur).  Pour plus d’informations sur cette fonctionnalité, voir la section [Instructions de mise en cache](#$caching) dans l’aperçu technique d’Android.

Enfin, l’état d’authentification est communiqué à l’application via le rappel *setAuthenticationStatus()* .



| Appel API : initie le flux d’authentification |
| --- |
| public void getAuthentication() |

**Disponibilité :** v1.0+

| Appel API : initie le flux d’authentification |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;String, Object> genericData) |

**Disponibilité :** v1.8+

**Paramètres :**

- *forceAuthn* : indicateur spécifiant si le flux d’authentification doit être démarré, que l’utilisateur soit déjà authentifié ou non.
- *data* : carte composée de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Revenez à l’API Android...](#api)

### displayProviderDialog {#displayProviderDialog}

**Description** Rappel déclenché par l’activateur d’accès pour informer l’application que les éléments d’IU appropriés doivent être instanciés pour permettre à l’utilisateur de sélectionner le MVPD de votre choix. Le rappel fournit une liste d’objets MVPD avec des informations supplémentaires qui peuvent aider à créer correctement le panneau d’interface utilisateur de sélection (telles que l’URL pointant vers le logo du MVPD, le nom d’affichage convivial, etc.).

Une fois que l’utilisateur a sélectionné le MVPD souhaité, l’application de couche supérieure est requise pour reprendre le flux d’authentification en appelant *setSelectedProvider()* et en lui transmettant l’identifiant du MVPD correspondant à la sélection de l’utilisateur.

>[!NOTE]
>
> Abandon du flux d’authentification
> </br></br>
> Veuillez noter que c’est un point où l’utilisateur a la possibilité d’appuyer sur le bouton &quot;Retour&quot;, ce qui équivaut à interrompre le flux d’authentification. Dans un tel scénario, votre application doit appeler la méthode `setSelectedProvider()`, en transmettant *null* en tant que paramètre, pour donner à Access Enabler la possibilité de réinitialiser son ordinateur d’état d’authentification.

| Rappel : affichage de l’interface utilisateur de sélection MVPD |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**Disponibilité :** v1.0+

**Paramètres** :

- *mvpds* : liste des objets MVPD contenant des informations relatives au MVPD que l’application peut utiliser pour créer les éléments d’IU de sélection du MVPD.

**Déclenché par :** `getAuthentication(), getAuthorization()`

[Revenez à l’API Android...](#api)


### setSelectedProvider {#setSelectedProvider}

**Description :** Cette méthode est appelée par votre application pour informer l’Access Enabler de la sélection MVPD de l’utilisateur. L’application peut utiliser cette méthode pour sélectionner ou modifier le fournisseur de services utilisé pour l’authentification.

Si le MVPD sélectionné est un MVPD TempPass , il s’authentifiera automatiquement avec ce MVPD sans avoir à appeler getAuthentication() par la suite.

Veuillez noter que ceci n’est pas possible pour la transmission temporaire de conversion où des paramètres supplémentaires sont donnés sur la méthode getAuthentication() .

Lors de la transmission de *null* en tant que paramètre, Access Enabler suppose que l’utilisateur a annulé le flux d’authentification (c’est-à-dire qu’il a appuyé sur le bouton &quot;Précédent&quot;), et répond en réinitialisant la machine-état d’authentification et en appelant le rappel *setAuthenticationStatus()* avec le code d’erreur `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR`.

| Appel API : définir le fournisseur actuellement sélectionné |
| --- |
| public void setSelectedProvider(String mvpdId) |

**Disponibilité :** v1.0+

**Paramètres :** Aucun

**Rappels déclenchés :** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Revenez à l’API Android...](#api)


### navigateToUrl {#navigagteToUrl}

**Obsolète :** À partir de la version 3.0 du SDK Android, navigateToUrl n’est utilisé que si l’onglet personnalisé de Chrome n’est pas présent sur l’appareil.

**Description :** Rappel déclenché par l’activateur d’accès qui informe l’application que l’utilisateur doit se voir présenter la page de connexion MVPD pour pouvoir saisir ses informations d’identification. Access Enabler transmet en paramètre l’URL de la page de connexion MVPD. Votre application est nécessaire pour instancier un contrôle WebView et le diriger vers cette URL. En outre, l&#39;application est requise pour surveiller les URL chargées par le contrôle WebView et intercepter l&#39;opération de redirection ciblant l&#39;URL personnalisée définie par la constante `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)`. Lors de cet événement, l’application doit fermer ou masquer le contrôle WebView et revenir à la bibliothèque Access Enabler en appelant la méthode *getAuthenticationToken()* . Access Enabler complète le flux d’authentification en récupérant le jeton d’authentification du serveur principal et en le stockant localement dans le stockage du jeton.

>[!WARNING]
>
> **Abandon du flux d’authentification** <br>Notez que c’est un point où l’utilisateur a la possibilité d’appuyer sur le bouton &quot;Retour&quot;, ce qui équivaut à interrompre le flux d’authentification. Dans un tel scénario, votre application doit appeler la méthode _setSelectedProvider()_ en transmettant _null_ comme paramètre et en donnant la possibilité à Access Enabler de réinitialiser son ordinateur d’état d’authentification.

| Rappel : afficher la page de connexion MVPD |
| --- |
| public void navigateToUrl(String url) |

**Disponibilité :** v1.0+

**Paramètres :**

- *url* : URL pointant vers la page de connexion du MVPD

**Déclenché par :** `getAuthentication(), setSelectedProvider()`

[Revenez à l’API Android...](#api)


### getAuthenticationToken {#getAuthNToken}

**Obsolète :** À partir de la version 3.0 du SDK Android, comme l’onglet personnalisé Chrome est utilisé pour l’authentification, cette méthode n’est plus utilisée à partir de l’application.

**Description :** exécute le flux d’authentification en demandant le jeton d’authentification auprès du serveur principal. Cette méthode ne doit être appelée par votre application qu’en réponse à un événement où le contrôle WebView hébergeant la page de connexion MVPD est redirigé vers l’URL personnalisée définie par la constante `AccessEnabler.ADOBEPASS_REDIRECT_URL`.

| Appel API : récupération du jeton d’authentification |
| --- |
| public void getAuthenticationToken() |

**Disponibilité :** v1.0+

**Paramètres :**

- *cookies* : cookies définis sur le domaine cible (voir l’application de démonstration dans le SDK pour une implémentation de référence).

**Rappels déclenchés :** `setAuthenticationStatus()`, `sendTrackingData()`

[Revenez à l’API Android...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**Description :** Rappel déclenché par l’activateur d’accès qui informe
l’application de l’état du flux d’authentification. Il y en a beaucoup
les emplacements où ce flux peut échouer, en raison de la variable
interaction ou en raison d&#39;autres scénarios imprévus (c&#39;est-à-dire réseau
problèmes de connectivité, etc.). Ce rappel informe l’application de la variable
l’état de réussite/d’échec du flux d’authentification, mais également
fournir des informations supplémentaires sur la raison de l’échec, le cas échéant ;

| Rappel : signalez l’état du flux d’authentification. |
| --- |
| public void setAuthenticationStatus(int status, String errorCode) |

**Disponibilité :** v1.0+

**Paramètres :**

- *status* : peut prendre l’une des valeurs suivantes :
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - le flux d’authentification a été terminé avec succès
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - Échec du flux d’authentification
- *code* : raison de l’échec. Si *status* est `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`, alors *code* est une chaîne vide (c’est-à-dire définie par la constante `AccessEnablerConstants.USER_AUTHENTICATED`). En cas d’échec, ce paramètre peut prendre l’une des valeurs suivantes :
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - L’utilisateur n’est pas authentifié. En réponse à l’appel de la méthode *checkAuthentication()* lorsqu’il n’existe pas de jeton d’authentification valide dans le cache de jeton local.
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler a réinitialisé l’ordinateur-état d’authentification après que l’application de couche supérieure a passé *null* à `setSelectedProvider()` pour interrompre le flux d’authentification.  L’utilisateur a probablement annulé le flux d’authentification (c’est-à-dire qu’il a appuyé sur le bouton &quot;Retour&quot;).
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - Le flux d’authentification a échoué pour des raisons telles que l’indisponibilité du réseau ou l’utilisateur a explicitement annulé le flux d’authentification.

**Déclenché par :** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Revenez à l’API Android...](#api)


### checkPreauthorizedResources {#checkPreauth}

>**Obsolète :** À partir du SDK Android 3.6, l’API de préautorisation remplace checkPreauthorizedResources, fournissant des codes d’erreur étendus.

**Description :** Cette méthode est utilisée par l’application pour déterminer si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques. L’objectif principal de cette méthode est de récupérer des informations à utiliser dans la décoration de l’interface utilisateur (par exemple, en indiquant l’état d’accès avec les icônes de verrouillage et de déverrouillage).

| Appel API : définir le fournisseur actuellement sélectionné |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Disponibilité :** v1.3+

**Paramètres :** Le paramètre `resources` est un tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’ID de ressource. L’ID de ressource est soumis aux mêmes limites que l’ID de ressource dans l’appel `getAuthorization()`, c’est-à-dire qu’il doit s’agir d’une valeur convenue établie entre le programmeur et le MVPD ou un fragment RSS multimédia.

**Rappel déclenché :** `preauthorizedResources()`

[Revenez à l’API Android...](#api)


### checkPreauthorizedResources {#checkPreauth2}

**Obsolète :** À partir du SDK Android 3.6, l’API de préautorisation remplace checkPreauthorizedResources, fournissant des codes d’erreur étendus.

**Description :** Cette méthode est utilisée par l’application pour déterminer si l’utilisateur est déjà autorisé à afficher des ressources protégées spécifiques. L’objectif principal de cette méthode est de récupérer des informations à utiliser dans la décoration de l’interface utilisateur (par exemple, en indiquant l’état d’accès avec les icônes de verrouillage et de déverrouillage).

| Appel API : définir le fournisseur actuellement sélectionné |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**Disponibilité :** v3.1+

**Paramètres :** Le paramètre `resources` est un tableau de ressources pour lequel l’autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’ID de ressource. L’ID de ressource est soumis aux mêmes limites que l’ID de ressource dans l’appel `getAuthorization()`, c’est-à-dire qu’il doit s’agir d’une valeur convenue établie entre le programmeur et le MVPD ou un fragment RSS multimédia.

Le paramètre `cache` indique si la réponse de préautorisation mise en cache peut être utilisée ou non. Par défaut, le cache est défini sur true, le SDK renvoie une réponse précédemment mise en cache si elle est disponible.

**Rappel déclenché :** `preauthorizedResources()`

[Revenez à l’API Android...](#api)

### preauthorizedResources {#preauthResources}

**Obsolète :** À partir du SDK Android 3.6, l’API de préautorisation remplace checkPreauthorizedResources, fournissant des codes d’erreur étendus. Le rappel preauthorizedResources ne sera pas appelé sur la nouvelle API.


**Description :** Rappel déclenché par checkPreauthorizedResources(). Fournit une liste des ressources que l’utilisateur est déjà autorisé à afficher.

| Appel API : définir le fournisseur actuellement sélectionné |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Disponibilité :** v1.3+

**Paramètres :** Le paramètre `resources` est un tableau de ressources pour lequel l’utilisateur est déjà autorisé à afficher.

**Déclenché par :** `checkPreauthorizedResources()`

[Revenez à l’API Android...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**Description :** Cette méthode est utilisée par l’application pour vérifier l’état de l’autorisation. Il commence par vérifier l’état d’authentification. Si elle n’est pas authentifiée, le rappel *setTokenRequestFailed()* est déclenché et la méthode quitte. Si l’utilisateur est authentifié, il déclenche également le flux d’autorisation. Voir les détails sur la méthode *getAuthorization()* .

| Appel API : vérification de l’état de l’autorisation |
| --- |
| public void checkAuthorization(String resourceId) |

**Disponibilité :** v1.0+

| Appel API : vérification de l’état de l’autorisation |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Disponibilité :** v1.8+

**Paramètres :**

- *resourceId* : identifiant de la ressource pour laquelle l’utilisateur demande l’autorisation.
- *data* : carte composée de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Revenez à l’API Android...](#api)


### <span id="getAuthZ"></span>getAuthorization

**Description :** Cette méthode est utilisée par l’application pour lancer le flux d’autorisation. Si l’utilisateur n’est pas déjà authentifié, il lance également le flux d’authentification. Si l’utilisateur est authentifié, Access Enabler émet des requêtes pour le jeton d’autorisation (si aucun jeton d’autorisation valide n’est présent dans le cache de jeton local) et pour le jeton multimédia de courte durée. Une fois le jeton multimédia court obtenu, le flux d’autorisation est considéré comme terminé. Le rappel *setToken()* est déclenché et le jeton multimédia court est diffusé en tant que paramètre à l’application. Si, pour une raison quelconque, l’autorisation échoue, le rappel *tokenRequestFailed()* est déclenché et le code d’erreur et les détails sont fournis.

| Appel API : lancer le flux d’autorisation |
| --- |
| public void getAuthorization(String resourceId) |

**Disponibilité :** v1.0+

| Appel API : lancer le flux d’autorisation |
| --- |
| public void getAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Disponibilité :** v1.8+

**Paramètres :**

- *resourceId* : identifiant de la ressource pour laquelle l’utilisateur demande l’autorisation.
- *data* : carte composée de paires clé-valeur à envoyer au service de passe de télévision payante. Adobe peut utiliser ces données pour activer les fonctionnalités futures sans modifier le SDK.

**Rappels déclenchés :** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **Rappels supplémentaires déclenchés** <br> Cette méthode peut également déclencher les rappels suivants (si le flux d’authentification est également lancé) : *setAuthenticationStatus()*, *displayProviderDialog()*, *navigateToUrl()*

**REMARQUE : Dans la mesure du possible, utilisez checkAuthorization() au lieu de getAuthorization(). La méthode getAuthorization() démarre un flux d’authentification complet (si l’utilisateur n’est pas authentifié), ce qui peut entraîner une mise en oeuvre compliquée du côté du programmeur.**

[Revenez à l’API Android...](#api)


### setToken {#setToken}

**Description :** Rappel déclenché par l’activateur d’accès qui informe votre application que le flux d’autorisation a été terminé avec succès. Le jeton multimédia de courte durée est également fourni en tant que paramètre.

| Rappel : le flux d’autorisation a réussi |
| --- |
| public void setToken(Jeton de chaîne, String resourceId) |

**Disponibilité :** v1.0+

**Paramètres :**

- *token* : jeton multimédia de courte durée
- *resourceId* : ressource pour laquelle l’autorisation a été obtenue

**Déclenché par :** `checkAuthorization()`, `getAuthorization()`


[Revenez à l’API Android...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**Description :** Rappel déclenché par l’activateur d’accès qui informe l’application de couche supérieure que le flux d’autorisation a échoué.

| Rappel : échec du flux d’autorisation |
| --- |
| public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription) |

**Disponibilité :** v1.0+

**Paramètres :**

- *resourceId* : ressource pour laquelle l’autorisation a été obtenue
- *errorCode* : code d’erreur associé au scénario d’échec. Valeurs possibles :
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` - L’utilisateur n’a pas pu autoriser pour la ressource donnée
- *errorDescription* : détails supplémentaires sur le scénario d’échec. Si cette chaîne descriptive n’est disponible pour aucune raison, l’authentification Adobe Pass envoie une chaîne vide **(&quot;&quot;)**.

  Cette chaîne peut être utilisée par un MVPD pour transmettre des messages d’erreur personnalisés ou des messages liés aux ventes. Par exemple, si l’autorisation d’une ressource est refusée à un abonné, le MVPD peut envoyer un message tel que : &quot;Vous n’avez actuellement pas accès à ce canal dans votre package. Si vous souhaitez mettre à niveau votre package, cliquez ici.&quot; Le message est transmis par l’authentification Adobe Pass via ce rappel au programmeur, qui a la possibilité de l’afficher ou de l’ignorer. L’authentification Adobe Pass peut également utiliser ce paramètre pour fournir une notification de la condition qui a pu entraîner une erreur. Par exemple, &quot;Une erreur de réseau s’est produite lors de la communication avec le service d’autorisation du fournisseur&quot;.

**Déclenché par :** `checkAuthorization(), getAuthorization()`

[Revenez à l’API Android...](#api)

### déconnexion {#logout}

**Description :** Utilisez cette méthode pour lancer le flux de déconnexion. La déconnexion est le résultat d’une série d’opérations de redirection HTTP en raison du fait que l’utilisateur doit être déconnecté des serveurs d’authentification Adobe Pass et des serveurs du MVPD. Par conséquent, ce flux ne peut pas être complété avec une requête HTTP simple émise par la bibliothèque Access Enabler. Un onglet personnalisé Chrome est utilisé par le SDK pour exécuter les opérations de redirection HTTP. Ce flux sera visible par l’utilisateur et fermé une fois terminé

| Appel API : lancer le flux de déconnexion |
| --- |
| public void logout() |

**Disponibilité :** v1.0+

**Paramètres :** Aucun

**Rappels déclenchés :**

- `navigateToUrl()` pour la version du SDK antérieure à 3.0
- `setAuthenticationStatus()` pour la version du SDK > 3.0


[Revenez à l’API Android...](#api)


### getSelectedProvider {#getSelectedProvider}

**Description :** Utilisez cette méthode pour déterminer le fournisseur actuellement sélectionné.

| Appel API : déterminer le MVPD actuellement sélectionné |
| --- |
| public void getSelectedProvider() |

**Disponibilité :** v1.0+

**Paramètres :** Aucun

**Rappels déclenchés :** `selectedProvider()`

[Revenez à l’API Android...](#api)


### <span id="selectedProvider"></span>selectedProvider

**Description :** Rappel déclenché par l’activateur d’accès qui fournit des informations sur le MVPD actuellement sélectionné à l’application.

| Rappel : informations sur le MVPD actuellement sélectionné |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**Disponibilité :** v1.0+

**Paramètres :**

- *mvpd* : objet contenant des informations sur le MVPD actuellement sélectionné

**Déclenché par :** `getSelectedProvider()`

[Revenez à l’API Android...](#api)


### getMetadata {#getMetadata}

**Description :** Utilisez cette méthode pour récupérer des informations exposées en tant que métadonnées par la bibliothèque Access Enabler. L’application peut accéder à ces informations en fournissant un objet composite MetadataKey.

| Appel API : requête de AccessEnabler pour les métadonnées |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**Disponibilité :** v1.0+

Deux types de métadonnées sont disponibles pour les programmeurs :

- Métadonnées statiques (jeton d’authentification TTL, jeton d’autorisation TTL et ID de périphérique)
- Métadonnées utilisateur (informations spécifiques à l’utilisateur, telles que l’ID utilisateur et le code postal ; transmises d’un MVPD à l’appareil d’un utilisateur lors des flux d’authentification et/ou d’autorisation)

**Paramètres :**

- *metadataKey* : structure de données qui encapsule une variable key et args, avec la signification suivante :
   - Si key est `METADATA_KEY_USER_META` et args contient un objet SerializableNameValuePair dont le nom est = `METADATA_ARG_USER_META` et la valeur = `[metadata_name]`, la requête est alors effectuée pour les métadonnées utilisateur. La liste actuelle des types de métadonnées utilisateur disponibles :
      - `zip` - Code postal

      - `householdID` - Identifiant du foyer. Si un MVPD ne prend pas en charge les sous-comptes, cela sera identique à `userID`.

      - `maxRating` - Note parentale maximale pour l’utilisateur

      - `userID` - Identifiant de l’utilisateur. Si un MVPD prend en charge des sous-comptes et que l’utilisateur n’est pas le compte principal, `userID` sera différent de `householdID`.

      - `channelID` - Liste des canaux que l’utilisateur est autorisé à afficher
   - Si la clé est `METADATA_KEY_DEVICE_ID`, la requête est effectuée pour obtenir l’identifiant de l’appareil actuel. Notez que cette fonctionnalité est désactivée par défaut et que les programmeurs doivent contacter Adobe pour plus d’informations sur l’activation et les frais.
   - Si la clé est `METADATA_KEY_TTL_AUTHZ` et que args contient un objet SerializableNameValuePair avec le nom = `METADATA_ARG_RESOURCE_ID` et la valeur = `[resource_id]`, la requête est effectuée pour obtenir le délai d’expiration du jeton d’autorisation associé à la ressource spécifiée.
   - Si la clé est `METADATA_KEY_TTL_AUTHN`, la requête est effectuée pour obtenir le délai d’expiration du jeton d’authentification.



>[!NOTE]
>
>Pour le SDK 3.4.0, les constantes : `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` sont disponibles à partir de com.adobe.adobepass.accessible.api.profile.UserProfileService.



>[!NOTE]
>
>Les métadonnées utilisateur réelles disponibles pour un programmeur dépendent de ce qu’un MVPD rend disponible.  Cette liste sera complétée à mesure que de nouvelles métadonnées seront disponibles et ajoutées au système d’authentification Adobe Pass.

**Rappels déclenchés :** [`setMetadataStatus()`](#setMetadaStatus)

**Plus d’informations :** [Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md)

[Revenez à l’API Android...](#api)

### setMetadataStatus {#setMetadaStatus}

**Description :** Rappel déclenché par l’activateur d’accès qui diffuse les métadonnées demandées par le biais d’un appel *getMetadata()* .

| Rappel : résultat de la requête de récupération de métadonnées |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Disponibilité :** v1.0+

**Paramètres :**

- *key* : objet MetadataKey contenant la clé pour laquelle la valeur de métadonnées est demandée et les paramètres associés (voir application de démonstration pour une mise en oeuvre de référence).
- *result* : objet composite contenant les métadonnées demandées. L’objet comporte les champs suivants :
   - *simpleResult* : chaîne représentant la valeur de métadonnées lorsque la demande a été faite pour Authentication TTL, Authorization TTL ou Device ID. Cette valeur est nulle si la requête a été effectuée pour les métadonnées utilisateur.

   - *userMetadataResult* : objet contenant la représentation Java d’une charge utile de métadonnées utilisateur JSON.\
     Par exemple :

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
```

est traduit en Java en tant que :

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

Cette valeur est nulle lorsque la demande a été effectuée pour des métadonnées simples (Authentification TTL, Autorisation TTL ou ID d’appareil).

- *encrypted* : valeur booléenne qui spécifie si les métadonnées récupérées sont chiffrées ou non. Ce paramètre n’est significatif que pour les requêtes de métadonnées utilisateur. Il n’a aucune signification pour les métadonnées statiques (par exemple : Authentification TTL) qui sont toujours reçues sans chiffrement. Si ce paramètre est défini sur True, c’est au programmeur d’obtenir la valeur des métadonnées utilisateur non chiffrées en effectuant un décryptage RSA à l’aide de la clé privée de mise en whiteliste (la même clé privée utilisée pour la signature de l’ID du demandeur dans l’appel [`setRequestor`](#setRequestor)).

**Déclenché par :** [`getMetadata()`](#getMetadata)

**Plus d’informations :** [Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md)


[Revenez à l’API Android...](#api)


### getVersion {#getVersion}

**Description :** Cette méthode peut être utilisée pour récupérer la version de la bibliothèque AccessEnabler.

| Appel API : obtenir la version AccessEnabler |
| --- |
| ```public static String getVersion()``` |


[Revenez à l’API Android...](#api)

</br>

## Suivi des événements {#tracking}

L’activation d’accès déclenche un rappel supplémentaire qui n’est pas nécessairement lié aux flux de droits. La mise en oeuvre de la fonction de rappel de suivi des événements nommée *sendTrackingData()* est facultative, mais elle permet à l’application de suivre des événements spécifiques et de compiler des statistiques telles que le nombre de tentatives d’authentification/d’autorisation réussies/échouées. Voici la spécification du rappel *sendTrackingData()* :


### sendTrackingData {#sendTrackingData}

**Description :** Rappel déclenché par l’activateur d’accès signalant à l’application l’occurrence de divers événements tels que l’achèvement/l’échec des flux d’authentification/d’autorisation. Le type d’appareil, le type de client Access Enabler et le système d’exploitation sont également signalés par sendTrackingData().

>[!WARNING]
>
> Le type d’appareil et le système d’exploitation sont dérivés de l’utilisation d’une bibliothèque Java publique ([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)) et de la chaîne de l’agent utilisateur. Notez que ces informations ne sont fournies que pour ventiler les mesures opérationnelles en catégories d’appareils, mais que cet Adobe ne peut pas assumer la responsabilité de résultats incorrects. Utilisez la nouvelle fonctionnalité en conséquence.


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

**Paramètres :**

- *event* : l’événement suivi. Il existe trois types d’événements de suivi possibles :
   - **authorizationDetection :** chaque fois qu’une requête de jeton d’autorisation est renvoyée (le type d’événement est `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection :** chaque fois qu’une vérification de l’authentification se produit (le type d’événement est `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** lorsque l’utilisateur sélectionne un MVPD dans le formulaire de sélection MVPD (le type d’événement est `EVENT_MVPD_SELECTION`)
- *data* : données supplémentaires associées à l’événement signalé. Ces données sont présentées sous la forme d&#39;une liste de valeurs.

Vous trouverez ci-dessous des instructions pour interpréter les valeurs dans *data*
tableau :

- Pour le type d’événement *`EVENT_AUTHN_DETECTION`:*
   - **0** - Indique si la requête de jeton a réussi (true/false) et si la valeur ci-dessus est vraie :
   - **1** - Chaîne d’identifiant MVPD
   - **2** - GUID (hachage md5)
   - **3** - Jeton déjà en cache (true/false)
   - **4** - Type de périphérique
   - **5** - Type de client Access Enabler
   - **6** - Type de système d’exploitation

- Pour le type d’événement `EVENT_AUTHZ_DETECTION`
   - **0** - Indique si la requête de jeton a réussi (true/false) et si elle a réussi :
   - **1** - ID MVPD
   - **2** - GUID (hachage md5)
   - **3** - Jeton déjà en cache (true/false)
   - **4** - Erreur
   - **5** - Détails
   - **6** - Type de périphérique
   - **7** - Type de client Access Enabler
   - **8** - Type de système d’exploitation

- Pour le type d’événement `EVENT_MVPD_SELECTION`
   - **0** - ID du MVPD actuellement sélectionné
   - **1** - Type de périphérique
   - **2** - Type de client Access Enabler
   - **3** - Type de système d’exploitation

**Déclenché par :** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Revenez à l’API Android...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
