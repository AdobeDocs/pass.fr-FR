---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 3.2.0
description: Notes De Mise À Jour De L’Authentification Adobe Pass 3.2.0
exl-id: 43aee317-dbac-4000-893e-839ee3e9f6ba
source-git-commit: fcdf50b2caad20deef15fceeb3e23f4195c0078d
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 3.2.0 {#authn-320-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-320}

* [Numéro de build](#build-number-320)
* [Présentation de la version](#release-overview-320)

### Numéro de build {#build-number-320}

Authentification Adobe Pass : adobe-pass-**3.2.0**

Date de publication : **06/10/2025 - 06/12/2025**

### Présentation de la version {#release-overview-320}

#### API REST v2

* Une nouvelle `missing_parameters_fallback` de raison a été ajoutée pour les cas où des paramètres sont manquants dans la réponse [API Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).
* Un nouveau champ « device » a été ajouté à la réponse [API Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### Nouvelles fonctionnalités

* Développez l’API de dégradation pour ajouter la possibilité d’appliquer des règles de dégradation à plusieurs MVPD en un seul appel

#### Correctifs

* Correction d’un problème qui empêchait la réussite du flux d’authentification dans les cas où le traitement des métadonnées de l’utilisateur échouait.
* Correction d’un problème en raison duquel le motif d’autorisation n’était pas correctement calculé.
* Correction d’un problème en raison duquel l’ID d’utilisateur en amont n’était pas présent dans la réponse du profil.
* Correction d’un problème en raison duquel la demande d’authentification n’incluait pas d’informations de définition pour les demandes d’authentification initiées par l’API REST V2.
