---
title: Authentification MVPD
description: Authentification MVPD
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1851'
ht-degree: 0%

---

# Authentification MVPD {#mvpd-authn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#mvpd-authn-overview}

Le rôle de fournisseur de services (SP) est détenu par un programmeur, mais l’authentification Adobe Pass sert de proxy SP pour ce programmeur. L’utilisation de l’authentification Adobe Pass en tant qu’intermédiaire permet aux MVPD et aux programmeurs d’éviter d’avoir à personnaliser leurs processus de droits au cas par cas.

Les étapes ci-dessous présentent la séquence d’événements, à l’aide de l’authentification Adobe Pass, lorsqu’un programmeur demande une authentification à partir d’un MVPD qui prend en charge SAML. Notez que le composant Adobe Pass Authentication Access Enabler est actif sur le client de l’utilisateur/de l’abonné. À partir de là, Access Enabler facilite toutes les étapes du flux d’authentification.

1. Lorsque l’utilisateur demande l’accès à un contenu protégé, Access Enabler lance l’authentification (AuthN) au nom du programmeur (SP).
1. L&#39;application du fournisseur de services présente un « MVPD Picker » à l&#39;utilisateur afin d&#39;obtenir son fournisseur de télévision payante (MVPD). Le fournisseur de services de chiffrement redirige ensuite le navigateur de l’utilisateur vers le service fournisseur d’identité (IdP) MVPD sélectionné.  Il s’agit de la mention « **Connexion initiée par le programmeur** ».  Le MVPD envoie la réponse de l’IdP au service client d’assertion SAML d’Adobe, où elle est traitée.
1. Enfin, Access Enabler redirige le navigateur vers le site SP, informant le SP du statut (succès/échec) de la demande d’authentification.

## La demande d’authentification {#authn-req}

Comme indiqué dans les étapes ci-dessus, pendant le flux AuthN, un MVPD doit à la fois accepter une requête AuthN basée sur SAML et envoyer une réponse AuthN SAML.

La [spécification de l’interface d’authentification et d’autorisation OLCA (Online Content Access)](https://www.cablelabs.com/specifications/search?query=&category=&subcat=&doctype=&content=false&archives=false){target=_blanck} présente une requête et une réponse AuthN standard. Bien que l’authentification Adobe Pass n’exige pas que les MVPD basent leur messagerie de droits sur cette norme, l’examen de la spécification peut fournir insight dans les attributs clés requis pour une transaction AuthN.

>[!NOTE]
>
>La requête AuthN qu’un MVPD reçoit avec l’authentification Adobe Pass contient une signature numérique. Toutefois, l’exemple ci-dessous n’affiche pas de signature, pour des raisons de concision. Pour un exemple qui affiche une signature numérique, reportez-vous à l’exemple dans [la réponse d’authentification](#authn-response) dans les sections suivantes.

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

Le tableau ci-dessous explique les attributs et les balises qui doivent être présents dans une demande d’authentification, avec les valeurs attendues par défaut.

**Détails de la demande d’authentification SAML**

| samlp :AuthnRequest | &lt;AuthnRequest> émis par le fournisseur de services au fournisseur d&#39;identité. |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | Il s’agit du point d’entrée Adobe à utiliser dans la réponse ultérieure. Valeur par défaut : **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| Destination | Référence URI indiquant l’adresse à laquelle cette requête a été envoyée. Cela s’avère utile pour empêcher le transfert malveillant des requêtes vers des destinataires non prévus, une protection requise par certaines liaisons de protocole. S’il est présent, le destinataire doit vérifier que la référence URI identifie l’emplacement où le message a été reçu. Dans le cas contraire, la requête DOIT être ignorée. Certaines liaisons de protocole peuvent nécessiter l’utilisation de cet attribut. |
| ForceAuthn | L’attribut ForceAuthn, s’il est présent avec une valeur true, oblige le fournisseur d’identité à établir cette identité à nouveau, plutôt que de compter sur une session existante qu’il peut avoir avec le principal. |
| ID | Identifiant de la requête. Voir [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} section 1.3.4 pour plus de détails. |
| Est passif | Valeur booléenne. Si la valeur est « true », le fournisseur d’identité et l’agent utilisateur lui-même NE DOIVENT PAS visiblement prendre le contrôle de l’interface utilisateur à partir du demandeur et interagir avec le présentateur de manière visible. Si aucune valeur n’est fournie, la valeur par défaut est « false ». |
| IssueInstant | L’instant d’émission de la réponse. La valeur temporelle est codée en UTC comme décrit dans [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} Section 1.3.3. |
| ProtocolBinding | Référence URI qui identifie une liaison de protocole SAML à utiliser lors du renvoi du message &lt;Response>. Voir [SAMLBind] pour plus d’informations sur les liaisons de protocole et les références URI définies pour celles-ci. Valeur par défaut : urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST |
| Version | Version de cette requête. |
| saml:Issuer | Identifie l’entité qui a généré le message de réponse. (Pour plus d’informations sur cet élément, voir SAML core 2.0-os Section 2.2.5) |
| ds:Signature | Signature XML qui protège l’intégrité de l’assertion et authentifie l’émetteur de celle-ci, comme décrit dans la section 5 de SAML core 2.0-os |
| samlp :NameIDPolicy | Spécifie les contraintes sur l&#39;identifiant de nom à utiliser pour représenter l&#39;objet demandé. |
| AllowCreate | Valeur booléenne utilisée pour indiquer si le fournisseur d’identité est autorisé, au cours de l’exécution de la requête, à créer un nouvel identifiant pour représenter le principal. Valeur par défaut : true |
| Format | Indique la référence URI correspondant à un format d’identifiant de nom. Par défaut : urn:oasis:names:tc:SAML:2.0:nameid-format:transient Adobe recommande : urn:oasis:names:tc:SAML:2.0:nameid-format:persistent. |
| SPNameQualifier | Spécifie éventuellement que l&#39;identifiant de l&#39;objet d&#39;assertion doit être renvoyé (ou créé) dans l&#39;espace de noms d&#39;un fournisseur de services autre que le demandeur. Valeur par défaut : http://saml.sp.adobe.adobe.com |

## La réponse d’authentification {#authn-response}

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


Dans l’exemple ci-dessus, le SP d’Adobe s’attend à récupérer l’ID utilisateur à partir de l’ID d’objet/de nom. Le SP d’Adobe peut être configuré pour obtenir l’ID utilisateur à partir d’un attribut défini sur mesure. La réponse doit contenir un élément du type suivant :

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

| samlp :Response | Réponse reçue par l’authentification Adobe Pass. |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Destination | Référence URI indiquant l’adresse à laquelle cette requête a été envoyée. Cela s’avère utile pour empêcher le transfert malveillant des requêtes vers des destinataires non prévus, une protection requise par certaines liaisons de protocole. S’il est présent, le destinataire doit vérifier que la référence URI identifie l’emplacement où le message a été reçu. Dans le cas contraire, la requête DOIT être ignorée. Certaines liaisons de protocole peuvent nécessiter l’utilisation de cet attribut. |
| ID | Identifiant de la requête. Il est de type xs:ID et DOIT respecter les exigences spécifiées dans la section 1.3.4 de [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} pour l&#39;unicité de l&#39;identifiant. Les valeurs de l’attribut ID dans une requête et de l’attribut InResponseTo dans la réponse correspondante DOIVENT correspondre. |
| InResponseTo | Identifiant d’un message de protocole SAML en réponse auquel une entité d’attestation peut présenter l’assertion. La valeur doit être égale à celle de l’attribut ID envoyé dans la requête d’authentification. Voir SAML core 2.0-os |
| IssueInstant | Moment d’émission de la requête. |
| Version | Version de la requête. |
| saml:Issuer | Identifie l’entité qui a généré le message de requête. (Pour plus d&#39;informations sur cet élément, voir la section 2.2.5. SAML core 2.0-os ) |
| samlp :Status | Code représentant le statut de la requête correspondante. |
| samlp :StatusCode | Un code représentant l’état de l’activité exécutée en réponse à la requête correspondante. |
| saml:Assertion | Ce type spécifie les informations de base communes à toutes les assertions. |
| ID | Identifiant de cette assertion. |
| Version | Version de cette assertion. |
| IssueInstant | Moment d’émission de la requête. |
| ds:Signature | Signature XML qui protège l’intégrité de l’assertion et authentifie l’émetteur de celle-ci, comme décrit dans la section 5 de SAML core 2.0-os |
| ds:SignedInfo | La structure de SignedInfo comprend l’algorithme de canonisation, un algorithme de signature et une ou plusieurs références. L’élément SignedInfo peut contenir un attribut d’ID facultatif qui lui permettra d’être référencé par d’autres signatures et objets. Voir Syntaxe et traitement de la signature XML |
| ds:CanonicalizationMethod | CanonicalizationMethod est un élément obligatoire qui spécifie l&#39;algorithme de canonisation appliqué à l&#39;élément SignedInfo avant d&#39;effectuer les calculs de signature. Voir Syntaxe et traitement de la signature XML |
| ds:SignatureMethod | SignatureMethod est un élément obligatoire qui spécifie l’algorithme utilisé pour la génération et la validation des signatures. Cet algorithme identifie toutes les fonctions cryptographiques impliquées dans l&#39;opération de signature (par exemple hachage, algorithmes de clé publique, MAC, padding, etc.) Voir Syntaxe et traitement de la signature XML |
| ds:Reference | La référence est un élément qui peut se produire une ou plusieurs fois. Il spécifie un algorithme de digest et une valeur de digest, et éventuellement un identifiant de l&#39;objet signé, le type de l&#39;objet, et/ou une liste de transformations à appliquer avant la digestion. Voir Syntaxe et traitement de la signature XML |
| ds:Transforms | L’élément facultatif Transforms contient une liste ordonnée d’éléments Transform ; ils décrivent la manière dont le signataire a obtenu l’objet de données qui a été assimilé. La sortie de chaque transformation sert d’entrée à la transformation suivante. L’entrée de la première transformation est le résultat du déréférencement de l’attribut URI de l’élément de référence. La sortie de la dernière transformation est l&#39;entrée de l&#39;algorithme DigestMethod. Voir Syntaxe et traitement de la signature XML |
| ds:DigestMethod | DigestMethod est un élément obligatoire qui identifie l&#39;algorithme de prétraitement à appliquer à l&#39;objet signé. Voir Syntaxe et traitement de la signature XML |
| ds:DigestValue | DigestValue est un élément qui contient la valeur codée du résumé. Le résumé est toujours codé en base64. Voir Syntaxe et traitement de la signature XML |
| ds:SignatureValue | L’élément SignatureValue contient la valeur réelle de la signature numérique ; elle est toujours codée en base64. Voir Syntaxe et traitement de la signature XML |
| ds:KeyInfo | KeyInfo est un élément facultatif permettant au(x) destinataire(s) d&#39;obtenir la clé nécessaire pour valider la signature. Voir Syntaxe et traitement de la signature XML |
| ds:X509Data | Un élément X509Data dans KeyInfo contient un ou plusieurs identifiants de clés ou de certificats X509. Voir Syntaxe et traitement de la signature XML |
| ds : certificat X509Certificate | L’élément X509Certificate, qui contient un certificat [X509v3] codé en base64 |
| saml:Subject | Objet de la ou des déclarations dans l’assertion. |
| saml:NameID | L’élément &lt;NameID> est de type NameIDType (voir la section 2.2.2 dans SAML core 2.0-os) et est utilisé dans divers concepts d’assertions SAML tels que les éléments &lt;Subject> et &lt;SubjectConfirmation>, ainsi que dans divers messages de protocole. |
| Format | Référence URI représentant la classification d’informations d’identifiant basées sur une chaîne. |
| SPNameQualifier | Qualifie en outre un nom avec le nom d’un fournisseur de services ou l’affiliation de fournisseurs. Cet attribut fournit un moyen supplémentaire de fédérer des noms sur la base de la ou des parties de confiance. |
| saml:SubjectConfirmation | Informations permettant de confirmer le titulaire. Si plusieurs confirmations du sujet sont fournies, il suffit de satisfaire l&#39;un d&#39;entre eux pour confirmer le sujet aux fins de l&#39;application de l&#39;assertion. |
| saml:SubjectConfirmationData | Informations de confirmation supplémentaires à utiliser par une méthode de confirmation spécifique. Par exemple, le contenu type de cet élément peut être un élément <!--<ds:KeyInfo>--> tel que défini dans la spécification Syntaxe et traitement de la signature XML |
| NotOnOrAfter | Moment auquel le sujet ne peut plus être confirmé. |
| Destinataire | URI spécifiant l’entité ou l’emplacement auquel une entité d’attestation peut présenter l’assertion. Par exemple, cet attribut peut indiquer que l’assertion doit être diffusée à un point d’entrée réseau particulier afin d’empêcher un intermédiaire de la rediriger vers un autre emplacement. |
| saml:Conditions | L’élément &lt;Condition> sert de point d’extension pour les nouvelles conditions. |
| NotBefore | L’attribut NotBefore spécifie l’instant auquel l’intervalle de validité commence. |
| saml:AudienceRestriction | L’élément &lt;AudienceRestriction> indique que l’assertion s’adresse à une ou plusieurs audiences spécifiques identifiées par les éléments &lt;Audience>. |
| saml:Audience | Référence URI qui identifie une audience ciblée. |
| saml:AuthnStatement | Instruction d’authentification. |
| AuthnInstant | Indique l’heure à laquelle l’authentification a eu lieu. |
| SessionIndex | Indique l’index d’une session particulière entre le principal identifié par le sujet et l’autorité d’authentification. |
| saml:AuthnContext | Contexte utilisé par l’autorité d’authentification jusqu’à l’événement d’authentification qui a généré cette instruction inclus. |
| saml:AuthnContextClassRef | Référence URI identifiant une classe de contexte d’authentification qui décrit la déclaration de contexte d’authentification qui suit. |
