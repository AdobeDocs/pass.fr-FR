---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 3.5.0
description: Découvrez les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version.
source-git-commit: 6ff46a124f5f3c78173028ae3efed68d71ee6e41
workflow-type: tm+mt
source-wordcount: '198'
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
