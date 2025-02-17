---
title: Notes De Mise À Jour De JavaScript 4.7.0 D’Authentification Adobe Pass
description: Notes De Mise À Jour De JavaScript 4.7.0 D’Authentification Adobe Pass
exl-id: 07f90270-e64a-4c6b-a072-183af0f53352
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---

# Notes De Mise À Jour De JavaScript 4.7.0 D’Authentification Adobe Pass {#javascript-sdk-470-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Numéro de build {#build-number-470}

Authentification Adobe Pass : JavaScript 4.7.0

Date de publication : **02/27/2024 - 02/29/2024**

## Présentation de la version {#release-overview-470}

* Suppression de la version 2.0.1 d’Access Enabler JavaScript SDK en raison de failles de sécurité.
  <br/><br/>
Les URL suivantes ne sont plus prises en charge et renverront un code d’état HTTP 410 :
   * https://entitlement.auth.adobe.com/entitlement/AccessEnabler.js
   * http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js
   * https://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.js
   * http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.js

## Package de version {#release-package-470}

L’URL de production est : https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

L’URL d’évaluation est : https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
