---
title: Flux d’accès dégradés
description: API REST V2 - Flux d’accès dégradés
exl-id: 9276f5d9-8b1a-4282-8458-0c1e1e06bcf5
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '1615'
ht-degree: 0%

---

# Flux d’accès dégradés {#degraded-access-flows}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Veillez également à consulter la [FAQ sur l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

La dégradation permet le contournement temporaire de points d’entrée d’authentification et d’autorisation MVPD spécifiques. En règle générale, le programmeur lance cette action, mais quelle que soit la personne qui déclenche un événement de dégradation, l’action dépend des dispositions préalables prises avec les MVPD affectées.

Pour plus d’informations sur la fonctionnalité de dégradation, consultez la documentation [Dégradation](/help/premium-workflow/degraded-access/degradation-feature.md).

Les flux d’accès dégradés vous permettent de rechercher les scénarios suivants :

* [Effectuer l’authentification lorsque la dégradation est appliquée](#perform-authentication-while-degradation-is-applied)
* [Récupérer les décisions d’autorisation pendant l’application de la dégradation](#retrieve-authorization-decisions-while-degradation-is-applied)
* [Récupérer les décisions de préautorisation pendant l’application de la dégradation](#retrieve-preauthorization-decisions-while-degradation-is-applied)
* [Récupérer le profil pendant l’application de la dégradation](#retrieve-profile-while-degradation-is-applied)

## Effectuer l’authentification lorsque la dégradation est appliquée {#perform-authentication-while-degradation-is-applied}

### Conditions préalables {#prerequisites-perform-authentication-while-degradation-is-applied}

Avant d’exécuter le flux d’authentification pendant l’application de la dégradation, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit lancer une session d’authentification lorsqu’elle doit se connecter avec le MVPD.

>[!IMPORTANT]
> 
> Hypothèses
> 
> <br/>
> 
> * L’application de diffusion en continu ne dispose pas d’un profil valide pour ce MVPD spécifique enregistré dans le serveur principal Adobe Pass.
> * Une règle de dégradation AuthNAll est appliquée à l’intégration entre le `serviceProvider` et le `mvpd` fournis.

### Workflow {#workflow-perform-authentication-while-degradation-is-applied}

Suivez les étapes données pour implémenter le flux d’authentification pendant l’application de la dégradation, comme illustré dans le diagramme suivant.

![Effectuer une authentification en cas de dégradation](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-perform-authentication-while-degradation-is-applied-flow.png)

*Effectuer une authentification en cas de dégradation*

1. **Créer une session d’authentification :** l’application de diffusion en continu rassemble toutes les données nécessaires pour lancer une session d’authentification en appelant le point d’entrée Sessions.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Créer une session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) pour plus d’informations sur :
   > 
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd`, `domainName` et `redirectUrl`
   > * Tous les en-têtes _obligatoires_, tels que `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Vérifier les règles de dégradation :** le serveur Adobe Pass vérifie si une règle de dégradation AuthNAll est appliquée à l’intégration entre le `serviceProvider` et le `mvpd` fournis.

1. **Indiquez l’action suivante :** la réponse du point d’entrée des sessions contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante :
   * L’attribut `actionName` est défini sur « autoriser ».
   * L’attribut `actionType` est défini sur « direct ».

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Créer une session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) pour plus d’informations sur les informations fournies dans une réponse de session.
   > 
   > <br/>
   > 
   > Le point d’entrée Sessions valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   > * L’intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   > 
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   > 
   > Le point d’entrée Sessions utilise les données de requête pour vérifier si les conditions d’accès dégradées sont remplies :
   >
   > * Une règle de dégradation AuthNAll doit être appliquée à l’intégration entre le `serviceProvider` fourni et `mvpd`.
   >
   > <br/>
   > 
   > Si la validation d’accès dégradé échoue, la réponse est définie par défaut sur le flux d’authentification de base.

1. **Poursuivre avec les flux de décisions :** l’application de diffusion en continu peut continuer avec les flux de décisions suivants.

## Récupérer les décisions d’autorisation pendant l’application de la dégradation {#retrieve-authorization-decisions-while-degradation-is-applied}

### Conditions préalables {#prerequisites-retrieve-authorization-decisions-while-degradation-is-applied}

Avant de récupérer des décisions d’autorisation lorsque la dégradation est appliquée, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit récupérer une décision d’autorisation avant de lire une ressource sélectionnée par l’utilisateur.

>[!IMPORTANT]
>
> Hypothèses
> 
> <br/>
> 
> * L’application de diffusion en continu n’a pas de profil valide pour ce MVPD spécifique.
> * Une règle de dégradation AuthZAll ou AuthNAll est appliquée à l’intégration entre les `serviceProvider` et `mvpd` fournis.

### Workflow {#workflow-retrieve-authorization-decisions-while-degradation-is-applied}

Suivez les étapes données pour implémenter le flux d’autorisation pendant l’application de la dégradation, comme illustré dans le diagramme suivant.

![Récupérer les décisions d’autorisation pendant l’application de la dégradation](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-authorization-decisions-while-degradation-is-applied-flow.png)

*Récupérer les décisions d’autorisation pendant l’application de la dégradation*

1. **Récupérer la décision d’autorisation :** l’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir une décision d’autorisation pour une ressource spécifique en appelant le point d’entrée Decisions Authorize.

   >[!IMPORTANT]
   > 
   > Reportez-vous à la documentation [Récupération des décisions d’autorisation à l’aide d’une API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) spécifique pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _obligatoires_, tels que `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Vérifier les règles de dégradation :** le serveur Adobe Pass vérifie si une règle de dégradation AuthZAll ou AuthNAll est appliquée à l’intégration entre le `serviceProvider` et le `mvpd` fournis.

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
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > Le point d’entrée Autoriser les décisions utilise les données de la requête pour vérifier si les conditions d’accès dégradées sont remplies :
   >
   > * Une règle de dégradation AuthZAll ou AuthNAll doit être appliquée à l’intégration entre le `serviceProvider` fourni et `mvpd`.
   >
   > <br/>
   > 
   > Si la validation d’accès dégradé échoue, la réponse est définie par défaut sur le flux d’autorisation de base.

1. **Démarrer le flux avec un jeton de média :** l’application de diffusion en continu utilise le jeton de média pour lire le contenu.

## Récupérer les décisions de préautorisation pendant l’application de la dégradation {#retrieve-preauthorization-decisions-while-degradation-is-applied}

### Conditions préalables {#prerequisites-retrieve-preauthorization-decisions-while-degradation-is-applied}

Avant de récupérer des décisions de préautorisation lorsque la dégradation est appliquée, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu souhaite récupérer les décisions de préautorisation pour afficher une liste de ressources avec leurs statuts associés.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * L’application de diffusion en continu n’a pas de profil valide pour ce MVPD spécifique.
> * Une règle de dégradation AuthZAll ou AuthNAll est appliquée à l’intégration entre les `serviceProvider` et `mvpd` fournis.

### Workflow {#workflow-retrieve-preauthorization-decisions-while-degradation-is-applied}

Suivez les étapes données pour implémenter le flux de préautorisation pendant l’application de la dégradation, comme illustré dans le diagramme suivant.

![Récupérer les décisions de préautorisation lorsque la dégradation est appliquée](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-preauthorization-decisions-while-degradation-is-applied-flow.png)

*Récupérer les décisions de préautorisation lorsque la dégradation est appliquée*

1. **Récupérer les décisions de préautorisation :** l’application de diffusion en continu rassemble toutes les données nécessaires pour obtenir des décisions de préautorisation pour une liste de ressources en appelant le point d’entrée Decisions Preauthorize.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupération des décisions de préautorisation à l’aide de mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) spécifique pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd` et `resources`
   > * Tous les en-têtes _obligatoires_, tels que `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Vérifier les règles de dégradation :** le serveur Adobe Pass vérifie si une règle de dégradation AuthZAll ou AuthNAll est appliquée à l’intégration entre le `serviceProvider` et le `mvpd` fournis.

1. **Renvoyer les décisions de préautorisation :** la réponse de point d’entrée Decisions Preauthorize contient une décision `Permit` pour chaque ressource.

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
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > Le point d’entrée de préautorisation des décisions utilise les données de la requête pour vérifier si les conditions d’accès dégradées sont remplies :
   >
   > * Une règle de dégradation AuthZAll ou AuthNAll doit être appliquée à l’intégration entre le `serviceProvider` fourni et `mvpd`.
   >
   > <br/>
   > 
   > Si la validation d’accès dégradé échoue, la réponse est définie par défaut sur le flux de préautorisation de base.

1. **Gérer les décisions de préautorisation :** l’application de diffusion en continu traite la réponse et peut l’utiliser pour afficher éventuellement le statut approprié pour chaque ressource sur l’interface utilisateur.

## Récupérer le profil pendant l’application de la dégradation {#retrieve-profile-while-degradation-is-applied}

>[!IMPORTANT]
>
> La requête de point d’entrée des profils est facultative lorsque la dégradation est appliquée.
>
> <br/>
> 
> La réponse du point d’entrée des sessions indique à l’application de poursuivre les flux de décisions pendant l’application de la dégradation. Pour plus d’informations, consultez la section [Effectuer l’authentification en cas de dégradation](#perform-authentication-while-degradation-is-applied).

### Conditions préalables {#prerequisites-retrieve-profile-while-degradation-is-applied}

Avant de récupérer le profil d’un MVPD spécifique pendant l’application de la dégradation, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu, qui dispose d’un identifiant de `mvpd` sélectionné ou mis en cache, souhaite récupérer le profil d’un MVPD spécifique.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * L’application de diffusion en continu n’a pas de profil valide pour ce MVPD spécifique.
> * Une règle de dégradation AuthNAll est appliquée à l’intégration entre le `serviceProvider` et le `mvpd` fournis.

### Workflow {#workflow-retrieve-profile-while-degradation-is-applied}

Suivez les étapes données pour implémenter le flux de récupération des profils pour un MVPD spécifique pendant l’application de la dégradation, comme illustré dans le diagramme ci-dessous.

![Récupérer le profil lorsque la dégradation est appliquée](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-profile-while-degradation-is-applied-flow.png)

*Récupérer le profil lorsque la dégradation est appliquée*

1. **Récupérer le profil pour un mvpd spécifique :** l’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil pour ce MVPD spécifique en envoyant une requête au point d’entrée Profils .

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupérer le profil pour des API mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) spécifiques pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider` et `mvpd`
   > * Tous les en-têtes _obligatoires_, tels que `Authorization` et `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

1. **Vérifier les règles de dégradation :** le serveur Adobe Pass vérifie si une règle de dégradation AuthNAll est appliquée à l’intégration entre le `serviceProvider` et le `mvpd` fournis.

1. **Renvoyer des informations sur le profil dégradé :** la réponse de point d’entrée des profils contient des informations sur le profil dégradé, y compris l’attribut `type` défini sur « dégradé ».

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
   > Si la validation de base échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   > 
   > Le point d’entrée des profils utilise les données de requête pour vérifier si les conditions d’accès dégradées sont remplies :
   >
   > * Une règle de dégradation AuthNAll doit être appliquée à l’intégration entre le `serviceProvider` fourni et `mvpd`.
   >
   > <br/>
   > 
   > Si la validation d’accès dégradé échoue, la réponse est définie par défaut sur le flux de récupération de profil de base.

1. **Poursuivre avec les flux de décisions :** si la réponse de point d’entrée des profils contient un profil, l’application de diffusion en continu utilise les informations de profil détériorées pour poursuivre avec les flux de décisions suivants.

1. **Indiquer un nouveau flux d’authentification de base :** si la réponse de point d’entrée des profils ne contient pas de profil, l’application de diffusion en continu indique à l’utilisateur de lancer un nouveau flux d’authentification de base.

>[!NOTE]
>
> Les étapes du flux de récupération du profil pour un code d’authentification spécifique sont les mêmes que ci-dessus, sauf que le point d’entrée utilisé est celui décrit dans la documentation [Récupérer le profil pour un code spécifique](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).
