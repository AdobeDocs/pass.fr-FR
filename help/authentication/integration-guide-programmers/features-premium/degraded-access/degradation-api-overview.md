---
title: Présentation de l’API de dégradation
description: Présentation de l’API de dégradation
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# Présentation de l’API de dégradation {#degradation-api-overview}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Avant d’utiliser l’API de dégradation, assurez-vous que les conditions préalables suivantes sont remplies :
>
> * Récupérez les informations d’identification du client comme décrit dans la documentation de l’API [Récupérez les informations d’identification du client](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenez le jeton d’accès comme décrit dans la documentation de l’API [Récupérer le jeton d’accès](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Pour plus d’informations sur la création d’une application enregistrée et le téléchargement de l’instruction logicielle, consultez la documentation [Aperçu de l’enregistrement du client dynamique](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) .

## Informations générales {#general_info}

>[!NOTE]
>
>Cette API n’est pas disponible en général. Contactez votre représentant Adobe pour connaître les mises à jour de disponibilité.

Cette fonctionnalité fournit les trois parties principales d’une intégration (programmeurs, MVPD et Adobe) avec la possibilité de contourner temporairement des points de terminaison d’authentification et d’autorisation MVPD spécifiques. En règle générale, c’est le programmeur qui lance une telle action, mais quelle que soit la personne qui déclenche un événement de dégradation, l’action dépend d’accords conclus antérieurement avec les distributeurs de programmes multivariés concernés.

Le cas d’utilisation principal de cette fonctionnalité se produit lors d’événements sportifs ou de grande envergure. Dans de tels scénarios de trafic élevé, il est possible que la charge sur un point de terminaison MVPD spécifique devienne trop élevée, ce qui entraîne des temps de réponse très longs pour les utilisateurs. Afin de préserver une bonne expérience utilisateur dans un tel scénario, le programmeur peut décider de déclencher une règle de dégradation qui peut temporairement s’authentifier/autoriser automatiquement les utilisateurs, ou désactiver un MVPD en le supprimant de la liste des MVPD disponibles.

Une règle de dégradation n’est appliquée que pour une période fixe. Bien que les principaux clients de cette fonctionnalité soient les canaux de sports et d’actualités en direct, tout programmeur peut souhaiter avoir accès à cette fonctionnalité, car les services des distributeurs multicanaux de programmes audiovisuels disparaissent de temps en temps.

Notes de mise à niveau :

- Cette fonctionnalité est conçue pour être utilisée avec l’API de surveillance de l’utilisation, qui fournit des informations en temps réel sur le nombre d’authentifications et d’autorisations par MVPD, la latence d’autorisation moyenne et d’autres mesures nécessaires pour une présentation complète du service.
- Cette fonctionnalité ne permet pas de contourner le service d’authentification Adobe Pass. Si l’authentification Adobe Pass est en panne, aucun mécanisme au sein du service ne peut être utilisé pour permettre aux utilisateurs de voir le contenu. Les sites ou les applications peuvent, cependant, contourner l’authentification Adobe Pass par eux-mêmes.
- Actuellement, l’Adobe ne déclenche pas directement la dégradation. La décision doit toujours résider avec un programmeur spécifique qui a accepté de telles conditions avec les MVPD. À l’avenir, l’authentification Adobe Pass pourrait être proactive dans le déclenchement des règles de dégradation si des accords (protection SLA) peuvent être conclus avec des distributeurs multicanaux de programmes audiovisuels.

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
