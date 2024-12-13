---
title: Enregistrement d'enregistrement de retour
description: Enregistrement d'enregistrement de retour
exl-id: 7b9e63a2-59b6-4123-a19b-ee1f021219ea
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 2%

---

# Enregistrement d’enregistrement de retour (hérité) {#return-registration-record}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Points d’entrée de l’API REST {#clientless-endpoints}

`<REGGIE_FQDN>` :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>` :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)




## Description {#description}

Renvoie l’enregistrement du code d’enregistrement contenant l’UUID du code d’enregistrement, le code d’enregistrement et l’ID de périphérique haché.






| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| `<REGGIE_FQDN>`;/reggie/v1/`{requestorId}`/regcode/`{registrationCode}`<p>Par exemple :<p>`<REGGIE_FQDN>`/reggie/v1/sampleRequestorId/regcode/TJJCFK?format=xml | Service de programmation</br></br>ou</br></br>d’application en flux continu | 1. </br> du demandeur    (Composant Chemin d’accès)</br>2.  </br> du code d’enregistrement    (Composant Chemin) | GET | XML ou JSON contenant un code d’enregistrement et des informations. Voir le schéma et l’exemple ci-dessous. | 200 |

{style="table-layout:auto"}




| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| code d&#39;enregistrement | Valeur du code d’enregistrement qui s’afficherait sur l’appareil de diffusion en continu (à saisir dans le flux d’authentification). |




## Schéma XML de réponse {#response-xml-schema}

### Code d’enregistrement XSD

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

| Nom de l’élément | Description |
| --- | --- |
| id | UUID généré par le service de code d’enregistrement |
| code | Code d’enregistrement généré par le service Registration Code |
| demandeur | ID du demandeur |
| mvpd | MVPD ID |
| généré | Date et heure de création du code d’enregistrement (en millisecondes depuis le 1er janvier 1970 GMT) |
| expire | Date et heure d’expiration du code d’enregistrement (en millisecondes depuis le 1er janvier 1970 GMT) |
| deviceId | ID d’appareil unique (ou jeton XSTS) |
| deviceType | Type d’appareil |
| deviceUser | Utilisateur connecté à l’appareil |
| appId | Id De L&#39;Application |
| appVersion | Version de l’application |
| registrationURL | URL de l’application web de connexion à afficher pour l’utilisateur final |

{style="table-layout:auto"}

### Exemple de réponse {#sample-response}

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
