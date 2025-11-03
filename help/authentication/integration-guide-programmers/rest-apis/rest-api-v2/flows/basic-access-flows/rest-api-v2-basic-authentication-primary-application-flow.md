---
title: Authentification de base - Application en Principal - Flux
description: API REST V2 - Authentification de base - Application en Principal - Flux
exl-id: 8122108d-e9da-43c5-9abb-ab177cb21eb6
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Flux d’authentification de base effectué dans l’application principale {#basic-authentication-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Veillez également à consulter la [FAQ sur l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

Le **flux d’authentification** dans les droits d’authentification Adobe Pass permet à l’application de diffusion en continu de vérifier qu’un utilisateur dispose d’un compte MVPD valide. Pour ce faire, l’utilisateur doit disposer d’un compte MVPD actif et saisir des informations de connexion valides sur la page de connexion de MVPD.

Le flux d’authentification est requis dans les cas suivants :

* Lorsque l’utilisateur ouvre une application pour la première fois.
* Lorsque l’authentification précédente de l’utilisateur a expiré.
* Lorsque l’utilisateur se déconnecte du compte MVPD.
* Lorsque l’utilisateur souhaite s’authentifier avec un autre MVPD.

Dans tous ces cas, l’application appelant l’un des points d’entrée de profils reçoit une réponse vide pour un ou plusieurs profils, mais pour différents MVPD.

Le **flux d’authentification** nécessite qu’un agent utilisateur (navigateur) effectue une série d’appels de l’application vers le serveur principal Adobe Pass, puis vers la page de connexion MVPD, et enfin vers l’application. Ce flux peut inclure plusieurs redirections vers les systèmes MVPD et la gestion des cookies ou des sessions stockés pour chaque domaine, ce qui peut être difficile à réaliser et à sécuriser sans agent utilisateur.

En fonction des fonctionnalités de l’application principale (application de diffusion en continu) permettant de prendre en charge l’interaction des utilisateurs pour sélectionner un MVPD et s’authentifier avec le MVPD sélectionné dans un agent utilisateur, les scénarios d’authentification sont les suivants :

* [Authentification dans l’application principale](./rest-api-v2-basic-authentication-primary-application-flow.md)
* [Authentification dans l’application secondaire avec mvpd présélectionné](rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Effectuer l’authentification dans l’application secondaire sans mvpd présélectionné](rest-api-v2-basic-authentication-secondary-application-flow.md)

## Authentification dans l’application principale {#perform-authentication-within-primary-application}

### Conditions préalables {#prerequisites-perform-authentication-within-primary-application}

Avant d’effectuer l’authentification par le biais de l’interaction utilisateur dans une application principale, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit sélectionner un MVPD.
* L’application de diffusion en continu doit lancer une session d’authentification pour se connecter avec le MVPD sélectionné.
* L’application de diffusion en continu doit s’authentifier avec le MVPD sélectionné dans un agent utilisateur.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * L’application de diffusion en continu prend en charge l’interaction utilisateur pour sélectionner un MVPD.
> * L’application de diffusion en continu prend en charge l’interaction utilisateur pour l’authentification avec le MVPD sélectionné dans un agent utilisateur.

### Workflow {#workflow-perform-authentication-completed-on-primary-application}

Suivez les étapes données pour implémenter le flux d’authentification de base effectué dans une application principale, comme illustré dans le diagramme suivant.

![Effectuer l’authentification dans l’application principale](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-primary-application.png)

*Effectuer l’authentification dans l’application principale*

1. **Créer une session d’authentification :** l’application de diffusion en continu rassemble toutes les données nécessaires pour lancer une session d’authentification en appelant le point d’entrée Sessions.

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Créer une session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) pour plus d’informations sur :
   > 
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `mvpd`, `domainName` et `redirectUrl`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes
   > 
   > <br/>
   > 
   > L’application de diffusion en continu doit fournir tous les paramètres requis en un seul appel lors de la création de la session d’authentification.

1. **Indiquez l’action suivante :** la réponse du point d’entrée des sessions contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante.

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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires qui sont conformes à la documentation [Codes d’erreur améliorés](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Poursuivre avec les flux de décisions :** la réponse du point d’entrée des sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur « autoriser ».
   * L’attribut `actionType` est défini sur « direct ».

   Si le serveur principal Adobe Pass identifie un profil valide, l’application de diffusion en continu n’a pas besoin de s’authentifier à nouveau avec le MVPD sélectionné, car il existe déjà un profil qui peut être utilisé pour les flux de décisions suivants.

1. **Ouvrir l’URL dans l’agent utilisateur :** la réponse de point d’entrée des sessions contient les données suivantes :
   * `url` qui peut être utilisé pour lancer l’authentification interactive dans la page de connexion de MVPD.
   * L’attribut `actionName` est défini sur « authentifier ».
   * L’attribut `actionType` est défini sur « interactif ».

   Si le serveur principal d’Adobe Pass n’identifie pas de profil valide, l’application de diffusion en continu ouvre un agent utilisateur pour charger le `url` fourni, envoyant une requête au point d’entrée Authentifier. Ce flux peut inclure plusieurs redirections et conduire finalement l’utilisateur à la page de connexion de MVPD et fournir des informations d’identification valides.

1. **Terminer l’authentification MVPD :** si le flux d’authentification est réussi, l’interaction de l’agent utilisateur enregistre un profil standard sur le serveur principal Adobe Pass et atteint le `redirectUrl` fourni.

1. **Récupérer le profil pour un code spécifique :** l’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil en envoyant une requête au point d’entrée Profils .

   >[!IMPORTANT]
   >
   > Consultez la documentation de l’API [Récupérer le profil pour un code spécifique](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) pour plus d’informations sur :
   >
   > * Tous les paramètres _obligatoires_ tels que `serviceProvider`, `code`
   > * Tous les en-têtes _obligatoires_ tels que `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres _facultatifs_ et en-têtes

   >[!TIP]
   >
   > L’application de diffusion en continu doit attendre que l’agent utilisateur atteigne le `redirectUrl` fourni pour vérifier si le profil standard a bien été généré et enregistré.

1. **Renvoyer des informations sur le profil normal :** la réponse de point d’entrée des profils contient des informations sur le profil normal associé aux paramètres et en-têtes reçus.

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
