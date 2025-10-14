---
title: Récupérer le jeton d’autorisation
description: Récupérer le jeton d’autorisation
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 0%

---

# (Hérité) Récupérer Le Jeton D’Autorisation {#retrieve-authorization-token}

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

Récupère le jeton d’autorisation (AuthZ).


| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authz</br></br>Par exemple :</br></br>&lt;SP_FQDN>/api/v1/tokens/authz | Service de programmation</br></br>ou</br></br>d’application en flux continu | 1. demandeur (obligatoire)</br>2.  deviceId (obligatoire)</br>3.  ressource (obligatoire)</br>4.  device_info/X-Device-Info (obligatoire)</br>5.  _deviceType_</br> 6  _deviceUser_ (obsolète)</br>7.  _appId_ (obsolète) | GET | 1. Réussite</br>2.  </br> du jeton d’authentification    introuvable ou expiré :   </br>    XML expliquant le motif </br>    jeton d’authentification introuvable</br>3.  Jeton d’autorisation </br>    introuvable : </br>    Explication XML </br>4.  Jeton d’autorisation </br>    expiré : </br>    Explication XML | 200 - Succès </br>412 - Pas d’AuthN</br></br>404 - Pas d’AuthZ</br></br>410 - AuthZ expiré |

{style="table-layout:auto"}

</br>

| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’ID de l’appareil. |
| ressource | Chaîne contenant un resourceId (ou un fragment MRSS), identifiant le contenu demandé par un utilisateur et reconnu par les points d’entrée d’autorisation MVPD. |
| device_info/</br></br>X-Device-Info | Informations sur l’appareil de diffusion en continu.</br></br>**Remarque** : cela PEUT être transmis à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de la longueur d’une URL de GET, il DOIT être transmis en tant que X-Device-Info dans l’en-tête http. </br></br>Voir les détails complets dans [Transmettre les informations sur l’appareil et la connexion](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Type d’appareil (par exemple, Roku, PC).</br></br>Si ce paramètre est défini correctement, ESM propose des mesures [ventilées par type d’appareil](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyse puissent être effectués, par exemple, Roku, AppleTV et Xbox.</br></br>Voir, [Avantages de l’utilisation d’un paramètre de type d’appareil sans client dans les mesures de réussite &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Remarque** : device_info remplacera ce paramètre. |
| _deviceUser_ | Identifiant utilisateur de l’appareil. |
| _appId_ | Nom/ID de l’application. </br></br>**Remarque** : device_info remplace ce paramètre. |

{style="table-layout:auto"}


### Exemple de réponse {#response}



#### Succès

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authorization>
        <expires>1348148289000</expires>
        <mvpd>sampleMvpdId</mvpd>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <proxyMvpd>sampleProxyMvpdId</proxyMvpd>
    </authorization>
```



**JSON:**

```JSON
    {
        "mvpd": "sampleMvpdId",
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348148289000",
        "proxyMvpd": "sampleProxyMvpdId"
    }
```

</br>


#### Jeton d’authentification introuvable ou expiré :

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>412</status>
        <message>User not authenticated</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 412,
        "message": "User not authenticated",
        "details": null
    }
```

</br>


#### Jeton d’autorisation introuvable :

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 404,
        "message": "Not Found",
        "details": null
    }
```

</br>



#### Jeton d’autorisation expiré :

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>410</status>
        <message>Gone</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 410,
        "message": "Gone",
        "details": null
    }
```
