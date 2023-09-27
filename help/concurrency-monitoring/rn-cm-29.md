---
title: Notes de mise à jour de la surveillance de la simultanéité des Adobes 2.9
description: Notes de mise à jour de la surveillance de la simultanéité des Adobes 2.9
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# Notes de mise à jour de la surveillance de la simultanéité 2.9 {#rn-cm29}

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version.

## Date de publication {#release-date}

03/14/2019


## Présentation des versions {#release-overview}

* À partir de cette version, nous avons introduit un nouveau rapport pour comprendre l’utilisation simultanée : un histogramme pour le niveau simultané, présentant :

* le nombre d’utilisateurs qui ont atteint chaque niveau de simultanéité (c.-à-d. le nombre d’utilisateurs qui ont déjà eu 2 flux simultanés, 3 flux simultanés, etc.) au cours de chaque intervalle de granularité
* durée totale pour chaque niveau de simultanéité, en minutes (la valeur moyenne peut être calculée en divisant simplement cette valeur par le nombre ci-dessus)
* Nombre total de fois où les utilisateurs ont rencontré chaque niveau de simultanéité, afin d’estimer l’impact d’une règle donnée en termes d’utilisateurs affectés et d’expérience utilisateur agrégée. Vous trouverez plus de détails sur la variable [Rapports d’utilisation](/help/concurrency-monitoring/cm-usage-reports.md) page.

Nous avons également amélioré la protection par injection SQL et ajouté plusieurs correctifs.

## Problèmes connus {#known-issues}

Aucun.
