---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 3.8.0
description: Notes De Mise À Jour De L’Authentification Adobe Pass 3.8.0
hold: true
source-git-commit: ce9e8de3d69699d03cf68c86be1bb811967501dc
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 3.8.0 {#authn-380-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-380}

* [Numéro de build](#build-number-380)
* [Présentation de la version](#release-overview-380)

### Numéro de build {#build-number-380}

Authentification Adobe Pass : adobe-pass-**3.8.0**\
Date de publication : **08/11/2026 - 08/13/2026**

### Présentation de la version {#release-overview-380}

Cette version se concentre sur la stabilité, les améliorations et les mises à jour de sécurité dans les services d’authentification Adobe Pass.

#### Correctifs

* Correction d’un problème qui provoquait des erreurs HTTP 500 sur les API V2 en raison de certains caractères non valides dans l’ID d’appareil.

#### Améliorations

* Amélioration de la gestion des jetons d’actualisation pour la prise en charge du renouvellement continu des jetons.
* Amélioration de la reconnaissance de l’identifiant visiteur sur les appareils secondaires pour Analytics.
* Validation améliorée des paramètres d’URL pour renforcer les contrôles de sécurité et améliorer l’intégrité globale du système.
* Tableau de bord TVE version 1.5.2 avec des améliorations mineures de l’interface utilisateur.
