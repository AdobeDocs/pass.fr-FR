---
title: Politique de conservation des données
description: Politique de conservation des données
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# Politique de conservation des données {#data-retention-policy}

>[!WARNING]
>
>**Remarque :** Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


## Introduction {#introduction}

Adobe, en tant que responsable du traitement des données, doit prendre les mesures appropriées pour aider ses clients à répondre aux demandes d’accès, de suppression et autres formulées par des individus. L’application de politiques de suppression appropriées, sécurisées et opportunes est un aspect important du respect de cette obligation.

## Définitions {#definitions}

Une politique de conservation des données détermine la durée pendant laquelle Adobe stocke les données du client. La stratégie de conservation des données par défaut pour la surveillance de la simultanéité est : **25 mois**.

| Période de conservation des données | La période de conservation des données est la période de conservation des données par défaut (25 mois). |
|---|---|
| **Période de conservation des données** | La fenêtre de rétention des données définit les paramètres pour lesquels des données complètes peuvent être visualisées et consignées. Le créneau de rétention des données est déterminé comme suit :<br/> *Date de début* = date actuelle - période de conservation des données <br/>*Date de fin* = date courante |

## Collecte de données {#data-collection}

*Données du parcours de navigation* représente les données partagées par les clients sur les pulsations de session (par exemple, subjectID, mvpdName et metadata). Tous les champs de métadonnées personnalisés sont référencés dans la variable [Attributs de métadonnées standard](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Types de clients {#customer-types}

### Clients actuels {#current-customers}

À moins que le client n’achète des extensions de rétention de données, la surveillance de la simultanéité sera conforme aux exigences de conservation des données client suivantes :

* *Données du parcours de navigation* collecté par la surveillance simultanée doit être supprimé au plus tard **25 mois** à partir de la date de la collection.

### Clients arrêtés {#terminated-customers}

Un client qui a pris fin est un client qui a mis fin à la relation avec Adobe et qui n’utilise plus la surveillance de la simultanéité.

* *Données du parcours de navigation* collecté par la surveillance simultanée doit être supprimé dans **6 mois** à partir de la date de fin du contrat du client.

## Suppression de données {#data-deletion}

Adobe conserve le droit de supprimer les données pour les dates au-delà de la période de conservation des données sans possibilité de récupération. Les données relatives aux clients actuels doivent être supprimées sur une base mensuelle glissante.

