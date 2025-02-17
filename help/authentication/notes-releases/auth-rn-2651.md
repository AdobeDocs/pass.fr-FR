---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 2.65.1
description: Notes De Mise À Jour De L’Authentification Adobe Pass 2.65.1
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 2.65.1 {#authn-2651-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

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
