---
title: Déconnexion de base - Application de Principal - Flux
description: API REST V2 - Déconnexion de base - Application de Principal - Flux
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '911'
ht-degree: 0%

---


# Flux de connexion de base exécuté dans l’application principale {#basic-logout-flow-performed-within-primary-application}

Le **flux de déconnexion** dans le droit d’authentification Adobe Pass permet à l’application de diffusion en continu d’effectuer deux étapes principales :

* Supprimez les profils réguliers enregistrés sur le serveur principal Adobe Pass.
* Utilisez un agent utilisateur (navigateur) pour accéder au point de terminaison de la déconnexion MVPD, déclenchant un nettoyage sur le serveur principal MVPD.

Le flux de déconnexion de base vous permet d’effectuer des requêtes pour les scénarios suivants :

* [Lancement de la déconnexion pour mvpd spécifique avec le point de terminaison de déconnexion](#initiate-logout-for-specific-mvpd-with-logout-endpoint)
* [Lancement de la déconnexion pour mvpd spécifique sans point de terminaison de déconnexion](#initiate-logout-for-specific-mvpd-without-logout-endpoint)

## Lancement de la déconnexion pour un mvpd spécifique ayant un point de terminaison de déconnexion {#initiate-logout-for-specific-mvpd-with-logout-endpoint}

### Conditions préalables {#prerequisites-initiate-logout-for-specific-mvpd-with-logout-endpoint}

Avant de commencer la déconnexion pour un MVPD spécifique avec un point de terminaison de déconnexion, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit avoir un profil régulier valide qui a été créé avec succès pour le MVPD à l’aide de l’un des flux d’authentification de base :
   * [Effectuer l’authentification dans l’application principale](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Effectuez l’authentification dans une application secondaire avec mvpd présélectionné.](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Effectuer une authentification dans une application secondaire sans mvpd présélectionné](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* L’application en continu doit lancer le flux de déconnexion lorsqu’elle doit se déconnecter du MVPD.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * Le MVPD prend en charge le flux de déconnexion et dispose d’un point de terminaison de déconnexion.

### Workflow {#workflow-initiate-logout-for-specific-mvpd-with-logout-endpoint}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux de déconnexion de base d’un MVPD spécifique avec un point de terminaison de déconnexion effectué dans une application principale, comme illustré dans le diagramme ci-dessous.

![Lancement de la déconnexion pour mvpd spécifique avec point de terminaison de déconnexion](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-with-logout-endpoint.png)

*Lancement de la déconnexion pour mvpd spécifique avec point de terminaison de déconnexion*

1. **Lancer la déconnexion Adobe Pass :** L’application de diffusion en continu rassemble toutes les données nécessaires pour lancer le flux de déconnexion en appelant le point de terminaison de la déconnexion Adobe Pass.

   Pour plus d’informations sur :[](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
   * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `redirectUrl`
   * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

1. **Rechercher un profil régulier :** Le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

1. **Supprimer le profil normal :** Le serveur Adobe Pass supprime le profil régulier identifié du serveur principal Adobe Pass.

1. **Indiquez l’action suivante :** La réponse du point de terminaison de la connexion Adobe Pass contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante :
   * L’attribut `url` est présent puisque le MVPD prend en charge le flux de déconnexion.
   * L’attribut `actionName` est défini sur &quot;logout&quot;.
   * L’attribut `actionType` est défini sur &quot;interactif&quot;.

   Pour plus d’informations sur les informations fournies dans une réponse de déconnexion, reportez-vous à la documentation de l’API [Initiate logout for specific mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) .

   >[!IMPORTANT]
   >
   > Le point de terminaison de connexion Adobe Pass valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   > * L&#39;intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Lancer la déconnexion MVPD :** L’application de diffusion en continu lit le `url` et utilise un agent utilisateur pour lancer le flux de déconnexion avec le MVPD. Le flux peut inclure plusieurs redirections vers les systèmes MVPD. Néanmoins, le MVPD effectue son nettoyage interne et envoie la confirmation finale de déconnexion au serveur principal Adobe Pass.

1. **Indique que la déconnexion est terminée :** L’application de diffusion en continu peut attendre que l’agent utilisateur atteigne le `redirectUrl` fourni et peut l’utiliser comme signal pour afficher éventuellement un message spécifique dans l’interface utilisateur.

## Lancement de la déconnexion pour mvpd spécifique sans point de terminaison de déconnexion {#initiate-logout-for-specific-mvpd-without-logout-endpoint}

### Conditions préalables {#prerequisites-initiate-logout-for-specific-mvpd-without-logout-endpoint}

Avant de commencer la déconnexion d’un MVPD spécifique sans point de terminaison de déconnexion, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit avoir un profil régulier valide qui a été créé avec succès pour le MVPD à l’aide de l’un des flux d’authentification de base :
   * [Effectuer l’authentification dans l’application principale](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Effectuez l’authentification dans une application secondaire avec mvpd présélectionné.](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Effectuer une authentification dans une application secondaire sans mvpd présélectionné](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* L’application en continu doit lancer le flux de déconnexion lorsqu’elle doit se déconnecter du MVPD.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * Le MVPD ne prend pas en charge le flux de déconnexion et ne dispose pas d’un point de terminaison de déconnexion.

### Workflow {#workflow-initiate-logout-for-specific-mvpd-without-logout-endpoint}

Suivez les étapes données pour mettre en oeuvre le flux de déconnexion de base pour un MVPD spécifique sans point de terminaison de déconnexion effectué dans une application principale, comme illustré dans le diagramme suivant.

![Lancement de la déconnexion pour mvpd spécifique sans point de terminaison de déconnexion](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-without-logout-endpoint.png)

*Lancement de la déconnexion pour mvpd spécifique sans point de terminaison de déconnexion*

1. **Lancer la déconnexion Adobe Pass :** L’application de diffusion en continu rassemble toutes les données nécessaires pour lancer le flux de déconnexion en appelant le point de terminaison de la déconnexion Adobe Pass.

   Pour plus d’informations sur :[](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
   * Tous les paramètres _required_, comme `serviceProvider`, `mvpd` et `redirectUrl`
   * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   * Tous les paramètres et en-têtes _optional_

1. **Rechercher un profil régulier :** Le serveur Adobe Pass identifie un profil valide en fonction des paramètres et des en-têtes reçus.

1. **Supprimer le profil normal :** Le serveur Adobe Pass supprime le profil régulier identifié.

1. **Indiquez l’action suivante :** La réponse du point de terminaison de la connexion Adobe Pass contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante :
   * L’attribut `url` est manquant car le MVPD ne prend pas en charge le flux de déconnexion.
   * L’attribut `actionName` est défini sur &quot;complete&quot;.
   * L’attribut `actionType` est défini sur &quot;none&quot;.

   Pour plus d’informations sur les informations fournies dans une réponse de déconnexion, reportez-vous à la documentation de l’API [Initiate logout for specific mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) .

   >[!IMPORTANT]
   >
   > Le point de terminaison de connexion Adobe Pass valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   > * L&#39;intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Indique que la déconnexion est terminée :** L’application de diffusion en continu traite la réponse et peut l’utiliser pour afficher éventuellement un message spécifique dans l’interface utilisateur.
