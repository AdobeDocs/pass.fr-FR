---
title: Rapports d'utilisation de la surveillance de simultanéité
description: Rapports d'utilisation de la surveillance de simultanéité
exl-id: 20220436-e748-4b22-8e7c-e074e0bfe242
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 0%

---

# Rapports d&#39;utilisation de la surveillance de simultanéité {#cm-usage-reports}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.



## Vue d’ensemble {#usage-rep-overview}

Le service **Rapports d’utilisation de la surveillance de simultanéité** est disponible par le biais d’une API REST qui fournit insight dans les rapports d’utilisation simultanée signalés par les applications du client.

## Conditions préalables {#usage-rep-prerequisites}

Pour accéder au produit Rapports d’utilisation de la surveillance d’accès simultané, un client doit d’abord contacter l’équipe d’assistance [Surveillance d’accès simultané](mailto:tve-support@adobe.com) et elle effectuera les étapes nécessaires pour vous permettre d’accéder au produit de l’API . Plus d’informations sur l’[accès à l’API CMU](/help/concurrency-monitoring/reports/cmu-api-access.md).

## Mesures et répartitions générales des rapports {#general-rep-metrics-breakdown}

### Les utilisateurs des rapports d’utilisation peuvent surveiller les mesures suivantes :{#monitor-metrics}

| Nom | Description |
|:---|:---|
| active-users | nombre de comptes utilisateur distincts ayant démarré au moins une session de streaming pendant l’intervalle, avec répartition par granularité temporelle |
| active-sessions | nombre de sessions en flux continu ayant signalé une activité pendant l’intervalle (sans compter les tentatives ayant échoué et les sessions ayant reçu une réponse de refus pour l’appel d’initialisation) |
| started-sessions | nombre de sessions en flux continu qui ont été lancées dans l’intervalle |
| completed-sessions | nombre de sessions de streaming qui ont été terminées pendant l’intervalle (explicitement ou en raison d’une temporisation) |
| failed-attempts | nombre de tentatives d’initialisation de session ayant reçu une réponse de refus |
| sessions-ignorées | nombre de sessions de streaming terminées pendant la lecture (en pulsation) en raison d’une violation de la politique. Cette mesure n’a de sens que lorsqu’une politique FIFO ou last_one_wins est adoptée. |
| sessions interrompues | nombre de sessions de streaming explicitement interrompues lors du démarrage d’une nouvelle session (via l’en-tête X-Terminate) |
| duration_0-15 | nombre de flux d’une durée comprise entre 0 et 15 minutes |
| duration_15-30 | nombre de flux d’une durée comprise entre 15 et 30 minutes |
| duration_30-60 | nombre de flux d’une durée comprise entre 30 et 60 minutes |
| duration_60-120 | nombre de flux d’une durée comprise entre 60 et 120 minutes |
| duration_2h-4h | nombre de flux d’une durée comprise entre 2 heures (120 minutes) et 4 heures |
| duration_4h-8h | nombre de flux d’une durée comprise entre 4 heures et 8 heures |
| duration_8h-16h | nombre de flux d’une durée comprise entre 8 heures et 16 heures |
| duration_16h-1d | nombre de flux d’une durée comprise entre 16 heures et 1 jour (24 heures) |
| duration_1d-3d | nombre de flux avec une durée comprise entre 1 jour et 3 jours |
| duration_3d-7d | nombre de flux d’une durée comprise entre 3 jours et 7 jours |
| duration_1w-1m | nombre de flux avec une durée comprise entre 1 semaine (7 jours) et 1 mois (30 jours) |
| duration_over-1m | nombre de flux avec une durée supérieure à 1 mois (30 jours) |

### Les utilisateurs des rapports d’utilisation peuvent filtrer les mesures répertoriées ci-dessus en fonction des dimensions suivantes : {#dimensions-2-filter-metrics}

| Nom du Dimension | Description |
|:---------------|:------------------------------------------------------------------------------------------------------------------|
| année | L’année à 4 chiffres |
| mois | Le mois de l&#39;année (1-12) |
| jour | Le jour du mois (1-31) |
| heure | Heure de la journée |
| minute | La minute de l’heure[^1] |
| application | Nom de l’application enregistré dans la surveillance de simultanéité utilisé pour gérer les sessions |
| application-id | ID d’application enregistré dans la surveillance de simultanéité utilisé pour gérer les sessions |
| canal | Métadonnées du canal envoyées lors de l’initialisation de la session (marquées comme Inconnues si aucune métadonnée n’est envoyée) |
| mvpd | MVPD fourni lors de la gestion de session |
| plate-forme | Métadonnées de plateforme fournies lors de l’initialisation de la session ou prédéfinies pour une application au niveau de la configuration |

## Mesures et répartitions des rapports sur la simultanéité {#concurrency-reports-metrics-breakdown}

À partir de la version 2.9.0 de la surveillance de l’accès simultané, nous avons introduit un nouveau rapport pour comprendre l’utilisation simultanée : un histogramme pour **niveau d’accès simultané** et **niveau d’activité**.

L’objectif principal de ce rapport est de vous aider à comprendre l’impact de la définition d’une politique avec une certaine limite de simultanéité et de vous donner suffisamment d’informations pour décider si vous devez augmenter la limite.

### Les utilisateurs des rapports d’utilisation peuvent surveiller les mesures suivantes : {#metrics-usage-rep-users}

| Nom du Dimension | Description |
|:---|:---|
| utilisateurs | nombre d&#39;utilisateurs ayant atteint chaque niveau d&#39;accès simultané/d&#39;activité |

### Les utilisateurs des rapports d’utilisation peuvent filtrer les mesures répertoriées ci-dessus en fonction des dimensions suivantes : {#dimensions-to-filter-metrics}

| Nom du Dimension | Description |
|:---|:---|
| année | L’année à 4 chiffres |
| mois | Le mois de l&#39;année (1-12) |
| jour | Le jour du mois (1-31) |
| concurrency-level | Représente toute activité **flux) distincte qui a été approuvée lors de la phase d’initialisation de la session** pour un utilisateur afin de pouvoir observer le nombre de flux simultanés **ouverts** par un utilisateur et de comprendre l’impact de l’application d’une certaine limite de simultanéité |
| activity-level | Représente toute activité distincte **flux, quel que soit son état : démarrée, active, arrêtée, rejetée)** pour un utilisateur, afin de pouvoir observer le nombre de flux simultanés qu’un utilisateur a tenté d’ouvrir et de comprendre l’impact de l’application d’une certaine limite de simultanéité |
| mvpd | MVPD fourni lors de la gestion de session |

### Exemples de rapports

Pour une précision optimale des données, nous vous recommandons les rapports présentés sur cette page [exemples de rapports CMU](/help/concurrency-monitoring/reports/cm-usage-reports-examples.md)

[^1] : les rapports Miniature ne sont pas disponibles par défaut. Veuillez contacter l’équipe de surveillance [assistance](mailto:tve-support@adobe.com) pour en faire la demande.