---
title: Modèles d’implémentation
description: Modèles d’implémentation
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%
---
# Modèles d’implémentation {#imp-models}

## Stratégies côté serveur {#ss-policies}

Ce modèle utilisera CM comme point de décision de politique, déléguant ainsi la décision d’accès au service.

Étant donné que le client ne doit pas faire d’hypothèses concernant les politiques appliquées, l’implémentation doit vérifier la décision lors de l’initialisation de la session, ainsi que régulièrement, pendant la lecture à partir de la réponse de pulsation.
