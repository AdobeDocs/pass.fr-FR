---
title: Notes de mise à jour de l’authentification Adobe Pass 3.0.3
description: Notes de mise à jour de l’authentification Adobe Pass 3.0.3
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Notes de mise à jour de l’authentification Adobe Pass 3.0.3 {#pt-authn-303-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et Web {#server-side-web-clients-303}

* [Numéro de build](#build-number-303)
* [Présentation des versions](#release-overview-303)

### Numéro de build {#build-number-2651}

Authentification Adobe Pass : adobe-pass-**3.0.3**
Date de publication : **10/29/2024 - 10/31/2024**

### Nouvelles fonctionnalités {#new-features-303}

#### API REST v2 {#rest-apis}

##### Code

* Améliorations de l’API REST V2 (depuis la version majeure d’Adobe Pass 3.0 fournie avec l’[API REST V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* Ajout des champs `notBefore` et `notAfter` sur `/sessions/{code}` pour renvoyer des informations sur la validité du code d’authentification.
* Amélioration de l’identification des plateformes.

##### Documentation

* Pour commencer avec la nouvelle API REST v2, reportez-vous au document [Présentation de l’API REST v2](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) .

##### Outils

* Pour essayer la nouvelle API REST v2, reportez-vous à la nouvelle page d’authentification Adobe Pass du site web [Adobe Developer](https://developer.adobe.com/adobe-pass).
