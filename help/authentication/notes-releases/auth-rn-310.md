---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 3.1.0
description: Notes De Mise À Jour De L’Authentification Adobe Pass 3.1.0
source-git-commit: 4ad5ea619f64a78a72f69228c9ae3c83a7b66f24
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 3.1.0 {#authn-310-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-310}

* [Numéro de build](#build-number-310)
* [Présentation de la version](#release-overview-310)

### Numéro de build {#build-number-310}

Authentification Adobe Pass : adobe-pass-**3.1.0**

Date de publication : **02/25/2025 - 02/27/2025**

### Présentation de la version {#release-overview-310}

#### API REST v2

* Nouveau nom de l’action `partner_logout` et nouveau type d’action `partner_interactive` pour différencier la déconnexion standard de la déconnexion unique du partenaire dans la réponse API REST v2 [API Logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md).
* Nouveau champ `reason` pour fournir plus d’informations sur le nom de l’action dans les réponses API REST v2 [API sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) et [API SSO sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md).

#### Correctifs

* Correction d’un problème qui empêchait les abonnés au spectre de s’authentifier via l’API REST v2 [Authentifier l’API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).
* Correction d’un problème qui empêchait l’agrégation correcte des événements générés via l’API REST V2 [Authenticate API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) dans ESM.
* Correction d’un problème qui entraînait un mauvais calcul de l’horodatage `notBefore` pour les profils utilisateur dans la réponse de l’API REST v2 [API Profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md).

#### JavaScript SDK

* Suppression de la version 3.5.0 de [AccessEnabler JavaScript SDK](authn-rn-javascript-471.md).