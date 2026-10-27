---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 3.10.0
description: Notes De Mise À Jour De L’Authentification Adobe Pass 3.10.0
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: d60b5256f5809b52122d70d04eb873018707fcaf
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%
---
# Notes De Mise À Jour De L’Authentification Adobe Pass 3.10.0 {#authn-3100-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-3100}

* [Numéro de build](#build-number-3100)
* [Présentation de la version](#release-overview-3100)

### Numéro de build {#build-number-3100}

Authentification Adobe Pass : adobe-pass-**3.10.0**\
Date de publication : **10/27/2026 - 10/29/2026**

### Présentation de la version {#release-overview-3100}

Cette version se concentre sur les contrôles d’authentification unique, les améliorations des métadonnées de l’utilisateur et les améliorations de la fiabilité pour la gestion de la configuration de l’API REST V2 et de MVPD.

#### Améliorations

* Ajout d’un contrôle au niveau de l’intégration pour l’authentification unique (SSO) du jeton de service. Le jeton de service SSO est utilisé uniquement lorsqu’il est explicitement activé pour l’intégration.
* Ajout de la prise en charge des métadonnées utilisateur personnalisées (par exemple, `packageType`) pendant l’authentification, l’autorisation et le contrôle en amont. Les décisions d’autorisation et de contrôle en amont de l’API REST V2 incluent désormais les métadonnées utilisateur configurées, y compris les réponses mises en cache, tout en respectant les paramètres d’accès et de chiffrement des métadonnées.

#### Correctifs

* Correction d’un problème en raison duquel les requêtes de profil SSO d’Apple V2 de l’API REST échouaient avec une erreur de réponse SAML non valide pour les intégrations utilisant des MVPD OAuth2 via SAML.
* Correction d’un problème en raison duquel les décisions de contrôle en amont et d’autorisation pour la même ressource pouvaient remplacer les délais d’expiration et les métadonnées utilisateur mis en cache les uns des autres.
* Correction d’un problème en raison duquel la suppression de MVPD proxy laissait des paramètres de plateforme non référencés qui empêchaient les enregistrements de configuration suivants.
