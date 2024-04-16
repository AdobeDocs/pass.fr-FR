---
title: Rapports sur les comptes partagés
description: Rapports sur les comptes partagés
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# Rapports sur les comptes partagés {#shared-accounts-reports}

Les rapports Comptes partagés fournissent un autre groupe de graphiques et de graphiques qui reflètent le comportement de partage et la consommation du segment actuel. Par exemple : **[!UICONTROL Over Moderate Probability]** et **[!UICONTROL Over Low Probability]** pour le segment actuel.

## Probabilité de partage des comptes {#accounts-sharing-probability}

Ces graphiques en anneau et à barres indiquent les pourcentages (et les nombres absolus) des comptes abonnés qui tombent dans des plages spécifiques de probabilité de partage. Ces plages sont définies comme suit :

* Très élevé (80 % à 100 %)
* Élevé (60 %-80 %)
* Modéré (40 % à 60 %)
* Faible (20 % à 40 %)
* Très faible (0 à 20 %)

La ligne rouge indique la plage de seuil sélectionnée dans la variable [Comptes supérieurs au seuil dans le segment actuel](#threshold-selector) et la zone rouge clair contient le total de tous les comptes au-dessus de ce seuil.

![](assets/accounts-sharing-probability-pie.png)

Le graphique à barres trace le nombre de comptes appartenant à chaque plage sur l’axe des ordonnées pour chacune des plages (tracées sur l’axe des abscisses).

![](assets/accounts-sharing-probability-bar.png)

Ici encore, la ligne rouge marque le seuil actuel, et la zone rouge claire contient le total de tous les comptes au-dessus de ce seuil.

>[!NOTE]
>
> L’axe Y du graphique à barres est logarithmique.

### Comptes supérieurs au seuil dans le segment actuel{#threshold-selector}

Ce panneau vous permet de sélectionner la plage de seuil pour les graphiques en anneau et en barres ci-dessus. Les quatre options sont les suivantes :

* Comptes **sur très faible** partage **probabilité**

* Comptes **over low** partage **probabilité**

* Comptes **sur modéré** partage **probabilité**

* Comptes **sur élevé** partage **probabilité**

![](assets/threshold-selector-shared-accounts.png)

Une fois que vous avez sélectionné le seuil, le panneau affiche le pourcentage (et le nombre) de comptes parmi tous les comptes abonnés du segment sélectionné.

## Requêtes de lecture de segment sur le total {#play-request-out-total}

Le graphique en anneau indique le pourcentage (et le nombre) des requêtes de lecture effectuées par les abonnés dans le segment, ce qui vous permet de comparer les requêtes de lecture effectuées par les abonnés qui ne se trouvent pas dans le segment défini.

![](assets/play-req-outof-total.png)

Lorsque vous déplacez le curseur sur le graphique en anneau, il affiche également les pourcentages et les nombres d’abonnés provenant de différentes plages de probabilités.

<!--![](assets/play-request-total.gif)-->

## Nombre moyen de segments d’appareils par compte{#avg-devices-account}

Le graphique à barres indique le nombre moyen d’appareils de chaque type actuellement utilisés par les abonnés dans le segment actuel et ceux qui ne le sont pas dans le segment actuel.

![](assets/avg-devices-per-acc.png)

## Codes segment-zip par période par compte {#zip-codes-period-account}

Ce graphique vous indique le nombre d’abonnés du segment actuel qui utilisent du contenu à différents emplacements (comme mesuré par le code postal) pendant l’intervalle de temps donné.

![](assets/zip-period-account.png)

>[!NOTE]
>
>Vous pouvez zoomer sur les barres qui représentent plusieurs jeux de codes postaux, représentés par un **+** (plus) en double-cliquant dessus (par exemple, 10+).


## Portée géographique du segment par période par compte {#geo-span-period-account}

Ce graphique à barres trace le nombre de comptes d’abonnés qui utilisent du contenu à partir d’emplacements qui se trouvent dans différentes plages géographiques, en kilomètres. La plage est basée sur la distance maximale entre les emplacements à partir desquels un abonné a diffusé en continu pendant l’intervalle de temps.

![](assets/geogr-span-account.png)

>[!NOTE]
>
> Vous pouvez effectuer un zoom sur les barres qui représentent plusieurs jeux de distances géographiques, représentées par une **+** (plus) en double-cliquant dessus (par exemple, 1 000+).

>[!MORELIKETHIS]
>
>* Découvrez comment exporter des rapports pour les 1 000 premiers abonnés du segment sélectionné à l’aide de filtres dans les rapports Comptes partagés à l’aide de [Exporter les 1 000 premiers comptes](/help/accountiq/export-acc-information.md) .
