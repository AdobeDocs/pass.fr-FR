---
title: Panneaux de données sur le tableau de bord
description: Le tableau de bord vous aide à identifier les instances de partage de mot de passe en analysant un large éventail de données d’abonnés.
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 0%

---

# Panneaux de données sur le tableau de bord {#data-panels}

Une fois que vous avez sélectionné un segment et un intervalle de temps, le tableau de bord affiche différents panneaux de données, tableaux et graphiques qui reflètent une vue de haut niveau de l’activité de partage au sein du segment sélectionné.

Le tableau ci-dessous décrit la disponibilité et les différences entre les panneaux de données dans différents [versions](/help/accountiq/versions-aiq.md) de Account IQ :

| Panneaux de données | Services D2C | Programmeurs TVE | TVE MVPD |
|---|---|---|---|
| [Score de partage moyen agrégé pour le segment actuel](#aggregated-sharing) | Disponible et cohérent | Disponible et cohérent | Disponible et cohérent |
| [Catégories de vidéos dans un segment](#video-categories-segment) | Disponible avec de légères variations | Disponible avec de légères variations | Disponible avec de légères variations |
| [Score de partage par canaux et MVPD](#sharin-score-by-channels-and-mvpds) | Non disponible | Disponible | Non disponible |
| [Probabilité de partage des comptes](#accounts-sharing-probability) | Disponible et cohérent | Disponible et cohérent | Disponible et cohérent |
| [Nombre de comptes et d’utilisations en partageant le niveau de probabilité](#number-of-accounts-usage-sharing-probability) | Disponible et cohérent | Disponible et cohérent | Disponible et cohérent |


## Score de partage moyen agrégé pour le segment actuel {#aggregated-sharing}

Le panneau Score de partage moyen fournit un aperçu de première ligne résumant la quantité et l’impact du partage en termes de comptes et de volume de diffusion en continu.

Les mesures vous aident à comprendre l’ampleur (allant de faible, moyen, élevé à anormal) du partage des informations d’identification par vos abonnés, mesurées en termes de comptes et de consommation.

![](assets/aggregate-sharing-score.png)


*Score de partage moyen agrégé par le panneau pour le segment actuel*

>[!NOTE]
>
> L’indicateur bleu dans la variable **Score de partage moyen agrégé pour le segment actuel** sert des objectifs différents pour les services D2C par rapport à TV Everywhere. Pour les services D2C, il représente la variable **Index moyen des services** comme illustré dans l’image précédente. Si vous vous connectez en tant que programmeur ou MVPD, ce libellé se transforme en **Index moyen du secteur industriel**.

Les mesures suivantes sont des composants du panneau Note de partage moyenne .

### Niveau de partage {#sharing-level}

La jauge du niveau de partage affiche le pourcentage de tous les comptes abonnés partagés dans le segment défini pendant l’intervalle de temps sélectionné.

Le pourcentage est calculé à partir d’une moyenne de la probabilité de partage calculée pour chaque compte du segment. Ce calcul comprend les comptes qui ont diffusé en continu au moins une fois au cours de l’intervalle sélectionné.

L’indicateur de tendance affiche le pourcentage de changement de la valeur de la mesure par rapport à l’intervalle de temps précédent.

![](assets/sharing-level.png){width="350" align="left"}


*Niveau de partage*

### Utilisation de comptes partagés {#usage-from-shared-accounts}

La jauge indique le pourcentage d’utilisation par les comptes partagés parmi tous les comptes d’abonnés pour le segment défini et la période. Ces plages, nommées Faible, Moyen, Élevé et Abnormal, sont basées sur les moyennes de l’industrie.

L’indicateur de tendance, qui illustre une augmentation ou une baisse de l’utilisation des comptes partagés par rapport à l’intervalle de temps précédent.

![](assets/usage-4mshared-accounts.png){width="350" align="left"}


*Utilisation de comptes partagés*

### Score de partage global {#overall-sharing-score}

Le score de partage global est une combinaison de scores de partage, y compris &quot;Niveau de partage&quot; et &quot;Utilisation des comptes partagés&quot;.

Il fournit un score qui reflète l’impact global du partage. Son objectif est similaire à celui d’un score de crédit, résumant le niveau de partage avec un seul nombre. Mais dans ce cas, un score plus élevé indique un niveau de partage plus élevé.

![](assets/overall-sharing-score.png){width="350" align="left"}


*Score de partage global*

## Catégories de vidéos dans un segment {#video-categories-segment}

Vous pouvez sélectionner les en-têtes de colonne pour trier les données dans toutes les versions du compte IQ.

+++Services D2C : régions dans le segment

Lorsque vous vous connectez en tant que service D2C, **Régions dans le segment** Le tableau fournit une vue comparative des différents scores de partage agrégés pour la variable [catégories vidéo](/help/accountiq/product-concepts.md#video-category-def) dans le segment actuel.

![](assets/sharing-scores-by-regions-in-segment.png)

*Score de partage par région dans le segment*

>[!NOTE]
>
> La variable [catégories vidéo](product-concepts.md#video-category-def)  affiché dans l’image précédente, comme **Régions** dans n’est qu’un exemple. Lorsque vous vous connectez au compte IQ, ce panneau affiche la catégorie vidéo spécifique à votre entreprise.

Sélectionner **Exporter** pour télécharger les données dans un fichier .csv. Formation [exportation des rapports de panneau de données](/help/accountiq/export-reports.md).

+++

+++Programmeurs : MVPD dans le segment

Lorsque vous vous connectez en tant que programmeur, **MVPD dans le segment** Le tableau fournit une vue comparative des différents scores de partage agrégés pour les MVPD dans le segment actuel.

![](assets/sharing-scores-by-mvpds-in-segment.png)

Sélectionner **Exporter** pour télécharger les données dans un fichier .csv. Formation [exportation des rapports de panneau de données](/help/accountiq/export-reports.md).

+++

+++MVPD : programmeurs dans un segment

Lorsque vous vous connectez en tant que MVPD, **Programmeurs dans un segment** Le tableau fournit une vue comparative des différents scores de partage agrégés pour les programmeurs dans le segment actuel.

Sélectionnez les en-têtes de colonne pour trier les données.

![](assets/sharing-scores-by-programmers-in-segment.png)

*Score de partage par programmeur dans le segment*

Sélectionner **Exporter** pour télécharger les données dans un fichier .csv. Formation [exportation des rapports de panneau de données](/help/accountiq/export-reports.md).

+++

## Score de partage par canaux et MVPD  {#sharin-score-by-channels-and-mvpds}

Lorsque vous vous connectez en tant que programmeur, ce tableau fournit une vue comparative du partage des scores des canaux sélectionnés pour les MVPD dans le segment actuel.

Sélectionnez les en-têtes de colonne pour trier les données.

![](assets/sharing-scores-by-channels-mvpds.png)


*Partage de scores par canaux et MVPD*

## Probabilité de partage des comptes {#accounts-sharing-probability}

Ce graphique partitionne les comptes en plages de quintiles de probabilité de partage, allant de très bas (0-20 %) à très élevé (80-100 %). En savoir plus sur les plages de [Probabilité de partage de compte](#accounts-sharing-probability).

>[!NOTE]
>
>Le graphique à barres utilise une échelle logarithmique.


![](assets/dashboard-ac-sharing-prob.png)


*Nombres et pourcentages de comptes abonnés dans différentes plages de probabilités de partage*


## Nombre de comptes et d’utilisations en partageant le niveau de probabilité {#number-of-accounts-usage-sharing-probability}

Ce panneau fournit une vue tabulaire des comptes partitionnés en plages de quintiles de probabilité de partage, allant de très bas (0-20 %) à très élevé (80-100 %), avec l’utilisation associée de chaque quintile à partir de comptes partagés. En savoir plus sur les plages de [Probabilité de partage de compte](#accounts-sharing-probability).

![](assets/no-acc-usage-prob-level.png)

*Nombre de comptes, de tendances et d’utilisations appartenant à diverses plages de probabilités*

Sélectionner **Exporter** pour télécharger les données dans un fichier .csv. Formation [exportation des rapports de panneau de données](/help/accountiq/export-reports.md).

