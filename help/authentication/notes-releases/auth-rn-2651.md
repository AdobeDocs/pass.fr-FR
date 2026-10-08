---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 2.65.1
description: Notes De Mise À Jour De L’Authentification Adobe Pass 2.65.1
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%
---
# Notes De Mise À Jour De L’Authentification Adobe Pass 2.65.1 {#authn-2651-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-2651}

* [Numéro de build](#build-number-2651)
* [Présentation de la version](#release-overview-2651)

### Numéro de build {#build-number-2651}

Authentification Adobe Pass : adobe-pass-**2.65.1**

Date de publication : **06/20/2023 - 06/22/2023**

### Présentation de la version {#release-overview-2651}

Cette version ajoute les éléments suivants :

À partir de cette version, nous introduirons une assistance technique pour l’ajout de règles de limitation de débit à toutes nos API, afin de protéger l’écosystème global d’une utilisation incorrecte.

L’objectif initial sera de surveiller le comportement des applications afin d’établir les valeurs limites appropriées. Aucune limite ne sera appliquée dans le cadre de cette version. Dans les cas où le trafic généré par une application spécifique dépasse certaines limites, nous travaillons avec chaque client pour valider la mise en œuvre.

Les règles de limitation de débit seront appliquées plus tard dans l’année, une fois que nous aurons validé le trafic généré depuis toutes les applications du client.
