---
title: Authentification unique - Partner - Flux
description: API REST V2 - Authentification unique - Partner - Flux
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '1317'
ht-degree: 0%

---


# Authentification unique à l’aide de flux de partenaires {#single-sign-on-partner-flows}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

## Récupération de la demande d’authentification du partenaire {#retrieve-partner-authentication-request}

### Conditions préalables {#prerequisites-retrieve-partner-authentication-request}

Avant de récupérer la demande d’authentification du partenaire, vérifiez que les conditions préalables suivantes sont remplies :

* Le cadre du partenaire doit sélectionner un MVPD.
* L’application en continu doit obtenir les informations d’état de la structure du partenaire de la structure du partenaire et les transmettre au serveur Adobe Pass.
* L’application en continu doit obtenir la demande d’authentification du partenaire auprès du serveur Adobe Pass et la transmettre à la structure du partenaire.

>[!IMPORTANT]
>
> Hypothèses
> 
> <br/>
> 
> * La structure du partenaire prend en charge l’interaction utilisateur pour sélectionner un MVPD.
> * La structure du partenaire prend en charge l’interaction de l’utilisateur pour s’authentifier auprès du MVPD sélectionné.
> * La structure du partenaire fournit les autorisations d’utilisateur et les informations sur le fournisseur.

### Workflow {#workflow-retrieve-partner-authentication-request}

Effectuez les étapes données pour récupérer la demande d’authentification du partenaire, comme illustré dans le diagramme ci-dessous.

![Récupérer la demande d’authentification du partenaire](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-partner-authentication-request-flow.png)

*Récupérer la demande d’authentification du partenaire*

1. **Récupérer l’état de la structure du partenaire :** L’application de diffusion en continu appelle la structure du partenaire, en dehors des systèmes Adobe Pass, pour obtenir les autorisations d’utilisateur et les informations sur le fournisseur.

1. **Informations sur l’état de la structure de partenaire de retour :** L’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * L’état d’accès aux autorisations de l’utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (le cas échéant) est valide.

1. **Récupérer la demande d’authentification du partenaire :** L’application de diffusion en continu rassemble toutes les données nécessaires pour lancer une session d’authentification en appelant le point de terminaison du partenaire sessions.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
   >
   > * Tous les paramètres _requis_, comme `serviceProvider` et `partner`
   > * Tous les en-têtes _requis_ tels que `Authorization`, `AP-Device-Identifier` et `AP-Partner-Framework-Status`
   > * Tous les en-têtes et paramètres _optional_
   >
   > <br/>
   >
   > L’application en continu doit s’assurer qu’elle inclut une valeur valide pour l’état de la structure du partenaire avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AP-Partner-Framework-Status`, consultez la documentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Indiquez l’action suivante :** La réponse du point de terminaison du partenaire sessions contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de session, reportez-vous à la documentation de l’API [Récupérer la requête d’authentification du partenaire](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md).
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
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).
   >
   > <br/>
   >
   > Le point de terminaison partenaire sessions valide les données de requête pour s’assurer que les conditions d’authentification unique du partenaire sont remplies :
   >
   >  * La configuration de l’authentification unique du partenaire dans le serveur Adobe Pass doit être valide et activée.
   >  * La payload d’état de la structure du partenaire reçue via l’en-tête [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) doit être valide.
   >
   > <br/>
   >
   > Si la validation de l’authentification unique du partenaire échoue, la réponse sera par défaut au flux d’authentification de base.

1. **Poursuivez avec le flux de récupération de profil à l’aide de la réponse d’authentification du partenaire :** La réponse du point de terminaison du partenaire sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur &quot;partner_profile&quot;.
   * L’attribut `actionType` est défini sur &quot;direct&quot;.
   * L’attribut `authenticationRequest - type` comprend le protocole de sécurité utilisé par la structure partenaire pour la connexion MVPD (actuellement défini sur SAML uniquement).
   * L’attribut `authenticationRequest - request` inclut la requête SAML transmise à la structure du partenaire.
   * L’attribut `authenticationRequest - attributesNames` comprend les attributs SAML transmis à la structure du partenaire.

   Si le serveur principal Adobe Pass n’identifie pas de profil valide et que la validation de l’authentification unique du partenaire est validée, l’application en continu reçoit une réponse avec des actions et des données à transmettre à la structure du partenaire pour démarrer le flux d’authentification avec le MVPD.

   Pour plus d’informations sur le flux de récupération des profils à l’aide d’une réponse d’authentification de partenaire, reportez-vous à la section [Récupérer le profil à l’aide de la réponse d’authentification de partenaire](#retrieve-profile-using-partner-authentication-response) .

1. **Poursuivez avec le flux d’authentification de base :** La réponse du point de terminaison du partenaire sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur &quot;authentifier&quot; ou &quot;reprendre&quot;.
   * L’attribut `actionType` est défini sur &quot;interactif&quot; ou &quot;direct&quot;.

   Si le serveur principal Adobe Pass n’identifie pas de profil valide et que la validation de l’authentification unique du partenaire échoue, le serveur Adobe Pass revient au flux d’authentification de base.

   Pour plus d’informations sur le flux d’authentification de base, reportez-vous aux documents suivants :
   * [Effectuer l’authentification dans l’application principale](../basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Effectuez l’authentification dans une application secondaire avec mvpd présélectionné.](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Effectuer une authentification dans une application secondaire sans mvpd présélectionné](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Passez aux flux de décisions :** La réponse du point de terminaison du partenaire sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur &quot;autoriser&quot;.
   * L’attribut `actionType` est défini sur &quot;direct&quot;.

   Si le serveur principal Adobe Pass identifie un profil valide, l’application en continu n’a pas besoin de se réauthentifier auprès du MVPD sélectionné, car un profil peut déjà être utilisé pour les flux de décisions suivants.

   >[!IMPORTANT]
   >
   > L’application en continu doit s’assurer qu’elle inclut une valeur valide pour l’état de la structure du partenaire avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AP-Partner-Framework-Status`, consultez la documentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

## Récupération du profil à l’aide de la réponse d’authentification du partenaire {#retrieve-profile-using-partner-authentication-response}

### Conditions préalables {#prerequisites-retrieve-profile-using-partner-authentication-response}

Avant de récupérer le profil à l’aide d’une réponse d’authentification du partenaire, assurez-vous que les conditions préalables suivantes sont remplies :

* La structure du partenaire doit effectuer l’authentification avec le MVPD sélectionné.
* L’application en continu doit obtenir la réponse d’authentification du partenaire ainsi que les informations d’état de la structure du partenaire de la structure du partenaire et la transmettre au serveur Adobe Pass.

>[!IMPORTANT]
>
> Hypothèse
>
> * La structure du partenaire prend en charge l’interaction utilisateur pour sélectionner un MVPD.
> * La structure du partenaire prend en charge l’interaction de l’utilisateur pour s’authentifier auprès du MVPD sélectionné.
> * La structure du partenaire fournit les autorisations d’utilisateur et les informations sur le fournisseur.

### Workflow {#workflow-retrieve-profile-using-partner-authentication-response}

Effectuez les étapes données pour mettre en oeuvre le flux de récupération des profils à l’aide d’une réponse d’authentification de partenaire, comme illustré dans le diagramme suivant.

![ Récupération du profil à l’aide de la réponse d’authentification du partenaire ](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-profile-using-partner-authentication-response-flow.png)

*Récupérer le profil authentifié à l’aide de la réponse d’authentification du partenaire*

1. **Authentification MVPD complète avec framework partenaire :** Si le flux d’authentification est réussi, l’interaction de framework partenaire avec MVPD produit une réponse d’authentification du partenaire (réponse SAML) qui est renvoyée avec les informations d’état de la structure du partenaire.

1. **Réponse de l’authentification du partenaire de retour :** L’application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de base sont remplies :
   * L’état d’accès aux autorisations de l’utilisateur est accordé.
   * L’identifiant de mappage du fournisseur d’utilisateurs est présent et valide.
   * La date d’expiration du profil du fournisseur d’utilisateurs (le cas échéant) est valide.

1. **Récupérer le profil à l’aide de la réponse d’authentification du partenaire :** L’application de diffusion en continu rassemble toutes les données nécessaires pour créer et récupérer un profil en appelant le point de terminaison du partenaire de profils.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
   >
   > * Tous les paramètres _required_, comme `serviceProvider`, `partner` et `SAMLResponse`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier` et `AP-Partner-Framework-Status`
   > * Tous les en-têtes et paramètres _optional_
   >
   > <br/>
   > 
   > L’application en continu doit s’assurer qu’elle inclut une valeur valide pour l’état de la structure du partenaire avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AP-Partner-Framework-Status`, consultez la documentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Créer et enregistrer le profil du partenaire :** Le serveur Adobe Pass crée et enregistre un profil de partenaire après s’être assuré que toutes les conditions sont remplies.

1. **Informations de retour sur le profil du partenaire :** La réponse du point de terminaison Profiles contient des informations sur le profil du partenaire, y compris l’attribut `type` défini sur &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de profil, reportez-vous à la documentation de l’API [Récupérer le profil à l’aide de la réponse d’authentification du partenaire](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md).
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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).
   >
   > <br/>
   >
   > Le point de terminaison Profiles Partner valide les données de requête pour s’assurer que les conditions d’authentification unique du partenaire sont remplies :
   >
   >  * La configuration de l’authentification unique du partenaire dans le serveur Adobe Pass doit être valide et activée.
   >  * La payload d’état de la structure du partenaire reçue via l’en-tête [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) doit être valide.
   >
   > <br/>
   >
   > Si la validation de l’authentification unique du partenaire échoue, la réponse est définie par défaut sur le flux de récupération du profil de base.

1. **Poursuivre avec les flux de décisions :** L’application de diffusion en continu peut continuer avec les flux de décisions suivants.

   >[!IMPORTANT]
   >
   > L’application en continu doit s’assurer qu’elle inclut une valeur valide pour l’état de la structure du partenaire avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AP-Partner-Framework-Status`, consultez la documentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .
