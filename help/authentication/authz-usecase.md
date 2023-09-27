---
title: Autorisation MVPD
description: Autorisation MVPD
exl-id: 215780e4-12b6-4ba6-8377-4d21b63b6975
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# Autorisation MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Présentation {#mvpd-authz-overview}

L’autorisation (AuthZ) est effectuée par le biais de communications back-channel (serveur à serveur) entre un serveur principal hébergé par l’Adobe et le point d’entrée MVPD AuthZ.

Pour les requêtes AuthZ, le point de terminaison d’autorisation doit pouvoir traiter au moins les paramètres suivants :

* **Uid**. ID utilisateur reçu de l’étape d’authentification.

* **ID de ressource**. Chaîne identifiant une ressource de contenu donnée. Cet identifiant de ressource est spécifié par le programmeur, et le MVPD doit renforcer les règles métier sur ces ressources (par exemple, en vérifiant que l’utilisateur est abonné à un certain canal).

En plus de déterminer si l’utilisateur est autorisé, la réponse doit inclure la durée de vie (TTL) de cette autorisation, c’est-à-dire le moment où l’autorisation expire. Si la durée de vie n’est pas définie, la requête AuthZ échoue.  Pour cette raison, **le délai d’activation est un paramètre de configuration obligatoire du côté de l’authentification Adobe Pass.**, afin de couvrir le cas où un MVPD n’inclut pas le TTL dans sa requête.

## La demande d’autorisation {#authz-req}

Une requête AuthZ doit inclure un sujet au nom duquel la requête est effectuée, la ou les ressources auxquelles le sujet tente d’accéder, l’action que le sujet tente d’effectuer sur la ressource et l’environnement dans lequel l’opération est sur le point d’avoir lieu. Dans le cas particulier de l’authentification Adobe Pass, ces éléments correspondent à :

| Elément XACML | Correspond à |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| Objet | Entité principale identifiée par la session authentifiée, référencée par la valeur AttributeValue &#39;subject-token&#39; de l’assertion SAML. |
| Ressource | URI de la ressource protégée. |
| Action | VUE. |
| Environnement | Inclut l’adresse IP du client demandeur, comme le voit le SP. |



À ce stade, le SP doit préparer une requête de décision d’autorisation XACML et l’envoyer (par l’intermédiaire d’un POST HTTP) au point de décision de stratégie (PDP) (précédemment convenu) pour l’IdP. Vous trouverez ci-dessous un exemple de requête XACML simple (voir Spécification de base XACML) :

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


Après réception de la requête AuthZ, le PDP du MVPD évalue la requête et détermine si l’objet doit être autorisé à effectuer l’action demandée sur la ressource. Le MVPD renvoie ensuite une réponse avec une décision, un code d’état et un message, comme décrit dans la réponse d’autorisation ci-dessous.

## Réponse de l’autorisation {#authz-response}

La réponse à la requête AuthZ intervient après que le MVPD a évalué la requête et appliqué les règles commerciales demandées pour déterminer si l’objet est autorisé à exécuter l’action demandée sur la ressource . La réponse renvoyée à l’authentification Adobe Pass est exprimée à nouveau à la suite de la spécification de base XACML avec une décision, un code d’état, un message et des obligations que le SP possède comme point d’application de la stratégie (PEP). Voici un exemple de réponse :

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

Voici une liste des obligations DENY que l’authentification Adobe Pass prend en charge et permet aux programmeurs de remplir :

* **urn:tve:xacml:2.0:obligations:restricted-pc** - L&#39;abonné n&#39;a pas effectué de contrôle parental et le SP doit prendre les mesures appropriées pour restreindre l&#39;accès à ce contenu.

* **urn:tve:xacml:2.0:obligations:upgrade** - L&#39;abonné ne dispose pas d&#39;un niveau d&#39;abonnement approprié.  Abonnement à la mise à niveau nécessaire pour accéder au contenu.

L’authentification Adobe Pass prend en charge les **PERMIT** Les obligations et permettent aux programmeurs de les remplir :

* **urn:cablelabs:olca:1.0:obligations:log** - Adobe Pass consigne la transaction et peut la rendre disponible via le mécanisme de reporting convenu.

* **urn:cablelabs:olca:1.0:obligations:re-authz** - L’authentification Adobe Pass actualise à nouveau l’autorisation en n secondes (spécifiée en tant qu’argument de l’obligation via une Attribution XACML - voir Spécification de base XACML , Section 5.46).

<!--
>![RelatedInformation]
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
>* [Authentication](/help/authentication/authn-usecase.md)
-->
