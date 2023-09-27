---
title: Notes de mise à jour d’Adobe Pass Concurrency Monitoring 2.3.2
description: Notes de mise à jour d’Adobe Pass Concurrency Monitoring 2.3.2
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# Notes de mise à jour d’Adobe Pass Concurrency Monitoring 2.3.2 {#cm-232}

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

Date de publication : 12/11/2015

## Nouvelles fonctionnalités et améliorations {#new-features}

* Nouvelles ventilations disponibles dans les rapports d’utilisation. Les nouvelles ventilations sont disponibles si l’application intégrée à la surveillance de simultanéité envoie des métadonnées personnalisées.
   * application : identifiant d’application signalé dans l’URL d’appel.
   * mvpd : MVPD signalé dans l’URL d’appel.
   * channel : canal de métadonnées personnalisé
   * platform - application de métadonnées personnalisée
* Nouvelles mesures liées à **durée du flux** disponibles dans les rapports d’utilisation. Les nouvelles mesures peuvent être utilisées pour créer un histogramme de la durée du flux. Les intervalles suivants, en minutes, sont actuellement disponibles :
   * duration_0-15
   * duration_15-30
   * duration_30-60
   * duration_60-120
   * duration_over-120

## Correctifs {#bug-fixes}

N/A

## Problèmes connus {#known-issues}

N/A
