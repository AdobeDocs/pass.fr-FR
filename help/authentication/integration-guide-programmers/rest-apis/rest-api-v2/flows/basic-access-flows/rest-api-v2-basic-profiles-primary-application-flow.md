---
title: Profils De Base - Application En Principal - Flux
description: API REST V2 - Profils de base - Application de Principal - Flux
exl-id: 19ddf382-9a32-4b94-aa84-7611c0e1780e
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '974'
ht-degree: 0%

---

# Flux de profils de base exécuté dans l’application principale {#basic-profiles-flow-primary-application}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Veillez également à consulter la [FAQ sur l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

Le **flux de profils** dans les droits d’authentification Adobe Pass permet à l’application de diffusion en continu d’accéder aux informations sur les connexions actives des utilisateurs.

Le flux des profils de base vous permet de rechercher les scénarios suivants :

* [Récupération des profils](#retrieve-profiles)
* [Récupération du profil pour un fichier mvpd spécifique](#retrieve-profile-for-specific-mvpd)
* [Récupération du profil pour un code spécifique](#retrieve-profile-for-specific-code)

## Récupération des profils {#retrieve-profiles}

### Conditions préalables {#prerequisites-retrieve-profiles}

Avant de récupérer des profils, vérifiez que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu souhaite récupérer tous les profils standard.

### Workflow {#workflow-retrieve-profiles}

Suivez les étapes données pour implémenter le flux de récupération des profils de base effectué dans une application principale, comme illustré dans le diagramme suivant.

![Récupération des profils](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profiles-within-primary-application.png)

*Récupération des profils*

1. **Récupérer les profils :** l’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer toutes les informations de profil en envoyant une requête au point d’entrée des profils.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération des profils](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ comme `serviceProvider`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Rechercher des profils standard :** le serveur Adobe Pass identifie tous les profils valides en fonction des paramètres et des en-têtes reçus.

1. **Renvoyer des informations sur les profils standard :** la réponse de point d’entrée des profils contient des informations sur les profils trouvés associés aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Récupération des profils](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) pour plus d’informations sur les informations fournies dans une réponse de profil.
   > 
   > <br/>
   > 
   > Le point d’entrée des profils valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Choisir un profil et poursuivre les flux de décisions :** si la réponse de point d’entrée des profils contient des profils, l’application de diffusion en continu utilise sa logique interne (éventuellement en interagissant avec l’utilisateur final) pour choisir l’un des profils disponibles pour continuer les flux de décisions suivants.

1. **Indiquer un nouveau flux d’authentification de base :** si la réponse de point d’entrée des profils ne contient pas de profil, l’application de diffusion en continu indique à l’utilisateur de lancer un nouveau flux d’authentification de base.

## Récupération du profil pour un fichier mvpd spécifique {#retrieve-profile-for-specific-mvpd}

### Conditions préalables {#prerequisites-retrieve-profile-for-specific-mvpd}

Avant de récupérer le profil d’un MVPD spécifique, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu, qui dispose d’un identifiant de `mvpd` sélectionné ou mis en cache, souhaite récupérer le profil standard d’un MVPD spécifique.

### Workflow {#workflow-retrieve-profile-for-specific-mvpd}

Suivez les étapes données pour implémenter le flux de récupération des profils de base pour un MVPD spécifique exécuté dans une application principale, comme illustré dans le diagramme ci-dessous.

![Récupération du profil pour un mvpd spécifique](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-mvpd.png)

*Récupération du profil pour un mvpd spécifique*

1. **Récupérer le profil pour un mvpd spécifique :** l’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil pour ce MVPD spécifique en envoyant une requête au point d’entrée Profils .

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupérer le profil pour des API mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) spécifiques pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider` et `mvpd`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Rechercher un profil normal :** le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

1. **Renvoyer des informations sur le profil normal :** la réponse de point d’entrée des profils contient des informations sur le profil trouvé associé aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Récupérer le profil pour des API mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) spécifiques pour plus d’informations sur les informations fournies dans une réponse de profil.
   > 
   > <br/>
   > 
   > Le point d’entrée des profils valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   > * L’intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Poursuivre avec les flux de décisions :** si la réponse de point d’entrée des profils contient un profil, l’application de diffusion en continu utilise les informations de profil pour poursuivre avec les flux de décisions suivants.

1. **Indiquer un nouveau flux d’authentification de base :** si la réponse de point d’entrée des profils ne contient pas de profil, l’application de diffusion en continu indique à l’utilisateur de lancer un nouveau flux d’authentification de base.

## Récupération du profil pour un code spécifique {#retrieve-profile-for-specific-code}

### Conditions préalables {#prerequisites-retrieve-profile-for-specific-code}

Avant de récupérer le profil pour un code d’authentification spécifique, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu, qui dispose d’un `code` utilisé pour effectuer l’authentification interactive avec MVPD, souhaite récupérer le profil pour un code d’authentification spécifique.

### Workflow {#workflow-retrieve-profile-for-specific-code}

Suivez les étapes données pour implémenter le flux de récupération des profils de base pour un code d’authentification spécifique effectué dans une application principale, comme illustré dans le diagramme suivant.

![Récupérer le profil pour un code spécifique](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-code.png)

*Récupérer le profil pour un code spécifique*

1. **Récupérer le profil pour un code spécifique :** l’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil pour ce code d’authentification spécifique en envoyant une requête au point d’entrée Profils .

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupérer le profil pour un code spécifique](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider` et `code`
   > * Tous les en-têtes _obligatoires_, comme `Authorization`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Rechercher un profil normal :** le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

1. **Renvoyer des informations sur le profil normal :** la réponse de point d’entrée des profils contient des informations sur le profil trouvé associé aux paramètres et en-têtes reçus.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Récupérer le profil pour un code spécifique](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) pour plus d’informations sur les informations fournies dans une réponse de profil.
   > 
   > <br/>
   > 
   > Le point d’entrée des profils valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Poursuivre avec les flux de décisions :** si la réponse de point d’entrée des profils contient un profil, l’application de diffusion en continu utilise les informations de profil pour poursuivre avec les flux de décisions suivants.

1. **Indiquer un nouveau flux d’authentification de base :** si la réponse de point d’entrée des profils ne contient pas de profil, l’application principale indique à l’utilisateur de lancer un nouveau flux d’authentification de base.
