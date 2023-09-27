---
title: API d’enregistrement de client dynamique
description: API d’enregistrement de client dynamique
exl-id: 06a76c71-bb19-4115-84bc-3d86ebcb60f3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# API d’enregistrement de client dynamique {#dynamic-client-registration-api}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Présentation {#overview}

Actuellement, l’authentification Adobe Pass peut identifier et enregistrer les applications de deux manières différentes :

* les clients basés sur un navigateur sont enregistrés via les autorisations [liste de domaines](/help/authentication/programmer-overview.md)
* les clients d’application natifs, tels que les applications iOS et Android, sont enregistrés par le biais du mécanisme de requête signé.

Adobe Pass Authentication propose un nouveau mécanisme d’enregistrement des applications. Ce mécanisme est décrit dans les paragraphes suivants.

## Le mécanisme d’enregistrement des applications {#appRegistrationMechanism}

### Raisons techniques {#reasons}

Le mécanisme d’authentification dans l’authentification Adobe Pass reposait sur des cookies de session, mais en raison de [Onglets personnalisés Chrome Android](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari View Controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}, cet objectif ne peut plus être atteint.

Compte tenu de ces limitations, Adobe introduit un nouveau mécanisme d’enregistrement pour tous ses clients. Il est basé sur la RFC OAuth 2.0 et comprend les étapes suivantes :

1. Récupération de l’instruction logicielle du tableau de bord TVE
1. Obtention des informations d’identification du client
1. Obtention du jeton d’accès

### Récupération du relevé logiciel à partir du tableau de bord TVE {#softwareStatement}

Pour chaque application que vous publiez, vous devez obtenir une instruction logicielle. Toutes les instructions logicielles sont fournies par le biais du tableau de bord TVE, une fois les applications créées. L’instruction logicielle doit être déployée avec l’application sur l’appareil de l’utilisateur.

>[!IMPORTANT]
>
>Lors de l’utilisation d’une instruction logicielle, le mécanisme d’identification du demandeur signé ne sera plus nécessaire.

Pour plus d’informations sur la création d’instructions logicielles, voir [Enregistrement du client dans le tableau de bord TVE](/help/authentication/dynamic-client-registration.md).

### Obtention des informations d’identification du client {#clientCredentials}

Après avoir récupéré une instruction logicielle du tableau de bord TVE, vous devez enregistrer votre application auprès du serveur d’autorisation Adobe Pass. Pour ce faire, effectuez un appel /register et récupérez votre identifiant client unique.

**Requête**

| appel HTTP |                    |
|-----------|--------------------|
| path | /o/client/register |
| method | POST |

| fields |                                                                           |           |
|--------------------|---------------------------------------------------------------------------|-----------|
| software_statement | Instruction logicielle créée dans le tableau de bord TVE. | mandatory |
| redirect_uri | URI utilisé par l’application pour terminer le flux d’authentification. | facultatif |

| en-têtes de requête |                                                                                |           |
|-----------------|--------------------------------------------------------------------------------|-----------|
| Content-Type | application/json | mandatory |
| X-Device-Info | Informations sur le périphérique telles que définies dans Transmission des informations de périphérique et de connexion | mandatory |
| User-Agent | L’agent utilisateur | mandatory |

**Réponse**

| en-têtes de réponse |                  |           |
|------------------|------------------|-----------|
| Content-Type | application/json | mandatory |

| champs de réponse |                 |                            |
|---------------------|-----------------|----------------------------|
| client_id | Chaîne | mandatory |
| client_secret | Chaîne | mandatory |
| client_id_issue_at | long | mandatory |
| redirect_uris | liste de chaînes | mandatory |
| grant_types | liste de chaînes<br/> **valeur acceptée**<br/> `client_credentials`: utilisé par les clients non sécurisés, tels que le SDK Android. | mandatory |
| error | **valeurs acceptées**<ul><li>invalid_request</li><li>invalid_redirect_uri</li><li>invalid_software_statement</li><li>unapproved_software_statement</li></ul> | obligatoire dans un flux d’erreurs |


#### Réponse d’erreur {#error-response}

En cas d’erreur, le serveur d’enregistrement répond avec un code d’état HTTP 400 (Bad Request) et inclut les paramètres suivants dans la réponse :

| code d&#39;état | corps de la réponse | description |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | Il manque un paramètre requis à la requête, inclut une valeur de paramètre non prise en charge, répète un paramètre ou il est incorrectement formé. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_redirect_uri&quot;} | Le redirect_uri n’est pas autorisé pour ce client en fonction de son application enregistrée. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_software_statement&quot;} | L’instruction logicielle n’est pas valide. |
| HTTP 400 | {&quot;error&quot;: &quot;unapproved_software_statement&quot;} | L’ID_logiciel est introuvable dans la configuration. |

#### Exemple d’informations d’identification client {#client-credentials-example}

**Requête :**

```HTTPS
POST /o/client/register HTTP/1.1
    User-Agent: Android
    X-Device-Info: ew0KICAibW9kZWwiOiAiVFYiLA0KICAidmVuZG9yIjogIkFwcGxlIiwNCiAgIm1hbnVmYWN0dXJlciI6ICJBcHBsZSIsDQogICJvc05hbWUiOiAidHZPUyIsDQogICJvc1ZlbmRvciI6ICJBcHBsZSIsDQogICJvc1ZlcnNpb24iOiAiMTAuMiIsDQogICJicm93c2VyVmVuZG9yIjogIkFwcGxlIiwNCiAgImJyb3dzZXJOYW1lIjogIlNhZmFyaSINCn0
    Content-Type: application/json
    Accept: application/json
    Host: sp.auth.adobe.com
 {
        "software_statement": "eyJhbGciOiJSUzI1NiJ9.
    eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
    bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
    aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
    GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
    zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
    5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
    fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
    U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
    IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
  "redirect_uri": "adobepass://com.programmer"  }
```

**Réponse :**

```HTTPS
HTTP/1.1 201 Created
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
    
{
 "client_id": "s6BhdRkqt3",
 "client_secret": "t7AkePiru4",
 "client_id_issued_at": 2893256800,
 "redirect_uris": [
   "app://com.programmer.adobe#sdasdsadas"],
 "grant_types": ["client_credentials"]
}
```

**Réponse d’erreur :**

```HTTPS
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

### Obtention d’un jeton d’accès {#accessToken}

Après avoir récupéré l’identifiant client unique (identifiant client et secret client) de votre application, vous devez obtenir un jeton d’accès. Le jeton d’accès est un jeton OAuth 2.0 obligatoire, utilisé pour appeler les API d’authentification Adobe Pass.

>[!NOTE]
>
>Actuellement, les jetons d’accès ont une durée de vie de 24 heures.

**Requête**


| **appel HTTP** | |
| --- | --- |
| path | `/o/client/token` |
| method | POST |

| **paramètres de requête** | |
| --- | --- |
| `grant_type` | Reçu dans le processus d’enregistrement du client.<br/> **Valeur acceptée**<br/>`client_credentials`: utilisé pour les clients non sécurisés, tels que le SDK Android. |
| `client_id` | Identifiant du client obtenu dans le processus d’enregistrement du client. |
| `client_secret` | Identifiant du client obtenu dans le processus d’enregistrement du client. |

**Réponse**

| champs de réponse | | |
| --- | --- | --- |
| `access_token` | La valeur du jeton d’accès que vous devez utiliser pour appeler les API Adobe Pass | mandatory |
| `expires_in` | Durée en secondes jusqu’à l’expiration de access_token | mandatory |
| `token_type` | Type du jeton **porteur** | mandatory |
| `created_at` | L’heure d’émission du jeton | mandatory |
| **en-têtes de réponse** | | |
| `Content-Type` | application/json | mandatory |

**Réponse d’erreur**

En cas d’erreur, le serveur d’autorisation répond avec un code d’état HTTP 400 (Bad Request) et inclut les paramètres suivants dans la réponse :

| code d&#39;état | corps de la réponse | description |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | Un paramètre requis est manquant dans la requête, inclut une valeur de paramètre non prise en charge (autre que le type d’octroi), répète un paramètre, inclut plusieurs informations d’identification, utilise plusieurs mécanismes pour authentifier le client ou est incorrectement formé. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_client&quot;} | L’authentification du client a échoué car le client était inconnu. Le SDK DOIT à nouveau s’enregistrer auprès du serveur d’autorisation. |
| HTTP 400 | {&quot;error&quot;: &quot;unauthorized_client&quot;} | Le client authentifié n’est pas autorisé à utiliser ce type d’autorisation. |

#### Exemple d’obtention du jeton d’accès : {#obt-access-token}

**Requête :**

```HTTPS
POST o/client/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
    
grant_type=client_credentials&client_id=AAA&client_secret=SSS
```

**Réponse :**

```JSON
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "created_at":123456789
}
```

**Réponse d’erreur :**

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

## Exécution de demandes d’authentification {#autheticationRequests}

Utilisation du jeton d’accès pour exécuter Adobe Pass [Appels d’API d’authentification](/help/authentication/initiate-authentication.md). Pour ce faire, le jeton d’accès doit être ajouté à la requête API, de l’une des manières suivantes :

* en ajoutant un nouveau paramètre de requête à la requête. Ce nouveau paramètre s’appelle **access_token**.

* en ajoutant un nouvel en-tête HTTP à la requête : Authorization: Bearer. Nous vous recommandons d’utiliser l’en-tête HTTP, car les chaînes de requête ont tendance à être visibles dans les journaux du serveur.

En cas d’erreur, les réponses d’erreur suivantes peuvent être renvoyées :

| Réponses d’erreur |     |                                                                                                        |
|-----------------|-----|--------------------------------------------------------------------------------------------------------|
| invalid_request | 400 | La requête est incorrecte. |
| invalid_client | 403 | L’ID client n’est plus autorisé à effectuer des requêtes. De nouvelles informations d’identification client doivent être générées. |
| access_denied | 401 | access_token n’est pas valide (expiré ou non valide). Le client DOIT demander un nouveau access_token. |

### Exemples de demandes d’authentification :

**Envoi du jeton d’accès en tant que paramètre de requête :**

```HTTPS
GET adobe-services/config?access_token=<access_token>&requestor_id=... HTTP/1.1
    
Host: sp.auth.adobe.com
```

**Envoi du jeton d’accès en tant qu’en-tête HTTP :**

```HTTPS
POST adobe-services/sessionDevice?device_id=platformDeviceId HTTP/1.1
    
Authorization: Bearer <access_token>
    
Host: sp.auth.adobe.com
```

**Réponses d’erreur en tant que corps de réponse :**

```HTTPS
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error":"invalid_client" }
```

**Réponses d’erreur en tant que paramètres d’URL :**

```HTTPS
HTTP/1.1 302 Found
    
Location: adobepass://com.programmer.adobe?error=access_denied
```
