---
title: Intégration SAML du proxy MVPD
description: Intégration SAML du proxy MVPD
exl-id: 6c83e703-d8cd-476b-8514-05b8230902be
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# Intégration SAML du proxy MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview-proxy-mvpd-saml-int}

Ce document décrit le flux d’authentification SAML pour les intégrations de proxy.  Ces flux dépendent des données de configuration du proxy présentes dans la configuration du serveur d’authentification Adobe Pass. Le MVPD du proxy envoie ses données de configuration du proxy au serveur d’authentification Adobe Pass via le service Web proxy d’authentification Adobe Pass.

## Données de configuration du proxy {#proxy-config-data}

Chaque proxy MVPD fournit des données de configuration de proxy pour leurs MVPD proxy au service Web proxy d’authentification Adobe Pass.  Les détails de cette opération sont présentés dans la documentation du service Web proxy.   Pour que le flux AuthN SAML fonctionne, les données de configuration du proxy doivent inclure les propriétés suivantes :

| Propriété | Description |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| MVPD ID | Chaîne représentant le MVPD proxy en interne à l’authentification Adobe Pass.  A confirmer par Adobe comme unique dans le contexte de l’authentification Adobe Pass. |
| URL du logo par défaut MVPD | URL d’un logo pouvant être affiché dans une expérience de sélecteur MVPD pour l’utilisateur.  Doit utiliser un fond transparent. |
| Nom d’affichage MVPD | Chaîne à utiliser comme texte de nom d’affichage pouvant être affiché avec le logo, éventuellement sous forme de texte de remplacement. |



## Flux d’intégration SAML {#saml-int-flows}

Lorsqu’un abonné MVPD visite le site ou l’application d’un programmeur, l’authentification Adobe Pass répond à un appel API du site ou de l’application avec une liste de MVPD activés pour ce programmeur.  L&#39;intégration peut être directe ou par proxy ; il n&#39;y a aucune distinction entre elles et le programmeur. Cela permet aux programmeurs de présenter la liste des MVPD actifs de toutes les façons qu’ils jugent appropriées. L’abonné choisit son MVPD et l’authentification Adobe Pass redirige l’abonné vers le fournisseur d’identité spécifique au MVPD.

Dans le cas d’un proxy MVPD intégré, l’intégration est effectuée entre l’authentification Adobe Pass et le proxy MVPD. L’authentification Adobe Pass envoie la demande d’authentification de l’utilisateur au proxy MVPD et le proxy MVPD gère la redirection. Pour que le proxy MVPD sache où rediriger la demande d’authentification de l’utilisateur, l’authentification Adobe Pass envoie un identifiant MVPD dans la demande d’authentification SAML.  Cet identifiant est l’identifiant MVPD spécifié par le fournisseur de proxy par le biais du service Web proxy comme spécifié ci-dessus.

### Authentification {#authn-saml-int}

Pour que l’authentification Adobe Pass s’intègre à un proxy MVPD, les conditions suivantes sont requises :

* Un MVPD proxy fournit la liste des MVPD proxy, transmise au service Web proxy Adobe.

* Métadonnées SAML pour le proxy MVPD parent

* (Recommandé) - Le MVPD du proxy gère une redirection supplémentaire vers l’URL de la page de connexion du MVPD du proxy.

* Le proxy MVPD doit ouvrir les ports 443 et 80 pour les adresses IP suivantes :
   * 192.150.4.5
   * 192.150.10.200
   * 192.150.11.4
   * 4.53.93.130
   * 193.105.140.131
   * 193.105.140.132
   * 76.74.170.204
   * 63.140.39.4
   * 66.235.132.38
   * 66.235.139.38
   * 66.235.139.168


#### Requête SAML d’authentification et réponse {#authn-saml-req-resp}

Dans la requête AuthN SAML, les intégrations de proxy incluent la propriété supplémentaire suivante qui doit être gérée par le proxy MVPD.  Cette propriété est nécessaire pour traiter correctement le demandeur pour le compte du MVPD validé et pour effectuer le rendu de la bonne connexion. (Cette propriété est mise en surbrillance dans l’exemple de requête ci-dessous.)

**Propriété de plage** - Inclut un élément IDPEntry qui inclut le MVPD_ID spécifique et le nom MVPD.  Cela représente le MVPD que l’utilisateur a réellement sélectionné dans le sélecteur du programmeur et correspond au MVPD_ID spécifié dans le service Web proxy.

Il existe une propriété d’étendue supplémentaire pour RequestorID qui peut être utilisée pour personnaliser la connexion à une marque particulière du programmeur (si nécessaire). Vous pouvez également l’utiliser uniquement pour les analyses sur l’emplacement d’origine de la requête.

Dans la réponse SAML AuthN, le MVPD du proxy doit spécifier le MVPD du proxy comme entité IdP dans les propriétés suivantes :

* Émetteur SAML
* Qualificateur de nom


**Exemple de requête AuthN**

```XML
<samlp:AuthnRequest
  AssertionConsumerServiceURL="https://sp.auth-staging.adobe.com/sp/saml/SAMLAssertionConsumer"
  Destination="DESTIONATION_URL"
  ForceAuthn="false"
  ID="_4cb70308-b445-462e-b044-f7d0323dde0c"
  IsPassive="false"
  IssueInstant="2012-04-03T15:41:25.884Z"
  ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
  Version="2.0"
  xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
  xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
      https://saml.sp.auth-staging.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        ...........
    </ds:Signature>
    <samlp:NameIDPolicy AllowCreate="true"
                        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                        SPNameQualifier="https://saml.sp.auth-staging.adobe.com"
                        xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" />
    <samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
        <samlp:IDPList xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
            <samlp:IDPEntry Name="MVPD NAME" ProviderID="MVPD_ID"/>
        </samlp:IDPList>
        <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
          RequestorID-Value
        </samlp:RequesterID>
    </samlp:Scoping>
</samlp:AuthnRequest>
```


**Exemple de réponse AuthN**

```XML
<samlp:Response Destination="https://sp.auth-staging.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_1d39be60-66de-012f-bfd5-0030488a31a4"
                InResponseTo="_4cb70308-b445-462e-b044-f7d0323dde0c"
                IssueInstant="2012-04-12T15:00:06Z"
                Version="2.0"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" >
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">ISSUER_VALUE</saml:Issuer>
    <samlp:Status>
        <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="_1d39c280-66de-012f-bfd6-0030488a31a4"
                    IssueInstant="2012-04-12T15:00:06Z"
                    Version="2.0"
                    xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" >
        <saml:Issuer>ISSUER_VALUE</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            ...........
        </ds:Signature>
        <saml:Subject>
            <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                         NameQualifier="IDP_NameQualifier"
                         SPNameQualifier="https://saml.sp.auth-staging.adobe.com">
                oRD6ALr5jlzkofNR1OaSCDbC6GaXV1cq8gF7Eotf
            </saml:NameID>
            <saml:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
                <saml:SubjectConfirmationData
                  InResponseTo="_4cb70308-b445-462e-b044-f7d0323dde0c"
                  NotOnOrAfter="2012-04-12T15:10:06Z"
                  Recipient="https://sp.auth-staging.adobe.com/sp/saml/SAMLAssertionConsumer" />
            </saml:SubjectConfirmation>
        </saml:Subject>
        <saml:Conditions NotBefore="2012-04-12T15:00:06Z"
                         NotOnOrAfter="2012-04-12T15:10:06Z">
            <saml:AudienceRestriction>
                <saml:Audience>https://saml.sp.auth-staging.adobe.com</saml:Audience>
            </saml:AudienceRestriction>
        </saml:Conditions>
        <saml:AuthnStatement AuthnInstant="2012-04-12T15:00:06Z"
                             SessionIndex="f6d15540cf27966115028d35c94eefb9" >
            <saml:AuthnContext>
                <saml:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
                </saml:AuthnContextClassRef>
            </saml:AuthnContext>
        </saml:AuthnStatement>
    </saml:Assertion>
</samlp:Response>
```

### Autorisation {#authz-proxy-mvpd-saml-int}

Pour la partie autorisation, le MVPD doit accepter pour l’autorisation la ressource spécifiée par le programmeur.  Dans la plupart des cas, il s’agit d’un identifiant de chaîne pour le réseau de canaux, tel que ThirllyNet ou TNT.

#### Requête et réponse SAML d’autorisation {#authz-saml-req-resp}

Dans la réponse AuthZ, l’ÉMETTEUR doit correspondre à l’ÉMETTEUR de la réponse SAML qui doit être l’identifiant MVPD proxy.

**Exemple de requête XACML AuthZ**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/">
<soap11:Header/>
<soap11:Body>
    <xacml-samlp:XACMLAuthzDecisionQuery
            xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
            ID="_c2346a8f2c9cfb205b6b8bf12c2db4d0" IssueInstant="2012-04-12T15:07:51.280Z" Version="2.0">
        <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com
        </saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
                <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                <ds:Reference URI="#_c2346a8f2c9cfb205b6b8bf12c2db4d0">
                    <ds:Transforms>
                        <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                        <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#">
                            <ec:InclusiveNamespaces xmlns:ec="http://www.w3.org/2001/10/xml-exc-c14n#"
                                                    PrefixList="ds saml xacml-context xacml-samlp"/>
                        </ds:Transform>
                    </ds:Transforms>
                    <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                    <ds:DigestValue>GmEkSZI+SDS1i4vV2ApGh0mx1X4=</ds:DigestValue>
                </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>..........</ds:SignatureValue>
        </ds:Signature>
        <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">          
            <xacml-context:Subject
              SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject">
                <xacml-context:Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id"
                  DataType="http://www.w3.org/2001/XMLSchema#string">
                    <xacml-context:AttributeValue
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:type="xacml-context:AttributeValueType">
                        oRD6ALr5jlzkofNR1OaSCDbC6GaXV1cq8gF7Eotf
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Subject>
            <xacml-context:Resource>
                <xacml-context:Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
                  DataType="http://www.w3.org/2001/XMLSchema#string">
                    <xacml-context:AttributeValue
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:type="xacml-context:AttributeValueType">TBS
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Resource>
            <xacml-context:Action>
                <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
                                         DataType="http://www.w3.org/2001/XMLSchema#string">
                    <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                                  xsi:type="xacml-context:AttributeValueType">VIEW
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Action>
            <xacml-context:Environment>
                <xacml-context:Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address"
                  DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress">
                    <xacml-context:AttributeValue
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:type="xacml-context:AttributeValueType">127.0.0.1
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Environment>
        </xacml-context:Request>
    </xacml-samlp:XACMLAuthzDecisionQuery>
</soap11:Body>
</soap11:Envelope>
```

**Exemple de réponse XACML AuthZ (autorisation accordée)**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap-env:Envelope xmlns:soap-env="http://schemas.xmlsoap.org/soap/envelope/">
    <soap-env:Body>
        <samlp:Response xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" ID="_311fa030-66df-012f-bfd7-0030488a31a4"
                        IssueInstant="2012-04-12T15:07:49Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">ISSUER</saml:Issuer>
            <samlp:Status>
                <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
            </samlp:Status>
            <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
                <ds:SignedInfo>
                    <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                    <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                    <ds:Reference URI="#_311fa030-66df-012f-bfd7-0030488a31a4">
                        <ds:Transforms>
                            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                        </ds:Transforms>
                        <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                        <ds:DigestValue>2+fDBPYnOT1w5dufJZoVsgckRkM=</ds:DigestValue>
                    </ds:Reference>
                </ds:SignedInfo>
                <ds:SignatureValue>..........</ds:SignatureValue>
                <ds:KeyInfo>
                    <ds:X509Data>
                        <ds:X509Certificate>.........</ds:X509Certificate>
                    </ds:X509Data>
                </ds:KeyInfo>
            </ds:Signature>
            <xacml-samlp:Assertion xmlns:xacml-samlp="urn:oasis:names:tc:SAML:2.0:assertion"
                                   ID="_311fa5a0-66df-012f-bfd8-0030488a31a4" IssueInstant="2012-04-12T15:07:49Z"
                                   Version="2.0">
                <xacml-samlp:Issuer>ISSUER</xacml-samlp:Issuer>
                <xacml-samlp:Conditions NotBefore="2012-04-12T15:07:49Z" NotOnOrAfter="2012-04-13T15:07:49Z">
                    <xacml-samlp:AudienceRestriction>
                        <xacml-samlp:Audience>https://saml.sp.auth-staging.adobe.com</xacml-samlp:Audience>
                    </xacml-samlp:AudienceRestriction>
                </xacml-samlp:Conditions>
                <xacml-saml:XACMLAuthzDecisionStatement
                        xmlns:xacml-saml="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:assertion"
                        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
                    <xacml-context:Response xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                        <xacml-context:Result ResourceId="TBS">
                            <xacml-context:Decision>Permit</xacml-context:Decision>
                        </xacml-context:Result>
                    </xacml-context:Response>
                </xacml-saml:XACMLAuthzDecisionStatement>
            </xacml-samlp:Assertion>
        </samlp:Response>
    </soap-env:Body>
</soap-env:Envelope>
```

**Exemple de réponse XACML AuthZ (autorisation refusée)**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap-env:Envelope xmlns:soap-env="http://schemas.xmlsoap.org/soap/envelope/">
<soap-env:Body>
    <samlp:Response xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" ID="_69ed8d80-66df-012f-bfda-0030488a31a4"
                    IssueInstant="2012-04-12T15:09:24Z" Version="2.0">
        <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">ISSUER</saml:Issuer>
        <samlp:Status>
            <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
        </samlp:Status>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
                <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                <ds:Reference URI="#_69ed8d80-66df-012f-bfda-0030488a31a4">
                    <ds:Transforms>
                        <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                        <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                    </ds:Transforms>
                    <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                    <ds:DigestValue>2SXNFA4pb/283wq5FVQdp4Ms5SQ=</ds:DigestValue>
                </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>........</ds:SignatureValue>
            <ds:KeyInfo>
                <ds:X509Data>
                    <ds:X509Certificate>........</ds:X509Certificate>
                </ds:X509Data>
            </ds:KeyInfo>
        </ds:Signature>
        <xacml-samlp:Assertion xmlns:xacml-samlp="urn:oasis:names:tc:SAML:2.0:assertion"
                               ID="_69ed91e0-66df-012f-bfdb-0030488a31a4" IssueInstant="2012-04-12T15:09:24Z"
                               Version="2.0">
            <xacml-samlp:Issuer>ISSUER</xacml-samlp:Issuer>
            <xacml-samlp:Conditions NotBefore="2012-04-12T15:09:24Z" NotOnOrAfter="2012-04-13T15:09:24Z">
                <xacml-samlp:AudienceRestriction>
                    <xacml-samlp:Audience>https://saml.sp.auth-staging.adobe.com</xacml-samlp:Audience>
                </xacml-samlp:AudienceRestriction>
            </xacml-samlp:Conditions>
            <xacml-saml:XACMLAuthzDecisionStatement
                    xmlns:xacml-saml="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:assertion"
                    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
                <xacml-context:Response xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                    <xacml-context:Result ResourceId="NOT_Authorized_Resouce">
                        <xacml-context:Decision>Deny</xacml-context:Decision>
                    </xacml-context:Result>
                </xacml-context:Response>
            </xacml-saml:XACMLAuthzDecisionStatement>
        </xacml-samlp:Assertion>
    </samlp:Response>
</soap-env:Body>
</soap-env:Envelope>
```

<!--
>![RelatedInformation]
>* [ProxyMVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Service Provider Scoping](/help/authentication/serv-provider-scoping.md)
-->
