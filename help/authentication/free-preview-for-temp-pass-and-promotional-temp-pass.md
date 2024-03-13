---
title: Aperçu gratuit pour la transmission temporaire et la transmission temporaire promotionnelle
description: Aperçu gratuit pour la transmission temporaire et la transmission temporaire promotionnelle
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Aperçu gratuit pour la transmission temporaire et la transmission temporaire promotionnelle {#free-preview-for-temp-pass-and-promotional-temp-pass}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par [Mécanisme de ralentissement](/help/authentication/throttling-mechanism.md)

## Points de terminaison de l’API REST {#clientless-endpoints}

&lt;reggie_fqdn>:

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Permet la création d’un jeton d’authentification pour la transmission temporaire et la transmission temporaire promotionnelle sans qu’il faille passer par un second écran.


| Point d’entrée | Appelé  </br>Par | Entrée   </br>Paramètres | HTTP  </br>Méthode | Réponse | HTTP  </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate/freepreview | Application de diffusion en continu</br></br>ou</br></br>Service de programmation | 1. requestor_id (obligatoire)</br>    </br>2.  deviceId (obligatoire)</br>    </br>3.  mso_id (obligatoire)</br>    </br>4.  domain_name (obligatoire)</br>    </br>5.  device_info/X-Device-Info (obligatoire)</br>6.  deviceType</br>    </br>7.  deviceUser (obsolète)</br>    </br>8.  appId (obsolète)</br>    </br>9.  generic_data (facultatif) | POST | La réponse réussie sera un &quot;No Content&quot; 204, indiquant que le jeton a été créé avec succès et qu’il est prêt à être utilisé pour les flux de création. | 204 - Aucun contenu   </br>400 - Mauvaise requête |

<div>


| Paramètre d’entrée | Description |
| --- | --- |
| requestor_id | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’identifiant de l’appareil. |
| mso_id | Identifiant MVPD pour lequel cette opération est valide. |
| domain_name | Nom de domaine pour lequel un jeton sera accordé. Ceci est comparé aux domaines du prestataire lors de l&#39;octroi d&#39;un jeton d&#39;autorisation. |
| device_info/</br></br>X-Device-Info | Informations sur les périphériques de diffusion en continu.</br></br>**Remarque**: cette variable peut être transmise à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de longueur d’une URL de GET, elle doit être transmise sous la forme X-Device-Info dans l’en-tête http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Type d’appareil (par exemple, Roku, PC).</br></br>Si ce paramètre est correctement défini, ESM propose des mesures qui sont [ventilation par type d’appareil](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyses puissent être effectués pour Roku, Apple TV, Xbox, etc.</br></br>Voir [Avantages de l’utilisation de paramètres de type d’appareil sans client ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Remarque**: device_info remplace ce paramètre. |
| _deviceUser_ | Identifiant de l’utilisateur de l’appareil.</br></br>**Remarque**: s’il est utilisé, deviceUser doit avoir les mêmes valeurs que dans la variable [Créer un code d’enregistrement](/help/authentication/registration-code-request.md) requête. |
| _appId_ | ID/nom de l’application. </br></br>**Remarque**: device_info remplace ce paramètre. En cas d’utilisation, `appId` doivent avoir les mêmes valeurs que dans la variable [Créer un code d’enregistrement](/help/authentication/registration-code-request.md) requête. |
| generic_data | Utilisé pour limiter la portée du jeton pour la transmission temporaire promotionnelle. |


### [Retour à la référence de l’API REST](/help/authentication/rest-api-reference.md)
