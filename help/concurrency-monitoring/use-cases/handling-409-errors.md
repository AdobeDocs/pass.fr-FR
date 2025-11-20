---
title: Gestion des erreurs de conflit 409
description: Découvrez comment gérer les erreurs de conflit 409 lorsque des limites d’utilisation simultanées sont atteintes
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---


# Gestion des erreurs de conflit 409 {#handling-409-errors}

Lorsqu’un utilisateur ou une utilisatrice tente de démarrer un nouveau flux et atteint une limite d’utilisation simultanée, la surveillance de simultanéité renvoie une réponse de conflit **409**. Il est essentiel de comprendre comment gérer cette erreur pour offrir une bonne expérience utilisateur.

## Qu&#39;est-ce qu&#39;un conflit 409 ? {#what-is-a-409-conflict}

Un conflit 409 survient lorsque :

1. **L’utilisateur tente de démarrer un nouveau flux**
2. **L’évaluation des politiques détermine** la requête dépasserait les limites.
3. **Le système renvoie la valeur 409** avec des informations détaillées sur les conflits
4. **L’application doit décider** comment gérer le conflit.

### Structure de réponse 409

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "id": "advice-1",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1",
                "contentType": "live"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Comprendre la réponse {#understanding-the-response}

### Champs clés

- **`decision`** : toujours « REFUSER » pour 409 conflits
- **`associatedAdvice`** : tableau d’objets de conseil expliquant la violation
- **`conflicts`** : liste des sessions actives à l’origine du conflit
- **`terminationCode`** : code unique pour terminer une session spécifique

### Types de conseils

#### Avis de violation de règle

```json
{
  "adviceType": "rule-violation",
  "attributes": {
    "rule": "max_streams",
    "threshold": 3,
    "current": 4,
    "conflicts": [...]
  }
}
```

#### Conseil de résiliation à distance

```json
{
  "adviceType": "remote-termination",
  "attributes": {
    "terminatedBy": "session-456",
    "reason": "New session requested with X-Terminate header"
  }
}
```

## Stratégies de gestion {#handling-strategies}

- En mode FIFO, CM permet à la nouvelle session de démarrer en mettant fin à une session existante.
- En mode LIFO, CM bloque la nouvelle session et en informe l&#39;utilisateur


## Bonnes pratiques {#best-practices}

### &#x200B;1. Effacer la communication utilisateur

- **Expliquez la limite** - Les utilisateurs doivent comprendre pourquoi ils sont bloqués.
- **Afficher les options disponibles** - Ce qu’ils peuvent faire pour résoudre le conflit
- **Fournir un contexte** - Afficher les sessions actives

### &#x200B;2. Gestion Des Erreurs Correcte

- **Ne pas planter** - Gérer les erreurs 409 de manière élégante
- **Proposer des alternatives** - Proposer des moyens de résoudre le conflit
- **Enregistrer l’état utilisateur** - Ne perdez pas leur sélection de contenu

### &#x200B;3. Considérations Relatives À L’Expérience Utilisateur

- **Résolution rapide** - Faciliter la résolution des conflits
- **Des choix clairs** - Les utilisateurs doivent comprendre leurs options.
- **Comportement cohérent** - Gérez les conflits de la même manière à chaque fois

### &#x200B;4. Considérations Techniques

- **Analyser la réponse avec soin** - Extraire toutes les informations pertinentes
- **Gérer les cas de périphérie** - Que se passe-t-il si aucun conflit n’est renvoyé ?
- **Consignation des conflits** - Suivi des violations de politique pour analyse


