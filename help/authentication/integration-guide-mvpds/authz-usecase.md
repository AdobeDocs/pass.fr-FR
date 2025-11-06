---
title: Autorisation MVPD
description: Autorisation MVPD
exl-id: 215780e4-12b6-4ba6-8377-4d21b63b6975
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# Autorisation MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#mvpd-authz-overview}

L’autorisation (AuthZ) est effectuée via des communications back-canal (serveur à serveur) entre un serveur principal hébergé par Adobe et le point d’entrée AuthZ de MVPD.

Pour les requêtes AuthZ, le point d’entrée d’autorisation doit pouvoir traiter au moins les paramètres suivants :

* **Uid** Identifiant utilisateur reçu de l’étape d’authentification .

* **ID de ressource**. Chaîne identifiant une ressource de contenu donnée. Cet identifiant de ressource est spécifié par le programmeur et le MVPD doit renforcer les règles métier relatives à ces ressources (par exemple, en vérifiant que l’utilisateur est abonné à un certain canal).

Outre le fait de déterminer si l’utilisateur est autorisé, la réponse doit inclure la durée de vie (TTL) de cette autorisation, c’est-à-dire le moment où l’autorisation expire. Si la TTL n’est pas définie, la requête AuthZ échoue.  C’est pourquoi la **TTL est un paramètre de configuration obligatoire du côté de l’authentification Adobe Pass** afin de couvrir le cas où un MVPD n’inclut pas la TTL dans sa requête.

## La Demande D’Autorisation {#authz-req}

Une requête AuthZ doit inclure un objet au nom duquel la requête est effectuée, la ou les ressources auxquelles l’objet tente d’accéder, l’action que l’objet tente d’effectuer sur la ressource et l’environnement dans lequel l’opération est sur le point d’avoir lieu. Dans le cas particulier de l’authentification Adobe Pass, ces éléments correspondent à :

| Élément XML | Correspond à |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| Objet | Principal identifié par la session authentifiée, référencé par l’attribut « subject-token » de l’assertion SAML. |
| Ressource | URI de la ressource protégée. |
| Action | VUE. |
| Environnement | Inclut l’adresse IP du client demandeur, telle qu’elle est vue par le fournisseur de service. |



À ce stade, le fournisseur de service doit préparer une requête de décision d’autorisation XML et l’envoyer (via HTTP POST) au point de décision de politique (PDP) pour l’IdP (accord préalable). Voici un exemple de requête XACML simple (voir Spécification de base XACML) :

```XML
POST https://authz.site.com/XACML_endpoint
<Request  xmlns="urn:oasis:names:tc:xacm:2.0:context:schema:os"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="urn:oasis:names:tc:xacml:2.0:context:schema:os
http://docs.oasis-open.org/xacml/access_control-xacml-2.0-context-schema-os.xsd">
<Subject>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-token"
        DataType="http://www.w3.org/2001/XMLSchema#base64Binary">
      <AttributeValue>{Base64 Data}</AttributeValue>
   </Attribute>
</Subject>
<Resource>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
        DataType="http://www.w3.org/2001/XMLSchema#anyURI">
<AttributeValue>urn:tve:tms:1234</AttributeValue>
   </Attribute>
</Resource>
<Action>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
        DataType="http://www.w3.org/2001/XMLSchema#string">
       <AttributeValue>VIEW</AttributeValue>
   </Attribute>
</Action>
<Environment>
   <Attribute
       AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address"
       DataType="http://www.w3.org/2001/XMLSchema#string">
      <AttributeValue>1.2.3.4</AttributeValue>
   </Attribute>
</Environment>
</Request>
```


Après réception de la requête AuthZ, le PDP MVPD évalue la requête et détermine si le sujet doit être autorisé à effectuer l’action demandée sur la ressource. Le MVPD renvoie ensuite une réponse avec une décision, un code d’état et un message, comme décrit dans la réponse d’autorisation ci-dessous.

## La réponse d’autorisation {#authz-response}

La réponse à la requête AuthZ survient après que le MVPD a évalué la requête et appliqué les règles métier demandées afin de déterminer si l’objet est autorisé à effectuer l’action demandée sur la ressource . La réponse renvoyée à l’authentification Adobe Pass est exprimée à nouveau à l’aide de la spécification de base XACML avec une décision, un code d’état, un message et des obligations que le fournisseur de service a comme point d’application de politique (PEP). Voici un exemple de réponse :

```XML
<Response xmlns="urn:oasis:names:tc:xacml:2.0:context:schema:os">
  <Result>
  <Decision>Permit</Decision>
  <Status>
     <StatusCode Value="urn:oasis:names:tc:xacml:1.0:status:ok"/>
     <StatusMessage>ok</StatusMessage>
  </Status>
  <xacml:Obligations     
          xmlns:xacml="urn:oasis:names:tc:xacml:2.0:policy:schema:os">
     <xacml:Obligation    
              ObligationId="urn:cablelabs:olca:1.0:obligations:log"
              FulfillOn="Permit" />
  </xacml:Obligations>
 </Result>
</Response>
```

Voici une liste des obligations DENY prises en charge par l’authentification Adobe Pass et auxquelles les programmeurs peuvent satisfaire :

* **urn:tve:xacml:2.0:obligations:restrictionpc** - L’abonné n’a pas effectué de vérification du contrôle parental et le fournisseur de services doit prendre les mesures appropriées pour restreindre l’accès à ce contenu.

* **urn:tve:xacml:2.0:obligations:upgrade** - L&#39;abonné ne dispose pas d&#39;un niveau d&#39;abonnement approprié.  L’abonnement doit être mis à niveau pour accéder au contenu.

L’authentification Adobe Pass prend en charge les obligations **PERMIT** suivantes et permet aux programmeurs de les remplir :

* **urn:cablelabs:olca:1.0:obligations:log** - Adobe Pass consigne la transaction et peut la rendre disponible via le mécanisme de reporting convenu.

* **urn:cablelabs:olca:1.0:obligations:re-authz** - L’authentification Adobe Pass actualise à nouveau l’autorisation en n secondes (spécifiée comme argument de l’obligation via un AttributeAssignment XACML - voir Spécification de base XACML , section 5.46).

<!--
>![RelatedInformation]
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
>* [Authentication](/help/authentication/authn-usecase.md)
-->
