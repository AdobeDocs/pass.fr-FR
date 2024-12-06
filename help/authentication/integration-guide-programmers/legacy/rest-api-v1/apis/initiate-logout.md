---
title: Lancer la déconnexion
description: Lancement de la déconnexion
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 1%

---

# Lancer la déconnexion {#initiate-logout}

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

Supprimez les jetons AuthN et AuthZ du stockage.


| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/logout | Application de diffusion en continu</br></br>ou</br></br>Service de programmation | 1. demandeur</br>2.  deviceId (obligatoire)</br>3.  device_info/X-Device-Info (obligatoire)</br>4.  _deviceType_</br> 5.  _deviceUser_ (obsolète)</br>6.  _appId_ (obsolète) | DELETE | Aucun | 204 |


| Paramètre d’entrée | Description |
|-------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’identifiant de l’appareil. |
| device_info/</br></br>X-Device-Info | Informations sur les périphériques de diffusion en continu.</br></br>**Remarque** : cette variable peut être transmise à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de longueur d’une URL de GET, elle doit être transmise sous la forme X-Device-Info dans l’en-tête http. </br></br>Pour plus d&#39;informations, reportez-vous à la section [Transmission des informations de périphérique et de connexion](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Type d’appareil (par exemple, Roku, PC).</br></br>Si ce paramètre est défini correctement, ESM offre des mesures [ventilées par type d’appareil](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyses puissent être effectués pour Roku, AppleTV, Xbox, etc.</br></br>Voir [Avantages de l’utilisation du paramètre de type d’appareil sans client dans les mesures de transmission ](/help/authentication/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Remarque** : l’info_périphérique remplacera ce paramètre. |
| _deviceUser_ | Identifiant de l’utilisateur de l’appareil.</br></br>**Remarque** : Si elle est utilisée, deviceUser doit avoir les mêmes valeurs que dans la requête [Create Registration Code](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) . |
| _appId_ | ID/nom de l’application. </br></br>**Remarque** : device_info remplace ce paramètre. S’il est utilisé, `appId` doit avoir les mêmes valeurs que dans la requête [Create Registration Code](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) . |

>[!IMPORTANT]
> 
>L’appel de déconnexion présente actuellement la limitation suivante : il efface les jetons AuthN et AuthZ du stockage (c’est-à-dire, côté Programmeur/Authentification Adobe Pass), mais **n’appelle pas** le point de terminaison de déconnexion MVPD.
