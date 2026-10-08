---
title: Échanger un jeton SSO Platform contre un jeton Adobe
description: Échanger un jeton SSO Platform contre un jeton Adobe
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 3%
---
# (Hérité) Échanger un jeton SSO Platform contre un jeton Adobe {#exchange-a-platform-sso-token-for-an-adobe-token}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

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

Permet l’« échange » d’un profil SSO de Platform contre un jeton Adobe.

| Point d’entrée | Appelé </br>Par | Input </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn | Service de programmation</br></br>ou</br></br>d’application en flux continu | &#x200B;1.  demandeur (obligatoire)</br>    </br>2.  deviceId (obligatoire)</br>    </br>3.  mvpd (obligatoire)</br>    </br>4.  deviceType (obligatoire)</br>    </br>5.  SAMLResponse (obligatoire)</br>    </br>6.  deviceUser (obsolète)</br>    </br>7.  appId (obsolète) | POSTER | La réponse réussie sera un 204 No Content, indiquant que le jeton a été créé avec succès et est prêt à être utilisé pour les flux authz. | 204 - Aucun contenu </br>400 - Requête incorrecte |


| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’ID de l’appareil. |
| mvpd | Identifiant MVPD pour lequel cette opération est valide. |
| deviceType | Plateforme Apple pour laquelle nous tentons d’obtenir une requête de profil.  Soit **** soit **tvOS**. |
| SAMLResponse | Profil réel renvoyé par l’authentification unique de Platform. |
| _deviceUser_ | Identifiant utilisateur de l’appareil. |
| _appId_ | Nom/ID de l’application. |
