---
title: Segments d’abonné et intervalle de temps
description: Définissez des cohortes ou sélectionnez des segments d’abonnés pour évaluer les possibilités et les modèles de partage de compte des visionneuses de canaux afin d’utiliser des outils graphiques et des rapports dans le compte IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: 6b790728f3d6a8eed5dfc0f8b3d0dad283af6418
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---


# Segments d’abonné et intervalle de temps {#cohorts-segments}


Lorsque vous vous connectez au compte IQ, le panneau du lanceur de segments dans la partie supérieure vous permet de spécifier l’abonné. [segment](/help/accountiq/product-concepts.md#segment-segmet-def). Cela permet de filtrer les résultats lors de l’affichage de rapports sur le comportement et les modèles de partage des abonnés. Un segment par défaut nommé &quot;Tous les comptes&quot; dans vos propriétés est déjà sélectionné. Les options suivantes s’affichent dans le lanceur de segments :

![](assets/new-segment-selector-collapsed.png){width="800" align="left"}

*Figure : Lanceur de segments avec résumé de segment réduit*

**A** Nom de segment actuellement sélectionné<br/>
**B** Sélecteur d’intervalle et de granularité<br/>
**C** Résumé du segment réduit<br/>
**D** Option permettant d’étendre le résumé du segment<br/>
**E** Données de segment (en termes de nombre de comptes d’abonnés dans le segment pour une durée donnée)<br/>
**F** Option Ouvrir la liste de segments<br/>
**G** Option Modifier le segment<br/>
**h** Option Créer un segment<br/>

## Sélection de segment {#segment-selection}

Pour les utilisateurs programmeurs ou MVPD, accédez au **Ouvrir le segment** . Sélectionnez un segment dans la liste, puis choisissez **Ouvrir le segment** pour afficher les rapports de partage de compte.

Utilisez la variable **Oye** icône pour afficher le résumé détaillé du segment, présentant les informations sur le nombre de comptes d’abonnés et les requêtes de lecture qu’ils effectuent dans l’intervalle de temps choisi.

+++Panneau de sélection des segments pour les programmeurs/MVPD

![](assets/segment-panel-programmers-mvpds.png) {width="800" align="left"}

*Figure : Panneau Segment pour les programmeurs/MVPD*

+++

La synthèse du segment permet de définir les paramètres suivants :

**[!UICONTROL Programmers in segment]**

**[!UICONTROL Channels in segment]**

**[!UICONTROL MVPD in segment]**

**[!UICONTROL Metrics in segment]**

<!-- The definitions of these parameters will be defined in the glossary article-->

## [!UICONTROL Granularity and time interval] {#granularity-timeinterval}

La variable **[!UICONTROL Granularity and time interval]** Le sélecteur vous permet de spécifier les dates et la durée agrégées toutes les semaines/tous les mois afin d’observer le comportement de partage des comptes d’abonnés. La sélection par défaut de l’intervalle correspond à la semaine en cours, mais vous pouvez modifier la durée à l’aide des options affichées dans l’image.

![[!UICONTROL Granularity and timeinterval]](assets/granularity-timeinterval-weekwise.png){width="350" align="left"}

*Figure : Boîte de dialogue Granularité et intervalle de temps*

**A** Sélection d’une date à partir du sélecteur de date<br/>
**B** Sélectionnez la flèche vers la gauche pour reculer<br/>
**C** Sélectionner la flèche droite pour avancer<br/>
**D** Sélection de la granularité par semaine/mois<br/>
**E** Intervalle temporel sélectionné<br/>

En appliquant ces contrôles, vous pouvez définir votre instruction de problème comme &quot;abonnés du MVPD A qui ont regardé les canaux X, Y et Z au cours du mois d’octobre&quot;.

