---
title: Profils de base - Application Secondaire - Flux
description: API REST V2 - Profils de base - Application Secondaire - Flux
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---


# Flux de profils de base exécuté dans une application secondaire {#basic-profiles-flow-secondary-application}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

Le **flux de profils** dans le droit d’authentification Adobe Pass permet à l’application secondaire d’accéder aux informations sur les connexions d’utilisateurs actives.

Le flux de profils de base vous permet d’effectuer des requêtes pour les scénarios suivants :

* [Récupération du profil pour un code spécifique](#retrieve-profile-for-specific-code)

## Récupération du profil pour un code spécifique {#retrieve-profile-for-specific-code}

### Conditions préalables {#prerequisites-retrieve-profile-for-specific-code}

Avant de récupérer le profil pour un code d’authentification spécifique, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application secondaire, qui possède un `code` utilisé pour effectuer l’authentification interactive avec le MVPD, souhaite récupérer le profil pour un code d’authentification spécifique.

### Workflow {#workflow-retrieve-profile-for-specific-code}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux de récupération de profil de base pour un code d’authentification spécifique exécuté dans une application secondaire, comme illustré dans le diagramme ci-dessous.

![Récupération du profil pour un code spécifique](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*Récupération du profil pour un code spécifique*

1. **Récupérer le profil pour un code spécifique :** L’application secondaire rassemble toutes les données nécessaires pour récupérer les informations de profil pour ce code d’authentification spécifique en envoyant une requête au point de terminaison Profiles.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
   >
   > * Tous les paramètres _required_, comme `serviceProvider` et `code`
   > * Tous les en-têtes _requis_, comme `Authorization`
   > * Tous les paramètres et en-têtes _optional_

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

1. **Indique que le flux d’authentification s’est terminé avec succès :** Si la réponse du point de terminaison Profiles contient un profil, l’application secondaire traite la réponse et peut l’utiliser pour afficher éventuellement un message spécifique dans l’interface utilisateur.

1. **Indique que le flux d’authentification a rencontré un problème :** Si la réponse du point de terminaison Profiles ne contient pas de profil, l’application secondaire traite la réponse et peut l’utiliser pour afficher éventuellement un message spécifique dans l’interface utilisateur.
