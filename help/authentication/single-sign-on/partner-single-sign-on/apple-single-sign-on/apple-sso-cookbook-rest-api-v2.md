---
title: Guide pas à pas Apple SSO (API REST V2)
description: Guide pas à pas Apple SSO (API REST V2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: dbf68d75962e3e34f0c569c409f8c98ae6b9e036
workflow-type: tm+mt
source-wordcount: '3442'
ht-degree: 0%

---

# Guide pas à pas Apple SSO (API REST V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

L’API REST d’authentification Adobe Pass V2 prend en charge l’authentification unique du partenaire (SSO) pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS.

Ce document agit comme une extension de l’ [’API REST V2 Overview](/help/authentication/rest-api-v2/rest-api-v2-overview.md) qui fournit une vue de haut niveau et le document qui décrit comment implémenter l’ [authentification unique à l’aide de flux de partenaires](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).

## Authentification unique Apple à l’aide de flux de partenaires {#cookbook}

### Conditions préalables {#prerequisites}

Avant de poursuivre l’authentification unique Apple à l’aide de flux de partenaires, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit rassembler toutes les données nécessaires requises par les en-têtes `X-Device-Info` et/ou `User-Agent` afin que le serveur principal d’authentification Adobe Pass puisse identifier la plateforme de l’appareil et ses fonctionnalités. Pour plus d’informations sur l’en-tête `X-Device-Info`, consultez la documentation [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md).

* L’application de diffusion en continu doit demander l’accès aux informations d’abonnement de l’utilisateur enregistrées au niveau de l’appareil, pour lesquelles l’utilisateur doit autoriser l’application à continuer, comme pour permettre l’accès à la caméra ou au microphone de l’appareil. Cette autorisation doit être demandée par application en utilisant la [structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) d’Apple et l’appareil enregistrera la sélection de l’utilisateur.

  Nous recommandons aux utilisateurs qui refusent d’autoriser l’accès aux informations d’abonnement en expliquant les avantages de l’expérience de connexion unique d’Apple, mais sachez que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès aux autorisations du fournisseur de télévision) ou à *`Settings -> TV Provider`* sur iOS et iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS.

  L’application de diffusion en continu peut demander l’autorisation de l’utilisateur lorsque l’application entre en premier plan, car elle peut rechercher l’autorisation [d’accéder à](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur à tout moment avant d’avoir besoin d’une authentification de l’utilisateur.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
>
> * L’application de diffusion en continu a rempli les [ conditions préalables à l’intégration](/help/authentication/single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-overview.md#apple-sso-prerequisites-programmer) qui s’appliquent à un programmeur et sont requises pour activer l’expérience de connexion unique d’Apple.

### Workflow {#workflow}

Exécutez les étapes données pour mettre en oeuvre l’authentification unique Apple à l’aide des flux de partenaires, comme illustré dans le diagramme ci-dessous.

![Connexion unique Apple à l’aide de flux de partenaires](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*Connexion unique Apple à l’aide de flux de partenaires*

+++A. Phase d’enregistrement

1. **Récupérer les informations d’identification du client :** L’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations d’identification du client en appelant le point de terminaison de l’enregistrement du client.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request)
   >
   > * Tous les paramètres _requis_, comme `software_statement`
   > * Tous les en-têtes _requis_, comme `Content-Type`, `X-Device-Info`
   > * Tous les paramètres et en-têtes _optional_

1. **Renvoi des informations d’identification du client :** La réponse du point de terminaison de l’enregistrement du client contient des informations sur les informations d’identification du client associées aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse aux informations d’identification du client, reportez-vous à la documentation de l’API [Récupérer les informations d’identification du client](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) .
   >
   > <br/>
   >
   > Le registre du client valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation de l’API [Récupérer les informations d’identification du client](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error).

   >[!TIP]
   >
   > Suggestion : les informations d’identification du client doivent être mises en cache et peuvent être utilisées indéfiniment.

1. **Récupérer le jeton d’accès :** L’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer le jeton d’accès en appelant le point d’entrée du jeton client.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#request)
   >
   > * Tous les paramètres _required_, comme `client_id`, `client_secret` et `grant_type`
   > * Tous les en-têtes _requis_, comme `Content-Type`, `X-Device-Info`
   > * Tous les paramètres et en-têtes _optional_

1. **Jeton d’accès de retour :** La réponse du point de terminaison du jeton client contient des informations sur le jeton d’accès associé aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de jeton d’accès, reportez-vous à la documentation de l’API [ Récupérer le jeton d’accès](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) .
   >
   > <br/>
   >
   > Le jeton client valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation de l’API [Récupérer le jeton d’accès](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#error).

   >[!TIP]
   >
   > Suggestion : le jeton d’accès doit être mis en cache et utilisé uniquement pendant la durée spécifiée (par exemple, durée de vie de 24 heures). Après son expiration, l’application en continu doit demander un nouveau jeton d’accès.

+++

+++B. Vérifier la phase d’authentification

1. **Récupérer l’état de la structure du partenaire :** L’application de diffusion en continu appelle la [structure du compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développée par Apple pour obtenir les autorisations d’utilisateur et les informations sur le fournisseur.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](https://developer.apple.com/documentation/videosubscriberaccount)
   >
   > <br/>
   >
   > * L’application de diffusion en continu doit rechercher l’autorisation [ d’accéder aux informations d’abonnement de l’utilisateur et continuer uniquement si l’utilisateur l’a autorisée.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
   > * L’application de diffusion en continu doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour `VSAccountManager`.
   > * L’application de diffusion en continu doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
   > * L’application de diffusion en continu doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L’application en continu doit s’assurer qu’elle spécifie une valeur booléenne égale à `false` pour la propriété [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) dans l’objet `VSAccountMetadataRequest`, pour indiquer que l’utilisateur ne peut pas être interrompu à cette phase.

1. **Informations sur l’état de la structure de partenaire de retour :** L’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * L’état d’accès aux autorisations de l’utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (le cas échéant) est valide.

1. **Récupérer les profils :** L’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer toutes les informations de profil en envoyant une requête au point de terminaison Profils.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request)
   >
   > * Tous les paramètres _requis_, comme `serviceProvider`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier` et `AP-Partner-Framework-Status`
   > * Tous les paramètres et en-têtes _optional_
   >
   > <br/>
   >
   > L’application en continu doit s’assurer qu’elle inclut une valeur valide pour l’état de la structure du partenaire de sorte que la réponse récupérée puisse inclure un profil de type &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Pour plus d’informations sur l’en-tête `AP-Partner-Framework-Status`, consultez la documentation [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Informations de retour sur les profils trouvés :** La réponse du point de terminaison Profiles contient des informations sur les profils trouvés associés aux paramètres et aux en-têtes reçus.

1. **Sélectionnez un profil et passez aux flux de décisions :** Si la réponse du point de terminaison Profiles contient des profils, l’application en continu utilise sa logique interne (éventuellement en interagissant avec l’utilisateur final) pour choisir l’un des profils disponibles afin de continuer les flux de décisions suivants.

1. **Poursuivre avec le flux d’authentification du partenaire :** Si la réponse du point de terminaison Profiles ne contient pas de profil, l’application en continu continue avec le flux d’authentification du partenaire.

+++

+++C. Phase d’authentification du partenaire

1. **Récupérer la configuration :** L’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer la liste des MVPD ayant une intégration active en envoyant une requête au point de terminaison Configuration.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request)
   >
   > * Tous les paramètres _requis_, comme `serviceProvider`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier` et `X-Device-Info`
   > * Tous les paramètres et en-têtes _optional_

1. **Configuration de retour :** La réponse du point de terminaison de configuration contient des informations sur les MVPD ayant une intégration active avec le fournisseur de services.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de configuration, reportez-vous à la documentation de l’API [Récupérer la configuration pour un prestataire spécifique](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response) .
   >
   > <br/>
   >
   > Le point de terminaison Configuration valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes aux [ codes d’erreur améliorés](/help/authentication/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > L’application de diffusion en continu doit s’assurer qu’elle traite les détails suivants fournis pour chaque MVPD lors de la poursuite :
   >
   > * `enablePlatformServices` : indique si le MVPD prend actuellement en charge l’authentification unique Apple.
   > * `displayInPlatformPicker` : indique si le MVPD peut être affiché dans le sélecteur Apple.
   > * `boardingStatus` : indique si le MVPD est intégré dans l’authentification unique Apple.

1. **Récupérer l’état de la structure du partenaire :** L’application de diffusion en continu appelle la [structure du compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développée par Apple pour obtenir les autorisations d’utilisateur et les informations sur le fournisseur.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](https://developer.apple.com/documentation/videosubscriberaccount)
   >
   > <br/>
   >
   > * L’application de diffusion en continu doit rechercher l’autorisation [ d’accéder aux informations d’abonnement de l’utilisateur et continuer uniquement si l’utilisateur l’a autorisée.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
   > * L’application de diffusion en continu doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour `VSAccountManager`.
   > * L’application de diffusion en continu doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
   > * L’application de diffusion en continu doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle spécifie une valeur booléenne égale à `true` pour la propriété [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) dans l’objet `VSAccountMetadataRequest`, pour indiquer que l’utilisateur peut être interrompu pour sélectionner un fournisseur de télévision à cette phase.

1. **Informations sur l’état de la structure de partenaire de retour :** L’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * L’état d’accès aux autorisations de l’utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (le cas échéant) est valide.

1. **Récupérer la demande d’authentification du partenaire :** L’application de diffusion en continu rassemble toutes les données nécessaires pour lancer une session d’authentification en appelant le point de terminaison du partenaire sessions.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request)
   >
   > * Tous les paramètres _requis_, comme `serviceProvider` et `partner`
   > * Tous les en-têtes _requis_ tels que `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` et `AP-Partner-Framework-Status`
   > * Tous les en-têtes et paramètres _optional_
   >
   > <br/>
   >
   > L’application en continu doit s’assurer qu’elle inclut une valeur valide pour l’état de la structure du partenaire de sorte que la réponse récupérée puisse inclure une demande d’authentification du partenaire (requête SAML).
   >
   > <br/>
   >
   > Pour plus d’informations sur l’en-tête `AP-Partner-Framework-Status`, consultez la documentation [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Indiquez l’action suivante :** La réponse du point de terminaison du partenaire sessions contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de session, reportez-vous à la documentation de l’API [Récupérer la requête d’authentification du partenaire](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response).
   >
   > <br/>
   >
   > Le point de terminaison du partenaire sessions valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   > * L&#39;intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   >
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](/help/authentication/enhanced-error-codes.md).
   >
   > <br/>
   >
   > Le point de terminaison partenaire sessions valide les données de requête pour s’assurer que les conditions d’authentification unique du partenaire sont remplies :
   >
   >  * La configuration de l’authentification unique du partenaire dans le serveur Adobe Pass doit être valide et activée.
   >  * La payload d’état de la structure du partenaire reçue via l’en-tête [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) doit être valide.
   >
   > <br/>
   >
   > Si la validation de l’authentification unique du partenaire échoue, la réponse sera par défaut au flux d’authentification de base.

1. **Passez aux flux de décisions :** La réponse du point de terminaison du partenaire sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur &quot;autoriser&quot;.
   * L’attribut `actionType` est défini sur &quot;direct&quot;.

   Si le serveur principal Adobe Pass identifie un profil valide, l’application en continu n’a pas besoin de se réauthentifier auprès du MVPD sélectionné, car un profil peut déjà être utilisé pour les flux de décisions suivants.

1. **Poursuivez avec le flux d’authentification de base :** La réponse du point de terminaison du partenaire sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur &quot;authentifier&quot; ou &quot;reprendre&quot;.
   * L’attribut `actionType` est défini sur &quot;interactif&quot; ou &quot;direct&quot;.

   Si le serveur principal Adobe Pass n’identifie pas de profil valide et que la validation de l’authentification unique du partenaire échoue, le serveur Adobe Pass revient au flux d’authentification de base.

   Pour plus d’informations sur le flux d’authentification de base, reportez-vous aux documents suivants :
   * [Effectuer l’authentification dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Effectuez l’authentification dans une application secondaire avec mvpd présélectionné.](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Effectuer une authentification dans une application secondaire sans mvpd présélectionné](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Poursuivez la récupération du profil à l’aide du flux de réponse d’authentification du partenaire :** La réponse du point de terminaison du partenaire sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur &quot;partner_profile&quot;.
   * L’attribut `actionType` est défini sur &quot;direct&quot;.
   * L’attribut `authenticationRequest - type` comprend le protocole de sécurité utilisé par la structure partenaire pour la connexion MVPD (actuellement défini sur SAML uniquement).
   * L’attribut `authenticationRequest - request` inclut la requête SAML transmise à la structure du partenaire.
   * L’attribut `authenticationRequest - attributesNames` comprend les attributs SAML transmis à la structure du partenaire.

   Si le serveur principal Adobe Pass n’identifie pas de profil valide et que la validation de l’authentification unique du partenaire est validée, l’application en continu reçoit une réponse avec des actions et des données à transmettre à la structure du partenaire pour démarrer le flux d’authentification avec le MVPD.

1. **Effectuez l’authentification MVPD avec la structure du partenaire :** Transférez la demande d’authentification du partenaire (demande SAML) obtenue à l’étape précédente vers la [ Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount). Si le flux d’authentification est réussi, l’interaction [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) avec le MVPD produit une réponse d’authentification du partenaire (réponse SAML) qui est renvoyée avec les informations d’état de la structure du partenaire.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](https://developer.apple.com/documentation/videosubscriberaccount)
   >
   > <br/>
   >
   > * L’application de diffusion en continu doit rechercher l’autorisation [ d’accéder aux informations d’abonnement de l’utilisateur et continuer uniquement si l’utilisateur l’a autorisée.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
   > * L’application de diffusion en continu doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour `VSAccountManager`.
   > * L’application de diffusion en continu doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné et doit inclure la demande d’authentification du partenaire (requête SAML) obtenue à l’étape précédente.
   > * L’application de diffusion en continu doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle spécifie une valeur booléenne égale à `true` pour la propriété [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) dans l’objet `VSAccountMetadataRequest`, pour indiquer que l’utilisateur peut être interrompu pour s’authentifier auprès du fournisseur de télévision sélectionné à cette phase.

1. **Réponse de l’authentification du partenaire de retour :** L’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * L’état d’accès aux autorisations de l’utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (le cas échéant) est valide.
   * La réponse d’authentification du partenaire (réponse SAML) est présente et valide.

1. **Récupérer le profil à l’aide de la réponse d’authentification du partenaire :** L’application de diffusion en continu rassemble toutes les données nécessaires pour créer et récupérer un profil en appelant le point de terminaison du partenaire de profils.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request)
   >
   > * Tous les paramètres _required_, comme `serviceProvider`, `partner` et `SAMLResponse`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` et `AP-Partner-Framework-Status`
   > * Tous les en-têtes et paramètres _optional_
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut des valeurs valides pour l’état de la structure du partenaire et la réponse d’authentification du partenaire (réponse SAML), de sorte que la réponse récupérée puisse inclure un profil de type &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Pour plus d’informations sur l’en-tête `AP-Partner-Framework-Status`, consultez la documentation [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Informations de retour sur le profil du partenaire :** La réponse du point de terminaison Profiles contient des informations sur le profil du partenaire, y compris l’attribut `type` défini sur &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de profil, reportez-vous à la documentation de l’API [Récupérer le profil à l’aide de la réponse d’authentification du partenaire](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response).
   >
   > <br/>
   >
   > Le point de terminaison Profiles Partner valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   > * L&#39;intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](/help/authentication/enhanced-error-codes.md).
   >
   > <br/>
   >
   > Le point de terminaison Profiles Partner valide les données de requête pour s’assurer que les conditions d’authentification unique du partenaire sont remplies :
   >
   >  * La configuration de l’authentification unique du partenaire dans le serveur Adobe Pass doit être valide et activée.
   >  * La payload d’état de la structure du partenaire reçue via l’en-tête [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) doit être valide.
   >
   > <br/>
   >
   > Si la validation de l’authentification unique du partenaire échoue, la réponse est définie par défaut sur le flux de récupération du profil de base.

1. **Poursuivre avec les flux de décisions :** L’application de diffusion en continu peut continuer avec les flux de décisions suivants.

+++

+++ D. Phase des décisions

1. **Récupérer l’état de la structure du partenaire :** L’application de diffusion en continu appelle la [structure du compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développée par Apple pour obtenir les autorisations d’utilisateur et les informations sur le fournisseur.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](https://developer.apple.com/documentation/videosubscriberaccount)
   >
   > <br/>
   >
   > * L’application de diffusion en continu doit rechercher l’autorisation [ d’accéder aux informations d’abonnement de l’utilisateur et continuer uniquement si l’utilisateur l’a autorisée.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
   > * L’application de diffusion en continu doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour `VSAccountManager`.
   > * L’application de diffusion en continu doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
   > * L’application de diffusion en continu doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L’application en continu doit s’assurer qu’elle spécifie une valeur booléenne égale à `false` pour la propriété [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) dans l’objet `VSAccountMetadataRequest`, pour indiquer que l’utilisateur ne peut pas être interrompu à cette phase.

   >[!TIP]
   > 
   > Suggestion : l’application en continu peut utiliser une valeur mise en cache pour les informations d’état de la structure du partenaire, que nous vous recommandons d’actualiser lorsque l’application passe de l’état d’arrière-plan à l’état de premier plan.

1. **Informations sur l’état de la structure de partenaire de retour :** L’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * L’état d’accès aux autorisations de l’utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (le cas échéant) est valide.

1. **Récupérer les décisions de préautorisation :** L’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir des décisions de préautorisation pour une liste de ressources en appelant le point de terminaison Decisions Preauthorized .

   >[!IMPORTANT]
   >
   > Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions de préautorisation à l’aide de mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) spécifique :
   >
   > * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’état de la structure du partenaire avant d’effectuer une requête supplémentaire, lorsque le profil sélectionné est un profil de type &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Pour plus d’informations sur l’en-tête `AP-Partner-Framework-Status`, consultez la documentation [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Renvoi des décisions de préautorisation :** La réponse de préautorisation de décisions contient une décision `Permit` ou `Deny` pour chaque ressource :
   * Une décision `Permit` signifie que la ressource peut être lue. La réponse n’inclut pas de jeton multimédia, car le flux de préautorisation ne doit pas être utilisé pour lire les ressources.
   * Une décision `Deny` signifie que la ressource ne peut pas être lue. La réponse comprend un payload d’erreur conforme à la documentation [Enhanced Error Codes](/help/authentication/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse à une décision, reportez-vous à la documentation de l’API [Récupérer les décisions de préautorisation à l’aide de mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) spécifique.
   >
   > <br/>
   >
   > Le point de terminaison Decisions Preallow valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   > * L&#39;intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](/help/authentication/enhanced-error-codes.md).

1. **Récupérer l’état de la structure du partenaire :** L’application de diffusion en continu appelle la [structure du compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développée par Apple pour obtenir les autorisations d’utilisateur et les informations sur le fournisseur.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](https://developer.apple.com/documentation/videosubscriberaccount)
   >
   > <br/>
   >
   > * L’application de diffusion en continu doit rechercher l’autorisation [ d’accéder aux informations d’abonnement de l’utilisateur et continuer uniquement si l’utilisateur l’a autorisée.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
   > * L’application de diffusion en continu doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour `VSAccountManager`.
   > * L’application de diffusion en continu doit envoyer une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
   > * L’application de diffusion en continu doit attendre et traiter les informations [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L’application en continu doit s’assurer qu’elle spécifie une valeur booléenne égale à `false` pour la propriété [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) dans l’objet `VSAccountMetadataRequest`, pour indiquer que l’utilisateur ne peut pas être interrompu à cette phase.

   >[!TIP]
   >
   > Suggestion : l’application en continu peut utiliser une valeur mise en cache pour les informations d’état de la structure du partenaire, que nous vous recommandons d’actualiser lorsque l’application passe de l’état d’arrière-plan à l’état de premier plan.

1. **Informations sur l’état de la structure de partenaire de retour :** L’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * L’état d’accès aux autorisations de l’utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (le cas échéant) est valide.

1. **Récupérer la décision d’autorisation :** L’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point de terminaison Decisions Authorize.

   >[!IMPORTANT]
   >
   > Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) spécifique :
   >
   > * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’état de la structure du partenaire avant d’effectuer une requête supplémentaire, lorsque le profil sélectionné est un profil de type &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Pour plus d’informations sur l’en-tête `AP-Partner-Framework-Status`, consultez la documentation [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Décision d’autorisation de retour :** La réponse de point de terminaison Decisions Authorize contient une décision `Permit` ou `Deny` pour la ressource spécifique :
   * Une décision `Permit` signifie que la ressource peut être lue. La réponse comprend un jeton multimédia.
   * Une décision `Deny` signifie que la ressource ne peut pas être lue. La réponse comprend un payload d’erreur conforme à la documentation [Enhanced Error Codes](/help/authentication/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse à une décision, reportez-vous à la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) spécifique.
   >
   > <br/>
   >
   > Le point de terminaison Decisions Authorize valide les données de requête afin de s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   > * L&#39;intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](/help/authentication/enhanced-error-codes.md).

+++

+++ D. Phase de déconnexion

1. **Lancer la déconnexion Adobe Pass :** L’application de diffusion en continu rassemble toutes les données nécessaires pour lancer le flux de déconnexion en appelant le point de terminaison de la déconnexion Adobe Pass.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request)
   >
   > * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `redirectUrl`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_

1. **Indiquez l’action suivante :** La réponse du point de terminaison de la connexion Adobe Pass contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante :
   * L’attribut `url` est manquant, car l’utilisateur doit interagir avec le niveau du partenaire (système) pour terminer le flux de déconnexion.
   * L’attribut `actionName` est défini sur &quot;partner_logout&quot;.
   * L’attribut `actionType` est défini sur &quot;partner_interactive&quot;.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de déconnexion, reportez-vous à la documentation de l’API [Initiate logout for specific mvpd](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) .
   >
   > <br/>
   >
   > Le point de terminaison de connexion Adobe Pass valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   > * L&#39;intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](/help/authentication/enhanced-error-codes.md).

   >[!IMPORTANT]
   > 
   > L’application de diffusion en continu doit s’assurer qu’elle indique à l’utilisateur de continuer à se déconnecter au niveau du partenaire.

+++
