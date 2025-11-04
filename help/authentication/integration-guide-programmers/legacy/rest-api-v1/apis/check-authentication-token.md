---
title: Vérifier le jeton d’authentification
description: Vérifier le jeton d’authentification
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# (Hérité) Vérifier le jeton d’authentification {#check-authentication-token}

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

Indique si l’appareil possède un jeton d’authentification non expiré.

| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthn | Service de programmation</br></br>ou</br></br>d’application en flux continu | &#x200B;1. demandeur (obligatoire)</br>2.  deviceId (obligatoire)</br>3.  device_info/X-Device-Info (obligatoire)</br>4.  _deviceType_ </br>5.  _deviceUser_ (obsolète)</br>6.  _appId_ (obsolète) | GET | XML ou JSON contenant les détails de l’erreur en cas d’échec. | 200 - Succès   </br>403 - Aucun Succès |

{style="table-layout:auto"}


| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’ID de l’appareil. |
| device_info/</br></br>X-Device-Info | Informations sur l’appareil de diffusion en continu.</br></br>**Remarque** : cela PEUT être transmis à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de la longueur d’une URL GET, il DOIT être transmis en tant que X-Device-Info dans l’en-tête http. </br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)-->. |
| _deviceType_ | Type d’appareil (par exemple Roku, PC).</br></br>Si ce paramètre est défini correctement, ESM propose des mesures [ventilées par type d’appareil](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyse puissent être effectués pour Roku, AppleTV, Xbox, etc.</br></br>Pour plus d’informations, consultez la section [Avantages de l’utilisation du paramètre Clientless deviceType dans les mesures d’authentification d’Adobe Pass &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Remarque** : device_info remplacera ce paramètre. |
| _deviceUser_ | Identifiant utilisateur de l’appareil. |
| _appId_ | Nom/ID de l’application.</br>**Remarque** : device_info remplace ce paramètre. |

{style="table-layout:auto"}


## Réponse (en cas d’échec) {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [Retour à la référence de l’API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
