---
title: Déconnexion unique - Flux
description: API REST V2 - Déconnexion unique - Flux
exl-id: d7092ca7-ea7b-4e92-b45f-e373a6d673d6
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Flux de déconnexion unique {#single-logout-flow}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Veillez également à consulter la [FAQ sur l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

## Lancer une déconnexion unique pour un fichier mvpd spécifique {#initiate-single-logout-for-specific-mvpd}

### Conditions préalables {#prerequisites-initiate-single-logout-for-specific-mvpd}

Avant de lancer une déconnexion unique pour un MVPD spécifique, assurez-vous que les conditions préalables suivantes sont remplies :

* La deuxième application de diffusion en continu doit avoir un profil d’authentification unique valide qui a été créé avec succès pour MVPD à l’aide de l’un des flux d’authentification unique :
   * [Authentification par authentification unique à l’aide de l’identité de la plateforme](rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [Authentification par authentification unique à l’aide du jeton de service](rest-api-v2-single-sign-on-service-token-flows.md)
* La deuxième application de diffusion en continu doit lancer le flux de déconnexion unique lorsqu’elle doit se déconnecter du MVPD.

>[!IMPORTANT]
> 
> Hypothèses
>
> <br/>
> 
> * Les première et seconde applications de diffusion en continu obtiennent la même payload d&#39;identifiant de plateforme unique que `JWS` ou `JWE` ou la même payload d&#39;identifiant d&#39;utilisateur unique que `JWS`.

### Workflow {#workflow-initiate-single-logout-for-specific-mvpd}

Suivez les étapes données pour implémenter le flux de déconnexion unique pour un MVPD spécifique, comme illustré dans le diagramme ci-dessous.

![Lancer une déconnexion unique pour un fichier mvpd spécifique](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*Lancer une déconnexion unique pour un fichier mvpd spécifique*

1. **Lancer la déconnexion d’Adobe Pass :** l’application de diffusion en continu rassemble toutes les données nécessaires pour lancer le flux de déconnexion en appelant le point d’entrée de déconnexion Adobe Pass.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Lancer la déconnexion pour des API mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) spécifiques pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd` et `redirectUrl`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant unique de la plateforme ou l’identifiant unique de l’utilisateur avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d’informations sur `Adobe-Subject-Token`’en-tête , consultez la documentation de [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).
   > 
   > <br/>
   > 
   > Pour plus d’informations sur `AD-Service-Token`’en-tête , consultez la documentation d’ [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Rechercher des profils d’authentification standard et uniques :** le serveur Adobe Pass identifie les profils valides d’authentification standard et d’authentification unique en fonction des paramètres et des en-têtes reçus.

1. **Supprimer les profils d’authentification standard et uniques :** le serveur Adobe Pass supprime les profils d’authentification standard et uniques identifiés du serveur principal Adobe Pass.

1. **Indiquez l’action suivante :** la réponse du point d’entrée de déconnexion Adobe Pass contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante.

   >[!IMPORTANT]
   >
   > Reportez-vous à la documentation de l’API [Lancer la déconnexion pour des API mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) spécifiques pour plus d’informations sur les informations fournies dans une réponse de déconnexion.
   > 
   > <br/>
   > 
   > Le point d’entrée de la déconnexion Adobe Pass valide les données de la requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres _obligatoire_ et les en-têtes doivent être valides.
   > * L’intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Indiquer la déconnexion terminée :** si le MVPD ne prend pas en charge le flux de déconnexion, l’application de diffusion en continu traite la réponse et peut l’utiliser pour afficher éventuellement un message spécifique sur l’interface utilisateur.

1. **Déconnexion MVPD :** si le MVPD ne prend pas en charge le flux de déconnexion, l’application de diffusion en continu traite la réponse et utilise un agent utilisateur pour lancer le flux de déconnexion avec le MVPD. Le flux peut inclure plusieurs redirections vers des systèmes MVPD. Néanmoins, le résultat est que le MVPD effectue son nettoyage interne et envoie la confirmation de déconnexion finale au serveur principal d’Adobe Pass.

1. **Indiquer la déconnexion terminée :** l’application de diffusion en continu peut attendre que l’agent utilisateur atteigne le `redirectUrl` fourni et l’utiliser comme signal pour afficher éventuellement un message spécifique sur l’interface utilisateur.

>[!NOTE]
>
> Les étapes du flux de déconnexion unique sont les mêmes que ci-dessus, s’il est lancé à partir de la première application de diffusion en continu.
