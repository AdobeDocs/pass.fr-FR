---
title: Préautorisation de base - Demande de Principal - Flux
description: API REST V2 - Préautorisation de base - Application de Principal - Flux
exl-id: f557f6c3-d5b2-4ec8-be51-91a90fbd31c0
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# Flux de préautorisation de base exécuté dans l’application principale {#basic-preauthorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

Le **flux de préautorisation** dans les droits d’authentification Adobe Pass permet à l’application de diffusion en continu de déterminer si un MVPD peut autoriser ou refuser l’accès d’un utilisateur à une liste de ressources. Cette vérification permet de s’assurer que l’application peut présenter des informations précises à l’utilisateur ou à l’utilisatrice sur le contenu qu’il ou elle peut consulter.

## Récupérer des décisions de préautorisation à l’aide de mvpd spécifiques {#retrieve-preauthorization-decisions-using-specific-mvpd}

### Conditions préalables {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

Avant de récupérer des décisions de préautorisation à l’aide d’un MVPD spécifique, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit avoir un profil standard valide qui a été créé avec succès pour le MVPD à l’aide de l’un des flux d’authentification de base :
   * [Authentification dans l’application principale](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentification dans l’application secondaire avec mvpd présélectionné](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Effectuer l’authentification dans l’application secondaire sans mvpd présélectionné](rest-api-v2-basic-authentication-secondary-application-flow.md)
* L’application de diffusion en continu souhaite récupérer les décisions de préautorisation pour afficher une liste de ressources avec leurs statuts associés.

### Workflow {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

Suivez les étapes données pour implémenter le flux de préautorisation de base à l’aide d’un MVPD spécifique exécuté dans une application principale, comme illustré dans le diagramme ci-dessous.

![Récupérer des décisions de préautorisation à l’aide de mvpd spécifiques](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png)

*Récupérer des décisions de préautorisation à l’aide de mvpd spécifiques*

1. **Récupérer les décisions de préautorisation :** l’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir des décisions de préautorisation pour une liste de ressources en appelant le point d’entrée Decisions Preauthorize.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération des décisions de préautorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) spécifique pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _obligatoires_, tels que `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Rechercher un profil normal :** le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

1. **Récupérer les décisions MVPD pour les ressources demandées :** le serveur Adobe Pass appelle le point d’entrée de préautorisation MVPD pour obtenir une décision `Permit` ou `Deny` pour chaque ressource reçue de l’application de diffusion en continu.

1. **Renvoyer les décisions de préautorisation :** la réponse de point d’entrée Decisions Preauthorize contient une décision `Permit` ou `Deny` pour chaque ressource :
   * Une décision `Permit` signifie que la ressource est lisible. La réponse n’inclut pas de jeton multimédia, car le flux de préautorisation ne doit pas être utilisé pour lire les ressources.
   * Une décision `Deny` signifie que la ressource n’est pas lisible. La réponse inclut une payload d’erreur conforme à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation [Récupération des décisions de préautorisation à l’aide d’une API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) spécifique pour plus d’informations sur les informations fournies dans une réponse de décision.
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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Gérer les décisions de préautorisation :** l’application de diffusion en continu traite la réponse et peut l’utiliser pour afficher éventuellement le statut approprié pour chaque ressource sur l’interface utilisateur.
