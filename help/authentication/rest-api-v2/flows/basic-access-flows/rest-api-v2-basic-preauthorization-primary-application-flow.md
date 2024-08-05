---
title: Préautorisation de base - Application de Principal - Flux
description: API REST V2 - Préautorisation de base - Application de Principal - Flux
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---


# Flux de préautorisation de base effectué dans une application principale {#basic-preauthorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

Le **flux de préautorisation** dans le droit d’authentification Adobe Pass permet à l’application de diffusion en continu de déterminer si un MVPD peut autoriser ou refuser l’accès de l’utilisateur à une liste de ressources. Cette vérification garantit que l’application peut présenter à l’utilisateur des informations précises sur le contenu qu’il peut être autorisé à afficher.

## Récupération des décisions de préautorisation à l’aide de mvpd spécifique {#retrieve-preauthorization-decisions-using-specific-mvpd}

### Conditions préalables {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

Avant de récupérer les décisions de préautorisation à l’aide d’un MVPD spécifique, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit avoir un profil régulier valide qui a été créé avec succès pour le MVPD à l’aide de l’un des flux d’authentification de base :
   * [Effectuer l’authentification dans l’application principale](./rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Effectuez l’authentification dans une application secondaire avec mvpd présélectionné.](./rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Effectuer une authentification dans une application secondaire sans mvpd présélectionné](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* L’application de diffusion en continu souhaite récupérer les décisions de préautorisation afin d’afficher une liste des ressources avec leurs statuts associés.

### Workflow {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

Suivez les étapes données pour mettre en oeuvre le flux de préautorisation de base à l’aide d’un MVPD spécifique exécuté dans une application principale, comme illustré dans le diagramme suivant.

![Récupérer les décisions de préautorisation à l’aide de mvpd spécifique](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png)

*Récupérer les décisions de préautorisation à l’aide de mvpd spécifique*

1. **Récupérer les décisions de préautorisation :** L’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir des décisions de préautorisation pour une liste de ressources en appelant le point de terminaison Decisions Preauthorized .

   >[!IMPORTANT]
   >
   > Pour plus d’informations, consultez la documentation de l’API [Récupérer les décisions de préautorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) spécifique :
   >
   > * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _requis_, comme `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_

1. **Rechercher un profil régulier :** Le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

1. **Récupérer les décisions MVPD pour les ressources demandées :** Le serveur Adobe Pass appelle le point de terminaison de préautorisation MVPD pour obtenir une décision `Permit` ou `Deny` pour chaque ressource reçue de l’application de diffusion en continu.

1. **Renvoi des décisions de préautorisation :** La réponse de préautorisation de décisions contient une décision `Permit` ou `Deny` pour chaque ressource :
   * Une décision `Permit` signifie que la ressource peut être lue. La réponse n’inclut pas de jeton multimédia, car le flux de préautorisation ne doit pas être utilisé pour lire les ressources.
   * Une décision `Deny` signifie que la ressource ne peut pas être lue. La réponse comprend un payload d’erreur conforme à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse à une décision, reportez-vous à la documentation de l’API [Récupérer les décisions de préautorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) spécifique.
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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Gérer les décisions de préautorisation :** L’application de diffusion en continu traite la réponse et peut l’utiliser pour afficher éventuellement l’état approprié pour chaque ressource dans l’interface utilisateur.
