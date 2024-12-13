---
title: Récupérer la demande de profil SSO de Platform
description: Récupérer la demande de profil SSO de Platform
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---

# (Hérité) Récupérer la requête de profil SSO de Platform {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Points d’entrée de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Cette ressource génère des requêtes de profil pour un ID de demandeur et un tuple MVPD.


| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | Service de programmation</br></br>ou</br></br>d’application en flux continu | 1. demandeur (paramètre de chemin)</br>2. mvpd (paramètre de chemin)</br>3. deviceType (obligatoire) | GET | La réponse Content-Type sera application/octet-stream, car la payload réelle est opaque pour l&#39;application cliente.</br></br>La réponse doit être transmise par l’application au moteur Platform</br></br>SSO pour obtenir une authentification unique (SSO) de profil. | 200 - Succès   </br>400 - Requête incorrecte |


| Paramètre d’entrée | Description |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| mvpd | Identifiant MVPD pour lequel cette opération est valide. |
| deviceType | Plateforme Apple pour laquelle nous tentons d’obtenir une requête de profil.  Soit **iOS** soit **tvOS**. |
