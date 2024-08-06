---
title: Authentification de base - Application de Principal - Flux
description: API REST V2 - Authentification de base - Application de Principal - Flux
source-git-commit: dc9fab27c7eced2be5dd9f364ab8f2d64f8e4177
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 0%

---


# Flux d’authentification de base exécuté sur une application principale {#basic-authentication-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

Le **flux d’authentification** dans le droit d’authentification Adobe Pass permet à l’application de diffusion en continu de vérifier qu’un utilisateur dispose d’un compte MVPD valide. Ce processus nécessite que l’utilisateur dispose d’un compte MVPD actif et qu’il saisisse des informations de connexion valides sur la page de connexion MVPD.

Le flux d’authentification est requis dans les cas suivants :

* Lorsque l’utilisateur ouvre une application pour la première fois.
* Lorsque l’authentification précédente de l’utilisateur a expiré.
* Lorsque l’utilisateur se déconnecte du compte MVPD.
* Lorsque l’utilisateur souhaite s’authentifier avec un MVPD différent.

Dans tous ces cas, l’application qui appelle l’un des points de terminaison Profiles reçoit une réponse vide ou un ou plusieurs profils, mais pour différents MVPD.

Le **flux d’authentification** nécessite qu’un agent utilisateur (navigateur) effectue une série d’appels depuis l’application vers le serveur principal Adobe Pass, puis vers la page de connexion MVPD et enfin vers l’application. Ce flux peut inclure plusieurs redirections vers les systèmes MVPD et la gestion des cookies ou sessions stockés pour chaque domaine, ce qui peut s’avérer difficile à réaliser et à sécuriser sans agent utilisateur.

En fonction des fonctionnalités de l’application principale (application de diffusion en continu) pour prendre en charge l’interaction utilisateur afin de sélectionner un MVPD et de s’authentifier auprès du MVPD sélectionné dans un agent utilisateur, les scénarios d’authentification sont les suivants :

* [Effectuer l’authentification dans l’application principale](./rest-api-v2-basic-authentication-primary-application-flow.md)
* [Effectuez l’authentification dans une application secondaire avec mvpd présélectionné.](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Effectuer une authentification dans une application secondaire sans mvpd présélectionné](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## Effectuer l’authentification dans l’application principale {#perform-authentication-within-primary-application}

### Conditions préalables {#prerequisites-perform-authentication-within-primary-application}

Avant d’effectuer une authentification par interaction avec l’utilisateur au sein d’une application principale, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit sélectionner un MVPD.
* L’application de diffusion en continu doit lancer une session d’authentification pour se connecter avec le MVPD sélectionné.
* L’application de diffusion en continu doit s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * L’application de diffusion en continu prend en charge l’interaction utilisateur pour sélectionner un MVPD.
> * L’application de diffusion en continu prend en charge l’interaction de l’utilisateur pour s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.

### Workflow {#workflow-perform-authentication-completed-on-primary-application}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux d’authentification de base effectué dans une application principale, comme illustré dans le diagramme ci-dessous.

![Effectuer l&#39;authentification dans l&#39;application principale](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-primary-application.png)

*Effectuer l&#39;authentification dans l&#39;application principale*

1. **Créer une session d’authentification :** L’application de diffusion en continu rassemble toutes les données nécessaires pour lancer une session d’authentification en appelant le point de terminaison sessions .

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
   > 
   > * Tous les paramètres _required_, tels que `serviceProvider`, `mvpd`, `domainName` et `redirectUrl`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_
   > 
   > <br/>
   > 
   > L’application de diffusion en continu doit fournir tous les paramètres requis dans un seul appel lors de la création de la session d’authentification.

1. **Indiquez l’action suivante :** La réponse du point de terminaison sessions contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de session, reportez-vous à la documentation de l’API [Créer une session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).
   > 
   > <br/>
   > 
   > Le point de terminaison Sessions valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   > * L&#39;intégration entre les `serviceProvider` et `mvpd` fournis doit être active.
   > 
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../enhanced-error-codes.md).

1. **Passez aux flux de décisions :** La réponse du point de terminaison sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur &quot;autoriser&quot;.
   * L’attribut `actionType` est défini sur &quot;direct&quot;.

   Si le serveur principal Adobe Pass identifie un profil valide, l’application en continu n’a pas besoin de se réauthentifier auprès du MVPD sélectionné, car un profil peut déjà être utilisé pour les flux de décisions suivants.

1. **Ouvrir l’URL dans l’agent utilisateur :** La réponse du point de terminaison sessions contient les données suivantes :
   * `url` qui peut être utilisé pour lancer l’authentification interactive dans la page de connexion MVPD.
   * L’attribut `actionName` est défini sur &quot;authenticate&quot;.
   * L’attribut `actionType` est défini sur &quot;interactif&quot;.

   Si le serveur principal Adobe Pass n’identifie pas de profil valide, l’application de diffusion en continu ouvre un agent utilisateur pour charger le `url` fourni, effectuant une requête sur le point de terminaison Authentifier . Ce flux peut inclure plusieurs redirections, ce qui conduit finalement l’utilisateur à la page de connexion MVPD et à fournir des informations d’identification valides.

1. **Authentification MVPD complète :** Si le flux d’authentification est réussi, l’interaction de l’agent utilisateur enregistre un profil régulier dans le serveur principal Adobe Pass et atteint le `redirectUrl` fourni.

1. **Récupérer le profil pour un code spécifique :** L’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil en envoyant une requête au point de terminaison Profiles.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
   >
   > * Tous les paramètres _requis_, comme `serviceProvider`, `code`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_

   >[!NOTE]
   >
   > Suggestion : l’application de diffusion en continu peut attendre que l’agent utilisateur atteigne le `redirectUrl` fourni pour vérifier si le profil normal a été généré et enregistré avec succès.

1. **Renvoi d’informations sur le profil normal :** La réponse du point de terminaison Profiles contient des informations sur le profil normal associé aux paramètres et en-têtes reçus.

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
