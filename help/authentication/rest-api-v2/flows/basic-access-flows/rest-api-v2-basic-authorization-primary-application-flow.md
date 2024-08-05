---
title: Autorisation de base - Application de Principal - Flux
description: API REST V2 - Autorisation de base - Application de Principal - Flux
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---


# Flux d’autorisation de base exécuté sur une application principale {#basic-authorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

Le **flux d’autorisation** dans le droit d’authentification Adobe Pass permet à l’application de diffusion en continu de déterminer si un MVPD autorise ou refuse la demande de l’utilisateur pour diffuser du contenu. Si la décision est `Permit`, la réponse inclut un jeton multimédia. Le serveur Adobe Pass signe le jeton multimédia et permet à l’application de diffusion en continu d’utiliser la bibliothèque de vérification de jeton multimédia pour vérifier son authenticité avant que le flux ne soit publié.

La vérification avec la bibliothèque du vérificateur de jeton multimédia doit avoir lieu sur le service principal de l’application de diffusion en continu lié dans la chaîne d’autorisations pour publier un flux à partir du réseau de diffusion de contenu.

## Récupération des décisions d’autorisation à l’aide de mvpd spécifique {#retrieve-authorization-decisions-using-specific-mvpd}

### Conditions préalables {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

Avant de récupérer les décisions d’autorisation à l’aide d’un MVPD spécifique, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit avoir un profil régulier valide qui a été créé avec succès pour le MVPD à l’aide de l’un des flux d’authentification de base :
   * [Effectuer l’authentification dans l’application principale](./rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Effectuez l’authentification dans une application secondaire avec mvpd présélectionné.](./rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Effectuer une authentification dans une application secondaire sans mvpd présélectionné](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* L’application en continu doit récupérer une décision d’autorisation avant de lire une ressource sélectionnée par l’utilisateur.

### Workflow {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

Suivez les étapes données pour mettre en oeuvre le flux d’autorisation de base à l’aide d’un MVPD spécifique exécuté dans une application principale, comme illustré dans le diagramme suivant.

![Récupérer les décisions d’autorisation à l’aide de mvpd spécifique](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*Récupérer les décisions d’autorisation à l’aide de mvpd spécifique*

1. **Récupérer la décision d’autorisation :** L’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point de terminaison Decisions Authorize.

   >[!IMPORTANT]
   >
   > Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions d’autorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique :
   >
   > * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_

1. **Rechercher un profil régulier :** Le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

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

1. **Démarrer le flux avec le jeton multimédia :** L’application de diffusion en continu utilise le jeton multimédia pour lire le contenu.

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

1. **Gérer les `Deny` détails de décision :** L’application en continu traite les informations d’erreur de la réponse et peut les utiliser pour afficher éventuellement un message spécifique dans l’interface utilisateur.
