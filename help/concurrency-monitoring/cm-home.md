---
title: Présentation de la surveillance de la simultanéité
description: Présentation de la surveillance de la simultanéité
exl-id: 725cc64b-6b03-46e3-a038-41e9b1341c6b
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Présentation de la surveillance de la simultanéité {#intro}

La surveillance de simultanéité est un service qui permet aux fournisseurs de contenu et aux fournisseurs d’identité (MVPD et programmeurs) de définir et d’appliquer des limites sur la diffusion vidéo en continu simultanée sur plusieurs applications, appareils et plateformes. Que vous soyez un programmeur cherchant à contrôler le nombre de flux qu’un abonné peut regarder simultanément ou un MVPD voulant appliquer des politiques d’utilisation à l’ensemble de vos partenaires de contenu, la surveillance de simultanéité fournit les outils dont vous avez besoin.

## Qu&#39;est-ce que la surveillance simultanée ? {#what-is-cm}

La surveillance simultanée est un service centralisé qui suit et gère les sessions actives de streaming vidéo en temps réel. Il permet d’effectuer les opérations suivantes :

- **Limiter les flux simultanés par abonné** - Contrôlez le nombre de flux vidéo simultanés auxquels un utilisateur peut accéder
- **Application des restrictions d’appareil** - Limite la diffusion en continu à des types ou quantités d’appareil spécifiques
- **Implémentation de politiques basées sur l’emplacement** - Limitation de la diffusion en continu en fonction de l’emplacement géographique
- **Créer des règles spécifiques au contenu** - Appliquez des limites différentes pour le contenu en direct et pour le contenu VOD.
- **Surveiller les schémas d’utilisation** - Obtenez des informations sur la manière dont votre contenu est utilisé

## Fonctionnement {#how-it-works}

La surveillance de l’accès simultané fonctionne via une API simple mais puissante :

1. **Initialisation de session** - Lorsqu’un utilisateur ou une utilisatrice commence à regarder du contenu, votre application crée une session
2. **Évaluation des politiques** - Le service évalue vos politiques définies par rapport à l’utilisation actuelle
3. **Surveillance en temps réel** - Les appels de pulsation maintiennent les sessions actives et surveillent la conformité

## Vous découvrez la surveillance de simultanéité ? {#new-to-cm}

Commencez avec notre [Guide de prise en main](getting-started/getting-started-overview.md) pour comprendre les principes de base et configurer votre première intégration.

