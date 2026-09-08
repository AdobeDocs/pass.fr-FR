---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 3.9.0
description: Notes De Mise À Jour De L’Authentification Adobe Pass 3.9.0
source-git-commit: 7ec140485418d07e16a181d43b651ea6de331477
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 3.9.0 {#authn-390-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-390}

* [Numéro de build](#build-number-390)
* [Présentation de la version](#release-overview-390)

### Numéro de build {#build-number-390}

Authentification Adobe Pass : adobe-pass-**3.9.0.1**\
Date de publication : **09/08/2026 - 09/10/2026**

### Présentation de la version {#release-overview-390}

Cette version se concentre sur les améliorations des mesures de l’API REST V2 et d’ESM.

#### Améliorations

* Amélioration de l’authentification unique du partenaire V2 de l’API REST pour s’assurer qu’une requête d’authentification valide est renvoyée pour les MVPD configurés avec OAuth2.
* Amélioration de la prise de décision de l’API REST V2 de renvoyer une réponse d’erreur claire en cas d’échec de l’autorisation, au lieu d’une réponse vide.
* Amélioration de la génération du code d’enregistrement pour éviter les caractères visuellement ambigus, ce qui facilite la lecture et la saisie correctes des codes.
* Améliorations du tableau de bord ESM avec prise en charge des mesures d’authentification de contrôle en amont.
