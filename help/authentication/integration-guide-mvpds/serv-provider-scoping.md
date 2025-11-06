---
title: Définition de la portée du fournisseur de services
description: Définition de la portée du fournisseur de services
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Définition de la portée du fournisseur de services {#service-provoider-scoping}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’implémentation par défaut d’une intégration de l’authentification Adobe Pass à MVPD repose sur la **spécification OLCA**. La section Exigences d’authentification de la spécification OLCA (6.5, identifiant du sujet) indique qu’il est possible d’indiquer la portée du fournisseur de services (SP) pour l’identifiant du sujet. (L’identifiant du sujet est l’identifiant utilisateur obscurci que le MVPD renvoie au SP.)  Dans une intégration de l’authentification Adobe Pass, il est nécessaire que les fichiers MVPD activent la définition de la portée des requêtes d’authentification du fournisseur de services.

Avec Adobe Pass Authentication prenant le rôle de fournisseur de service pour le programmeur, il est nécessaire d’implémenter une personnalisation qui permet la définition de la portée du fournisseur de service de la requête d’authentification.  Cela doit être fait afin que le MVPD puisse identifier la marque réseau transmise dans l’assertion SAML au fournisseur d’identité (IdP) MVPD.  La définition de la portée peut être implémentée de l’une des deux façons décrites dans la section suivante.

## Définition de la portée du fournisseur de services {#service-provider-scoping}

L’authentification Adobe Pass prend en charge les deux méthodes suivantes pour activer la portée SP des requêtes d’authentification :

* **Approche de l’émetteur SAML.** Dans cette approche, l’« ID du demandeur » est ajouté à la chaîne de l’émetteur SAML dans la requête d’authentification SAML.

* **Approche de propriété de définition de la portée personnalisée.** Dans cette approche, l’« ID du demandeur » est explicitement inclus en tant que propriété de « définition de la portée » personnalisée dans la demande d’authentification SAML.

>[!NOTE]
>
>L’« ID du demandeur » est la manière dont l’authentification Adobe Pass fait référence à la marque de réseau du programmeur (par exemple : « CNN » est l’une des marques du réseau Turner).

### Approche SAML de l’émetteur {#saml-issuer-approach}

Cette approche utilise l’élément `<Issuer>` SAML dans la requête d’authentification SAML, comme illustré dans ce fragment de code :

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### Approche de propriété de définition de la portée personnalisée {#custom-scoping-property-approach}

Cette approche utilise une propriété personnalisée appelée « Définition de la portée », comme illustré dans ce fragment de code de requête d’authentification SAML :

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
