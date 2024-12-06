---
title: Authentification de base - Application Secondaire - Flux
description: API REST V2 - Authentification de base - Application Secondaire - Flux
exl-id: 83bf592e-c679-4cfe-984d-710a9598c620
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2000'
ht-degree: 0%

---

# Flux d’authentification de base exécuté sur une application secondaire {#basic-authentication-flow-performed-within-secondary-application}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md) .

Le **flux d’authentification** dans le droit d’authentification Adobe Pass permet à l’application de diffusion en continu de vérifier qu’un utilisateur dispose d’un compte MVPD valide. Ce processus nécessite que l’utilisateur dispose d’un compte MVPD actif et qu’il saisisse des informations de connexion valides sur la page de connexion MVPD.

Le flux d’authentification est requis dans les cas suivants :

* Lorsque l’utilisateur ouvre une application pour la première fois.
* Lorsque l’authentification précédente de l’utilisateur a expiré.
* Lorsque l’utilisateur se déconnecte du compte MVPD.
* Lorsque l’utilisateur souhaite s’authentifier avec un MVPD différent.

Dans tous ces cas, l’application qui appelle l’un des points de terminaison Profiles reçoit une réponse vide ou un ou plusieurs profils, mais pour différents MVPD.

Le **flux d’authentification** nécessite qu’un agent utilisateur (navigateur) effectue une série d’appels depuis l’application vers le serveur principal Adobe Pass, puis vers la page de connexion MVPD et enfin vers l’application. Ce flux peut inclure plusieurs redirections vers les systèmes MVPD et la gestion des cookies ou sessions stockés pour chaque domaine, ce qui peut s’avérer difficile à réaliser et à sécuriser sans agent utilisateur.

En fonction des fonctionnalités de l’application principale (application de diffusion en continu) pour prendre en charge l’interaction utilisateur afin de sélectionner un MVPD et de s’authentifier auprès du MVPD sélectionné dans un agent utilisateur, les scénarios d’authentification sont les suivants :

* [Effectuer l’authentification dans l’application principale](rest-api-v2-basic-authentication-primary-application-flow.md)
* [Effectuez l’authentification dans une application secondaire avec mvpd présélectionné.](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Effectuer une authentification dans une application secondaire sans mvpd présélectionné](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## Effectuez l’authentification dans une application secondaire avec mvpd présélectionné. {#perform-authentication-within-secondary-application-with-preselected-mvpd}

### Conditions préalables {#prerequisites-perform-authentication-within-secondary-application-with-preselected-mvpd}

Avant de démarrer le flux d’authentification dans une application principale et de le terminer par une interaction de l’utilisateur dans une application secondaire, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit sélectionner un MVPD.
* L’application de diffusion en continu doit lancer une session d’authentification pour se connecter avec le MVPD sélectionné.
* L’application secondaire doit s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * L’application de diffusion en continu prend en charge l’interaction utilisateur pour sélectionner un MVPD.
> * L’application secondaire (généralement sur un périphérique secondaire) prend en charge l’interaction de l’utilisateur pour s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.

### Workflow {#workflow-perform-authentication-within-secondary-application-with-preselected-mvpd}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux d’authentification de base effectué dans une application secondaire avec un MVPD présélectionné, comme illustré dans le diagramme ci-dessous.

![ Effectuez une authentification dans une application secondaire avec mvpd présélectionné](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-with-preselected-mvpd.png)

*Effectuez une authentification dans une application secondaire avec mvpd présélectionné*

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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Passez aux flux de décisions :** La réponse du point de terminaison sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur &quot;autoriser&quot;.
   * L’attribut `actionType` est défini sur &quot;direct&quot;.

   Si le serveur principal Adobe Pass identifie un profil valide, l’application en continu n’a pas besoin de se réauthentifier auprès du MVPD sélectionné, car un profil peut déjà être utilisé pour les flux de décisions suivants.

1. **Afficher le code d’authentification :** La réponse du point de terminaison sessions contient les données suivantes :
   * `code` qui peut être utilisé pour reprendre la session d’authentification dans une application secondaire.
   * L’attribut `actionName` est défini sur &quot;authenticate&quot;.
   * L’attribut `actionType` est défini sur &quot;interactif&quot;.

   Si le serveur principal Adobe Pass n’identifie pas de profil valide, l’application en continu affiche le `code` qui peut être utilisé pour reprendre la session d’authentification dans une application secondaire.

1. **Valider le code d’authentification :** L’application secondaire valide l’utilisateur fourni `code` pour s’assurer qu’il peut procéder à l’authentification MVPD dans l’agent utilisateur.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
   >
   > * Tous les paramètres _requis_, comme `serviceProvider` et `code`
   > * Tous les en-têtes _requis_, comme `Authorization`
   > * Tous les paramètres et en-têtes _optional_

1. **Renseignez les informations sur la session d’authentification :** La réponse du point de terminaison sessions contient les données suivantes :
   * L’attribut `existing` contient les paramètres existants qui ont déjà été fournis.
   * L’attribut `missing` contient les paramètres manquants qui doivent être fournis pour terminer le flux d’authentification.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de validation de session, reportez-vous à la documentation de l’API [Récupérer les informations de session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) .
   >
   > <br/>
   >
   > Le point de terminaison Sessions valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   >
   > <br/>
   >
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!TIP]
   >
   > Suggestion : l’application secondaire peut informer les utilisateurs que le `code` utilisé n’est pas valide en cas de réponse d’erreur indiquant une session d’authentification manquante et leur conseiller de réessayer d’en utiliser une nouvelle.

1. **Ouvrir l’URL dans l’agent utilisateur :** L’application secondaire ouvre un agent utilisateur pour charger l’auto-calcul `url`, effectuant une requête sur le point de terminaison Authentifier. Ce flux peut inclure plusieurs redirections, ce qui conduit finalement l’utilisateur à la page de connexion MVPD et à fournir des informations d’identification valides.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
   >
   > * Tous les paramètres _requis_, comme `serviceProvider` et `code`
   > * Tous les paramètres et en-têtes _optional_

1. **Authentification MVPD complète :** Si le flux d’authentification est réussi, l’interaction de l’agent utilisateur enregistre un profil régulier dans le serveur principal Adobe Pass et atteint le `redirectUrl` fourni.

1. **Récupérer le profil pour un code spécifique :** L’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil en envoyant une requête au point de terminaison Profiles.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
   > 
   > * Tous les paramètres _requis_, comme `serviceProvider` et `code`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_

   >[!TIP]
   >
   > Suggestion : l’application en continu peut mettre en oeuvre un mécanisme d’interrogation à l’aide de `code` pour vérifier si le profil normal a bien été généré et enregistré.

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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../../features-standard/error-reporting/enhanced-error-codes.md).

## Effectuer une authentification dans une application secondaire sans mvpd présélectionné {#perform-authentication-within-secondary-application-without-preselected-mvpd}

### Conditions préalables {#prerequisites-perform-authentication-within-secondary-application-without-preselected-mvpd}

Avant de démarrer le flux d’authentification dans une application principale et de le terminer par une interaction de l’utilisateur dans une application secondaire, assurez-vous que les conditions préalables suivantes sont remplies :

* L’application de diffusion en continu doit lancer une session d’authentification lorsqu’elle doit se connecter.
* L’application secondaire doit sélectionner un MVPD.
* L’application secondaire doit s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
> 
> * L’application secondaire (généralement sur un périphérique secondaire) prend en charge l’interaction de l’utilisateur pour sélectionner un MVPD.
> * L’application secondaire (généralement sur un périphérique secondaire) prend en charge l’interaction de l’utilisateur pour s’authentifier auprès du MVPD sélectionné dans un agent utilisateur.

### Workflow {#workflow-perform-authentication-within-secondary-application-without-preselected-mvpd}

Suivez les étapes ci-dessous pour mettre en oeuvre le flux d’authentification de base effectué dans une application secondaire sans MVPD présélectionné, comme illustré dans le diagramme ci-dessous.

![ Effectuez une authentification dans une application secondaire sans mvpd présélectionné](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-without-preselected-mvpd.png)

*Effectuez une authentification dans une application secondaire sans mvpd présélectionné*

1. **Créer une session d’authentification :** L’application de diffusion en continu rassemble certaines des données nécessaires pour lancer une session d’authentification en appelant le point de terminaison sessions .

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
   >
   > * Tous les paramètres _requis_, comme `serviceProvider`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_
   >
   > <br/>
   > 
   > L’application de diffusion en continu ne peut pas fournir tous les paramètres requis dans un seul appel lors de la création de la session d’authentification.

1. **Indiquez l’action suivante :** La réponse du point de terminaison sessions contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante :
   * `code` qui peut être utilisé pour reprendre la session d’authentification dans une application secondaire.
   * L’attribut `actionName` est défini sur &quot;resume&quot;.
   * L’attribut `actionType` est défini sur &quot;direct&quot;.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de session, reportez-vous à la documentation de l’API [Créer une session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).
   > 
   > <br/>
   > 
   > Le point de terminaison Sessions valide les données de requête pour s’assurer que les conditions de base sont remplies :
   >
   > * Les paramètres et en-têtes _required_ doivent être valides.
   >
   > <br/>
   > 
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Afficher le code d’authentification :** L’application en continu affiche le `code` qui peut être utilisé pour reprendre la session d’authentification dans une application secondaire.

1. **Fournir une session d’authentification Paramètres manquants :** L’application secondaire rassemble toutes les données manquantes requises pour reprendre la session d’authentification et appelle le point de terminaison sessions.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
   >
   > * Tous les paramètres _required_, tels que `serviceProvider`, `mvpd`, `domainName` et `redirectUrl`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_

1. **Indiquez l’action suivante :** La réponse du point de terminaison sessions contient les données nécessaires pour guider l’application de diffusion en continu concernant l’action suivante.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur les informations fournies dans une réponse de session, reportez-vous à la documentation de l’API [Reprendre la session d’authentification](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md).
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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!TIP]
   >
   > Suggestion : l’application secondaire peut informer les utilisateurs que le `code` utilisé n’est pas valide en cas de réponse d’erreur indiquant une session d’authentification manquante et leur conseiller de réessayer d’en utiliser une nouvelle.

1. **Indique le profil existant :** La réponse du point de terminaison sessions contient les données suivantes :
   * L’attribut `actionName` est défini sur &quot;autoriser&quot;.
   * L’attribut `actionType` est défini sur &quot;direct&quot;.

   Si le serveur principal Adobe Pass identifie un profil valide, l’application en continu n’a pas besoin de se réauthentifier auprès du MVPD sélectionné, car un profil peut déjà être utilisé pour les flux de décisions suivants.

1. **Ouvrir l’URL dans l’agent utilisateur :** La réponse du point de terminaison sessions contient les données suivantes :
   * `url` qui peut être utilisé pour lancer l’authentification interactive dans la page de connexion MVPD.
   * L’attribut `actionName` est défini sur &quot;authenticate&quot;.
   * L’attribut `actionType` est défini sur &quot;interactif&quot;.

   Si le serveur principal Adobe Pass n’identifie pas de profil valide, l’application secondaire ouvre un agent utilisateur pour charger le `url` fourni, effectuant une requête sur le point de terminaison Authenticate (Authentifier). Ce flux peut inclure plusieurs redirections, ce qui conduit finalement l’utilisateur à la page de connexion MVPD et à fournir des informations d’identification valides.

1. **Authentification MVPD complète :** Si le flux d’authentification est réussi, l’interaction de l’agent utilisateur enregistre un profil régulier dans le serveur principal Adobe Pass et atteint le `redirectUrl` fourni.

1. **Récupérer le profil pour un code spécifique :** L’application de diffusion en continu rassemble toutes les données nécessaires pour récupérer les informations de profil en envoyant une requête au point de terminaison Profiles.

   >[!IMPORTANT]
   >
   > Pour plus d’informations sur :[](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
   >
   > * Tous les paramètres _requis_, comme `serviceProvider` et `code`
   > * Tous les en-têtes _requis_, comme `Authorization`, `AP-Device-Identifier`
   > * Tous les paramètres et en-têtes _optional_

   >[!TIP]
   >
   > Suggestion : l’application en continu peut mettre en oeuvre un mécanisme d’interrogation à l’aide de `code` pour vérifier si le profil normal a bien été généré et enregistré.

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
   > Si la validation échoue, une réponse d’erreur est générée, fournissant des informations supplémentaires conformes à la documentation [Enhanced Error Codes](../../../../features-standard/error-reporting/enhanced-error-codes.md).
