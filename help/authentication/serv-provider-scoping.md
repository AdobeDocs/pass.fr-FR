---
title: Portée du fournisseur de services
description: Portée du fournisseur de services
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Portée du fournisseur de services {#service-provoider-scoping}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’implémentation par défaut d’une intégration d’authentification Adobe Pass avec un MVPD est basée sur la **spécification OLCA**. La section Exigences d’authentification de la spécification OLCA (6.5, identifiant de sujet) indique qu’il est possible d’indiquer la portée du fournisseur de service (SP) pour l’identifiant de sujet. (L’identifiant du sujet est l’identifiant utilisateur obscurci que le MVPD renvoie au SP.)  Dans une intégration d’authentification Adobe Pass, il est nécessaire que les MVPD activent la portée des demandes d’authentification SP.

Avec l’authentification Adobe Pass prenant le rôle de SP pour le programmeur, il est nécessaire de mettre en oeuvre une personnalisation qui active la définition de portée du SP de la demande d’authentification.  Cela doit être fait afin que le MVPD puisse identifier la marque réseau transmise dans l’assertion SAML au fournisseur d’identité (IdP) du MVPD.  La définition de la portée peut être implémentée de l’une des deux manières décrites dans la section suivante.

## Portée du fournisseur de services {#service-provider-scoping}

L’authentification Adobe Pass prend en charge les deux méthodes suivantes pour activer l’application de plage SP des demandes d’authentification :

* **Approche de l’émetteur SAML.** Dans cette approche, &quot;Requestor ID&quot; est ajouté à la chaîne de l’émetteur SAML dans la demande d’authentification SAML.

* **Méthode De Propriété De Portée Personnalisée.** Dans cette approche, l’&quot;ID du demandeur&quot; est inclus explicitement en tant que propriété &quot;Scoping&quot; personnalisée dans la demande d’authentification SAML.

>[!NOTE]
>
>&quot;Identifiant du demandeur&quot; est la manière dont l’authentification Adobe Pass fait référence à la marque réseau du programmeur (par exemple : &quot;CNN&quot; est l’une des marques du réseau Turner).

### Approche de l’émetteur SAML {#saml-issuer-approach}

Cette approche utilise l’élément SAML `<Issuer>` dans la requête d’authentification SAML, comme illustré dans ce fragment de code :

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### Approche de propriété de plage personnalisée {#custom-scoping-property-approach}

Cette approche utilise une propriété personnalisée nommée &quot;Scoping&quot;, comme illustré dans ce fragment de code d’une requête d’authentification SAML :

```xml
...
<samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">requestorID</samlp:RequesterID>
</samlp:Scoping>
...
```

<!--
>[!RELATEDINFORMATION]
>* [MVPD Authentication](/help/authentication/authn-usecase.md)
>* **OLCA Specification**
-->
