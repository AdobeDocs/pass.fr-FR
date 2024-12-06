---
title: Guide pas à pas Apple SSO (API REST V1)
description: Guide pas à pas Apple SSO (API REST V1)
exl-id: 072a011f-e1bb-4d3e-bcb5-697f2d1739cc
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1473'
ht-degree: 0%

---

# Guide pas à pas Apple SSO (API REST V1) {#apple-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

L’API REST d’authentification Adobe Pass V1 prend en charge l’authentification unique du partenaire (SSO) pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS.

Ce document sert d’extension à la documentation existante de l’API REST V1, que vous trouverez [ici](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md).

## Guide pas à pas {#apple-sso-cookbook-rest-api-v1-cookbook}

Pour bénéficier de l’expérience utilisateur de l’authentification unique Apple, l’application doit intégrer la [structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développée par Apple, tandis que pour la communication de l’API REST d’authentification Adobe Pass V1, elle doit suivre la séquence d’étapes présentée ci-dessous.

### Autorisation {#apple-sso-cookbook-rest-api-v1-permission}

>[!TIP]
>
> **<u>Conseil Pro :</u>** L’application de diffusion en continu doit demander l’accès aux informations d’abonnement de l’utilisateur enregistrées au niveau de l’appareil, pour lesquelles l’utilisateur doit accorder à l’application l’autorisation de continuer, de la même manière que l’accès à la caméra ou au microphone de l’appareil. Cette autorisation doit être demandée par application en utilisant la [structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) d’Apple et l’appareil enregistrera la sélection de l’utilisateur.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Nous recommandons aux utilisateurs qui refusent d’autoriser l’accès aux informations d’abonnement en expliquant les avantages de l’expérience de connexion unique d’Apple, mais sachez que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès aux autorisations du fournisseur de télévision) ou à *`Settings -> TV Provider`* sur iOS et iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Nous vous recommandons de demander l’autorisation de l’utilisateur lorsque l’application entre en premier plan, car l’application peut rechercher l’autorisation [d’accéder à](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur à tout moment avant d’avoir besoin de l’authentification de l’utilisateur.

### Authentification {#apple-sso-cookbook-rest-api-v1-authentication}

* [Existe-t-il un jeton d’authentification d’Adobe valide ?](#step1)
* [L’utilisateur est-il connecté via l’authentification unique du partenaire ?](#step2)
* [Récupérer la configuration des Adobes](#step3)
* [Lancement du processus d’authentification unique du partenaire avec configuration de l’Adobe](#step4)
* [La connexion de l’utilisateur est-elle réussie ?](#step5)
* [Obtenir une requête de profil de l’Adobe pour le MVPD sélectionné](#step6)
* [Transférez la demande d’Adobe à Partner SSO pour obtenir le profil.](#step7)
* [Exchange du profil d’authentification unique du partenaire pour un jeton d’authentification Adobe](#step8)
* [Le jeton d’Adobe est-il généré correctement ?](#step9)
* [Lancement d’un processus d’authentification régulier](#step10)
* [Poursuivre avec les flux d’autorisation](#step11)

![](../../../../../assets/rest-api-v1/apple-sso-cookbook-rest-api-v1.png)

#### Étape : &quot;Existe-t-il un jeton d’authentification d’Adobe valide ?&quot; {#step1}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode à l’aide du service API d’authentification Adobe Pass [Vérifier le jeton d’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md).

#### Étape : &quot;L’utilisateur est-il connecté via l’authentification unique du partenaire ?&quot; {#step2}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode au moyen de la [structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount).

* L’application doit rechercher l’autorisation [d’accéder aux informations d’abonnement de l’utilisateur ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) et procéder uniquement si l’utilisateur l’a autorisée.
* L’application doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
* L’application doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez le fragment de code et prêtez une attention particulière aux commentaires.

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

#### Étape : &quot;Récupérer la configuration des Adobes&quot; {#step3}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode par le biais du service d’API d’authentification Adobe Pass [Fournissez la liste MVPD](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md).

>[!TIP]
>
> **<u>Conseil Pro :</u>** Tenez compte des propriétés MVPD : *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* et prêtez une attention particulière aux commentaires présentés dans les fragments de code d’autres étapes.

#### Étape &quot;Lancement du workflow SSO du partenaire avec configuration de l’Adobe&quot; {#step4}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode au moyen de la [structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount).

* L’application doit rechercher l’autorisation [d’accéder aux informations d’abonnement de l’utilisateur ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) et procéder uniquement si l’utilisateur l’a autorisée.
* L’application doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour VSAccountManager.
* L’application doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
* L’application doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez le fragment de code et prêtez une attention particulière aux commentaires.

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

#### Étape : &quot;La connexion de l’utilisateur est-elle réussie ?&quot; {#step5}

>[!TIP]
>
> **<u>Conseil Pro :</u>** Veuillez tenir compte du fragment de code de l’étape [&quot;Lancer le processus SSO du partenaire avec configuration de l’Adobe&quot;](#step4) . La connexion de l’utilisateur réussit si *`vsaMetadata!.accountProviderIdentifier`* contient une valeur valide et que la date actuelle n’a pas dépassé la valeur *`vsaMetadata!.authenticationExpirationDate`*.

#### Étape &quot;Obtention d’une requête de profil de l’Adobe pour le MVPD sélectionné&quot; {#step6}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode à l’aide du service d’API [Requête de profil](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md) d’authentification Adobe Pass.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Notez que l’identifiant de fournisseur obtenu à partir de la structure de compte d’abonné vidéo représente le *`platformMappingId`* en termes de configuration de l’authentification Adobe Pass. Par conséquent, l’application doit déterminer la valeur de la propriété ID MVPD, à l’aide de la valeur *`platformMappingId`*, par le biais du service d’API [Fournir une liste MVPD](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) d’authentification Adobe Pass.

#### Étape : &quot;Transférez la demande d’Adobe à Partner SSO pour obtenir le profil&quot; {#step7}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode au moyen de la [structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount).


* L’application doit rechercher l’autorisation [d’accéder aux informations d’abonnement de l’utilisateur ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) et procéder uniquement si l’utilisateur l’a autorisée.
* L’application doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
* L’application doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez le fragment de code et prêtez une attention particulière aux commentaires.

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

#### Étape : &quot;Exchange du profil SSO du partenaire pour un jeton d’authentification d’Adobe&quot; {#step8}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode à l’aide du service API d’authentification Adobe Pass [Exchange de jeton](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md).

>[!TIP]
>
> **<u>Conseil Pro :</u>** Notez le fragment de code de l’étape [&quot;Transférez la demande d’Adobe à la SSO Partner pour obtenir le profil&quot;](#step7). Ce *`vsaMetadata!.samlAttributeQueryResponse!`* représente le *`SAMLResponse`*, qui doit être transmis sur [Exchange de jeton](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) et qui nécessite une manipulation et un codage de chaîne (*Base64* codé et *URL* encodé par la suite) avant d’effectuer l’appel.

#### Étape : &quot;Le jeton d’Adobe est-il généré avec succès ?&quot; {#step9}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode à l’aide de la réponse réussie [Exchange de jeton](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) de l’authentification Adobe Pass, qui sera un *`204 No Content`*, indiquant que le jeton a été créé avec succès et qu’il est prêt à être utilisé pour les flux d’autorisation.

#### Étape : &quot;Lancement d’un workflow d’authentification régulier&quot; {#step10}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode au moyen de l’authentification Adobe Pass [Demande de code d’enregistrement](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md), [Lancer l’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) et [Récupérer le jeton d’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) ou [Vérifier le jeton d’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) des services API.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations tvOS.

* L’application doit [obtenir un code d’enregistrement](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) et le présenter à l’utilisateur final sur le premier appareil (écran).
* L’application doit démarrer [polling pour reconnaître l’état d’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sur le 1er appareil (écran) après l’obtention du code d’enregistrement.
* Une autre application doit [lancer l’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) sur un 2e appareil (écran) lorsque le code d’enregistrement est utilisé.
* L’application doit arrêter [polling](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sur le 1er appareil (écran) lorsque le jeton d’authentification est généré.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations iOS/iPadOS.

* L’application doit [obtenir un code d’enregistrement](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) qui ne doit pas être présenté à l’utilisateur final sur le premier appareil (écran).
* L’application doit [lancer l’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) sur le premier appareil (écran) à l’aide du code d’enregistrement et d’un composant [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
* L’application doit démarrer [l’interrogation pour connaître l’état d’authentification](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sur le premier appareil (écran) après la fermeture du composant [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
* L’application doit arrêter [polling](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) sur le 1er appareil (écran) lorsque le jeton d’authentification est généré.

#### Étape : &quot;Poursuivre avec les flux d’autorisation&quot; {#step11}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode par le biais de l’authentification Adobe Pass [Lancer l’autorisation](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) et [Obtenir un jeton de média court](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) services API.

### Déconnexion {#apple-sso-cookbook-rest-api-v1-logout}

La [structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) ne fournit pas d’API pour déconnecter par programmation les personnes qui se sont connectées à leur compte de fournisseur de télévision au niveau du système de l’appareil. Par conséquent, pour que la déconnexion entre en vigueur, l’utilisateur final doit se déconnecter explicitement de *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS. L’autre option dont dispose l’utilisateur consiste à retirer l’autorisation d’accéder aux informations d’abonnement de l’utilisateur dans la section des paramètres de l’application spécifique (accès au fournisseur de télévision).

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode à l’aide des services d’API d’authentification Adobe Pass [appel de métadonnées utilisateur](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) et de [déconnexion](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md).

>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations tvOS.

* L’application doit déterminer si l’authentification a été effectuée suite à une connexion par le biais de l’authentification unique du partenaire ou non, à l’aide des &quot;*tokenSource&quot;* [métadonnées utilisateur](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) du service d’authentification Adobe Pass.
* L’application doit demander à l’utilisateur de se déconnecter explicitement de *`Settings -> Accounts -> TV Provider`* sur tvOS **uniquement** si la valeur *&quot;tokenSource&quot;* est égale à &quot;*Apple&quot;.*
* L’application doit [lancer la déconnexion](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) à partir du service d’authentification Adobe Pass à l’aide d’un appel HTTP direct. Cela ne faciliterait pas le nettoyage de session du côté MVPD.

>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations iOS/iPadOS.

* L’application doit déterminer si l’authentification a été effectuée suite à une connexion par le biais de l’authentification unique du partenaire ou non, à l’aide des &quot;*tokenSource&quot;* [métadonnées utilisateur](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) du service d’authentification Adobe Pass.
* L’application doit demander à l’utilisateur de se déconnecter explicitement de *`Settings -> TV Provider`* sur iOS/iPadOS **uniquement** si la valeur *&quot;tokenSource&quot;* est égale à *&quot;Apple&quot;*.
* L’application doit [lancer la déconnexion](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) à partir du service d’authentification Adobe Pass à l’aide d’un composant [ WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller). Cela faciliterait le nettoyage de session du côté MVPD.
