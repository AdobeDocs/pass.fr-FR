---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 3.7.0
description: Notes De Mise À Jour De L’Authentification Adobe Pass 3.7.0
hold: true
source-git-commit: 170d49b06e4ac8b31a840ee1bc5fac114bb3aa0b
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 3.7.0 {#authn-370-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-370}

* [Numéro de build](#build-number-370)
* [Présentation de la version](#release-overview-370)

### Numéro de build {#build-number-370}

Authentification Adobe Pass : adobe-pass-**3.7.0.2**\
Date de publication : **05/12/2026 - 05/14/2026**

### Présentation de la version {#release-overview-370}

Cette version se concentre sur les mises à niveau de l’intégration de MVPD, les correctifs et les améliorations du tableau de bord TVE.

#### Intégrations MVPD

* Ajout de la prise en charge de Bell MVPD à l’aide d’OAuth2 avec PKCE.

#### Améliorations

* Publication du tableau de bord TVE version 1.5.1.

#### Correctifs

* Correction d’un problème en raison duquel l’authentification unique Apple ignorait certaines incohérences de configuration de MVPD.
* Correction d’un problème qui provoquait des erreurs HTTP 500 lors du refus d’autorisation en raison de caractères non valides dans l’en-tête de réponse.
