---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 3.5.0
description: Découvrez les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version.
exl-id: b196f636-26a5-4974-903e-40b5f8b93a24
source-git-commit: 1cbddf081fc7d57a187c9701e4ade8593baf8759
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 3.5.0

Dernière mise à jour : Mar Dec 09 2025 00:00:00 GMT+0000 (Temps Universel Coordonné)

* Rubriques :
* Authentification

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](https://experienceleague.adobe.com/fr/docs/pass/authentication/product-announcements).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-350}

* [Numéro de build](#build-number-350)
* [Présentation de la version](#release-overview-350)

### Numéro de build {#build-number-350}

Authentification Adobe Pass : adobe-pass-**3.5.0**\
Date de publication : **12/09/2025 - 12/11/2025**

### Présentation de la version {#release-overview-350}

#### API REST v2

* Ajout d’une nouvelle dimension dans ESM (« api ») pour aider les clients à créer des rapports qui distinguent les événements en fonction du type d’implémentation : SDK, REST V1 ou REST V2.

#### Correctifs

* Correction d’un problème où les messages de dégradation personnalisés définis dans le tableau de bord TVE n’apparaissaient pas dans les détails d’erreur de l’API REST V2.
* Correction d’un problème dans l’API REST V2 en raison duquel `authenticated_profile_expired` code d’erreur n’était pas renvoyé lorsque les profils authentifiés expiraient.
* Correction d’un problème en raison duquel les calculs de latence d’autorisation et les valeurs de TTL de contrôle en amont étaient incorrects dans l’API REST V2.
* Correction d’un problème en raison duquel un format de réponse incohérent était renvoyé lorsque le jeton DCR arrivait à expiration.

## Mise À Jour De Maintenance - Février 2026 {#maintenance-update-february-2026}

Authentification Adobe Pass : adobe-pass-**3.5.0.5**\
Date de publication : **02/24/2026 - 02/26/2026**

Cette mise à jour de maintenance comprend des améliorations importantes pour améliorer la fiabilité et la sécurité du système :

### Améliorations

* Amélioration de la gestion de la dégradation de l’authentification pour les configurations MVPD par proxy dans l’API REST V2, assurant un comportement plus cohérent pendant les interruptions de service de MVPD.
* Amélioration de la validation des paramètres d’URL et de la gestion des redirections pour renforcer les contrôles de sécurité et améliorer l’intégrité globale du système.
