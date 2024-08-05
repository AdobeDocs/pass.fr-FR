---
title: Profils de base - Application Secondaire - Flux
description: API REST V2 - Profils de base - Application Secondaire - Flux
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%

---


# Flux de profils de base exécuté dans une application secondaire {#basic-profiles-flow-secondary-application}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Le **flux de profils** dans le droit d’authentification Adobe Pass permet à l’application secondaire d’accéder aux informations sur les connexions d’utilisateurs actives.

Le flux de profils de base vous permet d’effectuer des requêtes pour les scénarios suivants :

* [Récupération du profil pour un code spécifique](#retrieve-profile-for-specific-code)

## Récupération du profil pour un code spécifique {#retrieve-profile-for-specific-code}

### Conditions préalables {#prerequisites-retrieve-profile-for-specific-code}

Avant de récupérer le profil pour un code d’authentification spécifique, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application secondaire, qui possède un `code` utilisé pour effectuer l’authentification interactive avec le MVPD, souhaite récupérer le profil pour un code d’authentification spécifique.

### Workflow {#workflow-retrieve-profile-for-specific-code}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux de récupération de profil de base pour un code d’authentification spécifique exécuté dans une application secondaire, comme illustré dans le diagramme ci-dessous.

![Récupération du profil pour un code spécifique](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*Récupération du profil pour un code spécifique*

1. **Récupérer le profil pour un code spécifique :** L’application secondaire rassemble toutes les données nécessaires pour récupérer les informations de profil pour ce code d’authentification spécifique en envoyant une requête au point de terminaison Profiles.

   Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md)
   * Tous les paramètres _required_, comme `serviceProvider` et `code`
   * Tous les en-têtes _requis_, comme `Authorization`
   * Tous les paramètres et en-têtes _optional_

1. **Rechercher un profil régulier :** Le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

1. **Renvoi d’informations sur le profil normal :** La réponse du point de terminaison Profiles contient des informations sur le profil trouvé associé aux paramètres et en-têtes reçus.

   Pour plus d’informations sur les informations fournies dans une réponse de profil, reportez-vous à la documentation de l’API [Récupérer le profil pour un code spécifique](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md).

   >[!NOTE]
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
