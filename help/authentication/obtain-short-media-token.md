---
title: Obtention d’un jeton de média court
description: Obtention d’un jeton multimédia court
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# Obtention d’un jeton de média court {#obtain-short-media-token}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par le [mécanisme de limitation](/help/authentication/throttling-mechanism.md)

## Points de terminaison de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Évaluation - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Évaluation - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Obtient Un Jeton De Média Court.

| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken</br></br> ou</br></br>&lt;SP_FQDN>/api/v1/tokens/media</br></br>Par exemple :</br></br>&lt;SP_FQDN>/api/v1/tokens/media | Application de diffusion en continu</br></br>ou</br></br>Service de programmation | 1. demandeur (obligatoire)</br>2.  deviceId (obligatoire)</br>3.  resource (obligatoire)</br>4.  device_info/X-Device-Info (obligatoire)</br>5.  _deviceType_</br> 6.  _deviceUser_ (obsolète)</br>7.  _appId_ (obsolète) | GET | XML ou JSON contenant un jeton de média codé Base64 ou des détails d’erreur en cas d’échec. | 200 - Succès </br>403 - Pas de succès |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’identifiant de l’appareil. |
| resource | Chaîne contenant un resourceId (ou fragment MRSS), identifiant le contenu demandé par un utilisateur et reconnu par les points de terminaison d’autorisation MVPD. |
| device_info/</br></br>X-Device-Info | Informations sur les périphériques de diffusion en continu.</br></br>**Remarque** : cette variable peut être transmise à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de longueur d’une URL de GET, elle doit être transmise sous la forme X-Device-Info dans l’en-tête http. </br></br>Pour plus d&#39;informations, reportez-vous à la section [Transmission des informations de périphérique et de connexion](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Type d’appareil (par exemple, Roku, PC).</br></br>Si ce paramètre est défini correctement, ESM offre des mesures [ventilées par type d’appareil]/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyses puissent être effectués. Par exemple, Roku, Apple TV et Xbox.</br></br>Voir [Avantages de l’utilisation du paramètre de type de périphérique sans client ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Remarque** : device_info remplace ce paramètre. |
| _deviceUser_ | Identifiant de l’utilisateur de l’appareil.</br></br>**Remarque** : Si elle est utilisée, deviceUser doit avoir les mêmes valeurs que dans la requête [Create Registration Code](/help/authentication/registration-code-request.md) . |
| _appId_ | ID/nom de l’application. </br></br>**Remarque** : device_info remplace ce paramètre. S’il est utilisé, `appId` doit avoir les mêmes valeurs que dans la requête [Create Registration Code](/help/authentication/registration-code-request.md) . |

{style="table-layout:auto"}

### Exemple de réponse {#response}

**XML :**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <play>
        <expires>1348649569000</expires>
        <mvpdId>sampleMvpdId</mvpdId>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <serializedToken>PHNpZ25hdH[...]</serializedToken>
        <userId>sampleScrambledUserId</userId>
    </play>
```



**JSON:**

```JSON
    {
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348649614000",
        "serializedToken": "PHNpZ25hdH[...]",
        "userId": "sampleScrambledUserId",
        "mvpdId": "sampleMvpdId"
    }
```



### Compatibilité de la bibliothèque de vérification multimédia

Le champ `serializedToken` de l’appel &quot;Obtenir un jeton de média court&quot; est un jeton codé en base64 qui peut être vérifié par rapport à la bibliothèque de vérification Adobe Medium.
