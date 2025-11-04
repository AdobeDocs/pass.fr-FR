---
title: Fonctionnalité de dégradation
description: Fonctionnalité de dégradation
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Fonctionnalité de dégradation {#degradation-feature}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Dans le monde dynamique des sports en direct et des grands événements, il est essentiel d&#39;assurer une expérience de visionnage fluide. Le trafic élevé au cours de ces événements peut submerger les points d’entrée d’authentification et d’autorisation du Distributeur de programmation vidéo multicanaux (MVPD), entraînant des retards ou des perturbations.

L’authentification Adobe Pass résout ces problèmes grâce à sa **fonctionnalité de dégradation**, une solution qui permet de contourner temporairement des points d’entrée d’authentification et d’autorisation MVPD spécifiques. Cette fonctionnalité est particulièrement utile lors des événements de trafic de pointe, où les temps de réponse peuvent se dégrader en raison d’une charge importante sur les systèmes MVPD.

La **fonction de dégradation** peut être une protection vitale pour les programmeurs, assurant la continuité du service. Bien que son audience principale comprenne des chaînes de sport et d’actualités en direct, son utilité s’étend à tout programmeur cherchant à atténuer le risque de perturbations causées par les points d’entrée MVPD.

>[!IMPORTANT]
>
> L’API de dégradation est une fonctionnalité premium qui nécessite une licence actuelle d’Adobe.

En appliquant une règle de dégradation, les programmeurs peuvent temporairement activer l&#39;authentification automatique ou l&#39;autorisation automatique, assurant ainsi un accès ininterrompu au contenu pendant la période d&#39;application de la dégradation. Les actions de dégradation sont toujours initiées par un programmeur sur la base d&#39;accords préétablis avec les MVPD. Bien qu’Adobe ne déclenche pas directement la dégradation, les fonctionnalités futures peuvent inclure une gestion proactive si des accords de niveau de service (SLA) sont établis.

Conçue pour être utilisée avec une [API de surveillance de l’utilisation](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md) et basée sur des accords préalables avec les MVPD, l’authentification Adobe Pass offre un outil puissant pour équilibrer l’expérience utilisateur, la fiabilité et le contrôle opérationnel lors de moments critiques.

>[!IMPORTANT]
>
> Cette fonctionnalité ne permet pas de contourner le service d’authentification Adobe Pass lui-même. Si l’authentification Adobe Pass n’est pas disponible, il n’existe aucun mécanisme intégré dans ce service pour faciliter l’accès des utilisateurs. Dans ce cas, les sites ou les applications peuvent choisir de mettre en œuvre leurs propres solutions de routage alternatives pour gérer la diffusion de contenu.

## Accès à l’API de dégradation {#degradation-api-access}

Avant d’accéder à l’[API de dégradation](#degradation-api), vous devez suivre les étapes requises dans le processus d’enregistrement client dynamique (DCR). Ce processus obligatoire garantit que vous disposez du jeton d’accès nécessaire pour interagir avec l’API de dégradation.

Pour obtenir des instructions complètes, reportez-vous à la documentation [&#x200B; Présentation de l’enregistrement client dynamique &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## API de dégradation {#degradation-api}

L’API de dégradation est une API RESTful qui permet aux programmeurs de gérer les règles de dégradation pour des MVPD spécifiques. L’API permet d’activer, de supprimer et de récupérer le statut des règles de dégradation actives.

Pour en savoir plus sur l’API de dégradation, consultez le document Zendesk [Authentification Adobe Pass | API de dégradation v3](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3) et recherchez le fichier PDF à télécharger.

## API REST V2 {#rest-api-v2}

L’utilisation de la fonction de dégradation nécessite l’implémentation de mises à jour du code pour modifier la manière dont votre application TV Everywhere (TVE) interagit avec l’authentification Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

Pour obtenir un guide complet sur ces mises à jour et les workflows associés, reportez-vous à la documentation [Flux d’accès dégradés](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).
