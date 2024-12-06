---
title: Notes de mise à jour d’Adobe Pass Authentication 2.65.1
description: Notes de mise à jour d’Adobe Pass Authentication 2.65.1
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# Notes de mise à jour d’Adobe Pass Authentication 2.65.1 {#authn-2651-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et Web {#server-side-web-clients-2651}

* [Numéro de build](#build-number-2651)
* [Présentation des versions](#release-overview-2651)

### Numéro de build {#build-number-2651}

Authentification Adobe Pass : adobe-pass-**2.65.1**
Date de publication : **06/20/2023 - 06/22/2023**

### Présentation des versions {#release-overview-2651}

Cette version ajoute les éléments suivants :

À compter de cette version, nous allons introduire une assistance technique pour l’ajout de règles de limitation de débit à toutes nos API, afin de protéger l’écosystème global d’une utilisation incorrecte.

L’objectif initial est de surveiller le comportement des applications afin d’établir les valeurs limites appropriées et aucune limite ne sera appliquée dans le cadre de cette version. Dans les cas où le trafic généré par une application spécifique dépasse certaines limites, nous collaborerons avec chaque client pour valider l’implémentation.

Les règles de limitation de taux seront appliquées plus tard cette année, une fois le trafic généré à partir de toutes les applications du client validées.
