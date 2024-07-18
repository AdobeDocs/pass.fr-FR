---
title: Lancer l’autorisation
description: Lancer l’autorisation
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# Lancer l’autorisation {#initiate-authorization}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par le [mécanisme de limitation](/help/authentication/throttling-mechanism.md)

## Points de terminaison de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Obtient la réponse d’autorisation.

| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/allow | Application de diffusion en continu</br></br>ou</br></br>Service de programmation | 1. demandeur (obligatoire)</br>2.  deviceId (obligatoire)</br>3.  resource (obligatoire)</br>4.  device_info/X-Device-Info (obligatoire)</br>5.  _deviceType_</br> 6.  _deviceUser_ (obsolète)</br>7.  _appId_ (obsolète)</br>8.  Paramètres supplémentaires (facultatif) | GET | XML ou JSON contenant les détails de l’autorisation ou les détails de l’erreur en cas d’échec. Voir les exemples ci-dessous. | 200 - Succès </br>403 - Pas de succès |

{style="table-layout:auto"}

</br>


| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’identifiant de l’appareil. |
| resource | Chaîne contenant un resourceId (ou fragment MRSS), identifiant le contenu demandé par un utilisateur et reconnu par les points de terminaison d’autorisation MVPD. |
| device_info/</br></br>X-Device-Info | Informations sur les périphériques de diffusion en continu.</br></br>**Remarque** : cette variable peut être transmise à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de longueur d’une URL de GET, elle doit être transmise sous la forme X-Device-Info dans l’en-tête http. </br></br>Pour plus d&#39;informations, reportez-vous à la section [Transmission des informations de périphérique et de connexion](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Type d’appareil (par exemple, Roku, PC).</br></br>Si ce paramètre est défini correctement, ESM offre des mesures [ventilées par type d’appareil](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyses puissent être effectués pour Roku, AppleTV, Xbox, etc.</br></br>Voir [Avantages du paramètre de type d’appareil sans client dans les mesures de transmission ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Remarque** : l’info_périphérique remplacera ce paramètre. |
| _deviceUser_ | Identifiant de l’utilisateur de l’appareil. |
| _appId_ | ID/nom de l’application. </br></br>**Remarque** : device_info remplace ce paramètre. |
| paramètres supplémentaires | L’appel peut également contenir des paramètres facultatifs qui activent d’autres fonctionnalités telles que :</br></br>* generic_data - permet l’utilisation de [PromotiontempPass](/help/authentication/promotional-temp-pass.md)</br></br>Exemple : `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Adresse IP du périphérique de diffusion en continu**</br>
>Pour les mises en oeuvre client-serveur, l’adresse IP du périphérique en flux continu est implicitement envoyée avec cet appel.  Pour les implémentations serveur à serveur, où l’appel **regcode** est effectué par le service de programmation et non par le périphérique de diffusion en continu, l’en-tête suivant est nécessaire pour transmettre l’adresse IP du périphérique de diffusion en continu :</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>où `<streaming\_device\_ip>` est l’adresse IP publique de l’appareil en flux continu.</br></br>
>Exemple :</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Exemple de réponse {#sample-response}

* **Cas 1 : succès**
</br>
  * **XML:**
  </br>

    &quot;XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;sample>sample ResourceId&lt;/resource>
    &lt;/authorization>
    &quot;



* **JSON:**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>Lorsque la réponse provient d’un MVPD de proxy, il peut inclure un élément supplémentaire nommé `proxyMvpd`.



* **Cas 2 : autorisation refusée**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
