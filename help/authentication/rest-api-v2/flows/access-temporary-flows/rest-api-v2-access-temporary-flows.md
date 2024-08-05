---
title: Flux d’accès temporaires
description: API REST V2 - Flux d’accès temporaire
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '3173'
ht-degree: 0%

---


# Flux d’accès temporaires {#temporary-access-flows}

TempPass permet aux programmeurs de fournir un accès temporaire à leur contenu protégé sans demander aux utilisateurs de s’authentifier avec un compte MVPD valide.

Pour plus d’informations sur la fonctionnalité TempPass, consultez la documentation [TempPass](../../../temp-pass.md).

Les flux d’accès temporaires vous permettent d’effectuer des requêtes pour les scénarios suivants :

* [Récupération des décisions d’autorisation à l’aide de TempPass de base](#retrieve-authorization-decisions-using-basic-temppass)
* [Récupérer les décisions d’autorisation à l’aide de TempPass promotionnel](#retrieve-authorization-decisions-using-promotional-temppass)
* [Consommer le nombre maximal de ressources à l’aide de TempPass promotionnel](#consume-maximum-number-of-resources-using-promotional-temppass)
* [Récupérer les décisions d’autorisation à l’expiration d’un TempPass de base ou de promotion](#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires)
* [Récupération du profil pour TempPass de base](#retrieve-profile-for-basic-temppass)
* [Récupération du profil pour le TempPass promotionnel](#retrieve-profile-for-promotional-temppass)

## Récupération des décisions d’autorisation à l’aide de TempPass de base {#retrieve-authorization-decisions-using-basic-temppass}

### Conditions préalables {#prerequisites-retrieve-authorization-decisions-using-basic-temppass}

Avant de récupérer les décisions d’autorisation à l’aide de TempPass de base, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu souhaite fournir un accès temporaire pour lire le contenu sans demander à l’utilisateur de s’authentifier.
* L’application en continu doit récupérer une décision d’autorisation avant de lire une ressource sélectionnée par l’utilisateur.

>[!IMPORTANT]
>
> Hypothèses
> 
> <br/>
> 
> * Il doit y avoir une configuration valide de TempPass de base appliquée à l&#39;intégration entre les `serviceProvider` et `mvpd` fournis.
> * La durée de vie (TTL) configurée pour le TempPass de base n’a pas expiré.

### Workflow {#workflow-retrieve-authorization-decisions-using-basic-temppass}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux d’autorisation à l’aide de TempPass de base, comme illustré dans le diagramme ci-dessous.

![ Récupérer les décisions d’autorisation à l’aide de TempPass de base ](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-retrieve-authorization-decisions-using-basic-temppass-flow.png)

*Récupérer les décisions d’autorisation à l’aide de TempPass de base*

1. **Récupérer la décision d’autorisation :** L’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point de terminaison Decisions Authorize.

   Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique :
   * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

1. **Validez TempPass de base :** Le serveur Adobe Pass vérifie si une configuration de base TempPass est appliquée à l’intégration entre les `serviceProvider` et `mvpd` fournis.

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
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > Le point de terminaison Decisions Authorize utilise les données de requête pour vérifier si les conditions d’accès temporaires sont remplies :
   >
   > * La durée de vie (TTL) configurée pour le TempPass de base ne doit pas être expirée.
   >
   > <br/>
   > 
   > Si la validation temporaire de l’accès échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Démarrer le flux avec le jeton multimédia :** L’application de diffusion en continu utilise le jeton multimédia pour lire le contenu.

## Récupérer les décisions d’autorisation à l’aide de TempPass promotionnel {#retrieve-authorization-decisions-using-promotional-temppass}

### Conditions préalables {#prerequisites-retrieve-authorization-decisions-using-promotional-temppass}

Avant de récupérer les décisions d’autorisation à l’aide de la fonction promotionnelle TempPass, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu souhaite fournir un accès temporaire afin de lire un nombre maximum de ressources sans demander à l’utilisateur de s’authentifier.
* L’application de diffusion en continu doit inclure des informations uniques sur l’identité de l’utilisateur lors de la récupération d’une décision d’autorisation.
* L’application en continu doit récupérer une décision d’autorisation avant de lire une ressource sélectionnée par l’utilisateur.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * Une configuration valide de TempPass promotionnel doit être appliquée à l&#39;intégration entre le `serviceProvider` et le `mvpd` fourni.
> * La durée de vie (TTL) configurée pour le TempPass promotionnel n’a pas expiré.
> * Le nombre maximal de ressources configurées pour le TempPass promotionnel n’a pas été utilisé.

### Workflow {#workflow-retrieve-authorization-decisions-using-promotional-temppass}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux d’autorisation à l’aide de TempPass promotionnel, comme illustré dans le diagramme ci-dessous.

![ Récupérer les décisions d’autorisation à l’aide de la fonction promotionnelle TempPass](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-retrieve-authorization-decisions-using-promotional-temppass-flow.png)

*Récupérer les décisions d’autorisation à l’aide de la fonction promotionnelle TempPass*

1. **Récupérer la décision d’autorisation :** L’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point de terminaison Decisions Authorize.

   Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique :
   * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

   >[!IMPORTANT]
   >
   > Le point de terminaison Decisions Authorize requiert la présence de l’en-tête `AP-TempPass-Identity` lors de l’utilisation de TempPass promotionnel. L’en-tête comprend des informations uniques sur l’identité de l’utilisateur accédant au contenu.
   > 
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AP-TempPass-Identity`, consultez la documentation [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) .

1. **Valider le TempPass promotionnel :** Le serveur Adobe Pass vérifie si une configuration valide du TempPass promotionnel est appliquée à l’intégration entre les `serviceProvider` et `mvpd` fournis.

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
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > Le point de terminaison Decisions Authorize utilise les données de requête pour vérifier si les conditions d’accès temporaires sont remplies :
   >
   > * La durée de vie (TTL) configurée pour le TempPass promotionnel ne doit pas être expirée.
   > * Le nombre maximal de ressources configurées pour le TempPass promotionnel ne doit pas être utilisé.
   >
   > <br/>
   > 
   > Si la validation temporaire de l’accès échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Démarrer le flux avec le jeton multimédia :** L’application de diffusion en continu utilise le jeton multimédia pour lire le contenu.

## Consommer le nombre maximal de ressources à l’aide de TempPass promotionnel {#consume-maximum-number-of-resources-using-promotional-temppass}

### Conditions préalables {#prerequisites-consume-maximum-number-of-resources-using-promotional-temppass}

Avant d’utiliser un nombre maximum de ressources à l’aide de TempPass promotionnel, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu souhaite fournir un accès temporaire afin de lire un nombre maximum de ressources sans demander à l’utilisateur de s’authentifier.
* L’application de diffusion en continu doit inclure des informations uniques sur l’identité de l’utilisateur lors de la récupération d’une décision d’autorisation.
* L’application en continu doit récupérer une décision d’autorisation avant de lire une ressource sélectionnée par l’utilisateur.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * Une configuration valide de TempPass promotionnel doit être appliquée à l&#39;intégration entre le `serviceProvider` et le `mvpd` fourni.
> * La durée de vie (TTL) configurée pour le TempPass promotionnel n’a pas expiré.
> * Le nombre maximal de ressources configurées pour le TempPass promotionnel est de 1.

### Workflow {#workflow-consume-maximum-number-of-resources-using-promotional-temppass}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux d’autorisation lors de l’utilisation d’un nombre maximum de ressources à l’aide de TempPass promotionnel, comme illustré dans le diagramme ci-dessous.

![ Consommez le nombre maximal de ressources à l’aide de la promotion TempPass](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-consume-maximum-number-of-resources-using-promotional-temppass-flow.png)

*Consommez le nombre maximal de ressources à l’aide de la promotion TempPass*

1. **Récupérer le profil pour le TempPass promotionnel :** L’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil pour le TempPass promotionnel en envoyant une requête au point de terminaison Profils .

   Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md)
   * Tous les paramètres _required_, comme `serviceProvider` et `mvpd`
   * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

   >[!IMPORTANT]
   >
   > La requête de point de terminaison Profiles est facultative et peut être utilisée pour déterminer le nombre de ressources qui peuvent encore être lues à l’aide du TempPass promotionnel.

1. **Valider le TempPass promotionnel :** Le serveur Adobe Pass vérifie si une configuration valide du TempPass promotionnel est appliquée à l’intégration entre les `serviceProvider` et `mvpd` fournis.

1. **Informations de retour sur le profil temporaire :** La réponse du point de terminaison Profiles contient des informations sur le profil temporaire, y compris l’attribut `type` défini sur &quot;temporaire&quot;.

   Pour plus d’informations sur les informations fournies dans une réponse de profil, reportez-vous à la documentation de l’API [Retrieve profile for specific mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) .

   >[!IMPORTANT]
   >
   > Le point de terminaison Profiles valide les données de requête pour s’assurer que les conditions de base sont remplies :
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
   > Le point de terminaison Profils utilise les données de requête pour vérifier si les conditions d’accès temporaires sont remplies :
   >
   > * La durée de vie (TTL) configurée pour le TempPass promotionnel ne doit pas être expirée.
   > * Le nombre maximal de ressources configurées pour le TempPass promotionnel ne doit pas être utilisé.
   >
   > <br/>
   > 
   > Si la validation temporaire de l’accès échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Poursuivre avec les flux de décisions :** Si la réponse du point de terminaison Profiles contient un profil, l’application en continu utilise les informations de profil temporaires pour continuer avec les flux de décisions suivants.

1. **Récupérer la décision d’autorisation :** L’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point de terminaison Decisions Authorize.

   Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique :
   * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

   >[!IMPORTANT]
   >
   > Le point de terminaison Decisions Authorize requiert la présence de l’en-tête `AP-TempPass-Identity` lors de l’utilisation de TempPass promotionnel. L’en-tête comprend des informations uniques sur l’identité de l’utilisateur accédant au contenu.
   > 
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AP-TempPass-Identity`, consultez la documentation [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) .

1. **Valider le TempPass promotionnel :** Le serveur Adobe Pass vérifie si une configuration valide du TempPass promotionnel est appliquée à l’intégration entre les `serviceProvider` et `mvpd` fournis.

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
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).
   > 
   > <br/>
   > 
   > Le point de terminaison Decisions Authorize utilise les données de requête pour vérifier si les conditions d’accès temporaires sont remplies :
   >
   > * La durée de vie (TTL) configurée pour le TempPass promotionnel ne doit pas être expirée.
   > * Le nombre maximal de ressources configurées pour le TempPass promotionnel ne doit pas être utilisé.
   >
   > <br/>
   > 
   > Si la validation temporaire de l’accès échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Récupérer la décision d’autorisation :** L’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point de terminaison Decisions Authorize.

   Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique :
   * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

   >[!IMPORTANT]
   >
   > Le point de terminaison Decisions Authorize requiert la présence de l’en-tête `AP-TempPass-Identity` lors de l’utilisation de TempPass promotionnel. L’en-tête comprend des informations uniques sur l’identité de l’utilisateur accédant au contenu.
   >
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AP-TempPass-Identity`, consultez la documentation [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) .

1. **Valider le TempPass promotionnel :** Le serveur Adobe Pass vérifie si une configuration valide du TempPass promotionnel est appliquée à l’intégration entre les `serviceProvider` et `mvpd` fournis.

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
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > Le point de terminaison Decisions Authorize utilise les données de requête pour vérifier si les conditions d’accès temporaires sont remplies :
   >
   > * La durée de vie (TTL) configurée pour le TempPass promotionnel ne doit pas être expirée.
   > * Le nombre maximal de ressources configurées pour le TempPass promotionnel ne doit pas être utilisé.
   >
   > <br/>
   > 
   > Si la validation temporaire de l’accès échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Gérer les `Deny` détails de décision :** L’application en continu traite les informations d’erreur de la réponse et peut les utiliser pour afficher éventuellement un message spécifique dans l’interface utilisateur.

   >[!NOTE]
   >
   > Suggestion : l’application en continu peut informer les utilisateurs que le nombre maximum de ressources a été dépassé et leur conseiller de lancer un flux d’authentification de base à l’aide d’un MVPD standard pour continuer à regarder.

## Récupérer les décisions d’autorisation à l’expiration d’un TempPass de base ou de promotion {#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

### Conditions préalables {#prerequisites-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

Avant de récupérer les décisions d’autorisation à l’expiration d’un TempPass de base ou de promotion, assurez-vous que les conditions préalables suivantes sont remplies :

* [ Conditions préalables avant de récupérer les décisions d’autorisation à l’aide de TempPass](#prerequisites-retrieve-authorization-decisions-using-basic-temppass) de base.
* [ Conditions préalables avant de récupérer les décisions d’autorisation à l’aide de TempPass](#prerequisites-retrieve-authorization-decisions-using-promotional-temppass) promotionnel.

>[!IMPORTANT]
>
> Hypothèses
> 
> <br/>
> 
> * Une configuration valide de TempPass de base ou promotionnel doit être appliquée à l&#39;intégration entre les `serviceProvider` et `mvpd` fournis.
> * La durée de vie (TTL) configurée pour le TempPass de base ou promotionnel a expiré.

### Workflow {#workflow-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux d’autorisation à l’expiration de TempPass de base ou promotionnel, comme illustré dans le diagramme ci-dessous.

![Récupérer les décisions d’autorisation à l’expiration d’un TempPass de base ou d’opération promotionnelle](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires-flow.png)

*Récupérer les décisions d’autorisation à l’expiration d’un TempPass de base ou d’opération promotionnelle*

1. **Récupérer la décision d’autorisation :** L’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point de terminaison Decisions Authorize.

   Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique :
   * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

   >[!IMPORTANT]
   >
   > Le point de terminaison Decisions Authorize requiert la présence de l’en-tête `AP-TempPass-Identity` lors de l’utilisation de TempPass promotionnel. L’en-tête comprend des informations uniques sur l’identité de l’utilisateur accédant au contenu.
   > 
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AP-TempPass-Identity`, consultez la documentation [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) .

1. **Validez TempPass de base ou promotionnel :** Le serveur Adobe Pass vérifie si une configuration valide de TempPass de base ou promotionnel est appliquée à l’intégration entre les `serviceProvider` et `mvpd` fournis.

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
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > Le point de terminaison Decisions Authorize utilise les données de requête pour vérifier si les conditions d’accès temporaires sont remplies :
   >
   > * La durée de vie (TTL) configurée pour le TempPass de base ou promotionnel ne doit pas être expirée.
   > * Le nombre maximal de ressources configurées pour le TempPass promotionnel ne doit pas être utilisé.
   >
   > <br/>
   > 
   > Si la validation temporaire de l’accès échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Gérer les `Deny` détails de décision :** L’application en continu traite les informations d’erreur de la réponse et peut les utiliser pour afficher éventuellement un message spécifique dans l’interface utilisateur.

   >[!NOTE]
   >
   > Suggestion : l’application de diffusion en continu peut informer les utilisateurs que l’accès temporaire a expiré et leur conseiller de lancer un flux d’authentification de base à l’aide d’un MVPD standard pour continuer à regarder.

## Récupération du profil pour TempPass de base {#retrieve-profile-for-basic-temppass}

>[!IMPORTANT]
>
> La requête Profiles endpoint est facultative pour TempPass de base.

### Conditions préalables {#prerequisites-retrieve-profile-for-basic-temppass}

Avant de récupérer le profil pour TempPass de base, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu souhaite récupérer le profil temporaire afin de s’assurer que l’accès temporaire n’a pas expiré.

>[!IMPORTANT]
>
> Hypothèses
> 
> <br/>
> 
> * Il doit y avoir une configuration valide de TempPass de base appliquée à l&#39;intégration entre les `serviceProvider` et `mvpd` fournis.
> * La durée de vie (TTL) configurée pour le TempPass de base ne doit pas être expirée.

### Workflow {#workflow-retrieve-profile-information-for-basic-temppass}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux de récupération des profils pour TempPass de base, comme illustré dans le diagramme ci-dessous.

![Récupération du profil pour le TempPass de base](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-retrieve-profile-for-basic-temppass-flow.png)

*Récupération du profil pour le TempPass de base*

1. **Récupérer le profil pour TempPass de base :** L’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil pour TempPass de base en envoyant une requête au point de terminaison Profils .

   Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md)
   * Tous les paramètres _required_, comme `serviceProvider` et `mvpd`
   * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

1. **Validez TempPass de base :** Le serveur Adobe Pass vérifie si une configuration de base TempPass est appliquée à l’intégration entre les `serviceProvider` et `mvpd` fournis.

1. **Informations de retour sur le profil temporaire :** La réponse du point de terminaison Profiles contient des informations sur le profil temporaire, y compris l’attribut `type` défini sur &quot;temporaire&quot;.

   Pour plus d’informations sur les informations fournies dans une réponse de profil, reportez-vous à la documentation de l’API [Retrieve profile for specific mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) .

   >[!IMPORTANT]
   >
   > Le point de terminaison Profiles valide les données de requête pour s’assurer que les conditions de base sont remplies :
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
   > Le point de terminaison Profils utilise les données de requête pour vérifier si les conditions d’accès temporaires sont remplies :
   >
   > * La durée de vie (TTL) configurée pour le TempPass de base ne doit pas être expirée.
   >
   > <br/>
   > 
   > Si la validation temporaire de l’accès échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Poursuivre avec les flux de décisions :** Si la réponse du point de terminaison Profiles contient un profil, l’application en continu utilise les informations de profil temporaires pour continuer avec les flux de décisions suivants.

## Récupération du profil pour le TempPass promotionnel {#retrieve-profile-for-promotional-temppass}

>[!IMPORTANT]
>
> La requête Profiles endpoint est facultative pour le TempPass promotionnel.

### Conditions préalables {#prerequisites-retrieve-profile-for-promotional-temppass}

Avant de récupérer le profil pour promotionnel TempPass, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu souhaite récupérer le profil temporaire afin de s’assurer que l’accès temporaire n’a pas expiré ou de déterminer le nombre de ressources pouvant encore être lues.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * Une configuration valide de TempPass promotionnel doit être appliquée à l&#39;intégration entre le `serviceProvider` et le `mvpd` fourni.
> * La durée de vie (TTL) configurée pour le TempPass promotionnel n’a pas expiré.
> * Le nombre maximal de ressources configurées pour le TempPass promotionnel n’a pas été utilisé.

### Workflow {#workflow-retrieve-profile-information-for-promotional-temppass}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux de récupération des profils pour le modèle promotionnel TempPass, comme illustré dans le diagramme ci-dessous.

![Récupérer le profil pour la promotion TempPass](../../../assets/rest-api-v2/flows/access-temporary-flows/rest-api-v2-retrieve-profile-for-promotional-temppass-flow.png)

*Récupérer le profil pour la promotion TempPass*

1. **Récupérer le profil pour le TempPass promotionnel :** L’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil pour le TempPass promotionnel en envoyant une requête au point de terminaison Profils .

   Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md)
   * Tous les paramètres _required_, comme `serviceProvider` et `mvpd`
   * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

1. **Valider le TempPass promotionnel :** Le serveur Adobe Pass vérifie si une configuration valide du TempPass promotionnel est appliquée à l’intégration entre les `serviceProvider` et `mvpd` fournis.

1. **Informations de retour sur le profil temporaire :** La réponse du point de terminaison Profiles contient des informations sur le profil temporaire, y compris l’attribut `type` défini sur &quot;temporaire&quot;.

   Pour plus d’informations sur les informations fournies dans une réponse de profil, reportez-vous à la documentation de l’API [Retrieve profile for specific mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) .

   >[!IMPORTANT]
   >
   > Le point de terminaison Profiles valide les données de requête pour s’assurer que les conditions de base sont remplies :
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
   > Le point de terminaison Profils utilise les données de requête pour vérifier si les conditions d’accès temporaires sont remplies :
   >
   > * La durée de vie (TTL) configurée pour le TempPass promotionnel ne doit pas être expirée.
   > * Le nombre maximal de ressources configurées pour le TempPass promotionnel ne doit pas être utilisé.
   >
   > <br/>
   > 
   > Si la validation temporaire de l’accès échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Poursuivre avec les flux de décisions :** Si la réponse du point de terminaison Profiles contient un profil, l’application en continu utilise les informations de profil temporaires pour continuer avec les flux de décisions suivants.
