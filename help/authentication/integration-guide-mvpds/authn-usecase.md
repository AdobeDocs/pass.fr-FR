---
title: Authentification MVPD
description: Authentification MVPD
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1882'
ht-degree: 0%

---

# Authentification MVPD {#mvpd-authn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#mvpd-authn-overview}

Le rôle de fournisseur de services réel (SP) est détenu par un programmeur, mais l’authentification Adobe Pass sert de proxy SP pour ce programmeur. L’utilisation de l’authentification Adobe Pass en tant qu’intermédiaire permet aux programmes de développement multivarié et aux programmeurs d’éviter d’avoir à personnaliser leurs processus de droits au cas par cas.

Les étapes ci-dessous présentent la séquence d’événements, à l’aide de l’authentification Adobe Pass, lorsqu’un programmeur demande l’authentification auprès d’un MVPD qui prend en charge SAML. Notez que le composant Adobe Pass Authentication Access Enabler est actif sur le client de l’utilisateur/abonné. A partir de là, l’Access Enabler facilite toutes les étapes du flux d’authentification.

1. Lorsque l’utilisateur demande l’accès à un contenu protégé, Access Enabler lance l’authentification (AuthN) pour le compte du programmeur (SP).
1. L’application du SP présente un &quot;Sélecteur MVPD&quot; à l’utilisateur afin d’obtenir son fournisseur de télévision payante (MVPD). Le SP redirige ensuite le navigateur de l’utilisateur vers le service de fournisseur d’identité (IdP) du MVPD sélectionné.  Il s’agit de &quot;**Login** initié par le programmeur&quot;.  Le MVPD envoie la réponse de l’IdP au service consommateur d’assertion SAML de l’Adobe, où elle est traitée.
1. Enfin, Access Enabler redirige le navigateur vers le site du SP, informant le SP de l’état (succès/échec) de la requête AuthN.

## La demande d’authentification {#authn-req}

Comme présenté dans les étapes ci-dessus, pendant le flux AuthN, un MVPD doit accepter une requête AuthN basée sur SAML et envoyer une réponse AuthN SAML.

La [spécification de l’interface d’authentification et d’autorisation d’accès au contenu en ligne (OLCA)](https://www.cablelabs.com/specifications/search?query=&amp;category=&amp;subcat=&amp;doctype=&amp;content=false&amp;archives=false){target=_blanck} présente une requête et une réponse AuthN standard. Bien que l’authentification Adobe Pass ne nécessite pas que les MVPD basent leurs messages de droits sur cette norme, l’examen de la spécification peut fournir des informations sur les attributs clés requis pour une transaction AuthN.

>[!NOTE]
>
>La requête AuthN qu’un MVPD reçoit avec l’authentification Adobe Pass contient une signature numérique. Cependant, l’exemple ci-dessous n’affiche pas de signature pour des raisons de concision. Pour un exemple qui affiche une signature numérique, reportez-vous à l’exemple de la [réponse d’authentification](#authn-response) dans les sections suivantes.

Exemple de requête d’authentification SAML :

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:AuthnRequest  
    AssertionConsumerServiceURL=http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer          
    Destination=http://idp.com/SSOService
    ForceAuthn="false"
    ID="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
    IsPassive="false"
    IssueInstant="2010-08-03T14:14:54.372Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
    Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        http://saml.sp.adobe.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds=_signature_block_goes_here_
    </ds:Signature>
    <samlp:NameIDPolicy
        AllowCreate="true"
        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
        SPNameQualifier="http://saml.sp.adobe.adobe.com"/>
</samlp:AuthnRequest> 
```

Le tableau ci-dessous explique les attributs et balises qui doivent se trouver dans une requête d’authentification, avec les valeurs attendues par défaut.

**Détails de la demande d’authentification SAML**

| samlp:AuthnRequest | &lt;AuthnRequest> émise par le fournisseur de services au fournisseur d’identité. |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | Il s’agit du point de terminaison Adobe à utiliser dans la réponse suivante. Valeur par défaut : **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| Destination | Une référence URI indiquant l’adresse à laquelle cette demande a été envoyée. Cela s’avère utile pour empêcher le transfert malveillant des demandes vers des destinataires inattendus, une protection requise par certaines liaisons de protocole. S’il est présent, le destinataire réel DOIT vérifier que la référence à l’URI identifie l’emplacement où le message a été reçu. Dans le cas contraire, la requête DOIT être ignorée. Certaines liaisons de protocole peuvent nécessiter l’utilisation de cet attribut. |
| ForceAuthn | L’attribut ForceAuthn, s’il est présent avec la valeur true, oblige le fournisseur d’identité à établir fraîchement cette identité, plutôt que de compter sur une session existante qu’il peut avoir avec l’entité. |
| ID | Identifiant de la requête. Voir la section 1.3.4 [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} pour plus d’informations. |
| IsPassive | Valeur booléenne. Si la valeur est &quot;true&quot;, le fournisseur d’identité et l’agent utilisateur lui-même NE DOIVENT PAS visiblement prendre le contrôle de l’interface utilisateur du demandeur et interagir avec le présentateur de manière perceptible. Si aucune valeur n’est fournie, la valeur par défaut est &quot;false&quot;. |
| IssueInstant | Heure d’envoi de la réponse. La valeur de temps est codée en UTC comme décrit dans la section [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} Section 1.3.3. |
| ProtocolBinding | Référence d’URI qui identifie une liaison de protocole SAML à utiliser lors du renvoi du message &lt;Réponse>. Voir [SAMLBind] pour plus d’informations sur les liaisons de protocole et les références URI définies pour ces connexions. Valeur par défaut : urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST |
| Version | Version de cette requête. |
| saml:Issuer | Identifie l’entité qui a généré le message de réponse. (Pour plus d’informations sur cet élément, voir la section 2.2.5 de base SAML 2.0-os) |
| ds:Signature | Une signature XML qui protège l’intégrité de l’émetteur de l’assertion et l’authentifie, comme décrit à la section 5 de la section 2.0-os de base SAML. |
| samlp:NameIDPolicy | Spécifie les contraintes sur l’identifiant de nom à utiliser pour représenter l’objet demandé. |
| AllowCreate | Une valeur booléenne utilisée pour indiquer si le fournisseur d’identité est autorisé, dans le cadre de la satisfaction de la demande, à créer un identifiant pour représenter l’entité. Valeur par défaut : true |
| Format | Spécifie la référence URI correspondant à un format d’identifiant de nom Par défaut : urn:oasis:names:tc:SAML:2.0:nameid-format:transient Adobe recommande : urn:oasis:names:tc:SAML:2.0:nameid-format:persistent |
| SPNameQualifier | Vous pouvez éventuellement spécifier que l’identifiant du sujet de l’assertion soit renvoyé (ou créé) dans l’espace de noms d’un prestataire autre que le demandeur. Par défaut : http://saml.sp.adobe.adobe.com |

## Réponse d’authentification {#authn-response}

Après avoir reçu et géré la demande d’authentification, le MVPD doit maintenant envoyer une réponse d’authentification.

**Exemple de réponse d’authentification SAML**

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<samlp:Response Destination="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_0ac3a9dd5dae0ce05de20912af6f4f83a00ce19587"                             
                InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                IssueInstant="2010-08-17T11:17:50Z" Version="2.0"              
                xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
                xmlns:xs="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
             http://idp.com/SSOService
    </saml:Issuer>
    <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="pfxb0662d76-17a2-a7bd-375f-c11046a86742"
                   IssueInstant="2010-08-17T11:17:50Z"
                   Version="2.0">
        <saml:Issuer>http://idp.com/SSOService</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod
                     Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod
                     Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
            <ds:Reference URI="#pfxb0662d76-17a2-a7bd-375f-c11046a86742">
              <ds:Transforms>
                 <ds:Transform
                    Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>        
                 <ds:Transform
                            Algorithm=http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <ds:DigestValue>LgaPI2ASx/fHsoq0rB15Zk+CRQ0=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
                POw/mCKF__shortened_for_brevity__9xdktDu+iiQqmnTs/NIjV5dw==
          </ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
                <ds:X509Certificate>
                 MIIDVDCCAjygAwIBA__shortened_for_brevity_utQ==
                </ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
      </ds:Signature>
      <saml:Subject>
        <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                     SPNameQualifier="https://saml.sp.auth.adobe.com">
            _5afe9a437203354aa8480ce772acb703e6bbb8a3ad
        </saml:NameID>
        <saml:SubjectConfirmation
                     Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                  InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                  NotOnOrAfter="2010-08-17T11:22:50Z"                                          
                  Recipient="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"/>
           </saml:SubjectConfirmation>
       </saml:Subject>
       <saml:Conditions NotBefore="2010-08-17T11:17:20Z"
                        NotOnOrAfter="2010-08-17T19:17:50Z">
           <saml:AudienceRestriction>
              <saml:Audience>https://saml.sp.auth.adobe.com</saml:Audience>
           </saml:AudienceRestriction>
       </saml:Conditions>
       <saml:AuthnStatement AuthnInstant="2010-08-17T11:17:50Z"
                   SessionIndex="_1adc7692e0fffbb1f9b944aeafce62aaa7d770cd9e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                   urn:oasis:names:tc:SAML:2.0:ac:classes:Password
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
  </saml:Assertion>
</samlp:Response>
```


Dans l’exemple ci-dessus, le SP Adobe s’attend à récupérer l’ID utilisateur en dehors de l’ID de sujet/nom. Le SP d’Adobe peut être configuré pour obtenir l’ID utilisateur à partir d’un attribut défini personnalisé. La réponse doit contenir un élément comme suit :

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**Détails de la réponse d’authentification SAML**

| samlp:Response | Réponse reçue par l’authentification Adobe Pass. |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Destination | Une référence URI indiquant l’adresse à laquelle cette demande a été envoyée. Cela s’avère utile pour empêcher le transfert malveillant des demandes vers des destinataires inattendus, une protection requise par certaines liaisons de protocole. S’il est présent, le destinataire réel DOIT vérifier que la référence à l’URI identifie l’emplacement où le message a été reçu. Dans le cas contraire, la requête DOIT être ignorée. Certaines liaisons de protocole peuvent nécessiter l’utilisation de cet attribut. |
| ID | Identifiant de la requête. Il est de type xs:ID et DOIT respecter les exigences spécifiées à la section 1.3.4 de [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} pour l’unicité de l’identifiant. Les valeurs de l’attribut ID dans une requête et l’attribut InResponseTo de la réponse correspondante DOIVENT correspondre. |
| InResponseTo | L’identifiant d’un message de protocole SAML en réponse auquel une entité d’évaluation peut présenter l’assertion. La valeur doit être égale à celle de l’attribut d’identifiant envoyé dans la demande d’authentification. Voir SAML core 2.0-os |
| IssueInstant | Heure d’émission de la requête. |
| Version | Version de la requête. |
| saml:Issuer | Identifie l’entité qui a généré le message de requête. (Pour plus d’informations sur cet élément, voir Section 2.2.5. SAML core 2.0-os ) |
| samlp:Status | Code représentant l’état de la requête correspondante. |
| samlp:StatusCode | Code représentant l’état de l’activité réalisée en réponse à la requête correspondante. |
| saml:Assertion | Ce type spécifie les informations de base communes à toutes les assertions. |
| ID | L’identifiant de cette assertion. |
| Version | Version de cette assertion. |
| IssueInstant | Heure d’émission de la requête. |
| ds:Signature | Une signature XML qui protège l’intégrité de l’émetteur de l’assertion et l’authentifie, comme décrit à la section 5 de la section 2.0-os de base SAML. |
| ds:SignedInfo | La structure de SignedInfo inclut l’algorithme de canonisation, un algorithme de signature et une ou plusieurs références. L’élément SignedInfo peut contenir un attribut d’identifiant facultatif qui lui permettra d’être référencé par d’autres signatures et objets. Voir Syntaxe de la signature XML et traitement |
| ds:CanonicalizationMethod | CanonicalizationMethod est un élément obligatoire qui spécifie l’algorithme de canonisation appliqué à l’élément SignedInfo avant d’effectuer des calculs de signature. Voir Syntaxe de la signature XML et traitement |
| ds:SignatureMethod | SignatureMethod est un élément obligatoire qui spécifie l’algorithme utilisé pour la génération et la validation des signatures. Cet algorithme identifie toutes les fonctions cryptographiques impliquées dans l’opération de signature (par exemple, le hachage, les algorithmes de clé publique, les MAC, la marge intérieure, etc.) Voir Syntaxe de la signature XML et traitement |
| ds:Reference | La référence est un élément qui peut se produire une ou plusieurs fois. Il spécifie un algorithme de prétraitement et une valeur de prétraitement, et éventuellement un identifiant de l’objet signé, le type de l’objet et/ou une liste de transformations à appliquer avant la digestion. Voir Syntaxe de la signature XML et traitement |
| ds:Transforms | L’élément facultatif Transforms contient une liste ordonnée d’éléments Transform ; ces éléments décrivent comment le signataire a obtenu l’objet de données qui a été digéré. La sortie de chaque transformation sert d’entrée à la transformation suivante. L’entrée de la première transformation est le résultat du déréférencement de l’attribut URI de l’élément Référence . La sortie de la dernière transformation est l’entrée de l’algorithme DigestMethod. Voir Syntaxe de la signature XML et traitement |
| ds:DigestMethod | DigestMethod est un élément obligatoire qui identifie l’algorithme de condensé à appliquer à l’objet signé. Voir Syntaxe de la signature XML et traitement |
| ds:DigestValue | DigestValue est un élément qui contient la valeur codée du condensé. Le condensé est toujours codé en base64. Voir Syntaxe de la signature XML et traitement |
| ds:SignatureValue | L’élément SignatureValue contient la valeur réelle de la signature numérique ; il est toujours codé en base64. Voir Syntaxe de la signature XML et traitement |
| ds:KeyInfo | KeyInfo est un élément facultatif qui permet au ou aux destinataires d’obtenir la clé nécessaire pour valider la signature. Voir Syntaxe de la signature XML et traitement |
| ds:X509Data | Un élément X509Data dans KeyInfo contient un ou plusieurs identifiants de clés ou de certificats X509. Voir Syntaxe de la signature XML et traitement |
| ds : X509Certificate | L’élément X509Certificate, qui contient un certificat [X509v3] codé en base64 |
| saml:Subject | Objet de la ou des instructions dans l’assertion. |
| saml:NameID | L’élément &lt;NameID> est de type NameIDType (voir la section 2.2.2 dans le noyau SAML 2.0-os) et est utilisé dans divers éléments d’assertion SAML tels que les éléments &lt;Subject> et &lt;SubjectConfirmation>, ainsi que dans divers messages de protocole. |
| Format | Référence d’URI représentant la classification des informations d’identifiant basées sur des chaînes. |
| SPNameQualifier | De plus, qualifie un nom avec le nom d’un prestataire ou l’affiliation de celui-ci. Cet attribut fournit un moyen supplémentaire de fédérer les noms sur la base du ou des parties qui comptent. |
| saml:SubjectConfirmation | Informations permettant de confirmer l’objet. Si plus d’une confirmation de sujet est fournie, la satisfaction de l’un d’entre eux suffit à confirmer l’objet dans le but d’appliquer l’assertion. |
| saml:SubjectConfirmationData | Informations de confirmation supplémentaires à utiliser par une méthode de confirmation spécifique. Par exemple, le contenu type de cet élément peut être un élément <!--<ds:KeyInfo>--> tel que défini dans la syntaxe de signature XML et la spécification de traitement |
| NotOnOrAfter | Un moment au cours duquel le sujet ne peut plus être confirmé. |
| Destinataire | URI spécifiant l’entité ou l’emplacement auquel une entité test peut présenter l’assertion. Par exemple, cet attribut peut indiquer que l’assertion doit être transmise à un point d’entrée réseau particulier afin d’empêcher un intermédiaire de le rediriger ailleurs. |
| saml:Conditions | L’élément &lt;Condition> sert de point d’extension pour les nouvelles conditions. |
| NotBefore | L’attribut NotBefore spécifie l’heure à laquelle l’intervalle de validité commence. |
| saml:AudienceRestriction | L’élément &lt;AudienceRestriction> spécifie que l’assertion est adressée à une ou plusieurs audiences spécifiques identifiées par des éléments &lt;Audience> . |
| saml:Audience | Référence d’URI qui identifie une audience prévue. |
| saml:AuthnStatement | Une instruction d’authentification. |
| AuthnInstant | Indique l’heure à laquelle l’authentification a eu lieu. |
| SessionIndex | Spécifie l’index d’une session particulière entre l’entité identifiée par le sujet et l’autorité d’authentification. |
| saml:AuthnContext | Le contexte utilisé par l’autorité d’authentification jusqu’à l’événement d’authentification ayant généré cette instruction et y compris. |
| saml:AuthnContextClassRef | Référence d’URI identifiant une classe de contexte d’authentification qui décrit la déclaration de contexte d’authentification qui suit. |
