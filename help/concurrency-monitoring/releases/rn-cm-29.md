---
title: Notes de mise à jour de la surveillance simultanée 2.9 d’Adobe
description: Notes de mise à jour de la surveillance simultanée 2.9 d’Adobe
exl-id: fd793b1f-b704-492b-850c-dae6478b575a
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 1%

---

# Notes De Mise À Jour De La Surveillance D’Accès Simultané 2.9 {#rn-cm29}

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version.

## Date de publication {#release-date}

03/14/2019


## Présentation de la version {#release-overview}

* À partir de cette version, nous avons introduit un nouveau rapport pour comprendre l’utilisation simultanée : un histogramme pour le niveau de simultanéité, qui affiche :

* le nombre d’utilisateurs ayant atteint chaque niveau de simultanéité (c’est-à-dire le nombre d’utilisateurs ayant déjà eu 2 flux simultanés, 3 flux simultanés, etc.) au cours de chaque intervalle de granularité
* durée totale pour chaque niveau d&#39;accès simultané, en minutes (la valeur moyenne peut être calculée en divisant simplement cette valeur par le nombre ci-dessus)
* nombre total de fois où les utilisateurs et utilisatrices ont rencontré chaque niveau de simultanéité, afin d’estimer l’impact d’une règle donnée en termes d’utilisateurs et d’utilisatrices affectés et d’expérience utilisateur agrégée
Pour plus d’informations, consultez la page [Rapports d’utilisation](/help/concurrency-monitoring/reports/cm-usage-reports.md).

Nous avons également amélioré la protection contre les injections SQL et ajouté plusieurs correctifs.

## Problèmes connus {#known-issues}

Aucune.
