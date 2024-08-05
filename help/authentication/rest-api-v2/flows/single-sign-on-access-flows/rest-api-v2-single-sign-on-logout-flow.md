---
title: Déconnexion unique - Flux
description: API REST V2 - Déconnexion unique - Flux
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---


# Flux de déconnexion unique {#single-logout-flow}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

## Lancer une déconnexion unique pour mvpd spécifique {#initiate-single-logout-for-specific-mvpd}

### Conditions préalables {#prerequisites-initiate-single-logout-for-specific-mvpd}

Avant de lancer une déconnexion unique pour un MVPD spécifique, assurez-vous que les conditions préalables suivantes sont remplies :

* La deuxième application de diffusion en continu doit comporter un profil de connexion unique valide qui a été créé avec succès pour le MVPD à l’aide de l’un des flux d’authentification de connexion unique :
   * [Effectuer l’authentification par authentification unique à l’aide de l’identité de plateforme](./rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [Authentification par authentification unique à l’aide du jeton de service](./rest-api-v2-single-sign-on-service-token-flows.md)
* La seconde application de diffusion en continu doit lancer le flux de déconnexion unique lorsqu’elle doit se déconnecter du MVPD.

>[!IMPORTANT]
> 
> Hypothèses
>
> <br/>
> 
> * Les première et deuxième applications de diffusion en continu obtiennent la même charge utile d’identifiant de plateforme unique que `JWS` ou `JWE` ou la même charge utile d’identifiant d’utilisateur unique que `JWS`.

### Workflow {#workflow-initiate-single-logout-for-specific-mvpd}

Exécutez les étapes données pour mettre en oeuvre le flux de déconnexion unique pour un MVPD spécifique, comme illustré dans le diagramme suivant.

![Lancer une déconnexion unique pour mvpd spécifique](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*Lancer une déconnexion unique pour mvpd spécifique*

1. **Lancer la déconnexion Adobe Pass :** L’application de diffusion en continu rassemble toutes les données nécessaires pour lancer le flux de déconnexion en appelant le point de terminaison de la déconnexion Adobe Pass.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
   >
   > * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `redirectUrl`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_
   >
   > <br/>
   >
   > L’application de diffusion en continu doit s’assurer qu’elle inclut une valeur valide pour l’identifiant de plateforme unique ou l’identifiant utilisateur unique avant d’effectuer une requête.
   >
   > <br/>
   > 
   > Pour plus d&#39;informations sur l&#39;en-tête `Adobe-Subject-Token`, consultez la documentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).
   > 
   > <br/>
   > 
   > Pour plus d’informations sur l’en-tête `AD-Service-Token`, consultez la documentation [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) .

1. **Rechercher des profils d’authentification unique et standard :** Le serveur Adobe Pass identifie les profils valides d’authentification unique et standard en fonction des paramètres et des en-têtes reçus.

1. **Supprimer les profils d’authentification unique et standard :** Le serveur Adobe Pass supprime les profils d’authentification unique et standard identifiés du serveur principal Adobe Pass.

1. **Indiquez l’action suivante :** La réponse du point de terminaison de la connexion Adobe Pass contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de déconnexion, reportez-vous à la documentation de l’API [Initiate logout for specific mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) .
   > 
   > <br/>
   > 
   > Le point de terminaison de connexion Adobe Pass valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   > * L&#39;intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Indique que la déconnexion est terminée :** Si le MVPD ne prend pas en charge le flux de déconnexion, l’application de diffusion en continu traite la réponse et peut l’utiliser pour afficher éventuellement un message spécifique dans l’interface utilisateur.

1. **Lancement de la déconnexion MVPD :** Si le MVPD prend en charge le flux de déconnexion, l’application de diffusion en continu traite la réponse et utilise un agent utilisateur pour lancer le flux de déconnexion avec le MVPD. Le flux peut inclure plusieurs redirections vers les systèmes MVPD. Néanmoins, le MVPD effectue son nettoyage interne et envoie la confirmation finale de déconnexion au serveur principal Adobe Pass.

1. **Indique que la déconnexion est terminée :** L’application de diffusion en continu peut attendre que l’agent utilisateur atteigne le `redirectUrl` fourni et peut l’utiliser comme signal pour afficher éventuellement un message spécifique dans l’interface utilisateur.

>[!NOTE]
>
> Les étapes du flux de déconnexion unique sont les mêmes que ci-dessus, si elles sont initiées à partir de la première application de diffusion en continu.
