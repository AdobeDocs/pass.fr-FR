---
title: Manuel de l’authentification unique Apple (iOS/tvOS SDK)
description: Manuel de l’authentification unique Apple (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# Manuel de l’authentification unique Apple (hérité) (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Le SDK Adobe Pass Authentication AccessEnabler iOS/tvOS prend en charge l’authentification unique (SSO) des partenaires pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS.

Ce document agit comme une extension de la documentation SDK AccessEnabler iOS/tvOS, disponible [ici](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md).

## Livre de cuisine {#apple-sso-cookbook-iostvos-sdk-cookbook}

Pour bénéficier de l’expérience utilisateur de l’authentification unique d’Apple, l’application doit intégrer le SDK iOS/tvOS d’AccessEnabler et suivre la séquence d’étapes présentée ci-dessous.

### Conditions préalables {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### Autorisation {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>Conseil pro :</u>** l&#39;application de streaming doit demander l&#39;accès aux informations d&#39;abonnement de l&#39;utilisateur enregistrées au niveau de l&#39;appareil, pour lesquelles l&#39;utilisateur doit donner à l&#39;application l&#39;autorisation de continuer, comme pour fournir l&#39;accès à la caméra ou au microphone de l&#39;appareil. Cette autorisation doit être demandée par application à l’aide d’Apple [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) et l’appareil enregistre la sélection de l’utilisateur.

>[!TIP]
>
> **<u>Conseil pro :</u>** nous recommandons d’inciter les utilisateurs qui refusent de donner l’autorisation d’accéder aux informations d’abonnement en expliquant les avantages de l’expérience utilisateur d’authentification unique d’Apple, mais sachez que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès d’autorisation du fournisseur de télévision) ou en *`Settings -> TV Provider`* sur iOS et iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS.

>[!TIP]
>
> **<u>Conseil sur la pro :</u>** l’application de diffusion en continu peut demander l’autorisation de l’utilisateur lorsqu’elle passe en état de premier plan, car l’application peut vérifier [l’autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur à tout moment avant d’exiger l’authentification de l’utilisateur.

>[!TIP]
>
> **<u>Conseil Pro :</u>** si l&#39;utilisateur n&#39;accorde pas l&#39;accès à ses informations d&#39;abonnement ou si la communication avec le framework de compte d&#39;abonné vidéo échoue, le SDK AccessEnabler iOS/tvOS retourne au flux d&#39;authentification standard.

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

#### Rappels {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>Conseil pro :</u>** implémentez la liste suivante de [rappels](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) spécifiques au workflow SSO d’Apple.

* [*presentTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Rappel déclenché lorsque le sélecteur MVPD d’Apple s’ouvre.
* [*dismissTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Rappel déclenché lorsque le sélecteur MVPD d’Apple va se fermer.

#### Rapport d’erreurs {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>Conseil pro :</u>** implémentez la liste suivante de [codes d’erreur avancés](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) spécifiques au workflow SSO d’Apple.

* ***N003*** - L’utilisateur a sélectionné l’option « Autre fournisseur de télévision » dans le sélecteur MVPD d’Apple.
* ***N004*** - L’utilisateur a sélectionné un fournisseur de télévision dans le sélecteur MVPD d’Apple, qui n’est pas pris en charge (intégration ou authentification unique désactivée) par le demandeur actuel.
* ***N005*** - L’utilisateur a décidé d’annuler le sélecteur MVPD standard ou le sélecteur Apple MVPD.
* ***VSA403*** - L&#39;autorisation du fournisseur de télévision de l&#39;utilisateur est refusée pour l&#39;application.
* ***VSA404*** - L&#39;autorisation du fournisseur de télévision de l&#39;utilisateur est indéterminée pour l&#39;application.
* ***VSA503*** - Échec de la demande de métadonnées du compte d&#39;abonné vidéo. Le champ *message* fournit plus de contexte.
* ***AAPL / APPL_ERROR*** - Échec de la demande de métadonnées du compte d’abonné à la vidéo. Le champ *détails* fournit plus de contexte.

### Authentification {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>Conseil :</u>** suivez les étapes ci-dessous pour la ou les implémentations iOS/iPadOS/tvOS.

1. L’application doit [initialiser](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) le SDK AccessEnabler iOS/tvOS.


1. L’application doit [définir l’identifiant du demandeur actuel](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Important :** cette deuxième étape peut déclencher un [code d’erreur avancé](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) spécifique au workflow SSO d’Apple, dans le cas où **l’un des scénarios suivants est vrai** :

   * ***VSA403*** - L&#39;autorisation du fournisseur de télévision de l&#39;utilisateur est refusée pour l&#39;application.
   * ***VSA404*** - L&#39;autorisation du fournisseur de télévision de l&#39;utilisateur est indéterminée pour l&#39;application.
   * ***APPL*** - La communication entre le SDK AccessEnabler iOS/tvOS et le framework de compte d’abonné vidéo a rencontré une erreur.

   Cette deuxième étape tente d’échanger silencieusement le profil SSO d’Apple contre un jeton d’authentification Adobe, au cas où **tout ce qui précède est faux** et **tout ce qui suit est vrai** :

   * L’autorisation Fournisseur TV de l’utilisateur est accordée pour l’application.
   * L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil.
   * Le SDK AccessEnabler iOS/tvOS a reçu l’identifiant de fournisseur de télévision de l’utilisateur de la structure du compte d’abonné vidéo.
   * L’intégration du fournisseur de télévision de l’utilisateur à l’application est activée via le tableau de bord TVE d’Adobe Primetime.
   * L’authentification unique du fournisseur de télévision de l’utilisateur avec l’application est activée via le tableau de bord TVE d’Adobe Primetime.
   * Le fournisseur de télévision de l’utilisateur n’est pas dégradé via le tableau de bord TVE d’Adobe Primetime.
   * Le SDK AccessEnabler iOS/tvOS a reçu la réponse SAML du fournisseur de télévision de l’utilisateur de la structure du compte d’abonné vidéo.

   **<u>Conseil pro :</u>** cette deuxième étape ne déclenchera aucun autre rappel, à part le rappel [setRequestorComplete](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete), car l&#39;authentification n&#39;a pas été explicitement initiée par l&#39;application.


1. L’application doit [vérifier le statut de l’authentification](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Important :** cette troisième étape peut déclencher un [code d’erreur avancé](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) spécifique au workflow SSO d’Apple, dans le cas où **l’un des scénarios suivants est vrai** :

   * ***VSA403** - L’utilisateur est connecté à son compte de fournisseur de télévision à l’adresse
niveau du système de l’appareil, mais l’autorisation Fournisseur TV de l’utilisateur est
refusé pour l’application.
   * ***VSA404** - L’utilisateur est connecté à son compte de fournisseur de télévision à l’adresse
niveau du système de l’appareil, mais autorisation du fournisseur de télévision de l’utilisateur
est indéterminé pour l’application.
   * ***APPL\_ERROR** - L’utilisateur est connecté à son fournisseur de télévision
compte au niveau du système de l’appareil, mais la communication entre
le SDK AccessEnabler iOS/tvOS et le compte d’abonné vidéo ;
la structure a rencontré une erreur.

   **Important :** cette troisième étape déclenche le rappel [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) avec *status* égal à 0, au cas où **l&#39;un des éléments suivants est vrai** :

   * L’utilisateur n’est pas connecté à son compte de fournisseur de télévision au niveau du système de l’appareil ou par le biais d’un flux d’authentification régulier.
   * L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil ou par le biais d’un flux d’authentification normal, mais la TTL du jeton d’authentification du fournisseur de télévision de l’utilisateur est passée.
   * L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil ou par le biais d’un flux d’authentification régulier, mais l’intégration du fournisseur de télévision de l’utilisateur à l’application est désactivée via le tableau de bord TVE d’Adobe Primetime.
   * L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais l’authentification unique du fournisseur de télévision de l’utilisateur avec l’application est désactivée via le tableau de bord TVE d’Adobe Primetime.
   * L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais l’autorisation du fournisseur de télévision de l’utilisateur est refusée pour l’application.
   * L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais l’autorisation du fournisseur de télévision de l’utilisateur est indéterminée pour l’application.
   * L’utilisateur est connecté à son compte de fournisseur de télévision au niveau du système de l’appareil, mais la communication entre le SDK AccessEnabler iOS/tvOS et la structure du compte d’abonné vidéo a rencontré une erreur.

   **Important :** cette troisième étape déclenche le rappel [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) avec *status* égal à 1, au cas où **tous les éléments ci-dessus sont faux.**


1. L’application doit [initialiser l’authentification](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) au cas où la vérification du statut d’authentification précédent déclenchait le rappel [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) avec *status* égal à 0.

   **<u>Conseil de pro :</u>** implémentez l’une des API SDK AccessEnabler iOS/tvOS suivantes [getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) ou [getAuthentication:filter](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Important :** cette quatrième étape peut déclencher un [code d’erreur avancé](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) spécifique au workflow SSO d’Apple, dans le cas où **l’un des scénarios suivants est vrai** :

   * ***VSA403*** - L&#39;autorisation du fournisseur de télévision de l&#39;utilisateur est refusée pour l&#39;application.
   * ***VSA404*** - L&#39;autorisation du fournisseur de télévision de l&#39;utilisateur est indéterminée pour l&#39;application.
   * ***VSA503*** - La communication entre le SDK AccessEnabler iOS/tvOS et le framework de compte d&#39;abonné vidéo a rencontré une erreur.
   * ***N003*** - L’utilisateur a sélectionné l’option « Autre fournisseur de télévision » dans le sélecteur MVPD d’Apple.
   * ***N004*** - L’utilisateur a sélectionné un fournisseur de télévision dans le sélecteur MVPD d’Apple, qui n’est pas pris en charge (intégration ou authentification unique désactivée) par le demandeur actuel.
   * ***N005*** - L’utilisateur a décidé d’annuler le sélecteur MVPD standard ou le sélecteur Apple MVPD.

   **Important :** cette quatrième étape revient au flux d’authentification normal, en déclenchant le rappel [displayProviderDialog](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dispProvDialog) et **l’un** des [codes d’erreur avancés](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) ci-dessus, au cas où **l’un des deux est vrai**.

   **Important :** cette quatrième étape revient au flux d’authentification standard, en déclenchant le rappel [navigateToUrl](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2url) ou [navigateToUrl:useSVC](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2urlSVC) et **aucun** des [codes d’erreur avancés](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) ci-dessus, au cas où l’utilisateur ou l’utilisatrice sélectionne un fournisseur de télévision qui ne prend pas en charge la connexion unique Apple, mais qui est présent dans le sélecteur Apple MVPD.

   **<u>Conseil pro :</u>** le SDK AccessEnabler iOS/tvOS appelle silencieusement l’API [setSelectedProvder](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv), au cas où l’utilisateur ou l’utilisatrice sélectionnerait un fournisseur de télévision qui ne prend pas en charge l’authentification unique Apple, mais qui est présent dans le sélecteur Apple MVPD.

   **Important :** cette quatrième étape tente d’échanger silencieusement le profil SSO d’Apple contre un jeton d’authentification Adobe, au cas où **tout ce qui précède est faux** et **tout ce qui suit est vrai** :

   * L’autorisation Fournisseur TV de l’utilisateur est accordée pour l’application.
   * L’utilisateur est connecté/se connecte actuellement à son compte de fournisseur de télévision au niveau du système de l’appareil.
   * Le SDK AccessEnabler iOS/tvOS a reçu l’identifiant de fournisseur de télévision de l’utilisateur de la structure du compte d’abonné vidéo.
   * L’intégration du fournisseur de télévision de l’utilisateur à l’application est activée via le tableau de bord TVE d’Adobe Primetime.
   * L’authentification unique du fournisseur de télévision de l’utilisateur avec l’application est activée via le tableau de bord TVE d’Adobe Primetime.
   * Le fournisseur de télévision de l’utilisateur n’est pas dégradé via le tableau de bord TVE d’Adobe Primetime.
   * Le SDK AccessEnabler iOS/tvOS a reçu la réponse SAML du fournisseur de télévision de l’utilisateur de la structure du compte d’abonné vidéo.

   **<u>Conseil pro :</u>** cette quatrième étape déclenche le rappel [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setAuthNStatus), quel que soit le résultat *status*, car l&#39;authentification a été explicitement initiée par l&#39;application.

### Métadonnées {#apple-sso-cookbook-iostvos-sdk-metadata}

L’application a la possibilité de déterminer si l’authentification a eu lieu suite à une connexion via le SSO du partenaire ou non, à l’aide de l’API « *tokenSource »* [métadonnées de l’utilisateur](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) du SDK AccessEnabler iOS/tvOS.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### Déconnexion {#apple-sso-cookbook-iostvos-sdk-logout}

Le [framework de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) ne fournit pas d’API pour déconnecter par programmation les personnes qui se sont connectées au compte de leur fournisseur de télévision au niveau du système de l’appareil. Par conséquent, pour que la déconnexion prenne pleinement effet, l’utilisateur final doit se déconnecter explicitement d’*`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS. L&#39;autre possibilité pour l&#39;utilisateur est de retirer l&#39;autorisation d&#39;accéder aux informations d&#39;abonnement de l&#39;utilisateur à partir de la section des paramètres de l&#39;application spécifique (accès aux autorisations du fournisseur de télévision).

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ceci par le biais de l’API AccessEnabler iOS/tvOS SDK [logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout).

>[!TIP]
>
> Conseil **<u>Pro :</u>** Suivez les étapes ci-dessous pour la/les implémentation(s) tvOS.

* L’application doit [initier la déconnexion](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) à partir du SDK AccessEnabler iOS/tvOS. Cela ne facilite pas le nettoyage de la session côté MVPD.
* L’application doit demander à l’utilisateur de se déconnecter explicitement de *`Settings -> Accounts -> TV Provider`* sur tvOS uniquement dans le cas où le code d’état [*VSA203* est déclenché](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md).

>[!TIP]
>
> Conseil de **<u>Pro :</u>** suivez les étapes ci-dessous pour la/les implémentation(s) d’iOS/iPadOS.

* L’application doit [initier la déconnexion](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) à partir du SDK AccessEnabler iOS/tvOS. Cela facilite le nettoyage de la session côté MVPD.
* L’application doit demander à l’utilisateur de se déconnecter explicitement d’*`Settings -> TV Provider`* sur iOS/iPadOS uniquement dans le cas où le code d’état [*VSA203* est déclenché](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md).
