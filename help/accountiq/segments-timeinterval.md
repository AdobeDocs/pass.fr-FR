---
title: Segments et intervalle de temps
description: Définissez des cohortes ou sélectionnez des segments d’abonnés pour évaluer les possibilités et les modèles de partage de compte des visionneuses de canaux afin d’utiliser des outils graphiques et des rapports dans le compte IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# Segments et intervalle de temps {#segment-timeinterval}

Lorsque vous vous connectez au compte IQ, le panneau de segments et d’intervalle de temps situé au-dessus du tableau de bord vous permet de définir l’abonné. [segment](product-concepts.md#segmet-def). Ce panneau permet de filtrer les résultats et d’afficher des rapports sur le comportement et les modèles de partage des abonnés. Un segment nommé **TOUS LES COMPTES DE VOS PROPRIÉTÉS** est actuellement sélectionné par défaut, où vous pouvez afficher les options suivantes :

![](assets/new-segment-selector-collapsed.png){align="left"}

*Panneau Segment et intervalle de temps avec résumé de segment réduit*

**A.** Nom de segment actuellement sélectionné **B.** Ouvrir la liste de segments **C.** Modifier le segment **D.** Créer un segment **E.** Sélecteur de granularité et d’intervalle de temps **F.** Icône permettant de développer le résumé du segment **G.** Résumé du segment réduit **H.** Nombre de comptes dans le segment pour l’intervalle sélectionné

>[!NOTE]
>
> Le résumé du segment réduit affiche la variable [Catégories de vidéos](product-concepts.md#video-category-def) utilisé dans la version TV Everywhere de Account IQ. Si vous êtes connecté en tant que service D2C, ces étiquettes affichent les catégories vidéo spécifiques de votre entreprise.

En savoir plus sur [comment créer](work-with-segments.md#create-new-segment) et [gestion des segments](work-with-segments.md#manage-segment) de la **Segments** dans le panneau de gauche.

## Sélection de segment {#segment-selection}

Pour sélectionner un segment spécifique, procédez comme suit :

1. Accédez au **[!UICONTROL Open segment]** dans le panneau segment et intervalle de temps.
1. Sélectionnez la variable **Nom du segment** pour laquelle vous souhaitez afficher les rapports de partage de compte.

   ![](assets/open-segment.png){align="left"}

   *Sélectionner le nom du segment*

   >[!NOTE]
   >
   > Les catégories vidéo affichées dans l’image précédente, telles que **MVPD**, **Programmeurs**, et **Canaux** représentent les étiquettes utilisées dans la version TV Everywhere de Account IQ. Si vous êtes connecté en tant que service D2C, ces étiquettes affichent les catégories vidéo spécifiques de votre entreprise.

1. Sélectionner **[!UICONTROL Open segment]**.


## Sélection de la granularité et de l’intervalle de temps {#granularity-timeinterval}

La variable **Granularité et intervalle de temps** Le sélecteur vous permet de spécifier les dates et la durée agrégées toutes les semaines/tous les mois afin d’observer le comportement de partage des abonnés. La sélection par défaut correspond à la semaine en cours.

![Granularité et intervalle de temps](assets/granularity-timeinterval-weekwise.png){align="left"}

*Boîte de dialogue Granularité et intervalle de temps*

**A.** Sélecteur de granularité et d’intervalle de temps **B.** Flèche vers la droite pour accéder au mois/semaine suivant **C.** Option permettant de choisir la granularité par semaine/mois **D.** Intervalle de temps actuellement sélectionné **E.** Flèche vers la gauche pour accéder au mois/à la semaine précédent

Vous pouvez modifier la durée en procédant comme suit :

1. Sélectionnez la variable **[!UICONTROL Granularity and Time Interval]** du sélecteur de date.

1. Sélectionnez **[!UICONTROL Week]** ou **[!UICONTROL Month]** de **[!UICONTROL Aggregate By]** pour définir la granularité de votre évaluation.

1. Une fois la granularité sélectionnée, vous pouvez utiliser les flèches vers l’avant ou vers l’arrière pour naviguer dans la période.

1. Sélectionnez une période spécifique pour l’évaluation.

1. Sélectionner **[!UICONTROL Apply]** pour vous assurer que votre sélection prend effet.

Cela vous permet de définir votre énoncé de problème comme &quot;Abonnés de MVPD A qui ont regardé les canaux X, Y et Z pendant la semaine choisie de décembre&quot;.

## Synthèse des segments {#segment-summary}

Le résumé de segment est similaire pour les services D2C et TV Everywhere. Les catégories vidéo seront différentes pour chaque version respective de Account IQ.

Sélectionner <img alt= "développer le résumé du segment" src="./assets/expand-segment-summary.svg" width="25"> pour afficher le résumé détaillé du segment. Il présente également des informations sur le nombre de comptes abonnés et leurs demandes de lecture au cours de la période sélectionnée.

+++ Services D2C

![](assets/segment-panel-d2c.png){align="left"}

*Résumé des segments pour les services D2C*

>[!NOTE]
>
>La variable [catégories vidéo](product-concepts.md#video-category-def) affiché dans l’image précédente, comme **region** et **types de contenu** dans ne sont que des exemples. Lorsque vous vous connectez au compte IQ, ces étiquettes affichent les catégories vidéo spécifiques de votre société.

La variable **Résumé du segment** inclut les conditions suivantes qui définissent un segment :

**[Régions et types de contenu](product-concepts.md#video-category-def) dans le segment** reportez-vous aux étiquettes de métadonnées associées aux diffusions vidéo regardées par les comptes partagés représentés dans les rapports de partage de compte.

**[Mesures](product-concepts.md#metric) dans le segment** se rapportent aux attributs ou aux critères que les abonnés doivent avoir remplis pour être identifiés dans les rapports de partage de compte.

+++

+++ TV partout

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*Résumé du segment pour les programmeurs/MVPD*

La variable **Résumé du segment** inclut les conditions suivantes qui définissent un segment :

**[Programmeurs](product-concepts.md#programmer-def) dans le segment**  se rapportent aux fournisseurs de contenu dont les diffusions vidéo ont été visionnées par des comptes partagés représentés dans les rapports de partage de compte.

**[Canaux](product-concepts.md#channel-def) dans le segment** se rapportent aux canaux dont les diffusions vidéo ont été visionnées par des comptes partagés représentés dans les rapports de partage de compte.

**[MVPD](product-concepts.md#mvpd-def) dans le segment** se rapportent aux distributeurs de programmation multividéo auxquels sont associés les abonnés afin d’être identifiés dans les rapports de partage de compte.

**[Mesures](product-concepts.md#metric) dans le segment** se rapportent aux attributs ou aux critères que les abonnés doivent avoir remplis pour être identifiés dans les rapports de partage de compte.

+++
