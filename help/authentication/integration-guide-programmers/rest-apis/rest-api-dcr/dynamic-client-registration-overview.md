---
title: Présentation de l’enregistrement du client dynamique
description: Présentation de l’enregistrement du client dynamique
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# Présentation de l’enregistrement du client dynamique {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

L’enregistrement dynamique du client représente un mécanisme d’autorisation défini par le [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591), et il est basé sur le framework d’autorisation OAuth 2.0 décrit par le [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

Adobe Pass fournit un service d’enregistrement client dynamique qui permet d’accéder aux API protégées suivantes :

* API de gestion de l’authentification Adobe Pass :
   * [Réinitialiser l’API Temp Pass](../../features-premium/temporary-access/reset-temp-pass.md)
   * [API de dégradation](../../features-premium/degraded-access/degradation-api-overview.md)
   * [API MVPD de proxy](../../../integration-guide-mvpds/proxy-mvpd-webserv.md)
   * [API de surveillance du service de droits](../../features-premium/esm/entitlement-service-monitoring-api.md)
* API REST d’authentification Adobe Pass :
   * [API REST V1](../../legacy/rest-api-v1/rest-api-reference.md)
   * [API REST V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
* SDK d’authentification Adobe Pass :
   * [SDK JAVASCRIPT](../../legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
   * [SDK iOS/tvOS](../../legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
   * [SDK ANDROID](../../legacy/sdks/android-sdk/android-sdk-api-reference.md)
   * [SDK FireOS](../../legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> Le mécanisme d’autorisation d’enregistrement du client dynamique remplace les anciennes solutions d’authentification Adobe Pass qui peuvent être abandonnées :
>
> * Mécanisme d’ID de demandeur signé.
> * Mécanisme de liste de domaines.
> * Mécanisme de clé API.

Avec l’adoption de l’enregistrement dynamique des clients, les principaux avantages sont les suivants :

* Sécurité renforcée.
* Modèle unifié sur toutes les plateformes.
* Contrôle précis du cycle de vie de votre application.

Pour en savoir plus sur la gestion et l’utilisation de l’enregistrement dynamique de client, reportez-vous aux sections suivantes.

## Gestion de l’enregistrement du client dynamique {#dynamic-client-registration-management}

Le processus de gestion dynamique des enregistrements des clients permet aux applications clientes s’exécutant sur des plateformes spécifiques et ayant besoin d’un accès à des API d’authentification Adobe Pass spécifiques de s’enregistrer via le [tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

Le tableau de bord Adobe Pass TVE est un outil permettant aux clients d’authentification Adobe Pass (programmeurs) de gérer leur configuration et leurs données. Ce tableau de bord en libre-service permet de nombreuses fonctionnalités décrites dans le [Guide de l’utilisateur du tableau de bord Adobe Pass TVE](../../../user-guide-tve-dashboard/tve-dashboard-overview.md).

Si vous avez accès au [tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication), suivez les étapes des sections ci-dessous pour créer une application enregistrée et télécharger l’instruction logicielle.

### Gestion des applications enregistrées {#manage-registered-applications}

>[!IMPORTANT]
>
> Si vous n’avez pas accès au tableau de bord Adobe Pass TVE, créez un ticket à l’aide de notre [Zendesk](https://adobeprimetime.zendesk.com) et demandez à votre gestionnaire de compte technique (TAM) de créer une application enregistrée et de partager avec vous le relevé logiciel.

Vous pouvez créer une application enregistrée de deux manières différentes :

* **Niveau du programmeur**

  Le processus d&#39;enregistrement au niveau du programmeur permet de créer une application enregistrée liée à tous les canaux disponibles ou à un sous-ensemble sélectionné de canaux. Pour plus d’informations, consultez la documentation [Guide de l’utilisateur du tableau de bord TVE pour les programmeurs](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md) .


* **Niveau de canal**

  Le processus d’enregistrement au niveau du canal permet de créer une application enregistrée liée uniquement au canal sélectionné actuel. Pour plus d’informations, reportez-vous au [Guide de l’utilisateur du tableau de bord TVE pour les canaux](../../../user-guide-tve-dashboard/tve-dashboard-channels.md) .

>[!IMPORTANT]
>
> Il est recommandé de créer des applications enregistrées avec des autorisations plus spécifiques et limitées afin d’améliorer la sécurité et de prévenir les accès non autorisés. Par conséquent, lors de la création d’applications enregistrées, envisagez d’utiliser des options plus étroites pour les `channels`, `platforms` et `scopes` affectés.
>
> Il est recommandé de créer une nouvelle application enregistrée pour chaque mise à jour majeure de votre application cliente afin de gérer son cycle de vie et son utilisation. Si nécessaire, créez un ticket via notre [Zendesk](https://adobeprimetime.zendesk.com) et demandez à votre gestionnaire de compte technique (TAM) de révoquer une application enregistrée afin de bloquer les fonctionnalités d’une version spécifique de l’application cliente.

### Gestion des instructions logicielles {#manage-software-statements}

>[!IMPORTANT]
>
> Si vous n’avez pas accès au tableau de bord Adobe Pass TVE, créez un ticket à l’aide de notre [Zendesk](https://adobeprimetime.zendesk.com) et demandez à votre gestionnaire de compte technique (TAM) de créer une application enregistrée et de partager avec vous le relevé logiciel.

Avant de télécharger une instruction logicielle, assurez-vous qu’une application enregistrée est créée comme décrit dans la section [Gérer les applications enregistrées](#manage-registered-applications) qui répond aux exigences de votre application cliente.

Vous pouvez télécharger un relevé logiciel de deux manières différentes en fonction du niveau de création de l’application enregistrée :

* **Niveau du programmeur**

  Pour plus d’informations, consultez la documentation [Guide de l’utilisateur du tableau de bord TVE pour les programmeurs](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md) .

* **Niveau de canal**

  Pour plus d’informations, reportez-vous au [Guide de l’utilisateur du tableau de bord TVE pour les canaux](../../../user-guide-tve-dashboard/tve-dashboard-channels.md) .

L’instruction logicielle est un jeton Web JSON (`JWT`) qui contient des informations sur votre logiciel d’application client sous la forme d’un lot. Lorsqu’elle est présentée à l’API [Récupérer les informations d’identification du client](apis/dynamic-client-registration-apis-retrieve-client-credentials.md), l’instruction logicielle est signée numériquement à l’aide de la signature web JSON (`JWS`).

Pour une explication plus détaillée sur les instructions logicielles et leur fonctionnement, consultez la documentation [RFC 7591](https://tools.ietf.org/html/rfc7591) .

## Flux d’enregistrement de client dynamique  {#dynamic-client-registration-flow}

En résumé, le mécanisme d’autorisation d’enregistrement dynamique du client comprend plusieurs étapes :

**Gestion**

* Un représentant du client doit créer une application enregistrée comme décrit dans la section [Gérer les applications enregistrées](#manage-registered-applications) .
* Un représentant du client doit télécharger et incorporer une instruction logicielle comme décrit dans la section [Gérer les instructions de logiciel](#manage-software-statements) .

**Flux**

* L’application cliente doit obtenir les informations d’identification du client, comme décrit dans la documentation de l’API [Récupérer les informations d’identification du client](apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
* L’application cliente doit obtenir le jeton d’accès comme décrit dans la documentation de l’API [Récupérer le jeton d’accès](apis/dynamic-client-registration-apis-retrieve-access-token.md).

Reportez-vous à la documentation [Flux d’enregistrement de client dynamique](flows/dynamic-client-registration-flow.md) pour mieux comprendre comment accéder aux API protégées par Adobe Pass. De plus, vous pouvez également regarder cet enregistrement [webinar](https://my.adobeconnect.com/pzkp8ujrigg1/), qui fournit plus de contexte et inclut une démonstration.
