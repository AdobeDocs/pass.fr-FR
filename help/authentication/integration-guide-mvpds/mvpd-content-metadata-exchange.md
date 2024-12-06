---
title: Exchange de métadonnées de contenu MVPD
description: Exchange de métadonnées de contenu MVPD
exl-id: d17e60dc-6c61-4ca2-bad8-1840c95261e0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# Exchange de métadonnées de contenu MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#content-metadat-exchange-overview}

Cette page décrit deux mises en oeuvre standard qu’Adobe Pass Authentication utilise pour envoyer des données structurées aux MVPD sur la demande d’autorisation.  Les données structurées représentent la ressource (le programmeur) qui effectue la requête et, éventuellement, des données supplémentaires telles que l’évaluation du contenu.

Côté programmeur, l’authentification Adobe Pass prend en charge les ressources de données MRSS structurées comme suit :

1. Le programmeur envoie la ressource sous la forme d’une chaîne MRSS. L’authentification Adobe Pass ne le code pas côté client pour les appareils web ou natifs. Le MRSS est envoyé comme chaîne régulière au serveur d&#39;authentification Adobe Pass.
1. Côté serveur, le MRSS est validé par rapport au schéma prédéfini (http://search.yahoo.com/mrss/).  Si la validation est acceptée, l’authentification Adobe Pass extrait les informations des champs MRSS, notamment :
   * titre du canal
   * titre de l’élément
   * identifiant de ressource
   * valeur d’évaluation et type
1. Les valeurs extraites du MRSS sont utilisées pour créer la demande d’autorisation qui est transmise au MVPD.

L’authentification Adobe Pass prend en charge deux approches pour traduire le MRSS en formats pris en charge par les MVPD :

* **XACML**.  La première approche s’aligne sur la norme OLCA.  Il utilise XACML, dans lequel les valeurs MRSS sont extraites pour créer une XACMLResource avec des attributs qui correspondent aux éléments MRSS.  Il est ensuite transmis au MVPD.
* **REST**.  La seconde approche est basée sur REST.  Le MRSS est codé en base64 et transmis en tant que paramètre d&#39;URL lors de l&#39;appel REST.

Dans les deux approches, le MVPD traite la demande d’autorisation en incluant les valeurs extraites dans son propre flux logique et en renvoyant une réponse d’autorisation.

## Détails de l’intégration {#integration-details}

* Ressource structurée XACML basée sur OLCA
* Ressource structurée basée sur REST

### Ressource structurée XACML basée sur OLCA {#olca-based-xacml-struc-resource}

La plupart des MVPD câblés utilisent l’approche basée sur XACML, mais ne prennent pas encore en charge l’approche de données structurées complètes.  Les autres MVPD qui prennent en charge le langage XACML prennent le titre du canal et acceptent cela pour l’attribut ResourceID. L’exemple ci-dessous illustre l’approche complète structurée basée sur XACML. L’équipe d’authentification Adobe Pass recommande que les MVPD qui utilisent XACML, mais ne prennent pas encore en charge des fonctionnalités telles que les contrôles parentaux, adaptent leur intégration XACML à l’exemple suivant :

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

Certains MVPD ont normalisé le protocole REST suivant pour l’autorisation. Cette approche est aussi complète que l’approche XACML, mais fournit une mise en oeuvre &quot;plus légère&quot;.

`// The MRSS is base64 encoded by Adobe Pass Authentication, and passed in that format to the REST-based Authorization endpoint.`

`https://auth.somedomain.net/mediation/1/rest/client/authz?uuID=AC82CE4&mrss=base64encodedstring&IPAddress=123.456.78.901`

<!--
>[!RELATEDINFORMATION]
>* [User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Programmer Integration Guide: Identifying Protected Resources](/help/authentication/identify-protected-resources.md)
>* [Programmer Integration Guide: User Metadata Exchange](/help/authentication/user-metadata.md)
-->
