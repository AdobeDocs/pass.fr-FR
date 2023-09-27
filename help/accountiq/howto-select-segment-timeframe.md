---
title: Définition d’un segment et d’une période
description: Définition d’un segment et d’une période
exl-id: 86fe010d-3202-4ce2-b803-ff44f5538d7e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---

# Définition d’un segment et d’une période {#define-segment}

Tous les rapports d’analyse ou d’affichage dans [!UICONTROL Account IQ] commencez par définir le segment et sélectionner la période à évaluer. [Segment](/help/accountiq/product-concepts.md#segmet-def) fait référence à tous les abonnés ou visiteurs qui répondent à vos critères d’évaluation (abonnement à un programme multivarié et affichage de canaux spécifiques).

![](assets/segment-panel.png)

*Figure : Sélection de segments et de périodes*

En haut de toutes les pages de rapports dans [!UICONTROL Account IQ], il existe un panneau pour définir le segment en sélectionnant les programmes de développement multivarié, les programmeurs de canaux, la granularité et la période.

## Sélection de segment {#select-segment}

### Sélectionner les MVPD dans le segment {#select-segment-mvpds}

Pour sélectionner des MVPD depuis **[!UICONTROL MVPDs in segment]** option :

1. Cliquez ou appuyez sur **[!UICONTROL MVPDs in segment]** de la liste déroulante.

   >[!NOTE]
   >
   >**Tous** les MVPD de secteur sont sélectionnés par défaut. À partir de là, vous pouvez sélectionner l’un des **Les 10 meilleurs MVPD en partageant le score**, **Les 10 principaux distributeurs multicanaux de programmes audiovisuels par utilisation**, **Les 10 principaux distributeurs multicanaux de programmes audiovisuels par compte** ou MVPD individuels. Toutefois, pour sélectionner des MVPD individuels, vous devez les désélectionner. **Tous**.

1. Cliquez ou appuyez sur les MVPD souhaités.

   Vous pouvez supprimer un MVPD de la sélection en le désélectionnant.

1. Cliquez ou appuyez sur **[!UICONTROL Apply selection]** pour que votre sélection prenne effet. Sinon, vous perdrez la sélection que vous avez effectuée.

   >[!NOTE]
   >
   >Si vous sélectionnez le mode Isolation, aucun autre MVPD ne peut être sélectionné.

### Sélection de canaux dans un segment {#select-segment-channels}

Pour sélectionner les canaux de programmeur de votre choix dans la **[!UICONTROL Channels in segment]** option :

1. Cliquez ou appuyez sur **[!UICONTROL Channels in segment]** de la liste déroulante.

   >[!NOTE]
   >
   >**Tous** les canaux de programmation de votre entreprise sont sélectionnés par défaut. Pour sélectionner des canaux ou des programmeurs individuels, vous devez d’abord les désélectionner. **Tous**.

1. Cliquez ou appuyez sur les canaux ou les programmeurs de votre choix.

   Les éléments de liste de niveau supérieur dans la variable **[!UICONTROL Channels in segment]** are [programmeur](/help/accountiq/product-concepts.md#programmer-def) les sociétés et les éléments de liste sous les noms de programmeur sont leurs [channels](/help/accountiq/product-concepts.md#channel-def). Vous pouvez sélectionner des canaux individuels sous des programmeurs ou sélectionner des programmeurs et toutes les activités des canaux sous ce programmeur sont incluses dans les résultats des rapports et graphiques.

   ![](assets/programmer-channels.png)


   *Figure : Programmeurs et canaux répertoriés dans le sélecteur de canaux*

   >[!IMPORTANT]
   >
   >Les résultats de la sélection de canaux individuels sous un programmeur ne sont pas identiques à ceux de la sélection du programmeur.
   >
   >
   >Lorsque vous sélectionnez des canaux individuels, les activités de ces canaux sont ventilées individuellement dans certains rapports. Cependant, lorsque vous sélectionnez le programmeur parent de tous ces canaux, toutes les activités de ces canaux sont incluses, mais ne sont pas ventilées individuellement dans les rapports.

1. Cliquez ou appuyez sur **[!UICONTROL Apply selection]** pour que votre sélection prenne effet.

>[!NOTE]
>
>Vous ne pouvez pas sélectionner plus de 10 éléments dans les menus déroulants du MVPD ou du programmeur.

### Désélectionner les MVPD et les canaux {#deselect-segment-mvpds-channels}

En plus de modifier votre sélection dans la variable **[!UICONTROL MVPDs in segment]** et **[!UICONTROL Channels in segment]** sélecteurs de segments, vous pouvez désélectionner les MVPD et canaux sélectionnés précédemment en procédant comme suit :

* En sélectionnant le **[!UICONTROL Remove]** icône (![icône de suppression](assets/remove-icon.png)) sur les noms de ces MVPD et canaux sélectionnés affichés sous le sélecteur de segments.

* Vous pouvez également utiliser **[!UICONTROL Clear Selection]** pour supprimer tous les MVPD ou canaux sélectionnés précédemment.

![](assets/segment-panel-selection.png)

*Figure : MVPD et canaux sélectionnés dans le panneau de segments et de périodes*

## Granularité et sélection d’une période {#granularity-timeframe}

Pour sélectionner une période d’évaluation :

1. Sélectionnez la variable **[!UICONTROL Granularity and time frame]** Sélecteur de date.

1. Sélectionnez **[!UICONTROL Week]** ou **[!UICONTROL Month]** de **[!UICONTROL Aggregate By]** pour définir la granularité de votre évaluation.

   ![](assets/granularity-timeframe-weekwise.png)


   *Figure : Sélecteur de date pour sélectionner la granularité et la période*

1. Une fois la granularité sélectionnée, vous pouvez utiliser les flèches vers l’avant ou vers l’arrière pour avancer ou reculer dans le temps.

1. Spécifiez une période dans le passé (en mois ou en semaine selon la granularité sélectionnée) pour l’évaluation.

1. Sélectionner **[!UICONTROL Apply Selection]** pour vous assurer que votre sélection prend effet.
