---
title: Flux d’enregistrement client dynamique
description: Flux d’enregistrement client dynamique
exl-id: d881cf0a-de09-4b1d-a094-d5490f944796
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Flux d’enregistrement client dynamique {#dynamic-client-registration-flow}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API d’enregistrement client dynamique est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Accès aux API protégées par Adobe Pass {#access-adobe-pass-protected-apis}

### Conditions préalables {#prerequisites-access-adobe-pass-protected-apis}

Avant d’accéder aux API protégées d’Adobe Pass, assurez-vous que les conditions préalables suivantes sont remplies :

* Un représentant du client doit créer une application enregistrée, comme décrit dans la section [&#x200B; Gérer les applications enregistrées &#x200B;](../dynamic-client-registration-overview.md#manage-registered-applications).
* Un représentant client doit télécharger et incorporer une instruction logicielle comme décrit dans la section [&#x200B; Gérer les instructions logicielles &#x200B;](../dynamic-client-registration-overview.md#manage-software-statements).

>[!IMPORTANT]
>
> Les SDK d’authentification Adobe Pass sont chargés d’obtenir et d’actualiser les informations d’identification du client et le jeton d’accès au nom de l’application cliente.
> 
> Pour toutes les autres API protégées par Adobe Pass, l’application cliente doit suivre le workflow ci-dessous.

### Workflow {#workflow-access-adobe-pass-protected-apis}

Suivez les étapes données pour accéder aux API protégées d’Adobe Pass, comme illustré dans le diagramme ci-dessous.

![Accès aux API protégées par Adobe Pass](../../../../assets/dcr-api/dcr-api-access-adobe-pass-protected-apis.png)

*Accès aux API protégées par Adobe Pass*

1. **Récupérer les informations d’identification du client :** l’application cliente rassemble toutes les données nécessaires pour récupérer les informations d’identification du client en appelant le point d’entrée du registre client.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération des informations d’identification du client](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ comme `software_statement`
   > * Tous les en-têtes _obligatoires_ tels que `Content-Type`, `X-Device-Info`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Renvoyer les informations d’identification du client :** la réponse du point d’entrée du registre du client contient des informations sur les informations d’identification du client associées aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération des informations d’identification du client](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) pour plus d’informations sur les informations fournies dans une réponse d’informations d’identification du client.
   >
   > <br/>
   >
   > Le registre du client valide les données de la requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui respectent la documentation de l’API [&#x200B; Récupération des informations d’identification du client &#x200B;](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error).

   >[!TIP]
   >
   > Les informations d’identification du client doivent être mises en cache et utilisées indéfiniment.

1. **Récupérer le jeton d’accès :** l’application cliente rassemble toutes les données nécessaires pour récupérer le jeton d’accès en appelant le point d’entrée du jeton client.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [&#x200B; Récupération du jeton d’accès &#x200B;](../apis/dynamic-client-registration-apis-retrieve-access-token.md#request) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `client_id`, `client_secret` et `grant_type`
   > * Tous les en-têtes _obligatoires_ tels que `Content-Type`, `X-Device-Info`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Jeton d’accès de retour :** la réponse du point d’entrée du jeton client contient des informations sur le jeton d’accès associé aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Récupérer le jeton d’accès](../apis/dynamic-client-registration-apis-retrieve-access-token.md#success) pour plus d’informations sur les informations fournies dans une réponse de jeton d’accès.
   >
   > <br/>
   >
   > Le jeton client valide les données de la requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui respectent la documentation de l’API [&#x200B; Récupérer le jeton d’accès &#x200B;](../apis/dynamic-client-registration-apis-retrieve-access-token.md#error).

   >[!TIP]
   >
   > Le jeton d’accès doit être mis en cache et utilisé uniquement pendant la durée spécifiée (par exemple, durée de vie de 24 heures). Après son expiration, l’application cliente doit demander un nouveau jeton d’accès.

1. **Accéder aux API protégées :** l’application cliente utilise le jeton d’accès pour accéder à d’autres API protégées par Adobe Pass. L’application cliente doit inclure le jeton d’accès dans l’en-tête de requête `Authorization` à l’aide du schéma d’authentification `Bearer` (c’est-à-dire `Authorization: Bearer <access_token>`).

   >[!IMPORTANT]
   >
   > Les API protégées par Adobe Pass valident le jeton d’accès pour s’assurer que les conditions de base sont remplies :
   >
   > * Le _access_token_ doit être valide.
   > * Le _jeton_d’accès_ doit être associé à un _client_id_ et un _client_secret_ valides.
   > * Le _jeton_d’accès_ doit être associé à une _instruction_logicielle_ valide.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../features-standard/error-reporting/enhanced-error-codes.md).
