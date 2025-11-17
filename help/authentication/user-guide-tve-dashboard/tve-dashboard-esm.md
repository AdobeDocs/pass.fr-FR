---
title: Tableau de bord ESM
description: Découvrez comment utiliser le tableau de bord ESM pour surveiller les données de droits et d’événements entre les partenaires MVPD.
source-git-commit: 53ebbd82fc160f68fccdddb18cf98e249ad6ecce
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---


# Tableau de bord ESM {#esm-dashboard}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Le tableau de bord ESM fournit une vue unifiée des données de droits et d’événements pour vous aider à surveiller les performances, à identifier les anomalies et à comprendre les modèles d’accès utilisateur parmi les partenaires MVPD. Ce guide explique comment utiliser les filtres du tableau de bord, interpréter les rapports et comprendre les mesures clés sur des intervalles de temps configurables.

![Vue Tableau de bord ESM](../assets/tve-dashboard/new-tve-dashboard/esm/esm-full-page.png)

## Cas d’utilisation {#use-cases}

- Visualiser les tendances par plateforme ou MVPD
- Comparaison des performances de MVPD
- Comprendre l’utilisation des clients par application

Vous trouverez plus d’informations sur les données et les événements ESM dans la section [Présentation de la surveillance du service de droit](https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview).

## Rapports {#reports}

Les rapports disponibles sont les suivants :

### Requêtes de lecture {#play-requests}

Affiche le nombre de requêtes de lecture pour l’intervalle de temps sélectionné.

**Style de graphique** - Ligne.

### Conversion de l’authentification {#authentication-conversion}

Affiche le ratio entre les événements d’authentification réussis et le nombre total de tentatives d’authentification.

**Style de graphique** - Barre horizontale

### Conversion de l’autorisation {#authorization-conversion}

Affiche le ratio entre les événements d’authentification réussis et le nombre total de tentatives d’authentification.

**Style de graphique** - Barre horizontale

### Latence d’autorisation {#authorization-latency}

Affiche la latence moyenne (en millisecondes) pour les réponses de MVPD.

**Style de graphique** - Ligne.

### Utilisation de MVPD {#mvpd-usage}

Affiche une comparaison du nombre d’utilisateurs uniques par MVPD.

**Style de graphique** - Aires empilées.

### Authentifications réussies {#successful-authentications}

Affiche le nombre total d’authentifications réussies pour l’intervalle de temps sélectionné.

**Style de graphique** - Ligne.

### Autorisations réussies {#successful-authorizations}

Affiche le nombre total d’autorisations réussies (réponses « Autoriser » de MVPD) pour l’intervalle de temps sélectionné.

**Style de graphique** - Ligne.

## Actions {#actions}

### Afficher par {#view-by}

Pour chaque graphique, il existe une liste déroulante « Afficher par » qui vous permet de sélectionner exactement les données à afficher.

- **Agrégé** - affiche les données globales
- **MVPD / canaux / plateformes** - décrit les filtres spécifiques sélectionnés

### Télécharger {#download}

Vous pouvez télécharger les données brutes :

- **Télécharger les données du graphique au format CSV** - télécharge les données d’un graphique spécifique
- **Télécharger toutes les données au format CSV** - télécharge toutes les données ESM sur tous les graphiques

## Filtres {#filters}

Utilisez des filtres pour affiner le jeu de données et cibler l’analyse. Les filtres suivants sont disponibles :

- **Canal** : inclut tous les canaux (marques) disponibles
- **MVPD** : sélection sur un ou plusieurs fournisseurs
- **Plateforme** : web, mobile, télévision connectée ou famille d’appareils

Pour ajouter un nouveau filtre, sélectionnez le bouton « Ajouter des filtres ».

Dans la page « Filtres des jeux de données », vous pouvez faire glisser et déposer les filtres nécessaires.

![filtres du tableau de bord ESM](../assets/tve-dashboard/new-tve-dashboard/esm/filters-modal.png)

Pour chaque section, vous pouvez supprimer des filtres individuellement ou effacer l’ensemble de la sélection.

### Conseils sur les filtres {#filter-tips}

- Combinez plusieurs filtres pour isoler une cohorte (par exemple, un MVPD sur une plateforme mobile pour un canal).
- N’ajoutez pas de filtres pour effectuer un zoom arrière et établir une ligne de base avant d’effectuer une exploration.

## Intervalles de temps {#time-intervals}

Contrôlez la fenêtre d’analyse et la granularité.

![Intervalles de temps dans le tableau de bord ESM](../assets/tve-dashboard/new-tve-dashboard/esm/date-picker.png)

### Période {#date-range}

**Préréglages** : Aujourd’hui, Semaine en cours, 7 derniers jours, Mois en cours, 30 derniers jours, 3 derniers mois, 6 derniers mois, 12 derniers mois

**Personnalisé** : sélectionnez l’intervalle de temps souhaité.

### Granularité {#granularity}

Quotidien/Mensuel
