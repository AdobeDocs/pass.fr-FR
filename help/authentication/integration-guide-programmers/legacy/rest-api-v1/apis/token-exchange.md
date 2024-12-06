---
title: Exchange d’un jeton SSO Platform pour un jeton d’Adobe
description: Exchange d’un jeton SSO Platform pour un jeton d’Adobe
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Exchange d’un jeton SSO Platform pour un jeton d’Adobe {#exchange-a-platform-sso-token-for-an-adobe-token}

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

Permet d’échanger un profil d’authentification unique Platform contre un jeton d’Adobe.

| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn | Application de diffusion en continu</br></br>ou</br></br>Service de programmation | 1. demandeur (obligatoire)</br>    </br>2.  deviceId (obligatoire)</br>    </br>3.  mvpd (obligatoire)</br>    </br>4.  deviceType (obligatoire)</br>    </br>5.  SAMLResponse (obligatoire)</br>    </br>6.  deviceUser (obsolète)</br>    </br>7.  appId (obsolète) | POST | La réponse réussie sera un &quot;No Content&quot; 204, indiquant que le jeton a été créé avec succès et qu’il est prêt à être utilisé pour les flux de création. | 204 - Aucun contenu   </br>400 - Mauvaise requête |


| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’identifiant de l’appareil. |
| mvpd | Identifiant MVPD pour lequel cette opération est valide. |
| deviceType | Plateforme Apple pour laquelle nous tentons d’obtenir une requête de profil.  **iOS** ou **tvOS**. |
| SAMLResponse | Le profil réel tel que renvoyé par la connexion unique à Platform. |
| _deviceUser_ | Identifiant de l’utilisateur de l’appareil. |
| _appId_ | ID/nom de l’application. |
