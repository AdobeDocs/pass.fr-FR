---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 3.0.3
description: Notes De Mise À Jour De L’Authentification Adobe Pass 3.0.3
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 3.0.3 {#authn-303-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-303}

* [Numéro de build](#build-number-303)
* [Présentation de la version](#release-overview-303)

### Numéro de build {#build-number-303}

Authentification Adobe Pass : adobe-pass-**3.0.3**

Date de publication : **10/29/2024 - 10/31/2024**

### Présentation de la version {#release-overview-303}

#### API REST v2

##### Code

* Améliorations de l’API REST V2 (depuis la version majeure d’Adobe Pass 3.0 fournie [API REST V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* Ajout de champs `notBefore` et `notAfter` sur `/sessions/{code}` pour renvoyer des informations sur la validité du code d’authentification.
* Amélioration de l’identification des plateformes.

##### Documentation

* Pour commencer avec la nouvelle API REST v2, reportez-vous au document [Présentation de l’API REST v2](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) .

##### Outils

* Pour tester la nouvelle API REST v2, reportez-vous à la page nouvelle authentification Adobe Pass du site web [Adobe Developer](https://developer.adobe.com/adobe-pass).
