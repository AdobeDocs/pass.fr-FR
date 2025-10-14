---
title: Profils
description: Profils
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Profils {#profiles}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Les profils sont créés par l’authentification Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) lorsqu’un utilisateur s’authentifie avec son fournisseur de télévision payante (MVPD).

Le type de profil varie en fonction de la méthode d’authentification utilisée :

* **Standard**

  Créé par le biais de l’authentification de base.

* **SSO Apple**

  Création par authentification unique (SSO) à l’aide du framework de compte d’abonné vidéo Apple.

* **SSO Platform**

  Créé via l’authentification unique (SSO) à l’aide d’une identité de plateforme.

* **Jeton de service SSO**

  Créé par authentification unique (SSO) à l’aide d’un jeton de service.

Les profils stockent des données clés qui permettent aux applications clientes de :

* Déterminez le statut d’authentification de l’utilisateur.
* Identifiez la méthode d’authentification utilisée.
* Identifiez le fournisseur d’identité.
* Accédez à [métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

Les profils sont stockés en toute sécurité dans le serveur principal de l’authentification Adobe Pass et sont liés à l’application, à l’appareil et à l’identifiant du fournisseur de services qui les demandent. Ils restent valides pour une durée limitée, comme défini par la [&#x200B; Durée de vie (TTL) de l’authentification &#x200B;](#authentication-ttl-management).

## Gestion de la durée de vie (TTL) de l’authentification {#authentication-ttl-management}

La durée de vie (TTL) de l’authentification définit la durée pendant laquelle un utilisateur reste authentifié avant d’avoir besoin de s’authentifier à nouveau. Ce délai est limité et doit être convenu avec les représentants de MVPD. Les valeurs de durée de vie peuvent varier en fonction des éléments suivants :

* Catégorie de plateforme (par exemple, ordinateurs de bureau, appareils mobiles, appareils connectés à la télévision)
* Plateforme spécifique (par exemple, iOS, Android, tvOS, Roku, FireTV)

La durée de vie de l’authentification (authN) peut être consultée et modifiée via le tableau de bord Adobe Pass [TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre organisation ou par un représentant de l’authentification Adobe Pass agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

## API REST V2 {#rest-api-v2}

Les profils peuvent être récupérés à l’aide des API suivantes :

* [Récupération des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Récupération du profil pour un fichier mvpd spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Récupération du profil pour un code spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Reportez-vous aux sections **Réponse** et **Exemples** des API ci-dessus pour comprendre la structure des profils.

Pour plus d’informations sur comment et à quel moment intégrer les API ci-dessus, reportez-vous aux documents suivants :

* [Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [FAQ sur la phase d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
