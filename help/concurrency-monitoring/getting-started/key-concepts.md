---
title: Concepts clés
description: Découvrez les concepts fondamentaux de la surveillance d’accès simultané, notamment les sessions, les politiques, les métadonnées, etc
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---


# Concepts clés {#key-concepts}

La compréhension des concepts de base de la surveillance de concurrence est essentielle pour une implémentation réussie. Ce guide explique les blocs de création fondamentaux et la manière dont ils fonctionnent ensemble.

## Concepts de base {#core-concepts}

### Sessions

Une **session** représente une instance de diffusion vidéo en continu active. Considérez-le comme un « ticket » qui permet à un utilisateur ou une utilisatrice de regarder du contenu.

#### Cycle de vie de la session

1. **Création** - Lorsque l’utilisateur commence à regarder du contenu
2. **Actif** - Pendant que l’utilisateur ou l’utilisatrice regarde (maintenu par pulsations)
3. **Interruption** - Lorsque l’utilisateur cesse de regarder ou que la session expire

#### Propriétés de la session

```json
{
  "sessionId": "unique-session-identifier",
  "idp": "identity-provider",
  "subject": "user-identifier",
  "startTime": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-15T10:40:00Z",
  "metadata": {
    "deviceId": "device-123",
    "channel": "Channel1",
    "contentType": "live"
  }
}
```

### Politiques

**Politiques** sont des règles d’entreprise qui définissent des limites et des restrictions pour la diffusion en continu simultanée. Ils déterminent à quel moment les utilisateurs peuvent démarrer de nouveaux flux.

#### Composants de politique

- **Règles** - Limites et conditions spécifiques
- **Exigences en matière de métadonnées** - Quelles données sont nécessaires ?
- **Logique d’évaluation** - Application de la politique

#### Exemple de politique

```json
{
  "name": "basic_stream_limit",
  "description": "Maximum 3 concurrent streams per user",
  "rules": [
    {
      "type": "maxstreams",
      "limit": 3,
      "message": "You have reached your maximum of 3 concurrent streams"
    }
  ]
}
```

### Métadonnées

**Métadonnées** fournit un contexte pour chaque session. Il contient des informations sur l’appareil, le contenu, l’utilisateur et l’application.

#### Catégories de métadonnées

| Catégorie | Exemples | Objectif |
|-----------------|------------------------------------------|---------------------------------|
| **Appareil** | `deviceId`, `deviceType`, `osName` | Identification et catégorisation des appareils |
| **Contenu** | `channel`, `contentType`, `assetId` | Décrivez ce qui est visionné |
| **Utilisateur** | `subject`, `subscriptionTier` | Informations spécifiques à l’utilisateur |
| **Application** | `applicationName`, `applicationPlatform` | Détails spécifiques à l’application |
| **Emplacement** | `country`, `hba` | Informations géographiques et réseau |

#### Métadonnées obligatoires ou facultatives

- **Métadonnées obligatoires** - Doit être fourni pour la création de session
- **Métadonnées facultatives** - Peuvent être fournies pour des politiques améliorées
- **Métadonnées standard** - Champs prédéfinis disponibles pour toutes les applications
- **Métadonnées personnalisées** - Champs spécifiques à l’application que vous définissez

## Identité et authentification {#identity-and-authentication}

### Fournisseur d’identité (IdP)

Un **fournisseur d’identité** est le service qui authentifie les utilisateurs et fournit leurs informations d’identité. Dans l’intégration d’Adobe Pass, il s’agit généralement d’un MVPD.

#### Exemples d’IdP

- Câblodistributeurs (Comcast, charter, etc.)
- Fournisseurs de services par satellite (DirectTV, Dish)
- Services de streaming (Netflix, Hulu)
- Opérateurs de téléphonie mobile (Verizon, AT&amp;T)

### Objet

Un **objet** est l’identifiant unique d’un utilisateur dans un fournisseur d’identité. Il s’agit généralement de l’identifiant de compte ou d’abonné de l’utilisateur.

## Gestion de session {#session-management}

### Heartbeats

**Heartbeats** sont des appels API périodiques qui maintiennent les sessions actives. Ils indiquent au système que « cet utilisateur continue à regarder ».

#### Exigences de pulsation

- **Fréquence** - Toutes les 60 secondes (recommandé)
- **Objectif** - Maintenir la session active, mettre à jour les métadonnées
- **Interruption** - La session expire si les pulsations s’arrêtent


### Fin de session

Les sessions peuvent être interrompues de plusieurs manières :

1. **L’utilisateur arrête de regarder** - L’application appelle le point d’entrée DELETE
2. **Expiration de la session** - Pas de pulsation reçue au cours de la temporisation
3. **Violation de politique** - La nouvelle session met fin à l’ancienne (FIFO)
4. **Terminaison à distance** - Un autre appareil met fin à la session

## Évaluation Des Politiques {#policy-evaluation}

### Fonctionnement des politiques

1. **L’utilisateur demande un nouveau flux**
2. **Le système évalue les politiques** par rapport à l’utilisation actuelle.
3. **Décision prise** - Autoriser ou refuser
4. **Réponse envoyée** - Informations de succès ou de conflit

## Résolution de conflits {#conflict-resolution}

### Qu’est-ce qu’un conflit ?

Un **conflit** se produit lorsqu’un utilisateur ou une utilisatrice tente de démarrer un nouveau flux, mais dépasse ses limites.

### Réponse aux conflits

Lorsqu’un conflit se produit, le système renvoie une réponse 409 Conflit avec des informations détaillées :

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
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {...}
            }
          ]
        }
      }
    ]
  }
}
```

### Stratégies de résolution

#### LIFO (Last In, First Out)

- **Les anciennes sessions sont protégées**
- **Nouvelle session bloquée**
- **IU simplifiée**
- **Idéal pour un affichage étendu**


#### FIFO (First In, First Out)

- **Une nouvelle session met fin à une session existante**
- **L’utilisateur choisit la session à arrêter**
- **IU plus complexe requise**
- **Mieux pour le changement de contenu**

## Applications et clients {#applications-and-tenants}

### Application

Une **application** est un programme logiciel qui s&#39;intègre à la surveillance d&#39;accès simultané. Chaque application possède un identifiant unique et peut avoir ses propres politiques.

#### Exemples d’applications

- Application mobile (iOS/Android)
- Application web
- Application Smart TV
- Application de décodeur

### Locataire

Un **client** est une organisation qui possède une ou plusieurs applications. Les clients peuvent partager des politiques entre les applications.

#### Structure du client

```
Tenant: "Streaming Company"
├── Application: "Mobile App"
├── Application: "Web App"
└── Application: "TV App"
```

## Environnements {#environments}

### Environnement d’évaluation

- **Objectif** - Développement et tests
- **URL** - `https://streams-stage.adobeprimetime.com/v2/`
- **Informations d’identification** - Tester les identifiants d’application
- **Données** - Données de test uniquement

### Environnement de production

- **Objectif** - Trafic utilisateur en direct
- **URL** - `https://streams.adobeprimetime.com/v2/`
- **Informations d’identification** - ID de l’application de production
- **Data** - Données utilisateur réelles

## Flux d’intégration {#integration-flow}

### Flux de base

1. **Authentification de l’utilisateur** - S’authentifier avec Adobe Pass
2. **Création de session** - Créez une session CM avec des métadonnées utilisateur
3. **Gestion des pulsations** - Envoi de pulsations périodiques
4. **Interruption de session** - Se termine lorsque l’utilisateur ou l’utilisatrice cesse de regarder

### Gestion des erreurs

1. **404 Introuvable** - Gérez les requêtes avec un ID de session non généré par le service CM
2. **409 Conflits** - Gestion des violations de politique
3. **410 Gone** - Gestion de la fermeture de session
4. **Erreurs réseau** - Gestion des problèmes de connectivité
5. **Erreurs d’authentification** - Gestion des problèmes d’informations d’identification


## Terminologie courante {#common-terminology}

| Terme | Définition |
|-----------------|--------------------------------------------|
| **Session** | Instance de streaming vidéo active |
| **Politique** | Règle métier définissant des limites d’utilisation |
| **Métadonnées** | Informations contextuelles sur les sessions |
| **IDP** | Fournisseur d’identité (service d’authentification) |
| **Objet** | Identifiant utilisateur dans un fournisseur d’identité |
| **Heartbeat** | Appel périodique pour maintenir la session active |
| **Conflit** | Violation de politique lors du démarrage d’un nouveau flux |
| **LIFO** | Résolution des conflits « Dernier entré, premier sorti » |
| **FIFO** | Résolution des conflits du premier entré, premier sorti |
| **Client** | Application propriétaire de l’organisation |
| **Application** | Programme logiciel utilisant CM |

