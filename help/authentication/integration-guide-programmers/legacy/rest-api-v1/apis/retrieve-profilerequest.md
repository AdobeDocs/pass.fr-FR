---
title: Récupération de la requête de profil SSO Platform
description: Récupération de la requête de profil SSO Platform
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 1%

---

# Récupération de la requête de profil SSO Platform {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par le [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Points de terminaison de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Cette ressource génère des requêtes de profil pour un ID de demandeur et un tuple MVPD.


| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | Application de diffusion en continu</br></br>ou</br></br>Service de programmation | 1. demandeur (paramètre de chemin)</br>2. mvpd (paramètre de chemin)</br>3. deviceType (obligatoire) | GET | La réponse Content-Type est application/octet-stream, car la charge utile réelle est opaque pour l’application cliente.</br></br>La réponse doit être transmise par l’application au moteur d’authentification unique de Platform</br></br>pour obtenir une authentification unique de profil. | 200 - Succès   </br>400 - Mauvaise requête |


| Paramètre d’entrée | Description |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| mvpd | Identifiant MVPD pour lequel cette opération est valide. |
| deviceType | Plateforme Apple pour laquelle nous tentons d’obtenir une requête de profil.  **iOS** ou **tvOS**. |
