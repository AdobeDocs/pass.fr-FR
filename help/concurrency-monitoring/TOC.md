---
product: adobe primetime
feature: Concurrency Monitoring
audience: end-user
user-guide-title: Surveillance de la simultanéité dans Adobe Pass
user-guide-description: Découvrez comment définir et appliquer des limites à l’utilisation simultanée dans plusieurs applications.
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 5%

---


# Aide sur la surveillance simultanée d’Adobe Pass {#cm}

- [Présentation de CM](cm-home.md) {#cm-intro}
- Prise en main {#getting-started}
   - [Prise en main de la surveillance de la simultanéité](getting-started/getting-started-overview.md)
   - [Concepts clés](getting-started/key-concepts.md)
- Guide d’intégration {#integration-guide}
   - [Glossaire](cm-glossary.md)
   - Référence d’API {#api-reference}
      - [Aperçu](api/api-reference-overview.md)
      - [Points d’entrée de l’API](api/api-endpoints.md)
      - [Référence d’API](api/cm-api-overview.md)
   - [API d’utilisation de la surveillance de simultanéité](reports/cmu-api.md)
   - [Accès à l’API](reports/cmu-api-access.md)
   - [Exemples de rapports d’utilisation de la surveillance de simultanéité](reports/cm-usage-reports-examples.md)
- Cas d’utilisation et implémentation {#use-cases-implementation}
   - [Présentation des cas d’utilisation](use-cases/cm-use-cases.md)
   - [Stratégies LIFO vs FIFO](use-cases/lifo-fifo-strategies.md)
   - [Gestion des conflits de session](use-cases/handling-409-errors.md)
   - [Modèles de mise en œuvre](technical/implementation-models.md)
   - [Politique De Client Unique - Applications Multiples](technical/single-tenant-policy-mult-app.md)
   - [Limiter L’Utilisation Simultanée De Plusieurs Applications](technical/restrict-concurr-usage-mult-apps.md)
- [&#x200B; Rapports d’utilisation de CM &#x200B;](reports/cm-usage-reports.md) {#cm-usage-reports}
- Notes techniques {#tech-notes}
   - [Attributs de métadonnées standard](technical/standard-metadata-attributes.md)
   - [Métadonnées personnalisées](technical/custom-metadata.md)
   - [Point de décision de politique](technical/cm-policy-decision-point.md)
   - [Point d&#39;information sur la politique](technical/policy-info-pt-versionone.md)
   - [Limitation](technical/throttling-mechanism.md)
   - [Comment : faire la distinction entre VOD et le contenu dynamique dans la surveillance simultanée](technical/vod-live-dist.md)
   - [Politique de conservation des données](technical/data-retention-policy.md)
- Versions {#cm-release-notes}
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.6.2](releases/rn-cm-services-362.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.6.1](releases/rn-cm-services-361.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.6.0](releases/rn-cm-services-360.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.5.1](releases/rn-cm-services-351.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.5.0](releases/rn-cm-services-350.md)
   - [Surveillance de la simultanéité - Notes de mise à jour 3.4.4](releases/rn-cm-services-344.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.4.3](releases/rn-cm-services-343.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.4.2](releases/rn-cm-services-342.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.4.0](releases/rn-cm-services-340.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.3.7](releases/rn-cm-services-337.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.3.6](releases/rn-cm-services-336.md)
   - [Surveillance de la simultanéité - Notes de mise à jour 3.3.2](releases/rn-cm-services-332.md)
   - [Surveillance de la simultanéité - Notes de mise à jour 3.3.1](releases/rn-cm-services-331.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.3.0](releases/rn-cm-services-330.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.2.3](releases/rn-cm-services-323.md)
   - [Surveillance de la simultanéité - Notes de mise à jour 3.2.2](releases/rn-cm-services-322.md)
   - [Surveillance de la simultanéité - Notes de mise à jour 3.2.1](releases/rn-cm-services-321.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.2](releases/rn-cm-services-320.md)
   - [Surveillance de la simultanéité - Notes de mise à jour 3.1](releases/rn-cm-services-31.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 3.0](releases/rn-cm-services-30.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 2.9.6](releases/rn-cm-296.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 2.9](releases/rn-cm-29.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 2.8.2](releases/rn-cm-282.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 2.7.2](releases/rn-cm-272.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 2.6.0](releases/rn-cm-260.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 2.5.0](releases/rn-cm-250.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 2.3.2](releases/rn-cm-232.md)
   - [Surveillance de l’accès simultané - Notes de mise à jour 2.2.2](releases/rn-cm-222.md)
- [Procédures d’assistance](support/cm-escalation-procedures.md) {#support-procedures}
