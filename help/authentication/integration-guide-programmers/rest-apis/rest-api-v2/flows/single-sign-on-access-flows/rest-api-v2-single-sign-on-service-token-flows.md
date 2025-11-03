---
title: Authentification Single Sign-On - Jeton De Service - Flux
description: API REST V2 - Authentification unique - Jeton de service - Flux
exl-id: b0082d2a-e491-4cb5-bb40-35ba10db6b1a
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '1857'
ht-degree: 0%

---

# Authentification unique à l’aide des flux de jeton de service{#single-sign-on-service-token-full-flows}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Veillez également à consulter la [FAQ sur l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

La méthode du jeton de service permet à plusieurs applications d’utiliser un identifiant utilisateur unique pour obtenir l’authentification unique (SSO) sur plusieurs appareils et plateformes lors de l’utilisation des services Adobe Pass.

Les applications sont chargées de récupérer la payload d’identifiant utilisateur unique à l’aide de services d’identité externes, en dehors des systèmes Adobe Pass, par exemple :

* Un service de client direct (DTC), où les utilisateurs se connectent sur chaque appareil à l’aide des mêmes informations d’identification et sont associés au même ID utilisateur ou au même nom de compte utilisateur.
* Service d’authentification tiers, tel que Google ou Facebook, où les utilisateurs se connectent sur chaque appareil avec les mêmes informations d’identification et sont associés à la même adresse e-mail.

Les applications sont chargées d’inclure ce payload d’identifiant utilisateur unique dans l’en-tête `AD-Service-Token` pour toutes les requêtes qui le spécifient.

Pour plus d’informations sur `AD-Service-Token`’en-tête , consultez la documentation d’ [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

## Authentification par authentification unique à l’aide du jeton de service {#performing-authentication-flow-using-service-token-single-sign-on-method}

### Conditions préalables {#prerequisites-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Avant d’effectuer le flux d’authentification par authentification unique à l’aide d’un jeton de service, assurez-vous que les conditions préalables suivantes sont remplies :

* Le service d’identités externes doit renvoyer des informations cohérentes en tant que payload `JWS` dans toutes les applications sur plusieurs appareils et plateformes.
* La première application de diffusion en continu doit récupérer l’identifiant utilisateur unique et inclure la payload `JWS` dans l’en-tête [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) pour toutes les requêtes qui le spécifient.
* La première application de diffusion en continu doit sélectionner un MVPD.
* La première application de diffusion en continu doit lancer une session d’authentification pour se connecter avec le MVPD sélectionné.
* La première application en flux continu doit s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.
* La deuxième application de diffusion en continu doit récupérer l’identifiant utilisateur unique et inclure la payload `JWS` dans l’en-tête [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) pour toutes les requêtes qui le spécifient.

>[!IMPORTANT]
>
> Hypothèses
> 
> <br/>
> 
> * La première application de diffusion en continu prend en charge l’interaction utilisateur pour sélectionner un MVPD.
> * La première application de diffusion en continu prend en charge l’interaction utilisateur pour l’authentification avec le MVPD sélectionné dans un agent utilisateur.

### Workflow {#workflow-steps-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Suivez les étapes données pour implémenter le flux d’authentification par authentification unique à l’aide d’un jeton de service, comme illustré dans le diagramme suivant.

![Authentification par authentification unique à l’aide du jeton de service](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-service-token-flow.png)

*Authentification par authentification unique à l’aide du jeton de service*

1. **S’authentifier avec le service d’identités :** la première application de diffusion en continu appelle le service d’identités, en dehors des systèmes Adobe Pass, pour obtenir la payload `JWS` associée à l’identifiant utilisateur unique.

1. **Renvoyer l’identifiant utilisateur unique en tant que JWS :** la première application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de sécurité de base sont remplies :
   * La payload n’a pas expiré.
   * La payload est signée.

1. **Créer une session d’authentification :** la première application de diffusion en continu rassemble toutes les données nécessaires pour lancer une session d’authentification en appelant le point d’entrée Sessions.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Créer une session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd`, `domainName` et `redirectUrl`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes
   >
   > <br/>
   > 
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur `AD-Service-Token`’en-tête , consultez la documentation d’ [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Indiquez l’action suivante :** la réponse du point d’entrée des sessions contient les données nécessaires pour guider la première application de diffusion en continu concernant l’action suivante.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Créer une session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) pour plus d’informations sur les informations fournies dans une réponse de session.
   >
   > <br/>
   > 
   > Le point d’entrée Sessions valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   > * L’intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Ouvrir l’URL dans l’agent utilisateur :** la réponse de point d’entrée des sessions contient les données suivantes :
   * `url` qui peut être utilisé pour lancer l’authentification interactive dans la page de connexion de MVPD.
   * L’attribut `actionName` est défini sur « authentifier ».
   * L’attribut `actionType` est défini sur « interactif ».

   Si le serveur principal d’Adobe Pass n’identifie pas de profil valide, la première application de diffusion en continu ouvre un agent utilisateur pour charger le `url` fourni, envoyant une requête au point d’entrée Authentifier. Ce flux peut inclure plusieurs redirections et conduire finalement l’utilisateur à la page de connexion de MVPD et fournir des informations d’identification valides.

1. **Terminer l’authentification MVPD :** si le flux d’authentification est réussi, l’interaction de l’agent utilisateur enregistre un profil standard sur le serveur principal Adobe Pass et atteint le `redirectUrl` fourni.

1. **Récupérer le profil pour un code spécifique :** la première application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil en envoyant une requête au point d’entrée Profils .

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupérer le profil pour un code spécifique](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) pour plus d’informations sur :
   > 
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `code`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

   >[!TIP]
   >
   > L’application de diffusion en continu doit attendre que l’agent utilisateur atteigne le `redirectUrl` fourni pour vérifier si le profil standard a bien été généré et enregistré.

1. **Rechercher un profil normal :** le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

1. **Renvoyer des informations sur le profil normal :** la réponse de point d’entrée des profils contient des informations sur le profil trouvé associé aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Récupérer le profil pour un code spécifique](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) pour plus d’informations sur les informations fournies dans une réponse de profil.
   >
   > <br/>
   > 
   > Le point d’entrée des profils valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Poursuivre avec les flux de décisions :** la première application de diffusion en continu peut continuer avec les flux de décisions suivants.

   >[!IMPORTANT]
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur `AD-Service-Token`’en-tête , consultez la documentation d’ [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **S’authentifier avec le service d’identités :** la deuxième application de diffusion en continu appelle le service d’identités, en dehors des systèmes Adobe Pass, pour obtenir la payload `JWS` associée à l’identifiant utilisateur unique.

1. **Renvoyer l’identifiant utilisateur unique en tant que JWS :** la deuxième application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de sécurité de base sont remplies :
   * La payload n’a pas expiré.
   * La payload est signée.

1. **Récupérer les profils :** la deuxième application de diffusion en continu rassemble toutes les données nécessaires pour récupérer toutes les informations de profil en envoyant une requête au point d’entrée Profils .

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération des profils](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ comme `serviceProvider`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes
   >
   > <br/>
   > 
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur `AD-Service-Token`’en-tête , consultez la documentation d’ [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Rechercher un profil d’authentification unique :** le serveur Adobe Pass identifie un profil d’authentification unique valide en fonction des paramètres et des en-têtes reçus.

1. **Renvoyer des informations sur le profil d’authentification unique :** la réponse de point d’entrée des profils contient des informations sur le profil trouvé associé aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Récupération des profils](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) pour plus d’informations sur les informations fournies dans une réponse de profil.
   > 
   > <br/>
   > 
   > Le point d’entrée des profils valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Poursuivre avec les flux de décisions :** la deuxième application de diffusion en continu peut continuer avec les flux de décisions suivants.

   >[!IMPORTANT]
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur `AD-Service-Token`’en-tête , consultez la documentation d’ [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

## Récupérer des décisions d’autorisation par authentification unique à l’aide du jeton de service {#performing-authorization-flow-using-service-token-single-sign-on-method}

### Conditions préalables {#prerequisites-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Avant d’exécuter le flux d’autorisation par authentification unique à l’aide d’un jeton de service, assurez-vous que les conditions préalables suivantes sont remplies :

* Le service d’identités externes doit renvoyer des informations cohérentes en tant que payload `JWS` dans toutes les applications sur plusieurs appareils et plateformes.
* La première application de diffusion en continu doit récupérer l’identifiant utilisateur unique et inclure la payload `JWS` dans l’en-tête [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) pour toutes les requêtes qui le spécifient.
* La deuxième application de diffusion en continu doit récupérer une décision d’autorisation avant de lire une ressource sélectionnée par l’utilisateur.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * La première application de diffusion en continu a effectué l’authentification et a inclus une valeur valide pour l’en-tête de requête [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

### Workflow {#workflow-steps-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Suivez les étapes données pour implémenter le flux d’autorisation par le biais de l’authentification unique à l’aide d’un jeton de service, comme illustré dans le diagramme suivant.

![Récupérer des décisions d’autorisation par authentification unique à l’aide d’un jeton de service](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-service-token-flow.png)

*Récupérer des décisions d’autorisation par authentification unique à l’aide d’un jeton de service*

1. **S’authentifier avec le service d’identités :** la deuxième application de diffusion en continu appelle le service d’identités, en dehors des systèmes Adobe Pass, pour obtenir la payload `JWS` associée à l’identifiant utilisateur unique.

1. **Renvoyer l’identifiant utilisateur unique en tant que JWS :** la deuxième application de diffusion en continu valide les données de réponse pour s’assurer que les conditions de sécurité de base sont remplies :
   * La payload n’a pas expiré.
   * La payload est signée.

1. **Récupérer la décision d’autorisation :** la deuxième application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point d’entrée d’autorisation Decisions.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Récupération des décisions d’autorisation à l’aide d’une API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _obligatoires_, tels que `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes
   >
   > <br/>
   > 
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur `AD-Service-Token`’en-tête , consultez la documentation d’ [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Rechercher un profil d’authentification unique :** le serveur Adobe Pass identifie un profil d’authentification unique valide en fonction des paramètres et des en-têtes reçus.

1. **Récupérer la décision MVPD pour la ressource demandée :** le serveur Adobe Pass appelle le point d’entrée d’autorisation MVPD pour obtenir une décision `Permit` ou `Deny` pour la ressource spécifique reçue de l’application de diffusion en continu.

1. **Renvoyer `Permit` décision avec jeton média :** la réponse de point d’entrée Decisions Authorize contient une décision `Permit` et un jeton média.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Récupérer les décisions d’autorisation à l’aide d’une API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique pour plus d’informations sur les informations fournies dans une réponse de décision.
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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Démarrer le flux avec le jeton de média :** la deuxième application de diffusion en continu utilise le jeton de média pour lire le contenu.

1. **Renvoyer `Deny` décision avec les détails :** la réponse de point d’entrée Decisions Authorize contient une décision `Deny` et une payload d’erreur qui respecte la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Récupérer les décisions d’autorisation à l’aide d’une API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique pour plus d’informations sur les informations fournies dans une réponse de décision.
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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Gérer `Deny` détails de décision :** la deuxième application de diffusion en continu traite les informations d’erreur de la réponse et peut les utiliser pour afficher éventuellement un message spécifique sur l’interface utilisateur.

>[!NOTE]
>
> Les étapes du flux de préautorisation sont identiques à celles du flux d’autorisation, sauf que le point d’entrée utilisé est celui décrit dans la documentation [Récupérer les décisions de préautorisation à l’aide de mvpd spécifique](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md).
