---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 3.4.0
description: Notes De Mise À Jour De L’Authentification Adobe Pass 3.4.0
exl-id: ad572617-f607-419d-a085-70c025465080
source-git-commit: c9958a17ad9dfb518bab1d24087c85fdcb6fd057
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 3.4.0 {#authn-340-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-340}

* [Numéro de build](#build-number-340)
* [Présentation de la version](#release-overview-340)

### Numéro de build {#build-number-340}

Authentification Adobe Pass : adobe-pass-**3.4.0**
Date de publication : **09/16/2025 - 09/18/2025**

### Présentation de la version {#release-overview-340}

#### API REST v2

* Ajout de la prise en charge de l’[Experience Cloud ID (ECID)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md) pour améliorer les fonctionnalités d’identification et de suivi des utilisateurs.
* En-têtes AP-Device-Identifier et X-Device-Info modifiés en facultatifs pour l’API REST V2 [API de configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### Correctifs

* Correction d’un problème en raison duquel l’ID MVPD et l’ID MVPD du proxy étaient inversés dans le jeton de la réponse V2 de l’API REST [Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md).
* Correction d’un problème en raison duquel l’ID de demandeur n’était pas présent dans le jeton de la réponse V2 de l’API REST [Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md).
* Correction d’un problème en raison duquel la transmission d’un trop grand nombre de ressources à l’API REST V2 [Preauthorize Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) entraînait une liste de décisions vide dans la réponse au lieu du [Code d’erreur amélioré](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **too_many_resources**.

#### Divers

* Correctifs de vulnérabilités de sécurité.
