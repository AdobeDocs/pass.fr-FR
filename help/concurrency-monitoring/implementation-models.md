---
title: Modèles d’implémentation
description: Modèles d’implémentation
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---

# Modèles d’implémentation {#imp-models}

## Stratégies côté serveur {#ss-policies}

Ce modèle utilisera CM comme point de décision de politique, déléguant ainsi la décision d’accès au service.

Étant donné que le client ne doit pas faire d’hypothèses concernant les politiques appliquées, l’implémentation doit vérifier la décision lors de l’initialisation de la session, ainsi que régulièrement, pendant la lecture à partir de la réponse de pulsation.
