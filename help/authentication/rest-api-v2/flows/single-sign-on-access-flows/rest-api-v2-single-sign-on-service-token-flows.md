---
title: Connexion unique - Jeton de service - Flux
description: API REST V2 - Connexion unique - Jeton de service - Flux
exl-id: b0082d2a-e491-4cb5-bb40-35ba10db6b1a
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1848'
ht-degree: 0%

---

# Authentification unique à l’aide de flux de jetons de service{#single-sign-on-service-token-full-flows}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

La méthode Service Token permet à plusieurs applications d’utiliser un identifiant d’utilisateur unique afin d’effectuer une authentification unique (SSO) sur plusieurs appareils et plateformes lors de l’utilisation des services Adobe Pass.

Les applications sont chargées de récupérer la charge utile de l’identifiant utilisateur unique à l’aide de services d’identité externes, en dehors des systèmes Adobe Pass, tels que :

* Service DTC (Direct-to-consumer) où les utilisateurs se connectent sur chaque appareil à l’aide des mêmes informations d’identification et sont associés au même identifiant utilisateur ou nom de compte utilisateur.
* Service d’authentification tiers, tel que Google ou Facebook, où les utilisateurs se connectent sur chaque appareil à l’aide des mêmes informations d’identification et sont associés à la même adresse électronique.

Les applications sont chargées d’inclure cette charge utile d’identifiant utilisateur unique dans l’en-tête `AD-Service-Token` de toutes les requêtes qui la spécifient.

Pour plus d’informations sur l’en-tête `AD-Service-Token`, consultez la documentation [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) .

## Authentification par authentification unique à l’aide du jeton de service {#performing-authentication-flow-using-service-token-single-sign-on-method}

### Conditions préalables {#prerequisites-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Avant d’exécuter le flux d’authentification par authentification unique à l’aide d’un jeton de service, assurez-vous que les conditions préalables suivantes sont remplies :

* Le service d’identité externe doit renvoyer des informations cohérentes sous la forme d’une payload `JWS` sur toutes les applications sur plusieurs appareils et plateformes.
* La première application de diffusion en continu doit récupérer l’identifiant utilisateur unique et inclure la charge utile `JWS` dans l’en-tête [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) pour toutes les requêtes qui la spécifient.
* La première application de diffusion en continu doit sélectionner un MVPD.
* La première application de diffusion en continu doit lancer une session d’authentification pour se connecter avec le MVPD sélectionné.
* La première application en continu doit s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.
* La seconde application de diffusion en continu doit récupérer l’identifiant utilisateur unique et inclure la payload `JWS` dans l’en-tête [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) pour toutes les requêtes qui la spécifient.

>[!IMPORTANT]
>
> Hypothèses
> 
> <br/>
> 
> * La première application de diffusion en continu prend en charge l’interaction de l’utilisateur pour sélectionner un MVPD.
> * La première application de diffusion en continu prend en charge l’interaction de l’utilisateur pour s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.

### Workflow {#workflow-steps-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Effectuez les étapes données pour mettre en oeuvre le flux d’authentification par authentification unique à l’aide d’un jeton de service, comme illustré dans le diagramme ci-dessous.

![Effectuez l’authentification par authentification unique à l’aide du jeton de service](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-service-token-flow.png)

*Effectuez l’authentification par authentification unique à l’aide du jeton de service*

1. **Authentifier avec le service d’identité :** La première application de diffusion en continu appelle le service d’identité, en dehors des systèmes Adobe Pass, pour obtenir la charge utile `JWS` associée à l’identifiant utilisateur unique.

1. **Identifiant utilisateur unique de retour JWS :** La première application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de sécurité de base sont remplies :
   * La charge utile n’a pas expiré.
   * La charge utile est signée.

1. **Créer une session d’authentification :** La première application de diffusion en continu rassemble toutes les données nécessaires pour lancer une session d’authentification en appelant le point de terminaison sessions .

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
   >
   > * Tous les paramètres _required_, tels que `serviceProvider`, `mvpd`, `domainName` et `redirectUrl`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_
   >
   > <br/>
   > 
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AD-Service-Token`, consultez la documentation [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) .

1. **Indiquez l’action suivante :** La réponse du point de terminaison sessions contient les données nécessaires pour guider la première application de diffusion en continu concernant l’action suivante.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de session, reportez-vous à la documentation de l’API [Créer une session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).
   >
   > <br/>
   > 
   > Le point de terminaison Sessions valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   > * L&#39;intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Ouvrir l’URL dans l’agent utilisateur :** La réponse du point de terminaison sessions contient les données suivantes :
   * `url` qui peut être utilisé pour lancer l’authentification interactive dans la page de connexion MVPD.
   * L’attribut `actionName` est défini sur &quot;authenticate&quot;.
   * L’attribut `actionType` est défini sur &quot;interactif&quot;.

   Si le serveur principal Adobe Pass n’identifie pas de profil valide, la première application de diffusion en continu ouvre un agent utilisateur pour charger le `url` fourni, effectuant une requête sur le point de terminaison Authenticate (Authentifier). Ce flux peut inclure plusieurs redirections, ce qui conduit finalement l’utilisateur à la page de connexion MVPD et à fournir des informations d’identification valides.

1. **Authentification MVPD complète :** Si le flux d’authentification est réussi, l’interaction de l’agent utilisateur enregistre un profil régulier dans le serveur principal Adobe Pass et atteint le `redirectUrl` fourni.

1. **Récupérer le profil pour un code spécifique :** La première application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil en envoyant une requête au point de terminaison Profiles.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
   > 
   > * Tous les paramètres _requis_, comme `serviceProvider`, `code`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_

   >[!TIP]
   >
   > Suggestion : l’application de diffusion en continu peut attendre que l’agent utilisateur atteigne le `redirectUrl` fourni pour vérifier si le profil normal a été généré et enregistré avec succès.

1. **Rechercher un profil régulier :** Le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

1. **Renvoi d’informations sur le profil normal :** La réponse du point de terminaison Profiles contient des informations sur le profil trouvé associé aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de profil, reportez-vous à la documentation de l’API [Récupérer le profil pour un code spécifique](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).
   >
   > <br/>
   > 
   > Le point de terminaison Profiles valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Poursuivre avec les flux de décisions :** La première application de diffusion en continu peut continuer avec les flux de décisions suivants.

   >[!IMPORTANT]
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AD-Service-Token`, consultez la documentation [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) .

1. **Authentifier avec le service d’identité :** La deuxième application de diffusion en continu appelle le service d’identité, en dehors des systèmes Adobe Pass, pour obtenir la charge utile `JWS` associée à l’identifiant utilisateur unique.

1. **L’identifiant utilisateur unique de retour est JWS :** La deuxième application de diffusion en continu valide les données de réponse afin de s’assurer que les conditions de sécurité de base sont remplies :
   * La charge utile n’a pas expiré.
   * La charge utile est signée.

1. **Récupérer les profils :** La deuxième application de diffusion en continu rassemble toutes les données nécessaires pour récupérer toutes les informations de profil en envoyant une requête au point de terminaison Profiles.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
   >
   > * Tous les paramètres _requis_, comme `serviceProvider`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_
   >
   > <br/>
   > 
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AD-Service-Token`, consultez la documentation [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) .

1. **Rechercher un profil d’authentification unique :** Le serveur Adobe Pass identifie un profil d’authentification unique valide en fonction des paramètres et en-têtes reçus.

1. **Informations de retour sur le profil de connexion unique :** La réponse de point de terminaison Profiles contient des informations sur le profil trouvé associé aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de profil, reportez-vous à la documentation de l’API [Récupérer les profils](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) .
   > 
   > <br/>
   > 
   > Le point de terminaison Profiles valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Poursuivre avec les flux de décisions :** La deuxième application de diffusion en continu peut continuer avec les flux de décisions suivants.

   >[!IMPORTANT]
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AD-Service-Token`, consultez la documentation [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) .

## Récupération des décisions d’autorisation par authentification unique à l’aide du jeton de service {#performing-authorization-flow-using-service-token-single-sign-on-method}

### Conditions préalables {#prerequisites-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Avant d’exécuter le flux d’autorisation via l’authentification unique à l’aide d’un jeton de service, assurez-vous que les conditions préalables suivantes sont remplies :

* Le service d’identité externe doit renvoyer des informations cohérentes sous la forme d’une payload `JWS` sur toutes les applications sur plusieurs appareils et plateformes.
* La première application de diffusion en continu doit récupérer l’identifiant utilisateur unique et inclure la charge utile `JWS` dans l’en-tête [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) pour toutes les requêtes qui la spécifient.
* La seconde application de diffusion en continu doit récupérer une décision d’autorisation avant de lire une ressource sélectionnée par l’utilisateur.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * La première application de diffusion en continu a effectué une authentification et a inclus une valeur valide pour l’en-tête de requête [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

### Workflow {#workflow-steps-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Exécutez les étapes données pour mettre en oeuvre le flux d’autorisation par authentification unique à l’aide d’un jeton de service, comme illustré dans le diagramme ci-dessous.

![Récupérer les décisions d’autorisation par authentification unique à l’aide du jeton de service](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-service-token-flow.png)

*Récupérer les décisions d’autorisation par authentification unique à l’aide du jeton de service*

1. **Authentifier avec le service d’identité :** La deuxième application de diffusion en continu appelle le service d’identité, en dehors des systèmes Adobe Pass, pour obtenir la charge utile `JWS` associée à l’identifiant utilisateur unique.

1. **L’identifiant utilisateur unique de retour est JWS :** La deuxième application de diffusion en continu valide les données de réponse afin de s’assurer que les conditions de sécurité de base sont remplies :
   * La charge utile n’a pas expiré.
   * La charge utile est signée.

1. **Récupérer la décision d’autorisation :** La deuxième application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point de terminaison Decisions Authorize.

   >[!IMPORTANT]
   >
   > Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique :
   >
   > * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_
   >
   > <br/>
   > 
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AD-Service-Token`, consultez la documentation [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) .

1. **Rechercher un profil d’authentification unique :** Le serveur Adobe Pass identifie un profil d’authentification unique valide en fonction des paramètres et en-têtes reçus.

1. **Récupérer la décision MVPD pour la ressource demandée :** Le serveur Adobe Pass appelle le point de terminaison d’autorisation MVPD pour obtenir une décision `Permit` ou `Deny` pour la ressource spécifique reçue de l’application de diffusion en continu.

1. **Renvoi de la décision `Permit` avec jeton multimédia :** La réponse de point de terminaison Decisions Authorize contient une décision `Permit` et un jeton multimédia.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse à une décision, reportez-vous à la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique.
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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Commencer le flux avec le jeton multimédia :** La deuxième application de diffusion en continu utilise le jeton multimédia pour lire le contenu.

1. **Renvoi de la décision `Deny` avec des détails :** La réponse de point de terminaison Decisions Authorize contient une décision `Deny` et un payload d’erreur qui respectent la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse à une décision, reportez-vous à la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique.
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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Gérer les `Deny` détails de décision :** La deuxième application de diffusion en continu traite les informations d’erreur de la réponse et peut les utiliser pour afficher éventuellement un message spécifique dans l’interface utilisateur.

>[!NOTE]
>
> Les étapes du flux de préautorisation sont identiques à celles du flux d’autorisation, sauf que le point de terminaison utilisé est celui décrit dans la documentation [Récupérer les décisions de préautorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) spécifique.
