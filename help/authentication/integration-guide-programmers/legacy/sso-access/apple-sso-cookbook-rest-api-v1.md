---
title: Manuel de l’authentification unique Apple (API REST V1)
description: Manuel de l’authentification unique Apple (API REST V1)
exl-id: 072a011f-e1bb-4d3e-bcb5-697f2d1739cc
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1496'
ht-degree: 0%

---

# Manuel de l’authentification unique Apple (hérité) (API REST V1) {#apple-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

L’API REST d’authentification Adobe Pass V1 prend en charge l’authentification unique (SSO) du partenaire pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS.

Ce document agit comme une extension de la documentation existante de l’API REST V1, qui se trouve [ici](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md).

## Livre de cuisine {#apple-sso-cookbook-rest-api-v1-cookbook}

Pour bénéficier de l’expérience utilisateur de l’authentification unique d’Apple, l’application doit intégrer le [framework de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développé par Apple, tandis que pour la communication de l’API REST d’authentification Adobe Pass V1, elle doit suivre la séquence d’étapes présentée ci-dessous.

### Autorisation {#apple-sso-cookbook-rest-api-v1-permission}

>[!TIP]
>
> **<u>Conseil pro :</u>** l&#39;application de streaming doit demander l&#39;accès aux informations d&#39;abonnement de l&#39;utilisateur enregistrées au niveau de l&#39;appareil, pour lesquelles l&#39;utilisateur doit donner à l&#39;application l&#39;autorisation de continuer, comme pour fournir l&#39;accès à la caméra ou au microphone de l&#39;appareil. Cette autorisation doit être demandée par application à l’aide d’Apple [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) et l’appareil enregistre la sélection de l’utilisateur.

>[!TIP]
>
> **<u>Conseil pro :</u>** nous recommandons d’inciter les utilisateurs qui refusent de donner l’autorisation d’accéder aux informations d’abonnement en expliquant les avantages de l’expérience utilisateur d’authentification unique d’Apple, mais sachez que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès d’autorisation du fournisseur de télévision) ou en *`Settings -> TV Provider`* sur iOS et iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS.

>[!TIP]
>
> **<u>Conseil pro :</u>** nous vous recommandons de demander l&#39;autorisation de l&#39;utilisateur lorsque l&#39;application entre en état de premier plan, car l&#39;application peut vérifier [l&#39;autorisation d&#39;accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d&#39;abonnement de l&#39;utilisateur à tout moment avant d&#39;exiger l&#39;authentification de l&#39;utilisateur.

### Authentification {#apple-sso-cookbook-rest-api-v1-authentication}

* [Existe-t-il un jeton d’authentification d’Adobe valide ?](#step1)
* [L’utilisateur est-il connecté via l’authentification unique du partenaire ?](#step2)
* [Récupérer la configuration de l’Adobe](#step3)
* [Lancement du workflow SSO du partenaire avec la configuration d’Adobe](#step4)
* [La connexion de l’utilisateur a-t-elle réussi ?](#step5)
* [Obtenir une requête de profil de l’Adobe pour le MVPD sélectionné](#step6)
* [Transférer la demande d’Adobe au SSO du partenaire pour obtenir le profil](#step7)
* [Exchange du profil SSO du partenaire pour un jeton d’authentification Adobe](#step8)
* [Le jeton d’Adobe est-il généré avec succès ?](#step9)
* [Lancement du workflow d’authentification standard](#step10)
* [Poursuivre les flux d’autorisation](#step11)

![](../../../assets/rest-api-v1/apple-sso-cookbook-rest-api-v1.png)

#### Étape : « Existe-t-il un jeton d’authentification d’Adobe valide ? » {#step1}

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ceci par le biais du service d’API Adobe Pass Authentication [Check Authentication Token](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md).

#### Étape : « L’utilisateur est-il connecté via l’authentification unique du partenaire ? » {#step2}

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ce service via [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).

* L’application doit vérifier la [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur et continuer uniquement si l’utilisateur l’autorise.
* La demande devrait présenter une [demande](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour obtenir des renseignements sur le compte de l&#39;abonné.
* L’application doit attendre et traiter les informations [métadonnées](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

>[!TIP]
>
> **<u>Conseil pro :</u>** suivez le fragment de code et prêtez une attention particulière aux commentaires.

```swift
...
let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();

videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();

                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
                    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                    
                    // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
                    
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Initiate regular authentication workflow" step.
                ...
            }
}
...  
```

#### Étape : « Récupérer la configuration de l’Adobe » {#step3}

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ceci par le biais du service d’API Adobe Pass Authentication [Provide MVPD List](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md).

>[!TIP]
>
> **<u>Conseil pro :</u>** prenez connaissance des propriétés MVPD : *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* et prêtez une attention particulière aux commentaires présentés dans les fragments de code provenant d&#39;autres étapes.

#### Étape « Lancer le workflow SSO du partenaire avec la configuration d’Adobe » {#step4}

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ce service via [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).

* L’application doit vérifier la [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur et continuer uniquement si l’utilisateur l’autorise.
* L’application doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour VSAccountManager.
* La demande devrait présenter une [demande](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour obtenir des renseignements sur le compte de l&#39;abonné.
* L’application doit attendre et traiter les informations [métadonnées](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

>[!TIP]
>
> **<u>Conseil pro :</u>** suivez le fragment de code et prêtez une attention particulière aux commentaires.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
    
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate regular authentication workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate regular authentication workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate regular authentication workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate regular authentication workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Étape : « La connexion de l’utilisateur a-t-elle réussi ? » {#step5}

>[!TIP]
>
> **<u>Conseil pro :</u>** prenez connaissance de l’extrait de code de l’étape [&#x200B; Lancer le workflow de l’authentification unique du partenaire avec la configuration d’Adobe »](#step4) La connexion de l’utilisateur réussit si le *`vsaMetadata!.accountProviderIdentifier`* contient une valeur valide et que la date actuelle n’a pas transmis la valeur *`vsaMetadata!.authenticationExpirationDate`*.

#### Étape « Obtenir une requête de profil de l’Adobe pour le MVPD sélectionné » {#step6}

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ceci par le biais du service d’API Authentification Adobe Pass [Requête de profil](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md).

>[!TIP]
>
> **<u>Conseil pro :</u>** sachez que l&#39;identifiant du fournisseur obtenu à partir du framework de compte d&#39;abonné vidéo représente le *`platformMappingId`* en termes de configuration de l&#39;authentification Adobe Pass. Par conséquent, l’application doit déterminer la valeur de la propriété d’identifiant MVPD, à l’aide de la valeur *`platformMappingId`*, par le biais du service d’API Authentification Adobe Pass [Fournir une liste MVPD](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md).

#### Étape : « Transférer la demande d’Adobe au SSO du partenaire pour obtenir le profil » {#step7}

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ce service via [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).


* L’application doit vérifier la [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur et continuer uniquement si l’utilisateur l’autorise.
* La demande devrait présenter une [demande](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour obtenir des renseignements sur le compte de l&#39;abonné.
* L’application doit attendre et traiter les informations [métadonnées](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

>[!TIP]
>
> **<u>Conseil pro :</u>** suivez le fragment de code et prêtez une attention particulière aux commentaires.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
                                
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
                                
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
                                
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
                                
                                // Continue with the "Exchange the Partner SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Continue with the "Initiate regular authentication workflow" step.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Étape : « Exchange du profil SSO du partenaire pour un jeton d’authentification Adobe » {#step8}

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ceci par le biais du service d’API d’authentification Adobe Pass [Exchange du jeton](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md).

>[!TIP]
>
> **<u>Conseil pro :</u>** prenez connaissance du fragment de code de l&#39;étape [&#x200B; Transférer la demande d&#39;Adobe au SSO partenaire pour obtenir le profil](#step7). Cette *`vsaMetadata!.samlAttributeQueryResponse!`* représente le *`SAMLResponse`*, qui doit être transmis sur [Exchange du jeton](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) et nécessite une manipulation et un codage de chaîne (codés en *Base64* et en *URL* codés par la suite) avant d’effectuer l’appel.

#### Étape : « Le jeton d’Adobe a-t-il été généré avec succès ? » {#step9}

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ceci par le biais de l’authentification Adobe Pass [Exchange du jeton](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) réponse réussie, qui sera un *`204 No Content`*, indiquant que le jeton a été créé avec succès et est prêt à être utilisé pour les flux d’autorisation.

#### Étape : « Lancer le workflow d’authentification standard » {#step10}

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ceci par le biais des services d’API Adobe Pass Authentication [Demande de code d’enregistrement](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md), [Lancer l’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) et [Récupérer le jeton d’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) ou [Vérifier le jeton d’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md).

>[!TIP]
>
> Conseil **<u>Pro :</u>** Suivez les étapes ci-dessous pour la/les implémentation(s) tvOS.

* L&#39;application devrait [obtenir un code d&#39;enregistrement](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) et le présenter à l&#39;utilisateur final sur le 1er appareil (écran).
* L’application doit lancer l’[interrogation pour confirmer l’état d’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sur le premier appareil (écran) après l’obtention du code d’enregistrement.
* Une autre application devrait [initier l’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) sur un deuxième appareil (écran) lorsque le code d’enregistrement est utilisé.
* L’application doit arrêter l’[interrogation](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sur le premier appareil (écran) lorsque le jeton d’authentification est généré.

>[!TIP]
>
> Conseil de **<u>Pro :</u>** suivez les étapes ci-dessous pour la/les implémentation(s) d’iOS/iPadOS.

* L&#39;application devrait [obtenir un code d&#39;enregistrement](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) qui ne devrait pas être présenté à l&#39;utilisateur final sur le 1er appareil (écran).
* L’application doit [lancer l’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) sur le premier appareil (écran) à l’aide du code d’enregistrement et d’un composant [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
* L’application doit démarrer [interrogation pour connaître l’état d’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sur le premier appareil (écran) après la fermeture du composant [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
* L’application doit arrêter l’[interrogation](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sur le premier appareil (écran) lorsque le jeton d’authentification est généré.

#### Étape : « Poursuivre les flux d’autorisation » {#step11}

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ceci via l’authentification Adobe Pass [Lancer l’autorisation](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) et [Obtenir un jeton de média court](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) les services de l’API.

### Déconnexion {#apple-sso-cookbook-rest-api-v1-logout}

Le [framework de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) ne fournit pas d’API pour déconnecter par programmation les personnes qui se sont connectées au compte de leur fournisseur de télévision au niveau du système de l’appareil. Par conséquent, pour que la déconnexion prenne pleinement effet, l’utilisateur final doit se déconnecter explicitement d’*`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS. L&#39;autre option dont dispose l&#39;utilisateur est de retirer son autorisation d&#39;accès aux informations d&#39;abonnement de l&#39;utilisateur à partir de la section de paramétrage de l&#39;application spécifique (accès au fournisseur de télévision).

>[!TIP]
>
> **<u>Conseil :</u>** implémentez ceci par le biais de l’authentification Adobe Pass [appel de métadonnées de l’utilisateur](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) et des services d’API [Déconnexion](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md).

>[!TIP]
>
> Conseil **<u>Pro :</u>** Suivez les étapes ci-dessous pour la/les implémentation(s) tvOS.

* L’application doit déterminer si l’authentification a eu lieu suite à une connexion par le biais de l’authentification unique du partenaire, à l’aide de la « *tokenSource »* [métadonnées de l’utilisateur](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) du service d’authentification Adobe Pass.
* L’application doit demander à l’utilisateur de se déconnecter explicitement de *`Settings -> Accounts -> TV Provider`* sur tvOS **uniquement** au cas où la valeur *« tokenSource »* est égale à « *Apple ».*
* L’application doit [initier la déconnexion](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) à partir du service d’authentification Adobe Pass à l’aide d’un appel HTTP direct. Cela ne facilite pas le nettoyage de la session côté MVPD.

>[!TIP]
>
> Conseil de **<u>Pro :</u>** suivez les étapes ci-dessous pour la/les implémentation(s) d’iOS/iPadOS.

* L’application doit déterminer si l’authentification a eu lieu suite à une connexion par le biais de l’authentification unique du partenaire, à l’aide de la « *tokenSource »* [métadonnées de l’utilisateur](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) du service d’authentification Adobe Pass.
* L’application doit demander à l’utilisateur de se déconnecter explicitement d’*`Settings -> TV Provider`* sur iOS/iPadOS **uniquement** au cas où la valeur *« tokenSource »* est égale à *« Apple »*.
* L’application doit [initier la déconnexion](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) à partir du service d’authentification Adobe Pass à l’aide d’un composant [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller). Cela facilite le nettoyage de la session côté MVPD.
