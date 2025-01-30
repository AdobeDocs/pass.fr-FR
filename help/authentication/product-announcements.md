---
title: Annonces de produit
description: Annonces de produit
exl-id: 3c9c66e1-d31d-4af3-8ab2-eb32492f42ca
source-git-commit: 49a6a75944549dbfb062b1be8a053e6c99c90dc9
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 2%

---

# Annonces de produit {#product-announcements}

## Fin de vie {#eol}

Cette section décrit les dates de fin de prise en charge et de fin de vie de certaines fonctionnalités et de certains produits Adobe Pass Authentication dont la désactivation est prévue.

Assurez-vous de rester informé des délais de déclassement et de prendre les mesures indiquées ci-dessous.

| Annonce | Date d&#39;annonce | Date de fin de prise en charge | Date de fin de vie | Description |
|-----------------------------------------------------------------|-------------------|---------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fin de vie de JavaScript SDK v3.5 d’Adobe Pass Authentication AccessEnabler | 12/11/2024 | S/O | 01/08/2025 | Nous prévoyons la fin de vie d’Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 le **8 janvier 2025**. Passée cette date, les fonctionnalités et services fournis par AccessEnabler JavaScript SDK v3.5 ne fonctionneront plus, y compris l&#39;authentification et l&#39;autorisation des utilisateurs. |
| Fin de vie du mécanisme de sécurité non DCR de l&#39;authentification Adobe Pass | 12/11/2024 | S/O | 01/20/2025 | Nous prévoyons la fin de vie du mécanisme de sécurité non DCR de l’authentification Adobe Pass le **20 janvier 2025**. Ce mécanisme a été utilisé pour sécuriser l’accès aux API d’authentification Adobe Pass suivantes :<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md">Réinitialiser l’API Temp Pass</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md"> API de dégradation </a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md">API Proxy MVPD</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md">API de surveillance du service de droit</a></li></ul>Passée cette date, les fonctionnalités et services fournis par les API ci-dessus accessibles à l’aide de ce mécanisme de sécurité ne fonctionneront plus.</br></br>Pour assurer la continuité du service, vous devez migrer toutes vos applications pour utiliser le mécanisme [Enregistrement dynamique des clients](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md). |
| Fin de vie de l’API REST d’authentification Adobe Pass v1 | 12/11/2024 | 12/31/2025 | Fin 2026 (à déterminer) | Nous prévoyons d’interrompre la prise en charge de l’API REST d’authentification Adobe Pass v1 le **31 décembre 2025**. Passée cette date, les mises à jour et les correctifs ne seront plus fournis.</br></br>Pour assurer une prise en charge continue, vous devez migrer toutes vos applications vers <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API REST v2</a>.</br></br>Nous prévoyons la fin de vie de l’API REST d’authentification Adobe Pass v1 d’ici **fin 2026**. Passée cette date, les fonctionnalités et services fournis par l’API REST v1 ne fonctionneront plus, y compris l’authentification et l’autorisation des utilisateurs.</br></br>Pour assurer la continuité du service, vous devez migrer toutes vos applications vers <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API REST v2</a>. |
| Fin de vie des SDK AccessEnabler d’authentification Adobe Pass | 12/11/2024 | 05/31/2026 | Fin 2026 (à déterminer) | Nous prévoyons d’interrompre la prise en charge des SDK AccessEnabler d’authentification Adobe Pass le **31 mai 2026**. Passée cette date, les mises à jour et les correctifs ne seront plus fournis.</br></br>Pour assurer une prise en charge continue, vous devez migrer toutes vos applications vers <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API REST v2</a>.</br></br>Nous prévoyons la fin de vie des SDK AccessEnabler d’Adobe Pass Authentication d’ici **fin 2026**. Passée cette date, les fonctionnalités et services fournis par les SDK AccessEnabler ne fonctionneront plus, y compris l&#39;authentification et l&#39;autorisation des utilisateurs.</br></br>Pour assurer la continuité du service, vous devez migrer toutes vos applications vers <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API REST v2</a>. |

## Versions du produit {#product-releases}

Cette section compile les références à l’historique des versions et aux notes de mise à jour correspondantes pour l’authentification Adobe Pass.

### 2024 {#product-releases-2024}

| Notes de mise à jour | Dates |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notes de mise à jour de l’authentification Adobe Pass version 3.0.3](notes-releases/auth-rn-303.md) | 10/29/2024 - 10/31/2024 |
| [Notes de mise à jour de l’authentification Adobe Pass 3.0](notes-releases/auth-rn-300.md) | 09/10/2024 - 09/12/2024 |
| [Notes de mise à jour de l’authentification Adobe Pass 2.70](notes-releases/auth-rn-270.md) | 04/23/2024 - 04/25/2024 |
| [Notes de mise à jour de l’authentification Adobe Pass 2.69](notes-releases/auth-rn-269.md) | 02/27/2024 - 02/29/2024 |
| [Notes de mise à jour de la version 4.7.0 de JavaScript SDK Authentication Adobe Pass](notes-releases/authn-rn-javascript-470.md) | 02/27/2024 - 02/29/2024 |
| [Notes de mise à jour de l’authentification Adobe Pass iOS / tvOS SDK 3.9.2](notes-releases/authn-rn-ios-tvos-392.md) | 03/26/2024 |
| [Notes de mise à jour de l’authentification Adobe Pass iOS / tvOS SDK 3.8.4](notes-releases/authn-rn-ios-tvos-384.md) | 01/26/2024 |

### 2023 {#product-releases-2023}

| Notes de mise à jour | Dates |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| [Notes de mise à jour de l’authentification Adobe Pass 2.68](notes-releases/auth-rn-268.md) | 12/05/2023 - 12/07/2023 |
| [Notes de mise à jour de l’authentification Adobe Pass 2.67](notes-releases/auth-rn-267.md) | 09/12/2023 - 09/14/2023 |
| [Notes de mise à jour de l’authentification Adobe Pass 2.66](notes-releases/auth-rn-266.md) | 07/11/2023 - 07/13/2023 |
| [Notes de mise à jour de l’authentification Adobe Pass 2.65.1](notes-releases/auth-rn-2651.md) | 06/20/2023 - 06/22/2023 |
| [Notes de mise à jour de l’authentification Adobe Pass 2.65](notes-releases/auth-rn-265.md) | 25/04/2023 - 27/04/2023 |
| [Notes de mise à jour de l’authentification Adobe Pass 2.64.1](notes-releases/auth-rn-2641.md) | 01/31/2023 - 02/02/2023 |
| [Notes de mise à jour de l’authentification Adobe Pass iOS / tvOS SDK 3.8.3](notes-releases/authn-rn-ios-tvos-383.md) | 11/16/2023 |
| [Notes de mise à jour de l’authentification Adobe Pass iOS / tvOS SDK 3.8.2](notes-releases/authn-rn-ios-tvos-382.md) | 02/10/2023 |
| [Notes de mise à jour de l’authentification Adobe Pass iOS / tvOS SDK 3.8.1](notes-releases/authn-rn-ios-tvos-381.md) | 26/05/2023 |
| [Notes de mise à jour de l’authentification Adobe Pass Android SDK 3.7.3](notes-releases/authn-rn-android-373.md) | 09/19/2023 |

### 2022 {#product-releases-2022}

| Notes de mise à jour | Dates |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notes de mise à jour de l’authentification Adobe Pass 2.64](notes-releases/auth-rn-264.md) | 11/08/2022 - 11/10/2022 |
| [Notes de mise à jour de l’authentification Adobe Pass 2.63](notes-releases/auth-rn-263.md) | 09/20/2022 - 09/22/2022 |
| [Notes de mise à jour de l’authentification Adobe Pass 2.62.1](notes-releases/auth-rn-2621.md) | 08/02/2022 - 08/04/2022 |
| [Notes de mise à jour de la version 4.6.0 de JavaScript SDK Authentication Adobe Pass](notes-releases/authn-rn-javascript-460.md) | 09/20/2022 - 09/22/2022 |

### 2021 {#product-releases-2021}

| Notes de mise à jour | Dates |
|-----------------------------------------------------------------------------------------------------------|------------|
| [Notes de mise à jour de la version 4.4.0 de JavaScript SDK Authentication Adobe Pass](notes-releases/authn-rn-javascript-440.md) | 06/22/2021 |
| [Notes de mise à jour de l’authentification Adobe Pass iOS / tvOS SDK 3.7.0](notes-releases/authn-rn-ios-tvos-370.md) | 09/03/2021 |
