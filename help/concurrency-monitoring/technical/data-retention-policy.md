---
title: Politique de conservation des données
description: Politique de conservation des données
exl-id: aa7d2d5e-9a8b-404b-874c-9e5923417784
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---

# Politique de conservation des données {#data-retention-policy}

>[!WARNING]
>
>**Remarque :** le contenu de cette page est fourni uniquement à titre d’information. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.


## Introduction {#introduction}

Adobe, dans son rôle de responsable du traitement des données, doit prendre les mesures appropriées pour aider ses clients à répondre aux demandes d’accès, de suppression et autres des personnes. L’application de politiques de suppression appropriées, sécurisées et opportunes est un aspect important du respect de cette obligation.

## Définitions {#definitions}

Une politique de conservation des données détermine la durée pendant laquelle Adobe stocke les données du client. La politique de conservation des données par défaut pour la surveillance de la simultanéité est de **25 mois**.

| Période De Conservation Des Données | La période de conservation des données est la période de conservation des données par défaut (25 mois). |
|---|---|
| **Période De Conservation Des Données** | La fenêtre de conservation des données définit les paramètres pour lesquels des données complètes peuvent être affichées et rapportées. La fenêtre de conservation des données est déterminée comme suit :<br/> *Date de début* = date actuelle - période de conservation des données <br/>*Date de fin* = date actuelle |

## Collecte de données {#data-collection}

*Données Clickstream* représente les données partagées par les clients sur les pulsations de la session (par exemple, subjectID, mvpdName et metadata). Tous les champs de métadonnées personnalisés sont référencés dans la [Attributs de métadonnées standard](/help/concurrency-monitoring/technical/standard-metadata-attributes.md).

## Types de clients {#customer-types}

### Clients actuels {#current-customers}

À moins que le client n’achète des extensions de conservation des données, la surveillance simultanée se conforme aux exigences de conservation des données client suivantes :

* *Les données Clickstream* collectées par la surveillance de simultanéité doivent être supprimées au plus tard **25 mois** à compter de la date de collecte.

### Clients résiliés {#terminated-customers}

Un client résilié est un client qui a mis fin à la relation avec Adobe et n’utilise plus la surveillance de simultanéité.

* *Les données Clickstream* collectées par la surveillance de simultanéité doivent être supprimées dans les **6 mois** à compter de la date de résiliation du contrat du client.

## Suppression de données {#data-deletion}

Adobe se réserve le droit de supprimer des données pour les dates postérieures à la période de conservation des données, sans possibilité de récupération. Les données des clients actuels doivent être supprimées tous les mois.
