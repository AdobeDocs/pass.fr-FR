---
title: Manuel de l’authentification unique Apple (API REST V2)
description: Manuel de l’authentification unique Apple (API REST V2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '3443'
ht-degree: 0%

---

# Manuel de l’authentification unique Apple (API REST V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

L’API REST d’authentification Adobe Pass V2 prend en charge l’authentification unique (SSO) du partenaire pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS.

Ce document agit comme une extension de l’aperçu [API REST V2)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) existant qui fournit une vue d’ensemble et le document qui décrit comment implémenter [authentification unique à l’aide de flux de partenaires](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).

## Authentification unique Apple à l’aide des flux de partenaires {#cookbook}

### Conditions préalables {#prerequisites}

Avant de poursuivre l’authentification unique Apple à l’aide des flux de partenaire, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit collecter toutes les données nécessaires requises par les en-têtes `X-Device-Info` et/ou `User-Agent`, de sorte que le serveur principal d’authentification Adobe Pass puisse identifier la plateforme de l’appareil et ses fonctionnalités. Pour plus d’informations sur `X-Device-Info`’en-tête , consultez la documentation de [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md).

* L’application de diffusion en continu doit demander l’accès aux informations d’abonnement de l’utilisateur enregistrées au niveau de l’appareil, pour lesquelles l’utilisateur doit donner à l’application l’autorisation de continuer, comme pour l’accès à la caméra ou au microphone de l’appareil. Cette autorisation doit être demandée par application à l’aide d’Apple [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) et l’appareil enregistre la sélection de l’utilisateur.

  Nous vous recommandons d’inciter les utilisateurs qui refusent d’accorder l’autorisation d’accéder aux informations d’abonnement en expliquant les avantages de l’expérience d’utilisateur à authentification unique d’Apple, mais sachez que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès avec autorisation du fournisseur de télévision) ou en *`Settings -> TV Provider`* sur iOS et iPadOS ou en *`Settings -> Accounts -> TV Provider`* sur tvOS.

  L’application de diffusion en continu peut demander l’autorisation de l’utilisateur lorsque l’application passe en premier plan, car l’application peut vérifier à tout moment [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur avant d’exiger l’authentification de l’utilisateur.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
>
> * L’application de diffusion en continu a rempli les [ conditions préalables à l’intégration ](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites-programmer) qui s’appliquent à un programmeur et sont requises pour activer l’expérience utilisateur d’authentification unique d’Apple.

### Workflow {#workflow}

Suivez les étapes données pour implémenter l’authentification unique Apple à l’aide des flux de partenaires, comme illustré dans le diagramme ci-dessous.

![Authentification unique Apple à l’aide des flux de partenaires](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*Authentification unique Apple à l’aide des flux de partenaires*

+++A. Phase d’enregistrement

1. **Récupérer les informations d’identification du client :** l’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations d’identification du client en appelant le point d’entrée du registre client.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération des informations d’identification du client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ comme `software_statement`
   > * Tous les en-têtes _obligatoires_ tels que `Content-Type`, `X-Device-Info`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Renvoyer les informations d’identification du client :** la réponse du point d’entrée du registre du client contient des informations sur les informations d’identification du client associées aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération des informations d’identification du client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) pour plus d’informations sur les informations fournies dans une réponse d’informations d’identification du client.
   >
   > <br/>
   >
   > Le registre du client valide les données de la requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui respectent la documentation de l’API [ Récupération des informations d’identification du client ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error).

   >[!TIP]
   >
   > Suggestion : les informations d’identification du client doivent être mises en cache et peuvent être utilisées indéfiniment.

1. **Récupérer le jeton d’accès :** l’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer le jeton d’accès en appelant le point d’entrée du jeton client.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [ Récupération du jeton d’accès ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `client_id`, `client_secret` et `grant_type`
   > * Tous les en-têtes _obligatoires_ tels que `Content-Type`, `X-Device-Info`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Jeton d’accès de retour :** la réponse du point d’entrée du jeton client contient des informations sur le jeton d’accès associé aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Récupérer le jeton d’accès](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) pour plus d’informations sur les informations fournies dans une réponse de jeton d’accès.
   >
   > <br/>
   >
   > Le jeton client valide les données de la requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui respectent la documentation de l’API [ Récupérer le jeton d’accès ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#error).

   >[!TIP]
   >
   > Suggestion : le jeton d’accès doit être mis en cache et utilisé uniquement pendant la durée spécifiée (par exemple, durée de vie de 24 heures). Après son expiration, l’application de diffusion en continu doit demander un nouveau jeton d’accès.

+++

+++B. Vérifier la phase d’authentification

1. **Récupérer l’état du framework du partenaire :** l’application de diffusion en continu appelle le [Framework de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développé par Apple, pour obtenir les autorisations de l’utilisateur et des informations sur le fournisseur.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) pour plus d’informations sur :
   >
   > <br/>
   >
   > * L’application en flux continu doit vérifier [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur et ne continuer que si l’utilisateur l’a autorisée.
   > * L’application de diffusion en continu doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour le `VSAccountManager`.
   > * L’application de streaming doit soumettre une [demande](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
   > * L’application de diffusion en continu doit attendre et traiter les informations [métadonnées](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer de spécifier une valeur booléenne égale à `false` pour la propriété [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) dans l’objet `VSAccountMetadataRequest`, afin d’indiquer que l’utilisateur ne peut pas être interrompu à cette phase.

1. **Renvoyer les informations de statut du framework du partenaire :** l’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * Le statut d’accès de l’autorisation utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (si disponible) est valide.

1. **Récupérer les profils :** l’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer toutes les informations de profil en envoyant une requête au point d’entrée des profils.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ comme `serviceProvider`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier` et `AP-Partner-Framework-Status`
   > * Tous les paramètres _facultatifs_ et en-têtes
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour le statut du framework du partenaire, de sorte que la réponse récupérée puisse inclure un profil de type « appleSSO ».
   >
   > <br/>
   >
   > Pour plus d’informations sur `AP-Partner-Framework-Status`’en-tête , consultez la documentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Renvoyer des informations sur les profils trouvés :** la réponse de point d’entrée des profils contient des informations sur les profils trouvés associés aux paramètres et en-têtes reçus.

1. **Choisir un profil et poursuivre les flux de décisions :** si la réponse de point d’entrée des profils contient des profils, l’application de diffusion en continu utilise sa logique interne (éventuellement en interagissant avec l’utilisateur final) pour choisir l’un des profils disponibles pour continuer les flux de décisions suivants.

1. **Poursuivre avec le flux d’authentification du partenaire :** si la réponse de point d’entrée des profils ne contient pas de profil, l’application de diffusion en continu continue avec le flux d’authentification du partenaire.

+++

+++C. Phase d’authentification du partenaire

1. **Récupérer la configuration :** l’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer la liste des MVPD ayant une intégration active en envoyant une requête au point d’entrée de configuration.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération de la configuration pour un fournisseur de services spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ comme `serviceProvider`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier` et `X-Device-Info`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Renvoyer la configuration :** la réponse du point d’entrée de configuration contient des informations sur les fichiers MVPD ayant une intégration active avec le fournisseur d’accès d’accès.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération de la configuration pour un fournisseur de services spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response) pour plus d’informations sur les informations fournies dans une réponse de configuration.
   >
   > <br/>
   >
   > Le point d’entrée de configuration valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > L’application de diffusion en continu doit s’assurer de traiter les détails suivants fournis pour chaque MVPD lors du traitement ultérieur :
   >
   > * `enablePlatformServices` : indique si le MVPD prend actuellement en charge l’authentification unique Apple.
   > * `displayInPlatformPicker` : indique si le MVPD peut être affiché dans le sélecteur Apple.
   > * `boardingStatus` : indique si le MVPD est intégré à l’authentification unique Apple.

1. **Récupérer l’état du framework du partenaire :** l’application de diffusion en continu appelle le [Framework de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développé par Apple, pour obtenir les autorisations de l’utilisateur et des informations sur le fournisseur.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) pour plus d’informations sur :
   >
   > <br/>
   >
   > * L’application en flux continu doit vérifier [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur et ne continuer que si l’utilisateur l’a autorisée.
   > * L’application de diffusion en continu doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour le `VSAccountManager`.
   > * L’application de streaming doit soumettre une [demande](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
   > * L’application de diffusion en continu doit attendre et traiter les informations [métadonnées](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer de spécifier une valeur booléenne égale à `true` pour la propriété [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) dans l’objet `VSAccountMetadataRequest`, afin d’indiquer que l’utilisateur peut être interrompu pour sélectionner le fournisseur de télévision à cette phase.

1. **Renvoyer les informations de statut du framework du partenaire :** l’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * Le statut d’accès de l’autorisation utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (si disponible) est valide.

1. **Récupérer la demande d’authentification du partenaire :** l’application de diffusion en continu rassemble toutes les données nécessaires pour lancer une session d’authentification en appelant le point d’entrée Sessions Partner .

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération de la demande d’authentification du partenaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider` et `partner`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` et `AP-Partner-Framework-Status`
   > * Tous les en-têtes et paramètres _facultatifs_
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour le statut du framework du partenaire, de sorte que la réponse récupérée puisse inclure une requête d’authentification du partenaire (requête SAML).
   >
   > <br/>
   >
   > Pour plus d’informations sur `AP-Partner-Framework-Status`’en-tête , consultez la documentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Indiquez l’action suivante :** la réponse du point d’entrée du partenaire Sessions contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Récupération de la requête d’authentification du partenaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response) pour plus d’informations sur les informations fournies dans une réponse de session.
   >
   > <br/>
   >
   > Le point d’entrée du partenaire Sessions valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   > * L’intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   >
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > Le point d’entrée du partenaire Sessions valide les données de requête pour s’assurer que les conditions d’authentification unique du partenaire sont remplies :
   >
   >  * La configuration de l’authentification unique du partenaire dans le serveur Adobe Pass doit être valide et activée.
   >  * La payload de statut du framework du partenaire reçue via l’en-tête [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) doit être valide.
   >
   > <br/>
   >
   > Si la validation de l’authentification unique du partenaire échoue, la réponse est définie par défaut sur le flux d’authentification de base.

1. **Poursuivre avec les flux de décisions :** la réponse du point d’entrée du partenaire Sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur « autoriser ».
   * L’attribut `actionType` est défini sur « direct ».

   Si le serveur principal Adobe Pass identifie un profil valide, l’application de diffusion en continu n’a pas besoin de s’authentifier à nouveau avec le MVPD sélectionné, car il existe déjà un profil qui peut être utilisé pour les flux de décisions suivants.

1. **Poursuivre avec le flux d’authentification de base :** la réponse du point d’entrée du partenaire Sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur « authentifier » ou « reprendre ».
   * L’attribut `actionType` est défini sur « interactif » ou « direct ».

   Si le serveur principal d’Adobe Pass n’identifie pas de profil valide et que la validation de l’authentification unique du partenaire échoue, le serveur Adobe Pass revient au flux d’authentification de base.

   Pour plus d’informations sur le flux d’authentification de base, reportez-vous aux documents suivants :
   * [Authentification dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentification dans l’application secondaire avec mvpd présélectionné](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Effectuer l’authentification dans l’application secondaire sans mvpd présélectionné](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Procédez à la récupération du profil à l’aide du flux de réponse d’authentification du partenaire :** la réponse de point d’entrée du partenaire Sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur « partner_profile ».
   * L’attribut `actionType` est défini sur « direct ».
   * L’attribut `authenticationRequest - type` inclut le protocole de sécurité utilisé par le framework de partenaire pour la connexion à MVPD (actuellement défini sur SAML uniquement).
   * L’attribut `authenticationRequest - request` inclut la requête SAML transmise au framework du partenaire.
   * L’attribut `authenticationRequest - attributesNames` inclut les attributs SAML transmis au framework du partenaire.

   Si le serveur principal Adobe Pass n’identifie pas de profil valide et que la validation de l’authentification unique du partenaire réussit, l’application de diffusion en continu reçoit une réponse avec des actions et des données à transmettre au framework du partenaire pour démarrer le flux d’authentification avec MVPD.

1. **Authentification MVPD complète avec le framework du partenaire :** transférez la demande d’authentification du partenaire (demande SAML) obtenue à l’étape précédente vers le [ Framework de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount). Si le flux d’authentification est réussi, l’interaction du [framework de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) avec le MVPD produit une réponse d’authentification du partenaire (réponse SAML) qui est renvoyée avec les informations de statut du framework du partenaire.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) pour plus d’informations sur :
   >
   > <br/>
   >
   > * L’application en flux continu doit vérifier [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur et ne continuer que si l’utilisateur l’a autorisée.
   > * L’application de diffusion en continu doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour le `VSAccountManager`.
   > * L’application de streaming doit soumettre une [requête](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné et doit inclure la requête d’authentification du partenaire (requête SAML) obtenue à l’étape précédente.
   > * L’application de diffusion en continu doit attendre et traiter les informations [métadonnées](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer de spécifier une valeur booléenne égale à `true` pour la propriété [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) dans l’objet `VSAccountMetadataRequest`, afin d’indiquer que l’utilisateur peut être interrompu pour s’authentifier auprès du fournisseur de télévision sélectionné à cette phase.

1. **Réponse d’authentification du partenaire de retour :** l’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * Le statut d’accès de l’autorisation utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (si disponible) est valide.
   * La réponse d’authentification du partenaire (réponse SAML) est présente et valide.

1. **Récupérer le profil à l’aide de la réponse d’authentification du partenaire :** l’application de diffusion en continu rassemble toutes les données nécessaires pour créer et récupérer un profil en appelant le point d’entrée Profiles Partner .

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupérer le profil à l’aide de la réponse d’authentification du partenaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `partner` et `SAMLResponse`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` et `AP-Partner-Framework-Status`
   > * Tous les en-têtes et paramètres _facultatifs_
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut des valeurs valides pour le statut du framework du partenaire et la réponse d’authentification du partenaire (réponse SAML), de sorte que la réponse récupérée puisse inclure un profil de type « appleSSO ».
   >
   > <br/>
   >
   > Pour plus d’informations sur `AP-Partner-Framework-Status`’en-tête , consultez la documentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Renvoyer des informations sur le profil du partenaire :** la réponse de point d’entrée des profils contient des informations sur le profil du partenaire, y compris l’attribut `type` défini sur « appleSSO ».

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Récupérer le profil à l’aide de la réponse d’authentification du partenaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response) pour plus d’informations sur les informations fournies dans une réponse de profil.
   >
   > <br/>
   >
   > Le point d’entrée du partenaire Profiles valide les données de la requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   > * L’intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > Le point d’entrée Partenaire des profils valide les données de requête pour s’assurer que les conditions d’authentification unique du partenaire sont remplies :
   >
   >  * La configuration de l’authentification unique du partenaire dans le serveur Adobe Pass doit être valide et activée.
   >  * La payload de statut du framework du partenaire reçue via l’en-tête [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) doit être valide.
   >
   > <br/>
   >
   > Si la validation de l’authentification unique du partenaire échoue, la réponse est définie par défaut sur le flux de récupération de profil de base.

1. **Poursuivre avec les flux de décisions :** l’application de diffusion en continu peut continuer avec les flux de décisions suivants.

+++

+++ D. Phase des décisions

1. **Récupérer l’état du framework du partenaire :** l’application de diffusion en continu appelle le [Framework de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développé par Apple, pour obtenir les autorisations de l’utilisateur et des informations sur le fournisseur.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) pour plus d’informations sur :
   >
   > <br/>
   >
   > * L’application en flux continu doit vérifier [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur et ne continuer que si l’utilisateur l’a autorisée.
   > * L’application de diffusion en continu doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour le `VSAccountManager`.
   > * L’application de streaming doit soumettre une [demande](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
   > * L’application de diffusion en continu doit attendre et traiter les informations [métadonnées](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer de spécifier une valeur booléenne égale à `false` pour la propriété [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) dans l’objet `VSAccountMetadataRequest`, afin d’indiquer que l’utilisateur ne peut pas être interrompu à cette phase.

   >[!TIP]
   > 
   > Suggestion : l’application de diffusion en continu peut utiliser une valeur mise en cache pour les informations de statut du framework du partenaire. Nous vous recommandons de l’actualiser lorsque l’application passe de l’état d’arrière-plan à l’état de premier plan.

1. **Renvoyer les informations de statut du framework du partenaire :** l’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * Le statut d’accès de l’autorisation utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (si disponible) est valide.

1. **Récupérer les décisions de préautorisation :** l’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir des décisions de préautorisation pour une liste de ressources en appelant le point d’entrée Decisions Preauthorize.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération des décisions de préautorisation à l’aide de mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) spécifique pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _obligatoires_, tels que `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour le statut du framework du partenaire avant d’effectuer une requête supplémentaire, lorsque le profil choisi est un profil de type « appleSSO ».
   >
   > <br/>
   >
   > Pour plus d’informations sur `AP-Partner-Framework-Status`’en-tête , consultez la documentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Renvoyer les décisions de préautorisation :** la réponse de point d’entrée Decisions Preauthorize contient une décision `Permit` ou `Deny` pour chaque ressource :
   * Une décision `Permit` signifie que la ressource est lisible. La réponse n’inclut pas de jeton multimédia, car le flux de préautorisation ne doit pas être utilisé pour lire les ressources.
   * Une décision `Deny` signifie que la ressource n’est pas lisible. La réponse inclut une payload d’erreur conforme à la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Récupération des décisions de préautorisation à l’aide d’une API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) spécifique pour plus d’informations sur les informations fournies dans une réponse de décision.
   >
   > <br/>
   >
   > Le point d’entrée de préautorisation des décisions valide les données de la requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   > * L’intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

1. **Récupérer l’état du framework du partenaire :** l’application de diffusion en continu appelle le [Framework de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) développé par Apple, pour obtenir les autorisations de l’utilisateur et des informations sur le fournisseur.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) pour plus d’informations sur :
   >
   > <br/>
   >
   > * L’application en flux continu doit vérifier [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur et ne continuer que si l’utilisateur l’a autorisée.
   > * L’application de diffusion en continu doit fournir un [délégué](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) pour le `VSAccountManager`.
   > * L’application de streaming doit soumettre une [demande](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) pour les informations de compte d’abonné.
   > * L’application de diffusion en continu doit attendre et traiter les informations [métadonnées](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer de spécifier une valeur booléenne égale à `false` pour la propriété [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) dans l’objet `VSAccountMetadataRequest`, afin d’indiquer que l’utilisateur ne peut pas être interrompu à cette phase.

   >[!TIP]
   >
   > Suggestion : l’application de diffusion en continu peut utiliser une valeur mise en cache pour les informations de statut du framework du partenaire. Nous vous recommandons de l’actualiser lorsque l’application passe de l’état d’arrière-plan à l’état de premier plan.

1. **Renvoyer les informations de statut du framework du partenaire :** l’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * Le statut d’accès de l’autorisation utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (si disponible) est valide.

1. **Récupérer la décision d’autorisation :** l’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point d’entrée Decisions Authorize.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Récupération des décisions d’autorisation à l’aide d’une API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) spécifique pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _obligatoires_, tels que `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour le statut du framework du partenaire avant d’effectuer une requête supplémentaire, lorsque le profil choisi est un profil de type « appleSSO ».
   >
   > <br/>
   >
   > Pour plus d’informations sur `AP-Partner-Framework-Status`’en-tête , consultez la documentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Décision d’autorisation de retour :** la réponse de point d’entrée Decisions Authorize contient une décision `Permit` ou `Deny` pour la ressource spécifique :
   * Une décision `Permit` signifie que la ressource est lisible. La réponse inclut un jeton de média.
   * Une décision `Deny` signifie que la ressource n’est pas lisible. La réponse inclut une payload d’erreur conforme à la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Récupérer les décisions d’autorisation à l’aide d’une API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) spécifique pour plus d’informations sur les informations fournies dans une réponse de décision.
   >
   > <br/>
   >
   > Le point d’entrée Autoriser les décisions valide les données de la requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   > * L’intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

+++

+++ D. Phase de déconnexion

1. **Lancer la déconnexion d’Adobe Pass :** l’application de diffusion en continu rassemble toutes les données nécessaires pour lancer le flux de déconnexion en appelant le point d’entrée de déconnexion Adobe Pass.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Lancer la déconnexion pour des API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) spécifiques pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd` et `redirectUrl`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Indiquez l’action suivante :** la réponse du point d’entrée de déconnexion Adobe Pass contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante :
   * L’attribut `url` est manquant, car l’utilisateur doit interagir avec le niveau du partenaire (système) pour terminer le flux de déconnexion.
   * L’attribut `actionName` est défini sur « partner_logout ».
   * L’attribut `actionType` est défini sur « partner_interactive ».

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Lancer la déconnexion pour des API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) spécifiques pour plus d’informations sur les informations fournies dans une réponse de déconnexion.
   >
   > <br/>
   >
   > Le point d’entrée de la déconnexion Adobe Pass valide les données de la requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   > * L’intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   > 
   > L’application de diffusion en continu doit s’assurer qu’elle indique à l’utilisateur de continuer à se déconnecter au niveau du partenaire.

+++
