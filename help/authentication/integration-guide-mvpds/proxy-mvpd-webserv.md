---
title: Service Web MVPD de proxy
description: Service Web MVPD de proxy
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---


# Service Web MVPD de proxy {#proxy-mvpd-wbservice}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Avant d’utiliser le service Web MVPD proxy, assurez-vous que les conditions préalables suivantes sont remplies :
>
> * Récupérez les informations d’identification du client comme décrit dans la documentation de l’API [Récupérez les informations d’identification du client](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenez le jeton d’accès comme décrit dans la documentation de l’API [Récupérer le jeton d’accès](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Pour plus d’informations sur la création d’une application enregistrée et le téléchargement de l’instruction logicielle, consultez la documentation [Aperçu de l’enregistrement du client dynamique](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) .

## Vue d’ensemble {#overview-proxy-mvpd-webserv}

Un &quot;Proxy MVPD&quot; est un MVPD qui, en plus de gérer sa propre intégration avec l’authentification Adobe Pass, gère également le processus de droit pour le compte d’un groupe de &quot;MVPD proxy&quot; associés. Cet arrangement est transparent pour les programmeurs.

Pour mettre en oeuvre la fonctionnalité ProxyMVPD, l’authentification Adobe Pass fournit des services web RESTful, avec lesquels les ProxyMVPD peuvent envoyer et récupérer des listes de ProxiedMVPD. Le protocole utilisé pour cette API publique est REST HTTP, avec les suppositions suivantes :

- Le MVPD du proxy utilise la méthode de GET HTTP pour récupérer la liste des MVPD intégrés actuels.
- Le MVPD du proxy utilise la méthode de POST HTTP pour mettre à jour la liste des MVPD pris en charge.

## Services MVPD de proxy {#proxy-mvpd-services}

- [Récupération des MVPD proxy](#retriev-proxied-mvpds)
- [Envoi de MVPD proxy](#submit-proxied-mvpds)

### Récupération des MVPD proxy {#retriev-proxied-mvpds}

Récupère la liste actuelle des MVPD proxy intégrés au MVPD proxy identifié.

| Point d’entrée | Appelé par | Paramètres de requête | En-têtes de requête | Méthode HTTP | Réponse HTTP |
|--------------------------------------------------------------------------|-----------|-----------------------|---------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Autorisation (obligatoire) | GET | <ul><li> 200 (ok) - La requête a été traitée avec succès et la réponse contient une liste de ProxiedMVPD au format XML.</li><li>401 (non autorisé) - Indique l’un des éléments suivants :<ul><li>Le client DOIT demander un nouveau access_token</li><li>La requête provient d’une adresse IP qui n’est pas présente dans la liste autorisée.</li><li>Le jeton n’est pas valide</li></ul></li><li>403 (interdit) - Indique que l’opération n’est pas prise en charge pour les paramètres fournis ou que le proxy MVPD n’est pas défini comme proxy ou est manquant.</li><li>405 (méthode non autorisée) - Une méthode HTTP autre que GET ou POST a été utilisée. La méthode HTTP n’est généralement pas prise en charge ou n’est pas prise en charge pour ce point de terminaison spécifique.</li><li>500 (erreur de serveur interne) : une erreur a été générée côté serveur pendant le processus de demande.</li></ul> |

Exemple de curl :

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds"`


Exemple de réponse XML :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```

### Envoi de MVPD proxy {#submit-proxied-mvpds}

Envoie un tableau de MVPD intégrés au MVPD du proxy identifié.

| Point d’entrée | Appelé par | Paramètres de requête | En-têtes de requête | Méthode HTTP | Réponse HTTP |
|:------------------------------------------------------------------------:|:---------:|-----------------------|:---------------------------------------------------:|:-----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Autorisation (obligatoire) proxied-mvpds (obligatoire) | POST | <ul><li>201 (créé) - La notification push a été traitée avec succès</li><li>400 (mauvaise requête) - Le serveur ne sait pas comment traiter la requête :<ul><li>Le code XML entrant ne se conforme pas au schéma publié dans cette spécification.</li><li>Les mvpds proxy ne comportent pas d’identifiants uniques.</li><li>Les ID de demandeur poussés n’existent pas Autre raison de conteneur de servlet pour le code de réponse 400</li></ul><li>401 (non autorisé) - Indique l’un des éléments suivants :<ul><li>Le client DOIT demander un nouveau access_token</li><li>La requête provient d’une adresse IP qui n’est pas présente dans la liste autorisée.</li><li>Le jeton n’est pas valide</li></ul></li><li>403 (interdit) - Indique que l’opération n’est pas prise en charge pour les paramètres fournis ou que le proxy MVPD n’est pas défini comme proxy ou est manquant.</li><li>405 (méthode non autorisée) - Une méthode HTTP autre que GET ou POST a été utilisée. La méthode HTTP n’est généralement pas prise en charge ou n’est pas prise en charge pour ce point de terminaison spécifique.</li><li>500 (erreur de serveur interne) : une erreur a été générée côté serveur pendant le processus de demande.</li></ul> |

Exemple de curl :

`curl -X POST -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



Exemple XML :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```


### Fréquence de publication {#posting-frequency}

L’authentification Adobe Pass recommande que les ProxyMVPD ne poussent leur liste de ProxiedMVPD que lorsqu’il y a une modification par rapport à la notification push précédente.

### Suppression de MVPD proxy {#delete-proxied-freqency}

Si le ProxyMVPD envoie un enregistrement XML avec une liste ProxiedMVPD vide, cette liste vide sera stockée dans notre système comme toute liste, supprimant ainsi la liste précédente.



## Format XSD {#xsd-format}

Adobe a défini le format accepté suivant pour la publication/récupération de MVPD proxy depuis/vers notre service Web public :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns:pxm="http://tve.adobe.com/data/proxiedmvpd"
           targetNamespace="http://tve.adobe.com/data/proxiedmvpd"
           elementFormDefault="qualified"
           version="1.0">
    <xs:complexType name="iframeSize">
        <xs:all>
            <xs:element name="iframeHeight" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeWidth" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
        </xs:all>
    </xs:complexType>
    <xs:complexType name="requestorIds">
        <xs:annotation>
            <xs:documentation>List of requestors/programmers integrated with the proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:sequence>
            <xs:element name="requestorId" type="xs:string" minOccurs="1" maxOccurs="unbounded" nillable="false">
                <xs:annotation>
                    <xs:documentation>The requestor/programmer identifier recognized by Adobe</xs:documentation>
                </xs:annotation>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
    <xs:complexType name="proxiedMvpd">
        <xs:all>
            <xs:element name="id" minOccurs="1" maxOccurs="1" nillable="false">
                <xs:annotation>
                    <xs:documentation>The id must conform to the regular expression: ([a-zA-Z0-9]+((\-)|[_])*)</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:simpleContent>
                        <xs:extension base="xs:string">
                            <xs:attribute name="ProviderID">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1"/>
                                        <xs:maxLength value="128"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:attribute>
                        </xs:extension>
                    </xs:simpleContent>
                </xs:complexType>
            </xs:element>
            <xs:element name="displayName" type="xs:string" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="logoURL" type="xs:anyURI" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeSize" type="pxm:iframeSize" minOccurs="0" maxOccurs="1"/>
            <xs:element name="requestorIds" type="pxm:requestorIds" minOccurs="0" maxOccurs="1"/>
        </xs:all>
    </xs:complexType>
    <xs:element name="proxiedMvpds">
        <xs:annotation>
            <xs:documentation>List of Proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:complexType>
            <xs:sequence>
                <xs:element name="proxiedMvpd" type="pxm:proxiedMvpd" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

**Remarques sur les éléments :**

-   `id` (obligatoire) - L’identifiant MVPD proxy doit être une chaîne appropriée au nom du MVPD, à l’aide de l’un des caractères suivants (car il sera exposé aux programmeurs à des fins de suivi) :
-   Caractères alphanumériques, trait de soulignement (&quot;_&quot;) et tiret (&quot;-&quot;).
-   L’idID doit être conforme à l’expression régulière suivante :
`(a-zA-Z0-9((-)|_)*)`

     Il doit donc comporter au moins un caractère, commencer par une lettre et continuer avec n’importe quelle lettre, chiffre, tiret ou trait de soulignement.

-   `iframeSize` (facultatif) - L’élément iframeSize est facultatif et définit la taille de l’iFrame si la page d’authentification MVPD est censée se trouver dans un iFrame. Dans le cas contraire, si l’élément iframeSize n’est pas présent, l’authentification se produit dans une page de redirection complète du navigateur.
-   `requestorIds` (facultatif) - Les valeurs requestorIds seront fournies par Adobe. Une exigence est qu’un MVPD proxy soit intégré à au moins un requestorId. Si la balise &quot;requestorIds&quot; n’est pas présente sur l’élément MVPD proxy, ce MVPD proxy sera intégré à tous les demandeurs disponibles intégrés dans le MVPD proxy.
-   `ProviderID` (facultatif) - Lorsque l’attribut ProviderID est présent sur l’élément id, la valeur de ProviderID est envoyée sur la demande d’authentification SAML au MVPD du proxy en tant que MVPD proxy / SubMVPD ID (au lieu de la valeur id). Dans ce cas, la valeur de l’identifiant sera utilisée uniquement dans le sélecteur MVPD présenté sur la page Programmeur et en interne par l’authentification Adobe Pass. La longueur de l’attribut ProviderID doit être comprise entre 1 et 128 caractères.

## Sécurité {#security}

Pour qu’une demande soit considérée comme valide, elle doit respecter les règles suivantes :

- L’en-tête de requête doit contenir le jeton d’accès Oauth2 de sécurité obtenu comme décrit dans la documentation de l’API [Récupérer le jeton d’accès](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
- La demande doit provenir d’une adresse IP spécifique qui a été autorisée.
- La demande doit être envoyée via le protocole SSL.

Tous les paramètres présents dans l’en-tête de la requête qui ne sont pas répertoriés ci-dessus seront ignorés.

Exemple de curl :

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/<proxy-mvpd-identifier>/mvpds"`

## Points de terminaison du service Web MVPD de proxy pour les environnements d’authentification Adobe Pass {#proxy-mvpd-wevserv-endpoints}

- **URL de production :** https://mgmt.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **URL intermédiaire :** https://mgmt.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **URL de préproduction :** https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **URL PreQual-Staging :** https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
