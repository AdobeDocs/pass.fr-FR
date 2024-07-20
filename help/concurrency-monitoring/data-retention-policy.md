---
title: Politique de conservation des données
description: Politique de conservation des données
exl-id: aa7d2d5e-9a8b-404b-874c-9e5923417784
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Politique de conservation des données {#data-retention-policy}

>[!WARNING]
>
>**Remarque :** Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


## Introduction {#introduction}

Adobe, en tant que responsable du traitement des données, doit prendre les mesures appropriées pour aider ses clients à répondre aux demandes d’accès, de suppression et autres formulées par des individus. L’application de politiques de suppression appropriées, sécurisées et opportunes est un aspect important du respect de cette obligation.

## Définitions {#definitions}

Une politique de conservation des données détermine la durée pendant laquelle Adobe stocke les données du client. La politique de conservation des données par défaut pour la surveillance de la simultanéité est de **25 mois**.

| Période de conservation des données | La période de conservation des données est la période de conservation des données par défaut (25 mois). |
|---|---|
| **Période de rétention des données** | La fenêtre de rétention des données définit les paramètres pour lesquels des données complètes peuvent être visualisées et consignées. La fenêtre de rétention des données est déterminée comme suit :<br/> *Date de début* = date actuelle - période de conservation des données <br/>*Date de fin* = date actuelle |

## Collecte de données {#data-collection}

*Les données du parcours de navigation* représentent les données partagées par les clients sur les pulsations de session (par exemple, subjectID, mvpdName et metadata). Tous les champs de métadonnées personnalisés sont référencés dans les [attributs de métadonnées standard](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Types de clients {#customer-types}

### Clients actuels {#current-customers}

À moins que le client n’achète des extensions de rétention de données, la surveillance de la simultanéité sera conforme aux exigences de conservation des données client suivantes :

* *Les données du parcours de navigation* collectées par la surveillance de la simultanéité doivent être supprimées au plus tard **25 mois** à compter de la date de collecte.

### Clients arrêtés {#terminated-customers}

Un client qui a pris fin est un client qui a mis fin à la relation avec Adobe et qui n’utilise plus la surveillance de la simultanéité.

* *Les données du parcours de navigation* collectées par la surveillance de la simultanéité doivent être supprimées dans les **6 mois** à la date de fin du contrat du client.

## Suppression de données {#data-deletion}

Adobe conserve le droit de supprimer les données pour les dates au-delà de la période de conservation des données sans possibilité de récupération. Les données relatives aux clients actuels doivent être supprimées sur une base mensuelle glissante.
