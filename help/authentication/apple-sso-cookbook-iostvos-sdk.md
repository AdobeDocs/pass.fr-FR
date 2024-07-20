---
title: Guide pas à pas Apple SSO (SDK iOS/tvOS)
description: Guide pas à pas Apple SSO (SDK iOS/tvOS)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1861'
ht-degree: 0%

---

# Guide pas à pas Apple SSO (SDK iOS/tvOS) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#Introduction}

Le SDK Adobe Primetime Authentication AccessEnabler iOS/tvOS peut prendre en charge l’authentification SSO (Single Sign-On) par plateforme pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS via ce que nous appelons le processus SSO Apple.

Notez que ce document agit comme une extension de la documentation existante du SDK AccessEnabler iOS/tvOS, qui se trouve [ici](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## Guide pas à pas {#Cookbook}

Afin de bénéficier de l’expérience utilisateur SSO d’Apple, une application doit intégrer le SDK AccessEnabler iOS/tvOS et suivre la séquence de conseils présentée ci-dessous.

</br>

### Conditions préalables {#Prerequisites}

</br>

#### Autorisation

>[!TIP]
>
> **<u>Conseil Pro :</u>** Pour avoir accès aux informations d’abonnement de l’utilisateur, l’utilisateur doit accorder à l’application l’autorisation de continuer, comme pour permettre l’accès à la caméra ou au microphone de l’appareil. Cette autorisation doit être demandée par application et l’appareil enregistre la sélection de l’utilisateur. Gardez à l’esprit que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès aux autorisations du fournisseur de télévision) ou à la section depuis *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Nous vous recommandons de demander l’autorisation de l’utilisateur lorsque l’application entre en premier plan, mais il s’agit uniquement d’une suggestion, car l’application peut rechercher l’autorisation [d’accéder à](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur à tout moment avant d’avoir besoin de l’authentification de l’utilisateur. En outre, les API du SDK iOS/tvOS AccessEnabler demandent automatiquement l’autorisation de l’utilisateur lorsqu’il en a besoin.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Si l’utilisateur n’accorde pas l’accès à ses informations d’abonnement ou si la communication avec la structure de compte d’abonné vidéo échoue, le SDK AccessEnabler iOS/tvOS revient au flux d’authentification normal.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Nous recommandons aux utilisateurs qui refusent d’autoriser l’accès aux informations d’abonnement en expliquant les avantages de l’expérience utilisateur de connexion unique (SSO). Gardez à l’esprit que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès aux autorisations du fournisseur de télévision) ou à la section depuis *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS.


```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

</br>

#### Rappels

>[!TIP]
>
> **<u>Conseil Pro :</u>** Mettez en oeuvre la liste suivante de [rappels](/help/authentication/iostvos-sdk-api-reference.md) spécifiques au workflow SSO Apple.

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Rappel déclenché lorsque le sélecteur MVPD Apple s’ouvre.
- [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Rappel déclenché lorsque le sélecteur MVPD Apple se ferme.

</br>

#### Rapport d’erreurs

>[!TIP]
>
> **<u>Conseil Pro :</u>** Mettez en oeuvre la liste suivante de [codes d’erreur avancés](/help/authentication/error-reporting.md) spécifiques au workflow SSO Apple.

- ***N003*** - L’utilisateur a sélectionné l’option &quot;Autre fournisseur de télévision&quot; dans le sélecteur MVPD d’Apple.
- ***N004*** - L’utilisateur a sélectionné un fournisseur de télévision dans le sélecteur MVPD Apple, qui n’est pas pris en charge (intégration ou connexion unique désactivée) par le demandeur actuel.
- ***N005*** - L’utilisateur a décidé d’annuler le sélecteur MVPD standard ou le sélecteur MVPD Apple.
- ***VSA403*** - L’autorisation du fournisseur de télévision de l’utilisateur est refusée pour l’application.
- ***VSA404*** - L’autorisation du fournisseur de télévision de l’utilisateur n’est pas déterminée pour l’application.
- ***VSA503*** - Échec de la demande de métadonnées de compte d’abonné vidéo. Un contexte plus complet est fourni dans le champ *message* .
- ***AAPL / APPL_ERROR*** - Échec de la demande de métadonnées de compte d’abonné vidéo. Un contexte plus complet est fourni dans le champ *details* .

</br>

### Authentification {#Authentication}

>[!TIP]
>
> **<u>Conseil :</u>** Suivez les étapes ci-dessous pour la ou les implémentations iOS/iPadOS/tvOS.

1. L’application doit [initialiser](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) le SDK AccessEnabler iOS/tvOS.
1. L’application doit [définir l’identifiant du demandeur actuel](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Important :** Cette deuxième étape peut déclencher un [code d’erreur avancé](/help/authentication/error-reporting.md) spécifique au flux de travaux SSO Apple, au cas où **l’un des éléments suivants est true** :

   - ***VSA403*** - L’autorisation du fournisseur de télévision de l’utilisateur est refusée pour l’application.
   - ***VSA404*** - L’autorisation du fournisseur de télévision de l’utilisateur n’est pas déterminée pour l’application.
   - ***APPL*** - La communication entre le SDK AccessEnabler iOS/tvOS et la structure du compte d’abonné vidéo a rencontré une erreur.

   Cette deuxième étape consiste à essayer d’exchange silencieux du profil SSO Apple pour un jeton d’authentification d’Adobe, au cas où **tous les éléments ci-dessus seraient false** et **tous les éléments suivants sont vrais** :

   - La permission du fournisseur de télévision de l’utilisateur est accordée pour la demande.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil.
   - Le SDK AccessEnabler iOS/tvOS a reçu l’identifiant du fournisseur de télévision de l’utilisateur à partir de la structure du compte d’abonné vidéo.
   - L’intégration du fournisseur de télévision de l’utilisateur à l’application est activée par le biais du tableau de bord Adobe Primetime TVE.
   - L’authentification unique du fournisseur de télévision de l’utilisateur avec l’application est activée via le tableau de bord Adobe Primetime TVE.
   - Le fournisseur de télévision de l’utilisateur n’est pas dégradé par le biais du tableau de bord Adobe Primetime TVE.
   - Le SDK AccessEnabler iOS/tvOS a reçu la réponse SAML du fournisseur de télévision de l’utilisateur de la structure du compte d’abonné vidéo.

   **<u>Conseil Pro :</u>** Cette deuxième étape ne déclenche aucun autre rappel, à part le rappel [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete), car l’authentification n’a pas été explicitement initiée par l’application.

1. L&#39;application doit [vérifier l&#39;état d&#39;authentification](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Important :** Cette troisième étape peut déclencher un [code d’erreur avancé](/help/authentication/error-reporting.md) spécifique au workflow SSO Apple, au cas où **l’une des étapes suivantes est true** :

   - ***VSA403** - L’utilisateur est connecté à son compte de fournisseur de télévision à l’adresse
au niveau du système de l’appareil, mais l’autorisation du fournisseur de télévision de l’utilisateur est
refusé pour la demande.
   - ***VSA404** - L’utilisateur est connecté à son compte de fournisseur de télévision à l’adresse
au niveau du système de l’appareil, mais autorisation du fournisseur de télévision de l’utilisateur ;
est indéterminée pour l’application.
   - ***APPL\_ERROR** - L’utilisateur est connecté à son fournisseur de télévision
compte au niveau du système de l’appareil, mais la communication entre
le SDK AccessEnabler iOS/tvOS et le compte d’abonné vidéo
a rencontré une erreur.

   **Important :** Cette troisième étape déclenche le rappel [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) avec *status* égal à 0, au cas où **l’une des étapes suivantes est true** :

   - L’utilisateur n’est pas connecté à son compte de fournisseur de télévision au niveau du système de l’appareil ou par le biais d’un flux d’authentification régulier.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil ou par le biais d’un flux d’authentification régulier, mais le jeton d’authentification du fournisseur de télévision de l’utilisateur est transmis.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil ou par le biais d’un flux d’authentification standard, mais l’intégration du fournisseur de télévision de l’utilisateur à l’application est désactivée par le biais du tableau de bord Adobe Primetime TVE.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais l’authentification unique du fournisseur de télévision de l’utilisateur avec l’application est désactivée par le biais du tableau de bord Adobe Primetime TVE.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais son autorisation de fournisseur de télévision est refusée pour l’application.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais son autorisation de fournisseur de télévision n’est pas déterminée pour l’application.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais la communication entre le SDK AccessEnabler iOS/tvOS et la structure du compte d’abonné vidéo a rencontré une erreur.

   **Important :** Cette troisième étape déclenche le rappel [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) avec *status* égal à 1, au cas où **tous les éléments ci-dessus seraient faux.**


1. L&#39;application doit [initialiser l&#39;authentification](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) si la vérification de l&#39;état d&#39;authentification précédente a déclenché le rappel [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) avec *status* égal à 0.

   **<u>Conseil Pro :</u>** Mettez en oeuvre l’une des API SDK AccessEnabler iOS/tvOS suivantes [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) ou [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Important :** Cette quatrième étape peut déclencher un [code d’erreur avancé](/help/authentication/error-reporting.md) spécifique au workflow SSO Apple, au cas où **l’une des étapes suivantes est true** :

   - ***VSA403*** - L’autorisation du fournisseur de télévision de l’utilisateur est refusée pour l’application.
   - ***VSA404*** - L’autorisation du fournisseur de télévision de l’utilisateur n’est pas déterminée pour l’application.
   - ***VSA503*** - La communication entre le SDK AccessEnabler iOS/tvOS et la structure du compte d’abonné vidéo a rencontré une erreur.
   - ***N003*** - L’utilisateur a sélectionné l’option &quot;Autre fournisseur de télévision&quot; dans le sélecteur MVPD d’Apple.
   - ***N004*** - L’utilisateur a sélectionné un fournisseur de télévision dans le sélecteur MVPD Apple, qui n’est pas pris en charge (intégration ou connexion unique désactivée) par le demandeur actuel.
   - ***N005*** - L’utilisateur a décidé d’annuler le sélecteur MVPD standard ou le sélecteur MVPD Apple.

   **Important :** Cette quatrième étape reviendrait au flux d’authentification standard, en déclenchant le rappel [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) et **un** des [codes d’erreur avancés](/help/authentication/error-reporting.md) ci-dessus, au cas où **l’un des exemples ci-dessus est true**.

   **Important :** Cette quatrième étape revient au flux d’authentification standard, en déclenchant le rappel [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) ou [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) et **none** des [codes d’erreur avancés](/help/authentication/error-reporting.md) ci-dessus, si l’utilisateur a sélectionné un fournisseur de télévision qui ne prend pas en charge le SSO Apple, mais est présent, dans le sélecteur MVPD Apple.

   **<u>Conseil Pro :</u>** Le SDK AccessEnabler iOS/tvOS appelle en silence l’API [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv), au cas où l’utilisateur sélectionnerait un fournisseur de télévision, qui ne prend pas en charge la fonction SSO Apple, mais qui figure dans le sélecteur MVPD Apple.

   **Important :** Cette quatrième étape consiste à essayer d’exchange silencieux du profil d’authentification unique Apple pour un jeton d’authentification d’Adobe, au cas où **tous les éléments ci-dessus seraient false** et **tous les éléments suivants sont vrais** :

   - La permission du fournisseur de télévision de l’utilisateur est accordée pour la demande.
   - L’utilisateur est connecté/se connecte actuellement à son compte de fournisseur de télévision au niveau du système de l’appareil.
   - Le SDK AccessEnabler iOS/tvOS a reçu l’identifiant du fournisseur de télévision de l’utilisateur à partir de la structure du compte d’abonné vidéo.
   - L’intégration du fournisseur de télévision de l’utilisateur à l’application est activée par le biais du tableau de bord Adobe Primetime TVE.
   - L’authentification unique du fournisseur de télévision de l’utilisateur avec l’application est activée via le tableau de bord Adobe Primetime TVE.
   - Le fournisseur de télévision de l’utilisateur n’est pas dégradé par le biais du tableau de bord Adobe Primetime TVE.
   - Le SDK AccessEnabler iOS/tvOS a reçu la réponse SAML du fournisseur de télévision de l’utilisateur de la structure du compte d’abonné vidéo.



>**<u>Conseil Pro :</u>** Cette quatrième étape déclenche le rappel [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus), quel que soit le résultat *status*, puisque l’authentification a été explicitement initiée par l’application.


</br>

### Métadonnées {#Metadata}

L’application a la possibilité de déterminer si l’authentification a été effectuée suite à une connexion via la plateforme SSO ou non, à l’aide de l’API &quot;*tokenSource&quot;* [user metadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) du SDK AccessEnabler iOS/tvOS.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### Déconnexion {#Logout}

La structure [ du compte d’abonné vidéo ](https://developer.apple.com/documentation/videosubscriberaccount) ne fournit pas d’API pour déconnecter par programmation les personnes qui se sont connectées à leur compte de fournisseur de télévision au niveau du système de l’appareil. Par conséquent, pour que la déconnexion entre en vigueur, l’utilisateur final doit se déconnecter explicitement de *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS. L’autre option dont dispose l’utilisateur consiste à retirer l’autorisation d’accéder aux informations d’abonnement de l’utilisateur dans la section des paramètres de l’application spécifique (accès aux autorisations du fournisseur de télévision).

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode par le biais de l’API [logout](/help/authentication/iostvos-sdk-api-reference.md#logout) AccessEnabler iOS/tvOS SDK.


>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations tvOS.

- L’application doit [lancer la déconnexion](/help/authentication/iostvos-sdk-api-reference.md#logout) à partir du SDK AccessEnabler iOS/tvOS. Cela ne faciliterait pas le nettoyage de session du côté MVPD.
- L’application doit demander/inviter l’utilisateur à se déconnecter explicitement de *`Settings -> Accounts -> TV Provider`* sur tvOS uniquement au cas où le code d’état [*VSA203* est déclenché](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations iOS/iPadOS.

- L’application doit [lancer la déconnexion](/help/authentication/iostvos-sdk-api-reference.md#logout) à partir du SDK AccessEnabler iOS/tvOS. Cela faciliterait le nettoyage de session du côté MVPD.
- L’application doit demander/inviter l’utilisateur à se déconnecter explicitement de *`Settings -> TV Provider`* sur iOS/iPadOS uniquement au cas où le code d’état [*VSA203* est déclenché](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
