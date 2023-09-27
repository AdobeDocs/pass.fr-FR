---
title: Guide pas à pas Apple SSO (SDK iOS/tvOS)
description: Guide pas à pas Apple SSO (SDK iOS/tvOS)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 0%

---

# Guide pas à pas Apple SSO (SDK iOS/tvOS) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#Introduction}

Le SDK Adobe Primetime Authentication AccessEnabler iOS/tvOS peut prendre en charge l’authentification SSO (Single Sign-On) par plateforme pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS via ce que nous appelons le processus SSO Apple.

Notez que ce document agit comme une extension de la documentation existante du SDK AccessEnabler iOS/tvOS, qui se trouve [here](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## Guide pas à pas {#Cookbook}

Afin de bénéficier de l’expérience utilisateur SSO d’Apple, une application doit intégrer le SDK AccessEnabler iOS/tvOS et suivre la séquence de conseils présentée ci-dessous.

</br>

### Conditions préalables {#Prerequisites}

</br>

#### Autorisation

>[!TIP]
>
> **<u>Conseil Pro :</u>** Pour pouvoir accéder aux informations d’abonnement de l’utilisateur, celui-ci doit donner à l’application l’autorisation de continuer, comme s’il avait accès à la caméra ou au microphone de l’appareil. Cette autorisation doit être demandée par application et l’appareil enregistre la sélection de l’utilisateur. Gardez à l’esprit que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès aux autorisations du fournisseur de télévision) ou à la section depuis *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Nous vous recommandons de demander l’autorisation de l’utilisateur lorsque l’application entre en premier plan, mais il s’agit uniquement d’une suggestion, car l’application peut vérifier [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur à tout moment avant de nécessiter une authentification de l’utilisateur. En outre, les API du SDK iOS/tvOS AccessEnabler demandent automatiquement l’autorisation de l’utilisateur lorsqu’il en a besoin.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Si l’utilisateur n’accorde pas l’accès à ses informations d’abonnement ou si la communication avec la structure de compte d’abonné vidéo échoue, le SDK AccessEnabler iOS/tvOS revient au flux d’authentification ordinaire.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Nous vous recommandons d’inciter les utilisateurs qui refusent de donner l’autorisation d’accéder aux informations d’abonnement en expliquant les avantages de l’expérience utilisateur de connexion unique (SSO). Gardez à l’esprit que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès aux autorisations du fournisseur de télévision) ou à la section depuis *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS.


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
> **<u>Conseil Pro :</u>** Mettez en oeuvre la liste suivante de [callbacks](/help/authentication/iostvos-sdk-api-reference.md) qui sont spécifiques au workflow SSO Apple.

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Rappel déclenché lorsque le sélecteur MVPD Apple s’ouvre.
- [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Rappel déclenché lorsque le sélecteur MVPD Apple se ferme.

</br>

#### Rapport d’erreurs

>[!TIP]
>
> **<u>Conseil Pro :</u>** Mettez en oeuvre la liste suivante de [codes d’erreur avancés](/help/authentication/error-reporting.md) qui sont spécifiques au workflow SSO Apple.

- ***N003*** - L’utilisateur a sélectionné l’option &quot;Autre fournisseur de télévision&quot; dans le sélecteur MVPD Apple.
- ***N004*** - L’utilisateur a sélectionné un fournisseur de télévision dans le sélecteur MVPD Apple, qui n’est pas pris en charge (intégration ou connexion unique désactivée) par le demandeur actuel.
- ***N005*** - L’utilisateur a décidé d’annuler le sélecteur MVPD standard ou le sélecteur MVPD Apple.
- ***VSA403*** - La permission du fournisseur de télévision de l&#39;utilisateur est refusée pour la demande.
- ***VSA404*** - L’autorisation du fournisseur de télévision de l’utilisateur est indéterminée pour la demande.
- ***VSA503*** - Échec de la demande de métadonnées de compte d’abonné vidéo, plus de contexte est fourni dans la section *message* champ .
- ***AAPL / APPL_ERROR*** - Échec de la demande de métadonnées de compte d’abonné vidéo, plus de contexte est fourni dans la section *détails* champ .

</br>

### Authentification {#Authentication}

>[!TIP]
>
> **<u>Conseil :</u>** Suivez les étapes ci-dessous pour la ou les implémentations iOS/iPadOS/tvOS.

1. La demande doit [initialize](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) le SDK AccessEnabler iOS/tvOS.
1. La demande doit [définir l’identifiant du demandeur actuel ;](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Important :** Cette deuxième étape peut déclencher une [code d’erreur avancé](/help/authentication/error-reporting.md) qui est spécifique au workflow SSO d’Apple, le cas échéant **l’une des conditions suivantes est vraie.**:

   - ***VSA403*** - La permission du fournisseur de télévision de l&#39;utilisateur est refusée pour la demande.
   - ***VSA404*** - L’autorisation du fournisseur de télévision de l’utilisateur est indéterminée pour la demande.
   - ***APPL*** - La communication entre le SDK AccessEnabler iOS/tvOS et la structure du compte d’abonné vidéo a rencontré une erreur.

   Cette deuxième étape consiste à échanger en silence le profil SSO Apple contre un jeton d’authentification d’Adobe, au cas où **tous les éléments ci-dessus sont faux.** et **tous les éléments suivants sont vrais**:

   - La permission du fournisseur de télévision de l’utilisateur est accordée pour la demande.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil.
   - Le SDK AccessEnabler iOS/tvOS a reçu l’identifiant du fournisseur de télévision de l’utilisateur à partir de la structure du compte d’abonné vidéo.
   - L’intégration du fournisseur de télévision de l’utilisateur à l’application est activée par le biais du tableau de bord Adobe Primetime TVE.
   - L’authentification unique du fournisseur de télévision de l’utilisateur avec l’application est activée via le tableau de bord Adobe Primetime TVE.
   - Le fournisseur de télévision de l’utilisateur n’est pas dégradé par le biais du tableau de bord Adobe Primetime TVE.
   - Le SDK AccessEnabler iOS/tvOS a reçu la réponse SAML du fournisseur de télévision de l’utilisateur de la structure du compte d’abonné vidéo.

   **<u>Conseil Pro :</u>** Cette deuxième étape ne déclenche aucun autre rappel, hormis le [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) rappel, car l’authentification n’a pas été explicitement lancée par l’application.

1. La demande doit [vérifier l’état d’authentification](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Important :** Cette troisième étape peut déclencher une [code d’erreur avancé](/help/authentication/error-reporting.md) qui est spécifique au workflow SSO d’Apple, le cas échéant **l’une des conditions suivantes est vraie.**:

   - ***VSA403** - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais son autorisation de fournisseur de télévision est refusée pour l’application.
   - ***VSA404** - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais son autorisation de fournisseur de télévision n’est pas déterminée pour l’application.
   - ***APPL\_ERROR** - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais la communication entre le SDK AccessEnabler iOS/tvOS et la structure du compte d’abonné vidéo a rencontré une erreur.

   **Important :** Cette troisième étape déclenche la [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) rappel avec *status* égal à 0, en cas **l’une des conditions suivantes est vraie.**:

   - L’utilisateur n’est pas connecté à son compte de fournisseur de télévision au niveau du système de l’appareil ou par le biais d’un flux d’authentification régulier.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil ou par le biais d’un flux d’authentification régulier, mais le jeton d’authentification du fournisseur de télévision de l’utilisateur est transmis.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil ou par le biais d’un flux d’authentification standard, mais l’intégration du fournisseur de télévision de l’utilisateur à l’application est désactivée par le biais du tableau de bord Adobe Primetime TVE.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais l’authentification unique du fournisseur de télévision de l’utilisateur avec l’application est désactivée par le biais du tableau de bord Adobe Primetime TVE.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais son autorisation de fournisseur de télévision est refusée pour l’application.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais son autorisation de fournisseur de télévision n’est pas déterminée pour l’application.
   - L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais la communication entre le SDK AccessEnabler iOS/tvOS et la structure du compte d’abonné vidéo a rencontré une erreur.

   **Important :** Cette troisième étape déclenche la [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) rappel avec *status* égal à 1, au cas où **tous les éléments ci-dessus sont faux.**


1. La demande doit [initialisation de l’authentification](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) au cas où la vérification de l’état de l’authentification précédente déclencherait la variable [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) rappel avec *status* égal à 0.

   **<u>Conseil Pro :</u>** Mettez en oeuvre l’une des API SDK AccessEnabler iOS/tvOS suivantes [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) ou [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Important :** Cette quatrième étape peut déclencher une [code d’erreur avancé](/help/authentication/error-reporting.md) qui est spécifique au workflow SSO d’Apple, le cas échéant **l’une des conditions suivantes est vraie.**:

   - ***VSA403*** - La permission du fournisseur de télévision de l&#39;utilisateur est refusée pour la demande.
   - ***VSA404*** - L’autorisation du fournisseur de télévision de l’utilisateur est indéterminée pour la demande.
   - ***VSA503*** - La communication entre le SDK AccessEnabler iOS/tvOS et la structure du compte d’abonné vidéo a rencontré une erreur.
   - ***N003*** - L’utilisateur a sélectionné l’option &quot;Autre fournisseur de télévision&quot; dans le sélecteur MVPD Apple.
   - ***N004*** - L’utilisateur a sélectionné un fournisseur de télévision dans le sélecteur MVPD Apple, qui n’est pas pris en charge (intégration ou connexion unique désactivée) par le demandeur actuel.
   - ***N005*** - L’utilisateur a décidé d’annuler le sélecteur MVPD standard ou le sélecteur MVPD Apple.

   **Important :** Cette quatrième étape reviendrait au flux d’authentification standard, en déclenchant l’événement [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) callback et **one** de ce qui précède [codes d’erreur avancés](/help/authentication/error-reporting.md), au cas où **l’une des conditions ci-dessus est vraie.**.

   **Important :** Cette quatrième étape reviendrait au flux d’authentification standard, en déclenchant l’événement [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) ou [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) callback et **none** de ce qui précède [codes d’erreur avancés](/help/authentication/error-reporting.md), si l’utilisateur a sélectionné un fournisseur de télévision, qui ne prend pas en charge la fonction SSO d’Apple, mais qui est présent dans le sélecteur MVPD d’Apple.

   **<u>Conseil Pro :</u>** Le SDK AccessEnabler iOS/tvOS appelle silencieusement le [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) API, au cas où l’utilisateur sélectionnerait un fournisseur de télévision, qui ne prend pas en charge la connexion unique Apple, mais qui est présent dans le sélecteur MVPD Apple.

   **Important :** Cette quatrième étape consiste à échanger silencieusement le profil SSO Apple contre un jeton d’authentification d’Adobe, au cas où **tous les éléments ci-dessus sont faux.** et **tous les éléments suivants sont vrais**:

   - La permission du fournisseur de télévision de l’utilisateur est accordée pour la demande.
   - L’utilisateur est connecté/se connecte actuellement à son compte de fournisseur de télévision au niveau du système de l’appareil.
   - Le SDK AccessEnabler iOS/tvOS a reçu l’identifiant du fournisseur de télévision de l’utilisateur à partir de la structure du compte d’abonné vidéo.
   - L’intégration du fournisseur de télévision de l’utilisateur à l’application est activée par le biais du tableau de bord Adobe Primetime TVE.
   - L’authentification unique du fournisseur de télévision de l’utilisateur avec l’application est activée via le tableau de bord Adobe Primetime TVE.
   - Le fournisseur de télévision de l’utilisateur n’est pas dégradé par le biais du tableau de bord Adobe Primetime TVE.
   - Le SDK AccessEnabler iOS/tvOS a reçu la réponse SAML du fournisseur de télévision de l’utilisateur de la structure du compte d’abonné vidéo.



>**<u>Conseil Pro :</u>** Cette quatrième étape déclenche la [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) callback, indépendamment de *status* , car l’authentification a été explicitement lancée par l’application.


</br>

### Métadonnées {#Metadata}

L’application a la possibilité de déterminer si l’authentification s’est produite suite à une connexion via l’authentification unique (SSO) de la plateforme, ou non, en utilisant le *tokenSource&quot;* [métadonnées utilisateur](/help/authentication/iostvos-sdk-api-reference.md#getMeta) API du SDK AccessEnabler iOS/tvOS.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### Déconnexion {#Logout}

La variable [Compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) La structure ne fournit pas d’API pour déconnecter par programmation les personnes qui se sont connectées à leur compte de fournisseur de télévision au niveau du système de l’appareil. Par conséquent, pour que la déconnexion soit effective, l’utilisateur final doit se déconnecter explicitement de *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS. L’autre option dont dispose l’utilisateur consiste à retirer l’autorisation d’accéder aux informations d’abonnement de l’utilisateur dans la section des paramètres de l’application spécifique (accès aux autorisations du fournisseur de télévision).

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode à l’aide du SDK AccessEnabler iOS/tvOS [déconnexion](/help/authentication/iostvos-sdk-api-reference.md#logout) API.


>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les mises en oeuvre tvOS.

- La demande doit [lancer la déconnexion](/help/authentication/iostvos-sdk-api-reference.md#logout) à partir du SDK AccessEnabler iOS/tvOS. Cela ne faciliterait pas le nettoyage de session du côté MVPD.
- L’application doit demander à l’utilisateur de se déconnecter explicitement de . *`Settings -> Accounts -> TV Provider`* sur tvOS uniquement au cas où [*VSA203* code d’état déclenché](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations iOS/iPadOS.

- La demande doit [lancer la déconnexion](/help/authentication/iostvos-sdk-api-reference.md#logout) à partir du SDK AccessEnabler iOS/tvOS. Cela faciliterait le nettoyage de session du côté MVPD.
- L’application doit demander à l’utilisateur de se déconnecter explicitement de . *`Settings -> TV Provider`* sur iOS/iPadOS uniquement au cas où [*VSA203* code d’état déclenché](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
