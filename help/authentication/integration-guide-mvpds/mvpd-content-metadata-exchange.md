---
title: Échange de métadonnées de contenu MVPD
description: Échange de métadonnées de contenu MVPD
exl-id: d17e60dc-6c61-4ca2-bad8-1840c95261e0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# Échange de métadonnées de contenu MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#content-metadat-exchange-overview}

Cette page décrit deux mises en œuvre standard que l’authentification Adobe Pass utilise pour envoyer des données structurées aux MVPD lors de la demande d’autorisation.  Les données structurées représentent la ressource (le programmeur) qui effectue la requête, ainsi que, éventuellement, des données supplémentaires telles que l’évaluation du contenu.

En ce qui concerne le programmeur, l’authentification Adobe Pass prend en charge les ressources de données MRSS structurées comme suit :

1. Le programmeur envoie la ressource sous la forme d’une chaîne MRSS. L’authentification Adobe Pass ne l’encode pas côté client pour les appareils web ou natifs. Le SMS est envoyé en tant que chaîne régulière au serveur d’authentification Adobe Pass.
1. Côté serveur, le MRSS est validé par rapport au schéma prédéfini (http://search.yahoo.com/mrss/).  Si la validation réussit, l’authentification Adobe Pass extrait les informations des champs MRSS, notamment :
   * titre du canal
   * titre de l&#39;objet
   * identifiant de la ressource
   * valeur et type d’évaluation
1. Les valeurs extraites du MRSS sont utilisées pour construire la demande d&#39;autorisation qui est transmise au MVPD.

L’authentification Adobe Pass prend en charge deux approches pour traduire les fichiers MRSS dans des formats pris en charge par les fichiers MVPD :

* **XACML**  La première approche est conforme à la norme OLCA.  Il utilise XACML, dans lequel les valeurs MRSS sont extraites pour créer une XACMLResource avec des attributs qui mappent aux éléments MRSS.  Elle est ensuite transmise au MVPD.
* **REPOS**.  La deuxième approche est basée sur REST.  Le MRSS est codé en base64 et transmis en tant que paramètre d’URL sur l’appel REST.

Dans les deux approches, le MVPD traite la demande d’autorisation en incluant les valeurs extraites dans son propre flux logique et en renvoyant une réponse d’autorisation.

## Détails de l’intégration {#integration-details}

* Ressource structurée XML basée sur OLCA
* Ressource structurée basée sur REST

### Ressource structurée XML basée sur OLCA {#olca-based-xacml-struc-resource}

La plupart des MVPD orientées câble utilisent l’approche basée sur XML, mais ne prennent pas encore en charge l’approche de données structurées complètes.  D’autres MVPD qui prennent en charge XACML prennent le titre de canal et l’acceptent pour l’attribut ResourceID. L’exemple ci-dessous illustre l’approche entièrement structurée basée sur XML. L’équipe d’authentification d’Adobe Pass recommande aux MVPD qui utilisent XACML, mais qui ne prennent pas encore en charge des fonctionnalités telles que le contrôle parental, d’adapter leur intégration XACML à l’exemple suivant :

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11=">
    <soap11:Header/>
    <soap11:Body>
        <xacml-samlp:XACMLAuthzDecisionQuery
                xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
                Destination="
                ID="_f1dd34469c5aeac016760e51dbba007d" IssueInstant="2012-06-26T16:30:24.879Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
                https://saml.sp.auth.adobe.com/
            </saml:Issuer>
            <ds:Signature xmlns:ds=">.......</ds:Signature>
            <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                [....info skipped for brevity....]
                <xacml-context:Resource>
 
        // The MRSS item GUID is passed as the XACML Resource resource-id
                    <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id">
                        <xacml-context:AttributeValue>DISNEY_GUID_12345</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
        // The MRSS channel title is passed as the XACML Resource tv-network
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:tv-network">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // Adobe doesn't yet support an explicit namespace for the GUID, so we reuse the channel title as the GUID.  
        // We expect to add an explicit namespace later next year pulling it from the GUID scheme attribute.
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:id:namespace">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS item title is passed as the XACML Resource content title
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:title">
                        <xacml-context:AttributeValue>Disney Program X</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS media rating is passed as the XACML Resource content rating 
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:rating:vchip">
                        <xacml-context:AttributeValue>TV-Y</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
                </xacml-context:Resource>
 
                <xacml-context:Action>
                    <xacml-context:Attribute>
                        <xacml-context:AttributeValue>VIEW</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
                </xacml-context:Action>
 
                [.....info skipped for brevity....]
            </xacml-context:Request>
        </xacml-samlp:XACMLAuthzDecisionQuery>
    </soap11:Body>
</soap11:Envelope>
 
//formatted for readability
```

### Ressource structurée basée sur REST {#rest-based-struct-resource}

Certains MVPD ont standardisé le protocole REST suivant pour l&#39;autorisation. Cette approche est aussi complète que l’approche XML, mais fournit une implémentation « plus légère ».

`// The MRSS is base64 encoded by Adobe Pass Authentication, and passed in that format to the REST-based Authorization endpoint.`

`https://auth.somedomain.net/mediation/1/rest/client/authz?uuID=AC82CE4&mrss=base64encodedstring&IPAddress=123.456.78.901`

<!--
>[!RELATEDINFORMATION]
>* [User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Programmer Integration Guide: Identifying Protected Resources](/help/authentication/identify-protected-resources.md)
>* [Programmer Integration Guide: User Metadata Exchange](/help/authentication/user-metadata.md)
-->
