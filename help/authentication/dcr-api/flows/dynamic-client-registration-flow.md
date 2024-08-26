---
title: Flux d’enregistrement de client dynamique
description: Flux d’enregistrement de client dynamique
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---


# Flux d’enregistrement de client dynamique {#dynamic-client-registration-flow}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API d’enregistrement de client dynamique est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

## Accès aux API protégées par Adobe Pass {#access-adobe-pass-protected-apis}

### Conditions préalables {#prerequisites-access-adobe-pass-protected-apis}

Avant d’accéder aux API protégées par Adobe Pass, assurez-vous que les conditions préalables suivantes sont remplies :

* Un représentant du client doit créer une application enregistrée comme décrit dans la section [Gérer les applications enregistrées](../dynamic-client-registration-overview.md#manage-registered-applications) .
* Un représentant du client doit télécharger et incorporer une instruction logicielle comme décrit dans la section [Gérer les instructions de logiciel](../dynamic-client-registration-overview.md#manage-software-statements) .

>[!IMPORTANT]
>
> Les SDK d’authentification Adobe Pass sont chargés d’obtenir et d’actualiser les informations d’identification du client et le jeton d’accès pour le compte de l’application cliente.
> 
> Pour toutes les autres API protégées par Adobe Pass, l’application cliente doit suivre le workflow ci-dessous.

### Workflow {#workflow-access-adobe-pass-protected-apis}

Suivez les étapes ci-dessous pour accéder aux API protégées par Adobe Pass, comme illustré dans le diagramme ci-dessous.

![Accès aux API protégées par Adobe Pass](../../assets/dcr-api/dcr-api-access-adobe-pass-protected-apis.png)

*Accès aux API protégées par Adobe Pass*

1. **Récupérer les informations d’identification du client :** L’application client rassemble toutes les données nécessaires pour récupérer les informations d’identification du client en appelant le point de terminaison de l’enregistrement du client.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request)
   >
   > * Tous les paramètres _requis_, comme `software_statement`
   > * Tous les en-têtes _requis_, comme `Content-Type`, `X-Device-Info`
   > * Tous les paramètres et en-têtes _optional_

1. **Renvoi des informations d’identification du client :** La réponse du point de terminaison de l’enregistrement du client contient des informations sur les informations d’identification du client associées aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse aux informations d’identification du client, reportez-vous à la documentation de l’API [Récupérer les informations d’identification du client](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) .
   >
   > <br/>
   >
   > Le registre du client valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation de l’API [Récupérer les informations d’identification du client](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error).

   >[!TIP]
   >
   > Suggestion : les informations d’identification du client doivent être mises en cache et peuvent être utilisées indéfiniment.

1. **Récupérer le jeton d’accès :** L’application cliente rassemble toutes les données nécessaires pour récupérer le jeton d’accès en appelant le point d’entrée du jeton client.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../apis/dynamic-client-registration-apis-retrieve-access-token.md#request)
   >
   > * Tous les paramètres _required_, comme `client_id`, `client_secret` et `grant_type`
   > * Tous les en-têtes _requis_, comme `Content-Type`, `X-Device-Info`
   > * Tous les paramètres et en-têtes _optional_

1. **Jeton d’accès de retour :** La réponse du point de terminaison du jeton client contient des informations sur le jeton d’accès associé aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de jeton d’accès, reportez-vous à la documentation de l’API [ Récupérer le jeton d’accès](../apis/dynamic-client-registration-apis-retrieve-access-token.md#success) .
   >
   > <br/>
   >
   > Le jeton client valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation de l’API [Récupérer le jeton d’accès](../apis/dynamic-client-registration-apis-retrieve-access-token.md#error).

   >[!TIP]
   >
   > Suggestion : le jeton d’accès doit être mis en cache et utilisé uniquement pendant la durée spécifiée (par exemple, durée de vie de 24 heures). Une fois arrivé à expiration, l’application cliente doit demander un nouveau jeton d’accès.

1. **Poursuivez l’accès aux API protégées :** L’application cliente utilise le jeton d’accès pour accéder à d’autres API protégées par Adobe Pass. L’application cliente doit inclure le jeton d’accès dans l’en-tête de requête `Authorization` à l’aide du schéma d’authentification `Bearer` (c’est-à-dire `Authorization: Bearer <access_token>`).

   >[!IMPORTANT]
   >
   > Les API protégées par Adobe Pass valident le jeton d’accès pour s’assurer que les conditions de base sont remplies :
   >
   > * _access_token_ doit être valide.
   > * _access_token_ doit être associé à un _client_id_ et un _client_secret_ valide.
   > * _access_token_ doit être associé à une _software_statement_ valide.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../enhanced-error-codes.md).
