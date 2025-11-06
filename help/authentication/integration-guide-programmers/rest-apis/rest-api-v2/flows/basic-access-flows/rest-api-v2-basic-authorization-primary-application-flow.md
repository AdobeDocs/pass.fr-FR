---
title: Autorisation de base - Demande de Principal - Flux
description: API REST V2 - Autorisation de base - Application de Principal - Flux
exl-id: 46bc9326-966e-44fc-8546-2f58be01b7bc
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---

# Flux d’autorisation de base exécuté dans l’application principale {#basic-authorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

Le **flux d’autorisation** dans les droits d’authentification Adobe Pass permet à l’application de diffusion en continu de déterminer si un MVPD autorise ou refuse la demande de l’utilisateur de diffuser du contenu. Si la décision est `Permit`, la réponse inclut un jeton de média. Le serveur Adobe Pass signe le jeton de média et permet à l’application de diffusion en continu d’utiliser la bibliothèque du vérificateur de jeton de média pour vérifier son authenticité avant que le flux ne soit publié.

La vérification avec la bibliothèque du vérificateur de jeton de média doit se produire sur le service principal de l’application de diffusion en continu lié dans la chaîne d’autorisations pour la publication d’un flux à partir du réseau CDN.

## Récupérer des décisions d’autorisation à l’aide de mvpd spécifiques {#retrieve-authorization-decisions-using-specific-mvpd}

### Conditions préalables {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

Avant de récupérer des décisions d’autorisation à l’aide d’un MVPD spécifique, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit avoir un profil standard valide qui a été créé avec succès pour le MVPD à l’aide de l’un des flux d’authentification de base :
   * [Authentification dans l’application principale](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentification dans l’application secondaire avec mvpd présélectionné](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Effectuer l’authentification dans l’application secondaire sans mvpd présélectionné](rest-api-v2-basic-authentication-secondary-application-flow.md)
* L’application de diffusion en continu doit récupérer une décision d’autorisation avant de lire une ressource sélectionnée par l’utilisateur.

### Workflow {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

Suivez les étapes données pour implémenter le flux d’autorisation de base à l’aide d’un MVPD spécifique exécuté dans une application principale, comme illustré dans le diagramme ci-dessous.

![Récupérer des décisions d’autorisation à l’aide de mvpd spécifiques](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*Récupérer des décisions d’autorisation à l’aide de mvpd spécifiques*

1. **Récupérer la décision d’autorisation :** l’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point d’entrée Decisions Authorize.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Récupération des décisions d’autorisation à l’aide d’une API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _obligatoires_, tels que `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Rechercher un profil normal :** le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

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

1. **Démarrer le flux avec un jeton de média :** l’application de diffusion en continu utilise le jeton de média pour lire le contenu.

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

1. **Gérer `Deny` détails de décision :** l’application de diffusion en continu traite les informations d’erreur de la réponse et peut les utiliser pour afficher éventuellement un message spécifique sur l’interface utilisateur.
