---
title: Présentation de l’enregistrement client dynamique
description: Présentation de l’enregistrement client dynamique
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: 7ca9d8996756086a6b963c0b6d5b0bb64608ecbc
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 0%

---

# Présentation de l’enregistrement client dynamique {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

L’enregistrement client dynamique représente un mécanisme d’autorisation défini par la norme [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) et repose sur le cadre d’autorisation OAuth 2.0 décrit par la norme [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

Adobe Pass fournit un service d’enregistrement client dynamique qui permet d’accéder aux API protégées suivantes :

* API Adobe Pass Authentication Management :
   * [Réinitialiser l’API Temp Pass](/help/premium-workflow/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
   * [API de dégradation](/help/premium-workflow/degraded-access/degradation-feature.md#degradation-api-access)
   * [API Proxy MVPD](../../../integration-guide-mvpds/proxy-mvpd-webserv.md)
   * [API de surveillance du service de droit](/help/premium-workflow/esm/entitlement-service-monitoring-api.md)
* API REST d&#39;authentification Adobe Pass :
   * [API REST V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [API REST V1 (héritée)](../../legacy/rest-api-v1/rest-api-reference.md)
* SDK d’authentification Adobe Pass :
   * [SDK JavaScript (hérité)](../../legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
   * [SDK iOS/tvOS (hérité)](../../legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
   * [SDK Android (hérité)](../../legacy/sdks/android-sdk/android-sdk-api-reference.md)
   * [SDK FireOS (hérité)](../../legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> Le mécanisme d’autorisation de l’enregistrement dynamique des clients remplace les anciennes solutions d’authentification d’Adobe Pass qui risquent d’être abandonnées :
>
> * Mécanisme d’identification du demandeur signé.
> * Le mécanisme de liste de domaines.
> * Mécanisme de clé API.

Avec l&#39;adoption de l&#39;enregistrement dynamique des clients, les principaux avantages sont les suivants :

* Sécurité renforcée.
* Modèle unifié sur plusieurs plateformes.
* Contrôle affiné du cycle de vie de votre application.

Pour en savoir plus sur la gestion et l’utilisation de l’enregistrement client dynamique, reportez-vous aux sections suivantes.

## Dynamic Client Registration Management {#dynamic-client-registration-management}

Le processus de gestion de l’enregistrement dynamique des clients permet aux applications clientes s’exécutant sur des plateformes spécifiques et ayant besoin d’accéder à des API d’authentification Adobe Pass spécifiques pour s’enregistrer via le tableau de bord [Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

Le tableau de bord Adobe Pass TVE est un outil permettant aux clients du service d’authentification d’Adobe Pass (les programmeurs) de gérer leur configuration et leurs données. Ce tableau de bord en libre-service active un éventail de fonctionnalités qui sont décrites dans la documentation Guide de l’utilisateur du tableau de bord TVE d’Adobe Pass [](../../../user-guide-tve-dashboard/tve-dashboard-overview.md).

Si vous avez accès au tableau de bord [Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication), suivez les étapes des sections ci-dessous pour créer une application enregistrée et télécharger la déclaration du logiciel.

### Gestion des applications enregistrées {#manage-registered-applications}

>[!IMPORTANT]
>
> Si vous n’avez pas accès au tableau de bord Adobe Pass TVE, créez un ticket via notre [Zendesk](https://adobeprimetime.zendesk.com) et demandez à votre gestionnaire de compte technique (TAM) de créer une application enregistrée et de partager le relevé logiciel avec vous.

Vous pouvez créer une application enregistrée de deux manières :

* **Niveau du programmeur**

  Le processus d’enregistrement au niveau du programmeur vous permet de créer une application enregistrée liée à tous les canaux disponibles ou à un sous-ensemble sélectionné de canaux. Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation du tableau de bord TVE pour les programmeurs](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md) .


* **Niveau du canal**

  Le processus d’enregistrement au niveau du canal vous permet de créer une application enregistrée liée uniquement au canal sélectionné actuel. Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation du tableau de bord TVE pour les canaux](../../../user-guide-tve-dashboard/tve-dashboard-channels.md) .

>[!IMPORTANT]
>
> Il est recommandé de créer des applications enregistrées avec des autorisations plus spécifiques et limitées afin de renforcer la sécurité et d’empêcher tout accès non autorisé. Par conséquent, lors de la création d’applications enregistrées, pensez à utiliser des options plus étroites pour les `channels`, `platforms` et `scopes` attribués.
>
> Il est recommandé de créer une nouvelle application enregistrée pour chaque mise à jour majeure de votre application cliente afin de gérer son cycle de vie et son utilisation. Si nécessaire, créez un ticket via notre [Zendesk](https://adobeprimetime.zendesk.com) et demandez à votre gestionnaire de compte technique (TAM) de révoquer une application enregistrée afin de bloquer la fonctionnalité d’une version spécifique de l’application cliente.

### Gestion des instructions logicielles {#manage-software-statements}

>[!IMPORTANT]
>
> Si vous n’avez pas accès au tableau de bord Adobe Pass TVE, créez un ticket via notre [Zendesk](https://adobeprimetime.zendesk.com) et demandez à votre gestionnaire de compte technique (TAM) de créer une application enregistrée et de partager le relevé logiciel avec vous.

Avant de télécharger une déclaration de logiciel, assurez-vous qu&#39;une application enregistrée a été créée, comme décrit dans la section [Gérer les applications enregistrées](#manage-registered-applications) qui répond aux exigences de votre application cliente.

Il existe deux manières de télécharger une instruction logicielle en fonction du niveau de création de l&#39;application enregistrée :

* **Niveau du programmeur**

  Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation du tableau de bord TVE pour les programmeurs](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md) .

* **Niveau du canal**

  Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation du tableau de bord TVE pour les canaux](../../../user-guide-tve-dashboard/tve-dashboard-channels.md) .

L’instruction du logiciel est un jeton Web JSON (`JWT`) qui contient des informations sur votre logiciel d’application client sous forme de lot. Lorsqu’elle est présentée à l’API [Récupérer les informations d’identification du client](apis/dynamic-client-registration-apis-retrieve-client-credentials.md), l’instruction du logiciel est signée numériquement à l’aide de la signature web JSON (`JWS`).

Pour une explication plus détaillée des instructions de logiciel et de leur fonctionnement, reportez-vous à la documentation [RFC 7591](https://tools.ietf.org/html/rfc7591).

## Flux d’enregistrement client dynamique {#dynamic-client-registration-flow}

En résumé, le mécanisme d’autorisation de l’enregistrement client dynamique implique plusieurs étapes :

**Gestion**

* Un représentant du client doit créer une application enregistrée, comme décrit dans la section [ Gérer les applications enregistrées ](#manage-registered-applications).
* Un représentant client doit télécharger et incorporer une instruction logicielle comme décrit dans la section [ Gérer les instructions logicielles ](#manage-software-statements).

**Flux**

* L’application cliente doit obtenir les informations d’identification du client comme décrit dans la documentation de l’API [ Récupération des informations d’identification du client ](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) .
* L’application cliente doit obtenir le jeton d’accès comme décrit dans la documentation de l’API [ Récupérer le jeton d’accès ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) .

Reportez-vous à la documentation [Flux d’enregistrement client dynamique](flows/dynamic-client-registration-flow.md) pour mieux comprendre comment accéder aux API protégées Adobe Pass. De plus, vous pouvez également regarder cet enregistrement [webinaire](https://my.adobeconnect.com/pzkp8ujrigg1/) qui fournit plus de contexte et comprend une démonstration.
