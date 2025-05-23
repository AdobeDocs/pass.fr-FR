---
title: Lancer l’autorisation
description: Lancer l’autorisation
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# (Hérité) Lancer l’autorisation {#initiate-authorization}

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

Obtient la réponse d’autorisation.

| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize. | Service de programmation</br></br>ou</br></br>d’application en flux continu | 1. demandeur (obligatoire)</br>2.  deviceId (obligatoire)</br>3.  ressource (obligatoire)</br>4.  device_info/X-Device-Info (obligatoire)</br>5.  _deviceType_</br> 6  _deviceUser_ (obsolète)</br>7.  _appId_ (obsolète)</br>8.  paramètres supplémentaires (facultatif) | GET | XML ou JSON contenant les détails d’autorisation ou les détails d’erreur en cas d’échec. Voir les exemples ci-dessous. | 200 - Succès </br>403 - Aucun Succès |

{style="table-layout:auto"}

</br>


| Paramètre d’entrée | Description |
| --- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’ID de l’appareil. |
| ressource | Chaîne contenant un resourceId (ou un fragment MRSS), identifiant le contenu demandé par un utilisateur et reconnu par les points d’entrée d’autorisation MVPD. |
| device_info/</br></br>X-Device-Info | Informations sur l’appareil de diffusion en continu.</br></br>**Remarque** : cela PEUT être transmis à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de la longueur d’une URL de GET, il DOIT être transmis en tant que X-Device-Info dans l’en-tête http. </br></br>Voir les détails complets dans [Transmettre les informations sur l’appareil et la connexion](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Type d’appareil (par exemple Roku, PC).</br></br>Si ce paramètre est défini correctement, ESM propose des mesures [ventilées par type d’appareil](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyse puissent être effectués pour Roku, AppleTV, Xbox, etc.</br></br>Voir [Avantages du paramètre de type d’appareil sans client dans les mesures de réussite ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Remarque** : device_info remplacera ce paramètre. |
| _deviceUser_ | Identifiant utilisateur de l’appareil. |
| _appId_ | Nom/ID de l’application. </br></br>**Remarque** : device_info remplace ce paramètre. |
| paramètres supplémentaires | L’appel peut également contenir des paramètres facultatifs qui activent d’autres fonctionnalités telles que :</br></br>* generic_data - permet l’utilisation de [Promotional TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)</br></br>Exemple : `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Adresse IP de l’appareil de streaming**</br>
>Pour les implémentations client à serveur, l’adresse IP de l’appareil de diffusion en continu est implicitement envoyée avec cet appel.  Pour les implémentations serveur à serveur, où l’appel **regcode** est effectué par le service de programmation et non par l’appareil de diffusion en continu, l’en-tête suivant est requis pour transmettre l’adresse IP de l’appareil de diffusion en continu </br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>où `<streaming\_device\_ip>` est l’adresse IP publique de l’appareil de diffusion en continu.</br></br>
>Exemple : </br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Exemple de réponse {#sample-response}

* **Cas 1 : Succès**
</br>
  * **XML:**

  </br>

    « XML
    &lt;?xml version=« 1.0 » encoding=« UTF-8 » standalone=« yes »?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
     »



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
>Lorsque la réponse provient d’un MVPD proxy, elle peut inclure un élément supplémentaire nommé `proxyMvpd`.



* **Cas 2 : autorisation refusée**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
