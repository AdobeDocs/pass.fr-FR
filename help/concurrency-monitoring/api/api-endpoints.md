---
title: Points d’entrée de l’API
description: Liste complète des API de surveillance de simultanéité
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
source-git-commit: 39384d753e7808fa433f30d8dafabd531dbf3acf
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
