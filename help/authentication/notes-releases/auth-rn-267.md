---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 2.67
description: Notes De Mise À Jour De L’Authentification Adobe Pass 2.67
exl-id: d899fe96-a273-4681-90a5-bde54cc2f3b3
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 2.67 {#authn-267-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-267}

* [Numéro de build](#build-number-267)
* [Présentation de la version](#release-overview-267)

### Numéro de build {#build-number-267}

Authentification Adobe Pass : adobe-pass-**2.67.0.1**

Date de publication : **09/12/2023 - 09/14/2023**

### Présentation de la version {#release-overview-267}

* Poursuite des mises à jour internes pour la nouvelle API REST.
* Poursuite des améliorations de l’architecture interne.

#### Mises à jour de MVPD

* Mises à jour de l’intégration de **DirecTV Puerto Rico** à Adobe. Veuillez contacter votre TAM pour plus de détails.

#### Correctifs

* Correction d’un problème de rupture de l’authentification unique (SSO), obtenu à l’aide de l’en-tête Adobe-Subject-Token, entre les applications utilisant l’API REST et FireTV SDK.
