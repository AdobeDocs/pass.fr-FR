---
title: Récupération du jeton d’authentification
description: Récupération du jeton d’authentification
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 0%

---

# Récupération du jeton d’authentification {#retrieve-authentication-token}

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

Récupère le jeton d’authentification (AuthN).

| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn</br></br>Par exemple :</br></br>&lt;SP_FQDN>/api/v1/tokens/authn | Application de diffusion en continu</br></br>ou</br></br>Service de programmation | 1. demandeur (obligatoire)</br>2.  deviceId (obligatoire)</br>3.  device_info/X-Device-Info (obligatoire)</br>4.  _deviceType_ (obsolète)</br>5.  _deviceUser_ (obsolète)</br>6.  _appId_ (obsolète) | GET | XML ou JSON contenant des informations d’authentification ou des détails d’erreur en cas d’échec. | 200 - Réussite.  </br>404 - Jeton introuvable </br>410 - Jeton expiré |

{style="table-layout:auto"}


| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’identifiant de l’appareil. |
| device_info/</br></br>X-Device-Info | Informations sur les périphériques de diffusion en continu.</br></br>**Remarque** : cette variable peut être transmise à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de longueur d’une URL de GET, elle doit être transmise sous la forme X-Device-Info dans l’en-tête http. </br></br>Pour plus d&#39;informations, reportez-vous à la section [Transmission des informations de périphérique et de connexion](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Type d’appareil (par exemple, Roku, PC).</br></br>**Remarque** : device_info remplace ce paramètre. |
| _deviceUser_ | Identifiant de l’utilisateur de l’appareil.</br></br>**Remarque** : Si elle est utilisée, deviceUser doit avoir les mêmes valeurs que dans la requête [Create Registration Code](/help/authentication/registration-code-request.md) . |
| _appId_ | ID/nom de l’application. </br></br>**Remarque** : device_info remplace ce paramètre. S’il est utilisé, `appId` doit avoir les mêmes valeurs que dans la requête [Create Registration Code](/help/authentication/registration-code-request.md) . |

{style="table-layout:auto"}

</br>

### Exemple de réponse {#response}



#### Succès

**XML :**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authentication>
         <expires>1601114932000</expires>
         <userId>sampleUserId</userId>
         <mvpd>sampleMvpdId</mvpd>
         <requestor>sampleRequestor</requestor>
    </authentication>
```


**JSON:**

```JSON
    {
         "requestor": "sampleRequestor",
         "mvpd": "sampleMvpdId",
         "userId": "sampleUserId",
         "expires": "1601114932000"
    }
```





#### Jeton d’authentification introuvable :

**XML :**

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
        "message": "Not Found"
    }
```
