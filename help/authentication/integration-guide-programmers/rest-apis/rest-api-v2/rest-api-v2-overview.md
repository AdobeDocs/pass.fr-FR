---
title: Présentation de l’API REST V2
description: Présentation de l’API REST V2
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: a02ba4ca1b6579781e40ecd0d12dbfdd23ea7398
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# Présentation de l’API REST V2 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Voulez-vous améliorer la rentabilité de vos applications TVE ?

Souhaitez-vous réduire le temps et les ressources de développement nécessaires à la prise en charge des applications TVE sur plusieurs plateformes ?

Voulez-vous garantir une expérience utilisateur cohérente sur toutes les plateformes ?

Voulez-vous réduire les efforts de maintenance et simplifier la fourniture de mises à jour, de correctifs ou d’améliorations ?

## Présentation de l’API REST V2 {#rest-api-v2-introduction}

Adobe Pass Authentication est ravi d’annoncer le lancement de l’API REST V2, conçue pour améliorer l’expérience utilisateur et simplifier l’intégration avec les services Pass.

Nous sommes ravis des possibilités offertes par notre nouvelle API REST, qui représente une avancée majeure pour notre plateforme et ouvre la voie à de nouvelles fonctionnalités et à de nouveaux flux d’applications.

## Nouveautés {#rest-api-v2-whats-new}

Implémentation unique sur toutes les plateformes

Les applications des clients peuvent désormais utiliser la même implémentation sur toutes les plateformes, ce qui facilite le lancement de nouvelles fonctionnalités ou la maintenance d’applications en direct.

### SSO entre appareils {#rest-api-v2-cross-device-sso}

L’API REST V2 permet de transmettre en toute sécurité une session d’authentification entre différents appareils. En transmettant simplement la session entre les appareils, les utilisateurs peuvent s’authentifier sur leur appareil mobile et diffuser de la vidéo sur un appareil connecté à la télévision sans être réauthentifiés.

### Sessions d’authentification actives multiples {#rest-api-v2-multiple-active-authentication-sessions}

Différentes sessions MVPD actives sont désormais possibles, et les clients peuvent choisir de basculer entre l’intégration TempPass et l’intégration régulière de MVPD en cas de besoin.

### Mécanisme de sécurité renforcé {#rest-api-v2-enhanced-security-mechanism}

L’enregistrement client dynamique est le mécanisme de sécurité utilisé pour tous les flux et fonctionnalités. Il permet un contrôle plus sécurisé et granulaire des applications des clients et peut enregistrer des applications sur toutes les plateformes.

### Amélioration des performances pour des temps de réponse plus rapides {#rest-api-v2-improved-performance}

Les mécanismes de mise en cache améliorés permettent de réduire le trafic vers les MVPD, ce qui améliore les temps de réponse et réduit la latence. Dans l’ensemble, il y a un nombre réduit d’appels API jusqu’au démarrage de la vidéo.

### Codes d’erreur améliorés sur tous les flux {#rest-api-v2-enhanced-error-codes}

Les codes d’erreur avancés sont désormais disponibles sur tous les flux Pass au même format, avec des détails supplémentaires guidant les applications pour améliorer l’expérience globale de l’utilisateur.

### Amélioration du contrôle de toutes les sessions d’authentification {#rest-api-v2-improved-control}

La nouvelle API REST V2 permet d’agir sur plusieurs sessions authentifiées en même temps.

### Réduire les coûts de maintenance {#rest-api-v2-reduce-maintenance-costs}

Toutes les réponses et informations d’erreur sont désormais normalisées.

## Et après ? {#rest-api-v2-whats-next}

Tous les clients qui utilisent actuellement nos API par le biais de SDK ou d’appels REST peuvent continuer à le faire, car nous prévoyons de continuer à fournir une assistance jusqu’à la fin de 2025.

Cependant, tous les développements futurs seront créés sur l’API REST V2. Il est vivement recommandé de démarrer le processus de migration afin de bénéficier des dernières fonctionnalités d&#39;Adobe Pass.

## Vous souhaitez en savoir plus ? {#rest-api-v2-want-to-learn-more}

Pour commencer, consultez notre documentation publique :

- [Webinaire](#rest-api-v2-webinar)
- [Glossaire](rest-api-v2-glossary.md)
- [Checklist](rest-api-v2-checklist.md)
- [Questions fréquentes](rest-api-v2-faqs.md)
- [API](apis/rest-api-v2-apis-overview.md)
- [Flux](flows/rest-api-v2-flows-overview.md)
- Livres de cuisine
- Annexe
- [Configuration système minimale](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

Notre équipe d’assistance dédiée est également disponible pour vous aider à répondre à toutes les questions ou à toute assistance technique dont vous pourriez avoir besoin.

### Webinaire {#rest-api-v2-webinar}

Regardez le webinaire sur la nouvelle API REST V2, où nous vous avons fourni un aperçu des nouvelles fonctionnalités, ainsi qu’une démonstration en direct de l’API REST V2 en action.

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## Vous souhaitez essayer l’API REST V2 ? {#rest-api-v2-want-to-try}

Vous pouvez désormais explorer l’API REST V2 via notre page dédiée aux produits sur le site web [Adobe Developer](https://developer.adobe.com/adobe-pass/).
