---
title: Points d’entrée de l’API
description: Liste complète des API de surveillance de simultanéité
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 3%
---
# Points d’entrée de l’API

## Gestion des sessions principales

| Point d’entrée | Méthode | Description |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | POSTER | Créer une session de streaming |
| `/sessions/{idp}/{subject}/{session}` | POSTER | Envoyer une pulsation pour maintenir la session active |
| `/sessions/{idp}/{subject}/{session}` | DELETE | Terminer une session |
| `/runningStreams/{idp}/{subject}` | GET | Obtenir toutes les sessions actives pour un sujet |

## Gestion des métadonnées

| Point d’entrée | Méthode | Description |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | Obtention des champs de métadonnées obligatoires pour l’application |
