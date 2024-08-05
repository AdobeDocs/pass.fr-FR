---
title: Authentification unique - Identité de plateforme - Flux
description: API REST V2 - Authentification unique - Identité de plateforme - Flux
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '1793'
ht-degree: 0%

---


# Authentification unique à l’aide des flux d’identités de plateforme {#single-sign-on-platform-identity-full-flows}

La méthode Identité de plateforme permet à plusieurs applications d’utiliser un identifiant de plateforme unique afin d’obtenir une authentification unique (SSO) au niveau de l’appareil ou de la plateforme lors de l’utilisation des services Adobe Pass.

Les applications sont chargées de récupérer la charge utile de l’identifiant de plateforme unique à l’aide de services d’identité ou de bibliothèques spécifiques à un appareil en dehors des systèmes Adobe Pass.

Les applications sont chargées d’inclure cette payload d’identifiant de plateforme unique dans l’en-tête `Adobe-Subject-Token` de toutes les requêtes qui la spécifient.

Pour plus d&#39;informations sur l&#39;en-tête `Adobe-Subject-Token`, consultez la documentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

## Effectuer l’authentification par authentification unique à l’aide de l’identité de plateforme {#perform-authentication-through-single-sign-on-using-platform-identity}

### Conditions préalables {#prerequisites-perform-authentication-through-single-sign-on-using-platform-identity}

Avant d’effectuer le flux d’authentification par authentification unique à l’aide d’une identité de plateforme, assurez-vous que les conditions préalables suivantes sont remplies :

* La plateforme doit fournir un service d’identité ou une bibliothèque qui renvoie des informations cohérentes sous la forme de charge utile `JWS` ou `JWE` sur toutes les applications sur le même appareil ou la même plateforme.
* La première application de diffusion en continu doit récupérer l’identifiant unique de la plateforme et inclure la charge utile `JWS` ou `JWE` dans l’en-tête [ Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) pour toutes les requêtes qui la spécifient.
* La première application de diffusion en continu doit sélectionner un MVPD.
* La première application de diffusion en continu doit lancer une session d’authentification pour se connecter avec le MVPD sélectionné.
* La première application en continu doit s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.
* La seconde application de diffusion en continu doit récupérer l’identifiant unique de la plateforme et inclure la payload `JWS` ou `JWE` dans l’en-tête [ Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) pour toutes les requêtes qui la spécifient.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * La première application de diffusion en continu prend en charge l’interaction de l’utilisateur pour sélectionner un MVPD.
> * La première application de diffusion en continu prend en charge l’interaction de l’utilisateur pour s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.

### Workflow {#workflow-perform-authentication-through-single-sign-on-using-platform-identity}

Effectuez les étapes données pour mettre en oeuvre le flux d’authentification par authentification unique à l’aide d’une identité de plateforme, comme illustré dans le diagramme ci-dessous.

![ Effectuez l’authentification par authentification unique à l’aide de l’identité de la plateforme ](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-platform-identity-flow.png)

*Effectuez l’authentification par authentification unique à l’aide de l’identité de la plateforme*

1. **Récupérer l’identifiant de la plateforme :** La première application de diffusion en continu appelle le service d’identité ou la bibliothèque, en dehors des systèmes Adobe Pass, pour obtenir la charge utile `JWS` ou `JWE` associée à l’identifiant de plateforme unique.

1. **L’identifiant de plateforme unique de retour est JWS ou JWE :** La première application de diffusion en continu valide les données de réponse afin de s’assurer que les conditions de sécurité de base sont remplies :
   * La charge utile n’a pas expiré.
   * La charge utile est signée ou chiffrée.

1. **Créer une session d’authentification :** La première application de diffusion en continu rassemble toutes les données nécessaires pour lancer une session d’authentification en appelant le point de terminaison sessions .

   Pour plus d’informations sur :[](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
   * Tous les paramètres _required_, tels que `serviceProvider`, `mvpd`, `domainName` et `redirectUrl`
   * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

   >[!IMPORTANT]
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant de plateforme unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d&#39;informations sur l&#39;en-tête `Adobe-Subject-Token`, consultez la documentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Indiquez l’action suivante :** La réponse du point de terminaison sessions contient les données nécessaires pour guider la première application de diffusion en continu concernant l’action suivante.

   Pour plus d’informations sur les informations fournies dans une réponse de session, reportez-vous à la documentation de l’API [Créer une session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

   >[!IMPORTANT]
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

   Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md)
   * Tous les paramètres _requis_, comme `serviceProvider`, `code`
   * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

   >[!NOTE]
   >
   > Suggestion : l’application de diffusion en continu peut attendre que l’agent utilisateur atteigne le `redirectUrl` fourni pour vérifier si le profil normal a été généré et enregistré avec succès.

1. **Rechercher un profil régulier :** Le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

1. **Renvoi d’informations sur le profil normal :** La réponse du point de terminaison Profiles contient des informations sur le profil trouvé associé aux paramètres et en-têtes reçus.

   Pour plus d’informations sur les informations fournies dans une réponse de profil, reportez-vous à la documentation de l’API [Récupérer le profil pour un code spécifique](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md).

   >[!IMPORTANT]
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
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant de plateforme unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d&#39;informations sur l&#39;en-tête `Adobe-Subject-Token`, consultez la documentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Récupérer l’identifiant de la plateforme :** La deuxième application de diffusion en continu appelle le service d’identité ou la bibliothèque, en dehors des systèmes Adobe Pass, pour obtenir la charge utile `JWS` ou `JWE` associée à l’identifiant de plateforme unique.

1. **L’identifiant de plateforme unique de retour est JWS ou JWE :** La deuxième application de diffusion en continu valide les données de réponse afin de s’assurer que les conditions de sécurité de base sont remplies :
   * La charge utile n’a pas expiré.
   * La charge utile est signée ou chiffrée.

1. **Récupérer les profils :** La deuxième application de diffusion en continu rassemble toutes les données nécessaires pour récupérer toutes les informations de profil en envoyant une requête au point de terminaison Profiles.

   Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
   * Tous les paramètres _requis_, comme `serviceProvider`
   * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

   >[!IMPORTANT]
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant de plateforme unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d&#39;informations sur l&#39;en-tête `Adobe-Subject-Token`, consultez la documentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Rechercher un profil d’authentification unique :** Le serveur Adobe Pass identifie un profil d’authentification unique valide en fonction des paramètres et en-têtes reçus.

1. **Informations de retour sur le profil de connexion unique :** La réponse de point de terminaison Profiles contient des informations sur le profil trouvé associé aux paramètres et en-têtes reçus.

   Pour plus d’informations sur les informations fournies dans une réponse de profil, reportez-vous à la documentation de l’API [Récupérer les profils](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) .

   >[!IMPORTANT]
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
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant de plateforme unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d&#39;informations sur l&#39;en-tête `Adobe-Subject-Token`, consultez la documentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

## Récupération des décisions d’autorisation par authentification unique à l’aide de l’identité de plateforme{#performing-authorization-flow-using-platform-identity-single-sign-on-method}

### Conditions préalables {#prerequisites-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Avant d’exécuter le flux d’autorisation via l’authentification unique à l’aide d’une identité de plateforme, assurez-vous que les conditions préalables suivantes sont remplies :

* La plateforme doit fournir un service d’identité ou une bibliothèque qui renvoie des informations cohérentes sous la forme de charge utile `JWS` ou `JWE` sur toutes les applications sur le même appareil ou la même plateforme.
* La seconde application de diffusion en continu doit récupérer l’identifiant unique de la plateforme et inclure la payload `JWS` ou `JWE` dans l’en-tête [ Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) pour toutes les requêtes qui la spécifient.
* La seconde application de diffusion en continu doit récupérer une décision d’autorisation avant de lire une ressource sélectionnée par l’utilisateur.

>[!IMPORTANT]
>
> Hypothèses
> 
> <br/>
> 
> * La première application de diffusion en continu a effectué une authentification et a inclus une valeur valide pour l’en-tête de requête [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

### Workflow {#workflow-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Exécutez les étapes données pour mettre en oeuvre le flux d’autorisation par authentification unique à l’aide d’une identité de plateforme, comme illustré dans le diagramme ci-dessous.

![Récupérer les décisions d’autorisation par authentification unique à l’aide de l’identité de plateforme](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-platform-identity-flow.png)

*Récupérer les décisions d’autorisation par authentification unique à l’aide de l’identité de plateforme*

1. **Récupérer l’identifiant de la plateforme :** La deuxième application de diffusion en continu appelle le service d’identité ou la bibliothèque, en dehors des systèmes Adobe Pass, pour obtenir la charge utile `JWS` ou `JWE` associée à l’identifiant de plateforme unique.

1. **L’identifiant de plateforme unique de retour est JWS ou JWE :** La deuxième application de diffusion en continu valide les données de réponse afin de s’assurer que les conditions de sécurité de base sont remplies :
   * La charge utile n’a pas expiré.
   * La charge utile est signée ou chiffrée.

1. **Récupérer la décision d’autorisation :** La deuxième application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point de terminaison Decisions Authorize.

   Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique :
   * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

   >[!IMPORTANT]
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant de plateforme unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d&#39;informations sur l&#39;en-tête `Adobe-Subject-Token`, consultez la documentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Rechercher un profil d’authentification unique :** Le serveur Adobe Pass identifie un profil d’authentification unique valide en fonction des paramètres et en-têtes reçus.

1. **Récupérer la décision MVPD pour la ressource demandée :** Le serveur Adobe Pass appelle le point de terminaison d’autorisation MVPD pour obtenir une décision `Permit` ou `Deny` pour la ressource spécifique reçue de l’application de diffusion en continu.

1. **Renvoi de la décision `Permit` avec jeton multimédia :** La réponse de point de terminaison Decisions Authorize contient une décision `Permit` et un jeton multimédia.

   Pour plus d’informations sur les informations fournies dans une réponse à une décision, reportez-vous à la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique.

   >[!IMPORTANT]
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

   Pour plus d’informations sur les informations fournies dans une réponse à une décision, reportez-vous à la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique.

   >[!IMPORTANT]
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
