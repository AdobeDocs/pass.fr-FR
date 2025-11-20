---
title: Aperçu de la référence de l’API
description: Référence complète pour l’API de surveillance de simultanéité comprenant les points d’entrée, l’authentification et les formats de réponse
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# Aperçu de la référence de l’API {#api-reference-overview}

L’API Concurrency Monitoring fournit une interface RESTful pour gérer les sessions de diffusion en continu et appliquer des politiques d’utilisation simultanées. Cette référence fournit une documentation complète sur tous les points d’entrée, les méthodes d’authentification, les formats de requête/réponse et la gestion des erreurs.

## URL de base de l’API

### Environnement de production

```
https://streams.adobeprimetime.com/v2/
```

### Environnement d’évaluation

```
https://streams-stage.adobeprimetime.com/v2/
```

**Remarque :** utilisez toujours l’environnement d’évaluation pour le développement et les tests. Les informations d’identification de production ne sont fournies qu’après une intégration d’évaluation réussie.

## Authentification

Tous les appels API nécessitent une authentification HTTP de base à l’aide des informations d’identification de votre application :

- **Nom d’utilisateur :** identifiant de votre application (fourni par Adobe)
- **Password:** Chaîne vide

### Exemple d’en-tête d’authentification

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## Normes de format de réponse

### Réponses de succès

Toutes les réponses réussies suivent cette structure :

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Réponses d’erreur

Toutes les réponses d’erreur suivent cette structure :

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### Format du résultat de l’évaluation

Lors de l’évaluation des politiques (en particulier pour les conflits 409), les réponses incluent un résultat d’évaluation :

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Codes d’état HTTP courants

| Code | Description | Si Renvoyé |
|------|----------------------|------------------------------------------------|
| 200 | OK | Requêtes GET réussies |
| 202 | Créé/Accepté | Création de session/pulsation enregistrée avec succès |
| 400 | Requête incorrecte | Paramètres non valides ou champs obligatoires manquants |
| 401 | Non Autorisé | Authentification non valide ou manquante |
| 403 | Interdit | Autorisations insuffisantes |
| 404 | ID de session introuvable | ID de session non généré par le service CM |
| 409 | Conflit | Violation de politique (limite simultanée atteinte) |
| 410 | Autant | La session a expiré ou a été interrompue |
| 429 | Trop De Requêtes | Limite de taux dépassée |

## Méthodes de transmission des paramètres

### Paramètres de chemin

Paramètres requis qui font partie du chemin d’accès à l’URL :

1. `{idp}` - Identifiant du fournisseur d’identité
2. `{subject}` - Identifiant de l’utilisateur (généralement provenant d’Adobe Pass)
3. `{sessionId}` - Identifiant de session (renvoyé dans l’en-tête Emplacement)

### Paramètres supplémentaires

Des paramètres optionnels sont transmis dans l’URL :

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### Données de formulaire (POST/PUT)

Métadonnées et données de session dans le corps de la requête :

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### En-têtes

Paramètres spéciaux transmis dans les en-têtes HTTP :

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## Bonnes pratiques de gestion des erreurs

### 409 Gestion des conflits

Lorsque vous recevez une réponse 409 Conflit :

1. **Analysez le résultat de l’évaluation** pour comprendre la violation de la politique
2. **Extraire les informations de conflit** à partir de `associatedAdvice`
3. **présenter des options à l’utilisateur** en fonction de votre stratégie LIFO/FIFO
4. **Utilisez des codes de terminaison** si vous implémentez le comportement LIFO

### 410 Gone Handling

Lorsque vous recevez une réponse 410 Gone :

1. **Vérifier si la réponse comporte un corps** - Indique l’arrêt à distance.
2. **Analyser les conseils** pour comprendre pourquoi la session a été interrompue
3. **Mettre à jour l’interface utilisateur** pour refléter la fin de la session
4. **Gérer avec élégance** - la session a peut-être expiré naturellement
5. **Démarrer une nouvelle session** - si nécessaire, démarrez une nouvelle session

### Limitation de débit

Lorsque vous recevez un 429 Too Many Requests :

1. **Limiter la fréquence d’appel** à jusqu’à 200 requêtes par minute, ce qui correspond au niveau maximal accepté par CM.
2. **Envoyez des pulsations à l’intervalle requis** qui est d’une par minute.

## Outils de test

### Explorateur d’API interactive

Utilisez notre [interface utilisateur Swagger](https://streams-stage.adobeprimetime.com/swagger-ui/index.html) pour les tests interactifs :

1. Saisissez votre ID d’application dans le coin supérieur droit
2. Cliquez sur Explorer pour définir l’authentification
3. Tester les points d’entrée avec des paramètres réels
4. Afficher des exemples de requête/réponse
