---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 2.64
description: Notes De Mise À Jour De L’Authentification Adobe Pass 2.64
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 2.64 {#authn-264-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-264}

* [Numéro de build](#build-number-264)
* [Présentation de la version](#release-overview-264)

### Numéro de build {#build-number-264}

Authentification Adobe Pass : adobe-pass-**2.64**

Date de publication : **11/08/2022 - 11/10/2022**

### Présentation de la version {#release-overview-264}

* Mises à jour de l’infrastructure, visant à réduire les temps de réponse du serveur et à améliorer les performances globales du système.
* Améliorations apportées au nouveau mécanisme d’identification des plateformes.
* Possibilité de bloquer les réponses d&#39;authentification non sollicitées des MVPD où le paramètre « in_response_to » est absent de l&#39;assertion SAML.

#### Correctifs

* Correction d’un formatage incorrect de certains jetons TempPass hérités.
* Correction de problèmes mineurs sur le flux d’authentification du deuxième écran.
