---
title: Rapports d’utilisation généraux
description: En savoir plus sur les rapports d’utilisation généraux
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: 4a8a73d6c67508e88ba3ffbb9033b7e339f4fe8f
workflow-type: tm+mt
source-wordcount: '1055'
ht-degree: 0%

---

# [!UICONTROL General usage] rapports {#general-usage-reports}

Les rapports [!UICONTROL Account IQ] sont des outils d’analyse de base qui vous permettent d’analyser vos données pour isoler [cohortes](/help/accountiq/product-concepts.md#segmet-def), identifier les anomalies et créer une compréhension des caractéristiques de votre compte.

La page de rapports [!UICONTROL General usage] fournit des outils pour créer des mesures de sous-groupe en fonction du nombre de périphériques de compte utilisés, des adresses IP détectées et de leurs codes postaux respectifs.

Les rapports sont tous basés sur le segment actuel sélectionné dans le panneau [Segments et intervalle de temps](/help/accountiq/segments-timeinterval.md) . Vous pouvez affiner votre sélection et la réduire davantage en spécifiant les seuils (nombre d’appareils, nombre d’adresses IP et nombre de codes postaux) dans le panneau [Aperçu de l’instantané-Comptes au-dessus des seuils](#snapshot-overview).

## Lire les requêtes et les abonnés uniques {#playreq-uniquesubs}

Les graphiques linéaires présentent ici les modifications au fil du temps des valeurs, telles que les requêtes de lecture et les abonnés uniques dans un intervalle de temps sélectionné pour le segment défini.

+++ Services D2C : Lire les requêtes/abonnés uniques

![](assets/d2c-line-graph-gu.png)


*Lire les requêtes/abonnés uniques pour les services D2C*

+++

+++Programmeurs : Lire les requêtes/abonnés uniques

![](assets/progr-line-graph-gu.png)


*Lire les requêtes/abonnés uniques pour les programmeurs*

+++

+++MVPD : abonnés uniques

![](assets/mvpd-line-graph-gu.png)

*Abonnés uniques pour MVPD*

+++

<br/>

L’axe X représente la durée selon l’intervalle actuel et l’axe Y représente les mesures de base de l’activité des abonnés au cours de cette période. Les graphiques linéaires vous aident à visualiser et comparer l’activité des abonnés dans le segment actuel. Selon la version d’Account IQ, les mesures incluent :

* **AuthN OK** : nombre d’authentifications réussies. En savoir plus sur [AuthN OK](/help/accountiq/product-concepts.md#authn-ok-def).

* **AuthZ OK** : nombre d’autorisations réussies. En savoir plus sur [AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def).

* **Lire les requêtes** : nombre de requêtes de lecture. En savoir plus sur les [requêtes de lecture](/help/accountiq/product-concepts.md#play-requests-def).

* **Abonnés uniques** : nombre d’abonnés uniques réussis. En savoir plus sur les [abonnés uniques](/help/accountiq/product-concepts.md#unique-subscriber-def).

>[!NOTE]
>
>La disponibilité des mesures varie en fonction de la version d’Account IQ.

## Aperçu des instantanés - Comptes au-dessus des seuils {#snapshot-overview}

Affinez vos analyses et rapports à l’aide de ce filtre supplémentaire afin de définir différents seuils d’utilisation. Une fois que vous avez sélectionné un segment, vous pouvez également utiliser les filtres suivants pour analyser davantage le comportement des abonnés :

* Nombre seuil de périphériques

* Nombre seuil d’adresses IP

* Nombre seuil de codes postaux

Lorsque vous mettez à jour les valeurs de seuil dans le panneau [Segment de comptes basé sur les seuils sélectionnés](#account-segments-basedon-segments), vous verrez l’effet dans :

* [Périphériques par semaine (ou mois), par compte](#devices-week-account)

* [Emplacements par semaine (ou mois) par compte](#locations-week-account)

* [IP par semaine (ou mois) par compte](#ip-week-account)

* [Vue historique du segment des comptes](#account-segment-historical-view)

>[!NOTE]
>
>Chaque seuil est défini sur une valeur par défaut de 4. En d’autres termes, la page Utilisation générale présente l’analyse pour les abonnés utilisant plus de quatre appareils, consommant du contenu provenant de plus de quatre adresses IP différentes, *et* de plus de quatre codes postaux différents.

### Segment de comptes en fonction des seuils sélectionnés {#account-segments-basedon-segments}

Le panneau **Segment de compte basé sur les seuils sélectionnés** vous donne la possibilité de définir des seuils (entre 1 et 10) pour le nombre d’appareils, le nombre d’adresses IP et le nombre de codes postaux.

Le graphique vous montre les éléments suivants :

* Nombre absolu de comptes abonnés.

* Pourcentage du nombre total de comptes d&#39;abonnés dans le segment qui utilisent le nombre d&#39;appareils, par rapport au nombre d&#39;IP, dans le nombre de codes postaux tel que défini par les seuils.

![](assets/select-thresholds.png)

## Périphériques par semaine (ou mois), par compte {#devices-week-account}

Ce graphique à barres fournit des informations sur le comportement d’utilisation en termes de la manière dont les abonnés utilisent leurs appareils pour accéder au contenu.

L’axe X trace le nombre de comptes et l’axe Y trace le nombre de périphériques. En fonction du seuil que vous définissez pour le nombre d’appareils par compte, il marque le nombre absolu de comptes abonnés qui consomment du contenu d’un nombre spécifique d’appareils au cours d’une semaine.

![](assets/bar-gr-devices-w-acc.png)

Lorsque vous passez le curseur de la souris sur une barre (spécifique au nombre d’appareils), un libellé s’affiche, indiquant le nombre d’abonnés (et le pourcentage du nombre total de comptes d’abonnés dans le segment) qui diffusent du contenu de canal en continu à l’aide de ces nombreux appareils au cours d’une semaine.

Le graphique marque également les points suivants :

* Une ligne rouge pour marquer le seuil que vous définissez.

* Une ligne verte permettant de marquer la moyenne des différents appareils utilisés par un compte abonné par semaine (ou par mois).

L’anneau fournit une autre vue des appareils utilisés par les comptes dans le segment actuel au-dessus du seuil défini.

![](assets/donut-devices-w-acc.png)

## Emplacements par semaine (ou mois) par compte {#locations-week-account}

Comme pour la mesure [Périphériques par semaine (ou mois) par compte](#devices-week-account), la mesure Emplacements par semaine (ou mois) par compte vous permet d’analyser l’utilisation du compte d’abonné à partir de différents emplacements. L’axe X trace le nombre de comptes et l’axe Y trace le nombre d’emplacements.

![](assets/graph-loc-week-acc.png)

Une fois que vous avez défini le seuil du nombre d’emplacements, vous pouvez utiliser le graphique pour identifier les éléments suivants :

* Nombre (et pourcentage) d’abonnés qui consomment du contenu à partir (d’un emplacement spécifique) x nombre d’emplacements au cours d’une semaine.

* Pourcentage du nombre total de comptes d’abonnés qui visualisent du contenu à partir de plus d’emplacements que le seuil.

* Comparez la moyenne hebdomadaire (nombre d’emplacements différents pour un compte) avec le seuil.

## Ips par semaine (ou mois) par compte {#ip-week-account}

Tout comme la mesure **Nombre d’emplacements par semaine par compte**, la mesure **Nombre d’adresses IP par semaine par compte** vous permet d’évaluer la quantité de changement à la source de la diffusion en continu pour le segment actuel.

L’axe X trace Nombre de comptes et l’axe Y trace Nombre d’adresses IP.

![](assets/graph-ip-week-acc.png)

Une fois que vous avez défini un segment et défini le seuil du nombre d’adresses IP, vous pouvez utiliser le graphique pour identifier les éléments suivants :

* Nombre (et pourcentage) d’abonnés qui consomment du contenu d’un nombre spécifique d’adresses IP au cours d’une semaine.

* Pourcentage du nombre total de comptes d’abonnés qui visualisent le contenu à partir d’un plus grand nombre d’adresses IP que le seuil.

* Comparez la moyenne hebdomadaire (nombre d’adresses IP différentes pour un compte) avec le seuil.

## Vue de l’historique des segments des comptes {#account-segment-historical-view}

Le graphique à barres Affichage historique vous permet de comparer les mesures d’utilisation à différents intervalles de temps. En outre, il répertorie collectivement les différentes mesures d’utilisation, telles que [Périphériques par semaine (ou mois) par compte](#devices-week-account), [Emplacements par semaine (ou mois) par compte](#locations-week-account) et [IP par semaine (ou mois) par compte](#ip-week-account).

* L’axe X trace l’intervalle de temps et l’axe Y trace le nombre de comptes d’abonnés, de périphériques, d’emplacements et d’adresses IP.

* Les barres de couleur orange représentent des segments à différents intervalles de temps.

* Le graphique linéaire trace les modifications dans les valeurs [Périphériques par semaine (ou mois) par compte](#devices-week-account), [Emplacements par semaine (ou mois) par compte](#locations-week-account) et [IP par semaine (ou mois) par compte](#ip-week-account) sur l’intervalle de temps basé sur le seuil.

![](assets/historical-view.png)

* Les barres bleues représentent le nombre total d’abonnés actifs dans l’ensemble du secteur pendant un intervalle de temps.

* Vous pouvez sélectionner des légendes spécifiques qui vous aideront à mettre le graphique à l’échelle.

![](assets/historical-view-total.png)
