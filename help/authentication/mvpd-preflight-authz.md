---
title: Autorisation de contrôle en amont MVPD
description: Autorisation de contrôle en amont MVPD
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---

# Autorisation de contrôle en amont MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#mvpd-preflight-authz-intro}

&quot;Autorisation de contrôle en amont&quot; est une vérification d’autorisation légère pour plusieurs ressources. Les programmeurs l’utilisent principalement pour décorer leurs interfaces utilisateur (par exemple, en indiquant l’état d’accès avec les icônes de verrouillage et de déverrouillage).

L’authentification Adobe Pass peut actuellement prendre en charge l’autorisation de contrôle en amont de deux manières pour les MVPD, soit par l’intermédiaire des attributs de réponse AuthN, soit par une requête AuthZ multicanal.  Les scénarios suivants décrivent les coûts et les avantages des différentes façons dont vous pouvez mettre en oeuvre l’autorisation de contrôle en amont :

* **Scénario de cas idéal** - Le MVPD fournit la liste des ressources préautorisées pendant la phase d’autorisation (AuthZ multicanal).
* **Scénario du pire cas** - Si un MVPD ne prend en charge aucune forme d’autorisation de ressources multiples, le serveur d’authentification Adobe Pass effectue un appel d’autorisation au MVPD pour chaque ressource de la liste des ressources. Ce scénario a un impact (proportionnel au nombre de ressources) sur le temps de réponse de la demande d’autorisation de contrôle en amont. Cela peut augmenter la charge sur les serveurs Adobe et MVPD, ce qui entraîne des problèmes de performances. En outre, il génère des requêtes d’autorisation/événements de réponse sans avoir à effectuer une lecture.
* **Obsolète** - Le MVPD fournit la liste des ressources préautorisées pendant la phase d’authentification. Il n’y aura donc aucun appel réseau nécessaire, pas même la demande de contrôle en amont, puisque la liste est mise en cache sur le client.

Bien que les MVPD n’aient pas à prendre en charge l’autorisation de contrôle en amont, les sections suivantes décrivent certaines méthodes d’autorisation de contrôle en amont que l’authentification Adobe Pass peut prendre en charge, avant de revenir au pire scénario ci-dessus.

## Contrôle en amont dans AuthN {#preflight-authn}

Ce scénario de contrôle en amont est compatible OLCA (Cableabs). La section 7.5.2 de la spécification de l’interface d’authentification et d’autorisation intitulée &quot;Attribute Statement Within Authentication Assertion&quot; (Instruction d’attribut dans l’assertion d’authentification) décrit comment une réponse d’authentification SAML peut contenir une liste de ressources préautorisées. Si un IdP le prend en charge, le serveur d’authentification Adobe Pass pourra générer la liste des ressources préconfigurée au moment de l’authentification et la mettre en cache sur le client avec le jeton d’authentification. Cette méthode permet également d’obtenir le meilleur scénario possible, et aucun appel réseau ne sera effectué lorsque le programmeur appelle checkPreauthorizedResources(), car tout se trouve déjà sur le client.

### Liste des ressources personnalisées dans l’instruction d’attribut SAML {#custom-res-saml-attr}

La réponse d’authentification SAML de l’IdP doit inclure une instruction AttributeStatement contenant les noms de ressources qu’AdobePass doit autoriser.  Certains MVPD fournissent ceci au format suivant :

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

L&#39;exemple ci-dessus présente une liste contenant deux ressources préautorisées : &quot;MMOD&quot; et &quot;Jeux Olympiques 2012&quot;.

Cela permet d’atteindre le meilleur scénario, et aucun appel réseau ne sera effectué lorsque le programmeur appelle checkPreauthorizedResources(), puisque tout est déjà sur le client.

## Contrôle en amont multicanal dans AuthZ {#preflight-multich-authz}

Cette mise en oeuvre de contrôle en amont est également compatible OLCA (Cablelabs).  La spécification Authentication and Authorization Interface 1.0 (sections 7.5.3 et 7.5.4) décrit les méthodes de demande d’informations d’autorisation auprès d’un MVPD à l’aide d’assertions SAML ou de XACML. Il s’agit de la méthode recommandée pour interroger l’état d’autorisation pour les MVPD qui ne prennent pas en charge cette fonctionnalité dans le cadre du flux d’authentification. L’authentification Adobe Pass émet un appel réseau unique au MVPD pour récupérer la liste des ressources autorisées.


L’authentification Adobe Pass reçoit la liste des ressources de l’application du programmeur. L’intégration MVPD de l’authentification Adobe Pass peut alors effectuer un appel AuthZ comprenant toutes ces ressources, puis analyser la réponse et extraire les multiples décisions d’autorisation/de refus.  Le flux du contrôle en amont avec le scénario AuthZ multicanal fonctionne comme suit :

1. L’application du programmeur envoie une liste de ressources séparée par des virgules via l’API cliente de contrôle en amont, par exemple : &quot;TestChannel1,TestChannel2,TestChannel3&quot;.
1. L’appel de requête MVPD preflight AuthZ contient plusieurs ressources et possède la structure suivante :

```XML
<?xml version="1.0" encoding="UTF-8"?><soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/"> 
<soap11:Header/> 
<soap11:Body> 
  <xacml-samlp:XACMLAuthzDecisionQuery xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol" 
                                       CombinePolicies="false" Destination="https://login.idpexmaple.net/" ID="_3576604f382455d6495f342d9e07b69c" 
                                       IssueInstant="2013-02-07T10:31:40.333Z" Version="2.0"> 
  <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com/on-behalf-of/TestDistributors</saml2:Issuer> 
  <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os"> 
  <xacml-context:Subject SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject"> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VFZTAQEAABQCe[...]</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Subject> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel2</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:type="xacml-context:AttributeValueType">TestChannel3</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Action> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VIEW</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Action> 
  <xacml-context:Environment> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address" 
                           DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">127.0.0.1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Environment> 
  </xacml-context:Request> 
  </xacml-samlp:XACMLAuthzDecisionQuery> 
</soap11:Body> 
</soap11:Envelope>
```

## Autorisation Personnalisée Pour Plusieurs Ressources {#custom-authz}

Certains MVPD disposent de points de terminaison d’autorisation qui prennent en charge l’autorisation de plusieurs ressources dans une même requête, mais ils ne tombent pas sous le scénario décrit dans AuthZ multicanal. Ces MVPD spécifiques nécessitent un travail personnalisé.

Adobe peut également prendre en charge l’autorisation de plusieurs canaux sans modifier la mise en oeuvre existante.  Cette approche doit être revue entre l’Adobe et l’équipe technique du MVPD pour s’assurer qu’elle fonctionne comme prévu.

## MVPD prenant en charge l’autorisation de contrôle en amont {#mvpds-supp-preflight-authz}

Le tableau suivant répertorie les MVPD qui prennent en charge l’autorisation de contrôle en amont, ainsi que le type de contrôle en amont qu’ils prennent en charge et les limites connues :

| Approche de contrôle en amont | MVPD | Remarques |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| AuthZ multicanal | Composer AT&amp;T Proxy Clearleap Charter_Direct Proxy GLDS Verizon OSN Bell Optimum AlticeOne |                                                                    |
| Liaison de canaux dans les métadonnées utilisateur | Suddenlink HTC | Toutes les intégrations directes de Synacor peuvent également prendre en charge cette approche. |
| Branchement et jointure | Tous les autres ne sont pas répertoriés ci-dessus | Le nombre maximum de ressources coché par défaut est de 5. |

<!--
![RelatedInformation]
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Authorization](/help/authentication/authz-usecase.md)
>* [MVPD Integration Features](/help/authentication/mvpd-integr-features.md)
>* [MVPD User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Preflight Authorization - Programmer Integration Guide](/help/authentication/preflight-authz.md)
>* [AuthN and AuthZ Interface 1.0 Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank} 
-->
