---
title: Autorisation de contrôle en amont MVPD
description: Autorisation de contrôle en amont MVPD
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 0%

---

# Autorisation de contrôle en amont MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#mvpd-preflight-authz-intro}

L’« autorisation de contrôle en amont » est un contrôle d’autorisation léger pour plusieurs ressources. Les programmeurs l’utilisent principalement pour décorer leurs interfaces utilisateur (par exemple, en indiquant le statut d’accès avec des icônes de verrouillage et de déverrouillage).

L’authentification Adobe Pass peut actuellement prendre en charge l’autorisation de contrôle en amont de deux manières pour les fichiers MVPD, soit par le biais d’attributs de réponse AuthN, soit par le biais d’une requête AuthZ multicanal.  Les scénarios suivants décrivent le rapport coût/avantages des différentes manières de mettre en œuvre l’autorisation de contrôle en amont :

* **Scénario optimal** - Le MVPD fournit la liste des ressources préautorisées pendant la phase d’autorisation (authentification multicanal).
* **Scénario le plus pessimiste** - Si un MVPD ne prend en charge aucune forme d’autorisation de ressources multiples, le serveur d’authentification Adobe Pass effectue un appel d’autorisation au MVPD pour chaque ressource de la liste des ressources. Ce scénario a un impact (proportionnel au nombre de ressources) sur le temps de réponse de la demande d’autorisation de contrôle en amont. Cela peut augmenter la charge sur les serveurs Adobe et MVPD, ce qui entraîne des problèmes de performances. En outre, il génère des événements de demandes d’autorisation/réponses sans avoir besoin d’une lecture.
* **Obsolète** - Le MVPD fournit la liste des ressources préautorisées pendant la phase d’authentification, de sorte qu’aucun appel réseau n’est nécessaire, pas même la demande de contrôle en amont, puisque la liste est mise en cache sur le client.

Bien que les MVPD n’aient pas à prendre en charge l’autorisation de contrôle en amont, les sections suivantes décrivent certaines méthodes d’autorisation de contrôle en amont que l’authentification Adobe Pass peut prendre en charge, avant de revenir au scénario du pire ci-dessus.

## Contrôle en amont dans AuthN {#preflight-authn}

Ce scénario de contrôle en amont est compatible OLCA (câblages). La section 7.5.2 de la spécification de l’interface d’authentification et d’autorisation 1.0, intitulée « Instruction d’attribut dans l’assertion d’authentification », décrit comment une réponse d’authentification SAML peut contenir une liste de ressources préautorisées. Si un IdP le prend en charge, le serveur d’authentification d’Adobe Pass peut générer la liste des ressources en précontrôle au moment de l’authentification et la mettre en cache sur le client avec le jeton d’authentification. Cette méthode permet également d&#39;obtenir le meilleur scénario possible, et aucun appel réseau ne sera effectué lorsque le programmeur appelle checkPreauthorizedResources(), puisque tout est déjà sur le client.

### Liste des ressources personnalisées dans l’instruction d’attribut SAML {#custom-res-saml-attr}

La réponse d’authentification SAML de l’IdP doit inclure une instruction AttributeStatement contenant les noms de ressources qu’AdobePass doit autoriser.  Certains MVPD fournissent cette fonctionnalité au format suivant :

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

L&#39;exemple ci-dessus présente une liste contenant deux ressources préautorisées : « MMOD » et « Olympics2012 ».

Cela permet d&#39;obtenir le meilleur scénario possible, et aucun appel réseau ne sera effectué lorsque le programmeur appelle checkPreauthorizedResources(), puisque tout est déjà sur le client.

## Contrôle en amont multicanal dans AuthZ {#preflight-multich-authz}

Cette implémentation de contrôle en amont est également compatible OLCA (Cablelabs).  La spécification de l’interface d’authentification et d’autorisation 1.0 (sections 7.5.3 et 7.5.4) décrit les méthodes de demande d’informations d’autorisation à un MVPD à l’aide d’assertions SAML ou de XACML. Il s’agit de la méthode recommandée pour interroger le statut d’autorisation des fichiers MVPD qui ne prennent pas en charge cette opération dans le cadre du flux d’authentification. L’authentification Adobe Pass effectue un seul appel réseau au MVPD pour récupérer la liste des ressources autorisées.


L’authentification Adobe Pass reçoit la liste des ressources de l’application du programmeur. L’intégration MVPD de l’authentification Adobe Pass peut ensuite effectuer un appel AuthZ comprenant toutes ces ressources, puis analyser la réponse et extraire les multiples décisions d’autorisation/refus.  Le flux du scénario de contrôle en amont avec authentification multicanal fonctionne comme suit :

1. L’application du programmeur envoie une liste de ressources séparées par des virgules via l’API cliente de contrôle en amont, par exemple : « TestChannel1,TestChannel2,TestChannel3 ».
1. L’appel de requête AuthZ de contrôle en amont de MVPD contient les différentes ressources et possède la structure suivante :

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

Certaines MVPD disposent de points d’entrée d’autorisation qui prennent en charge l’autorisation de plusieurs ressources dans une seule requête, mais elles ne relèvent pas du scénario décrit dans Authentification multicanal. Ces fichiers MVPD spécifiques nécessitent un travail personnalisé.

Adobe peut également prendre en charge l’autorisation de plusieurs canaux sans modifier l’implémentation existante.  Cette approche doit être revue par l’équipe technique d’Adobe et de MVPD pour s’assurer qu’elle fonctionne comme prévu.

## MVPD qui prennent en charge l’autorisation de contrôle en amont {#mvpds-supp-preflight-authz}

Le tableau suivant répertorie les fichiers MVPD qui prennent en charge l’autorisation de contrôle en amont, ainsi que le type de contrôle en amont qu’ils prennent en charge et les limites connues :

| Approche Avant Vol | MVPD | Notes |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| Authentification multicanal | Comcast AT&amp;T Proxy Clearleap Charter_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| Alignement des canaux dans les métadonnées de l’utilisateur | Lien soudain HTC | Toutes les intégrations directes de Synacor peuvent également prendre en charge cette approche. |
| Branchement et jointure | Tous les autres non répertoriés ci-dessus | Nombre maximal de ressources vérifiées par défaut = 5. |

