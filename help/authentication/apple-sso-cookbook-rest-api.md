---
title: Guide pas à pas Apple SSO (API REST)
description: Guide pas à pas Apple SSO (API REST)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---

# Guide pas à pas Apple SSO (API REST) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#Introduction}

L’API REST d’authentification Adobe Pass peut prendre en charge l’authentification SSO (Single Sign-On) par plateforme pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS via ce que nous appelons le processus SSO Apple.

Veuillez noter que ce document agit comme une extension de la documentation de l&#39;API REST existante, qui se trouve [ici](/help/authentication/rest-api-reference.md).

## Cookbooks {#Cookbooks}

Pour bénéficier de l’expérience utilisateur de l’authentification unique Apple, une application doit intégrer le framework [Compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développé par Apple, tout en ce qui concerne la communication de l’API REST d’authentification Adobe Pass, elle doit suivre la séquence de conseils présentée ci-dessous.

### Authentification {#Authentication}

- [Existe-t-il un jeton d’authentification d’Adobe valide ?](#Is_there_a_valid_Adobe_authentication_token)
- [L’utilisateur est-il connecté via la connexion unique à Platform ?](#Is_the_user_logged_in_via_Platform_SSO)
- [Récupérer la configuration des Adobes](#Fetch_Adobe_configuration)
- [Lancement du processus d’authentification unique de Platform avec configuration d’Adobe](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [La connexion de l’utilisateur est-elle réussie ?](#Is_user_login_successful)
- [Obtenir une requête de profil de l’Adobe pour le MVPD sélectionné](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [Transférez la demande d’Adobe à Platform SSO pour obtenir le profil.](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [Exchange du profil d’authentification unique de Platform pour un jeton d’authentification d’Adobe](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [Le jeton d’Adobe est-il généré correctement ?](#Is_Adobe_token_generated_successfully)
- [Lancement du processus d’authentification du deuxième écran](#Initiate_second_screen_authentication_workflow)
- [Poursuivre avec les flux d’autorisation](#Proceed_with_authorization_flows)


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)


#### Étape : &quot;Existe-t-il un jeton d’authentification d’Adobe valide ?&quot; {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode par le biais du service [Authentification Adobe Pass](/help/authentication/check-authentication-token.md).


#### Étape : &quot;L’utilisateur est-il connecté via la connexion unique à Platform ?&quot; {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode au moyen de la structure [Compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount).

- L’application doit rechercher l’autorisation [d’accéder aux informations d’abonnement de l’utilisateur ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) et procéder uniquement si l’utilisateur l’a autorisée.
- L’application doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
- L’application doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



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
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

#### Étape : &quot;Récupérer la configuration des Adobes&quot; {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode par le biais du service [Authentification Adobe Pass](/help/authentication/provide-mvpd-list.md).


>[!TIP]
>
> **<u>Conseil Pro :</u>** Tenez compte des propriétés MVPD : *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* et prêtez une attention particulière aux commentaires présentés dans les fragments de code d’autres étapes.

#### Étape &quot;Lancement du workflow SSO Platform avec configuration Adobe&quot; {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode au moyen de la structure [Compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount).

- L’application doit rechercher l’autorisation [d’accéder aux informations d’abonnement de l’utilisateur ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) et procéder uniquement si l’utilisateur l’a autorisée.
- L’application doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour VSAccountManager.
- L’application doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
- L’application doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



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
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
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
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
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
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Étape : &quot;La connexion de l’utilisateur est-elle réussie ?&quot; {#Is_user_login_successful}

>[!TIP]
>
> **<u>Conseil Pro :</u>** Veuillez tenir compte du fragment de code de l’étape [&quot;Lancer le processus SSO Platform avec configuration d’Adobe&quot;](#Initiate_Platform_SSO_workflow_with_Adobe_config) . La connexion de l’utilisateur réussit si *`vsaMetadata!.accountProviderIdentifier`* contient une valeur valide et que la date actuelle n’a pas dépassé la valeur *`vsaMetadata!.authenticationExpirationDate`*.

#### Étape &quot;Obtention d’une requête de profil de l’Adobe pour le MVPD sélectionné&quot; {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode par le biais du service d’authentification Adobe Pass [Requête de profil](/help/authentication/retrieve-profilerequest.md).

>[!TIP]
>
> **<u>Conseil Pro :</u>** Notez que l’identifiant de fournisseur obtenu à partir de la structure de compte d’abonné vidéo représente le *`platformMappingId`* en termes de configuration de l’authentification Adobe Pass. Par conséquent, l’application doit déterminer la valeur de la propriété d’identifiant MVPD, à l’aide de la valeur *`platformMappingId`*, par le biais du service d’authentification Adobe Pass [Fournir une liste MVPD](/help/authentication/provide-mvpd-list.md).

#### Étape : &quot;Transfert de la demande d’Adobe vers Platform SSO pour obtenir le profil&quot; {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode au moyen de la structure [Compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount).


- L’application doit rechercher l’autorisation [d’accéder aux informations d’abonnement de l’utilisateur ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) et procéder uniquement si l’utilisateur l’a autorisée.
- L’application doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
- L’application doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



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
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Étape : &quot;Exchange du profil d’authentification unique de Platform pour un jeton d’authentification d’Adobe&quot; {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode par le biais du service d’authentification Adobe Pass [Exchange de jeton](/help/authentication/token-exchange.md).


>[!TIP]
>
> **<u>Conseil Pro :</u>** Soyez conscient du fragment de code de l’étape [&quot;Transférez la demande d’Adobe à la SSO Platform pour obtenir le profil&quot;](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile). Ce *`vsaMetadata!.samlAttributeQueryResponse!`* représente le *`SAMLResponse`*, qui doit être transmis sur [Exchange de jeton](/help/authentication/token-exchange.md) et qui nécessite une manipulation et un codage de chaîne (*Base64* codé et *URL* encodé par la suite) avant d’effectuer l’appel.

</br>

#### Étape : &quot;Le jeton d’Adobe est-il généré avec succès ?&quot; {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode par le biais de la réponse réussie [Exchange de jeton](/help/authentication/token-exchange.md) de l’authentification Adobe Pass, qui sera un *`204 No Content`*, indiquant que le jeton a été créé avec succès et qu’il est prêt à être utilisé pour les flux d’autorisation.

</br>

#### Étape : &quot;Lancement du deuxième workflow d’authentification d’écran&quot; {#Initiate_second_screen_authentication_workflow}

**Important :** La terminologie &quot;Processus d’authentification du deuxième écran&quot; est appropriée pour les Apple TV, tandis que la terminologie &quot;Processus d’authentification du premier écran&quot; / &quot;Processus d’authentification régulier&quot; serait plus appropriée pour les iPhone et les iPad.


>[!TIP]
>
> **<u>Conseil :</u>** Mettez cela en oeuvre par le biais de l’authentification Adobe Pass

[Demande de code d’enregistrement](/help/authentication/registration-code-request.md), [Lancer l’authentification](/help/authentication/initiate-authentication.md) et [API REST Récupérer le jeton d’authentification](/help/authentication/retrieve-authentication-token.md) ou les services [Vérifier le jeton d’authentification](/help/authentication/check-authentication-token.md).


>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations tvOS.

- L’application doit [obtenir un code d’enregistrement](/help/authentication/registration-code-request.md) et le présenter à l’utilisateur final sur le premier appareil (écran).
- L’application doit démarrer [polling pour reconnaître l’état d’authentification](/help/authentication/retrieve-authentication-token.md) sur le 1er appareil (écran) après l’obtention du code d’enregistrement.
- Une autre application doit [lancer l’authentification](/help/authentication/initiate-authentication.md) sur un 2e appareil (écran) lorsque le code d’enregistrement est utilisé.
- L’application doit arrêter [polling](/help/authentication/retrieve-authentication-token.md) sur le 1er appareil (écran) lorsque le jeton d’authentification est généré.



>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations iOS/iPadOS.

- L’application doit [obtenir un code d’enregistrement](/help/authentication/registration-code-request.md) qui ne doit pas être présenté à l’utilisateur final sur le premier appareil (écran).
- L’application doit [lancer l’authentification](/help/authentication/initiate-authentication.md) sur le premier appareil (écran) à l’aide du code d’enregistrement et d’un composant [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
- L’application doit démarrer [l’interrogation pour connaître l’état d’authentification](/help/authentication/retrieve-authentication-token.md) sur le premier appareil (écran) après la fermeture du composant [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
- L’application doit arrêter [polling](/help/authentication/retrieve-authentication-token.md) sur le 1er appareil (écran) lorsque le jeton d’authentification est généré.

</br>

#### Étape : &quot;Poursuivre avec les flux d’autorisation&quot; {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode par le biais des services d’authentification Adobe Pass [Lancer l’autorisation](/help/authentication/initiate-authorization.md) et [Obtenir un jeton de média court](/help/authentication/obtain-short-media-token.md) .

</br>

### Déconnexion {#Logout}

La structure [ du compte d’abonné vidéo ](https://developer.apple.com/documentation/videosubscriberaccount) ne fournit pas d’API pour déconnecter par programmation les personnes qui se sont connectées à leur compte de fournisseur de télévision au niveau du système de l’appareil. Par conséquent, pour que la déconnexion entre en vigueur, l’utilisateur final doit se déconnecter explicitement de *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS. L’autre option dont dispose l’utilisateur consiste à retirer l’autorisation d’accéder aux informations d’abonnement de l’utilisateur dans la section des paramètres de l’application spécifique (accès au fournisseur de télévision).

>[!TIP]
>
> **<u>Conseil :</u>** Mettez en oeuvre cette méthode à l’aide des services d’&#39;authentification Adobe Pass [appel de métadonnées utilisateur](/help/authentication/user-metadata.md) et de [déconnexion](/help/authentication/initiate-logout.md).


>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations tvOS.


- L’application doit déterminer si l’authentification s’est produite suite à une connexion par le biais de la plateforme SSO ou non, à l’aide des &quot;*tokenSource&quot;* [métadonnées utilisateur](/help/authentication/user-metadata.md) du service d’authentification Adobe Pass.
- L’application doit demander à l’utilisateur de se déconnecter explicitement de *`Settings -> Accounts -> TV Provider`* sur tvOS **uniquement** si la valeur *&quot;tokenSource&quot;* est égale à &quot;*Apple&quot;.*
- L’application doit [lancer la déconnexion](/help/authentication/initiate-logout.md) à partir du service d’authentification Adobe Pass à l’aide d’un appel HTTP direct. Cela ne faciliterait pas le nettoyage de session du côté MVPD.



>[!TIP]
>
> **<u>Conseil Pro :</u>** Suivez les étapes ci-dessous pour la ou les implémentations iOS/iPadOS.

- L’application doit déterminer si l’authentification s’est produite suite à une connexion par le biais de la plateforme SSO ou non, à l’aide des &quot;*tokenSource&quot;* [métadonnées utilisateur](/help/authentication/user-metadata.md) du service d’authentification Adobe Pass.
- L’application doit demander à l’utilisateur de se déconnecter explicitement de *`Settings -> TV Provider`* sur iOS/iPadOS **uniquement** si la valeur *&quot;tokenSource&quot;* est égale à *&quot;Apple&quot;*.
- L’application doit [lancer la déconnexion](/help/authentication/initiate-logout.md) à partir du service d’authentification Adobe Pass à l’aide d’un composant [ WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller). Cela faciliterait le nettoyage de session du côté MVPD.

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
