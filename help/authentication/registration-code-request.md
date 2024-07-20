---
title: Page d’enregistrement
description: Page d’enregistrement
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# Page d’enregistrement {#registration-page}

## Points de terminaison de l’API REST {#clientless-endpoints}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par le [mécanisme de limitation](/help/authentication/throttling-mechanism.md)

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#create-reg-code-svc}

Renvoie le code d’enregistrement généré de manière aléatoire et l’URI de page de connexion.

| Point d’entrée | Appelé </br> | Entrée   </br>Paramètre | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode</br>Par exemple :</br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Application de diffusion en continu</br>ou</br>Service de programmation | 1. demandeur </br>    (Composant Chemin)</br>2.  deviceId (Hashed)   </br>    (Obligatoire)</br>3.  device_info/X-Device-Info (obligatoire)</br>4.  mvpd (facultatif)</br>5.  ttl (facultatif)</br>6.  _deviceType_</br> 7.  _deviceUser_ (obsolète)</br>8.  _appId_ (obsolète) | POST | XML ou JSON contenant un code d’enregistrement et des informations ou des détails d’erreur en cas d’échec. Voir schémas et exemples ci-dessous. | 201 |

{style="table-layout:auto"}

| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’identifiant de l’appareil. |
| device_info/</br>X-Device-Info | Informations sur les périphériques de diffusion en continu.</br>**Remarque** : cette variable peut être transmise à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de longueur d’une URL de GET, elle doit être transmise sous la forme X-Device-Info dans l’en-tête http. </br>Pour plus d&#39;informations, reportez-vous à la section [Transmission des informations de périphérique et de connexion](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | Identifiant MVPD pour lequel cette opération est valide. |
| ttl | Durée de vie de ce regcode en secondes.</br>**Remarque** : la valeur maximale autorisée pour ttl est de 36 000 secondes (10 heures). Des valeurs plus élevées entraînent une réponse HTTP 400 (mauvaise requête). Si `ttl` n’est pas renseigné, l’authentification Adobe Pass définit une valeur par défaut de 30 minutes. |
| _deviceType_ | Type d’appareil (par exemple, Roku, PC).</br>Si ce paramètre est défini correctement, ESM offre des mesures [ventilées par type d’appareil](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Sans client, de sorte que différents types d’analyses puissent être effectués, par exemple, Roku, Apple TV et Xbox.</br>Voir [Avantages de l’utilisation d’un paramètre de type d’appareil sans client dans les mesures de transmission ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Remarque** : l’info_périphérique remplacera ce paramètre. |
| _deviceUser_ | Identifiant de l’utilisateur de l’appareil. |
| _appId_ | ID/nom de l’application. </br>**Remarque** : device_info remplace ce paramètre. |

{style="table-layout:auto"}


>[!CAUTION]
>
>**Adresse IP du périphérique de diffusion en continu**
></br>
>Pour les mises en oeuvre client-serveur, l’adresse IP du périphérique en flux continu est implicitement envoyée avec cet appel.  Pour les implémentations serveur à serveur, où l’appel **regcode** est effectué en tant que service de programmation et non comme périphérique de diffusion en continu, l’en-tête suivant est nécessaire pour transmettre l’adresse IP du périphérique de diffusion en continu :
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>où `<streaming\_device\_ip>` est l’adresse IP publique de l’appareil en flux continu.
></br></br>
>Exemple :</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1</br>X-Forwarded-For:203.45.101.20
>```
>
</br>

### Schéma XML de réponse {#xml-schema}


#### XSD du code d’enregistrement {#registration-code-xsd}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

</br>

| Nom de l’élément | Description |
| --------------- | ------------------------------------------------------------------------------------ |
| id | UUID généré par le service de code d’enregistrement |
| code | Code d’enregistrement généré par le service de code d’enregistrement |
| demandeur | Identifiant du demandeur |
| mvpd | Identifiant Mvpd |
| généré | Horodatage de création du code d’enregistrement (en millisecondes depuis le 1er janvier 1970 GMT) |
| expires | Horodatage de l’expiration du code d’enregistrement (en millisecondes depuis le 1er janvier 1970 GMT) |
| deviceId | ID d’appareil unique (ou jeton XSTS) |
| deviceType | Type de périphérique |
| deviceUser | Utilisateur connecté à l’appareil |
| appId | ID de l’application |
| appVersion | Version de l’application |
| registrationURL | URL de l’application Web de connexion à afficher à l’utilisateur final |

{style="table-layout:auto"}


</br>

### Message d’erreur XSD  {#error-message}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="rest.pass.adobe.com"
               targetNamespace="rest.pass.adobe.com"
               attributeFormDefault="unqualified"
               elementFormDefault="unqualified">
        <xs:element name="error">
            <xs:complexType>
                <xs:all>
                    <xs:element name="status" type="xs:int" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="message" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="details" type="xs:string" minOccurs="0" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
    </xs:schema>
```


### Exemple de réponse {#sample-response}

**XML :**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```

**JSON:**

```JSON
    {
        "id": "678f9fea-9d364b29-246c-488f-b97e-298566d1b9e0",
        "code": "D4BDU2W",
        "requestor": "sampleRequestorId",
        "mvpd": "sampleMvpdId",
        "generated": 1348039555877,
        "expires": 1348043155877,
        "info": {
            "deviceId": "dGhpc0l.kQUR1bW15RGV2.aWNlSWQ=",
            "deviceType": "xboxOne",
            "deviceUser": "JD",
            "appId": "2345",
            "appVersion": "2.0",
            "registrationURL": "http://loginwebapp.com"
        }
    }
```
