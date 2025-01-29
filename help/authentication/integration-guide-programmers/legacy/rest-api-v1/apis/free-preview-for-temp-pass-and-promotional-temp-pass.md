---
title: Aperçu gratuit pour Temp Pass et Promotionnel Temp Pass
description: Aperçu gratuit pour Temp Pass et Promotionnel Temp Pass
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# (Hérité) Aperçu gratuit pour Temp Pass et Promotionnel Temp Pass {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

Permet la création d’un jeton d’authentification pour la passe temporaire et la passe temporaire promotionnelle sans avoir besoin d’un deuxième écran.


| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | Service de programmation</br></br>ou</br></br>d’application en flux continu | 1. requestor_id (obligatoire)</br>    </br>2.  deviceId (obligatoire)</br>    3 </br>.  mso_id (obligatoire)</br>    4 </br>.  domain_name (obligatoire)</br>    5 </br>.  device_info/X-Device-Info (obligatoire)</br>6.  deviceType </br>    7 </br>.  deviceUser (obsolète)</br>    8 </br>.  appId (obsolète)</br>    9 </br>.  generic_data (facultatif) | POST | La réponse réussie sera un 204 No Content, indiquant que le jeton a été créé avec succès et est prêt à être utilisé pour les flux authz. | 204 - Aucun contenu   </br>400 - Requête incorrecte |

<div>


| Paramètre d’entrée | Description |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| requestor_id | ID de demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’ID de l’appareil. |
| mso_id | Identifiant MVPD pour lequel cette opération est valide. |
| domain_name | Nom de domaine pour lequel un jeton sera accordé. Cette valeur est comparée aux domaines du fournisseur de services lorsqu’un jeton d’autorisation est accordé. |
| device_info/</br></br>X-Device-Info | Informations sur l’appareil de diffusion en continu.</br></br>**Remarque** : cela PEUT être transmis à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de la longueur d’une URL de GET, il DOIT être transmis en tant que X-Device-Info dans l’en-tête http. </br></br>Voir les détails complets dans [Transmettre les informations sur l’appareil et la connexion](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Type d’appareil (par exemple Roku, PC).</br></br>Si ce paramètre est défini correctement, ESM propose des mesures [ventilées par type d’appareil](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyse puissent être effectués pour Roku, AppleTV, Xbox, etc.</br></br>Voir [Avantages de l’utilisation de paramètres de type d’appareil sans client ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Remarque** : device_info remplacera ce paramètre. |
| _deviceUser_ | Identifiant utilisateur de l’appareil.</br></br>**Remarque** : si cette option est utilisée, deviceUser doit avoir les mêmes valeurs que dans la requête [Créer un code d’enregistrement](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | Nom/ID de l’application. </br></br>**Remarque** : device_info remplace ce paramètre. S’il est utilisé, `appId` doit avoir les mêmes valeurs que dans la requête [Créer un code d’enregistrement](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| generic_data | Utilisé pour limiter la portée du jeton pour la passe temporaire promotionnelle. |


### [Retour à la référence de l’API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
