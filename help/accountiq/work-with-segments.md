---
title: Utilisation des segments
description: Comprendre et utiliser les segments. Découvrez comment créer et gérer un segment.
exl-id: 01431437-55f5-464d-8ee4-7a79ec553e4f
source-git-commit: 38402b411dd4fad3490115cff1854faa7f3cb293
workflow-type: tm+mt
source-wordcount: '1970'
ht-degree: 0%

---

# Utilisation des segments {#work-with-segments}

[Les segments](product-concepts.md#segmet-def) sont un ensemble de comptes d’abonnés qui vous permettent d’analyser le partage des informations d’identification dans des conditions définies par l’utilisateur. Vous pouvez utiliser des segments pour examiner différents ensembles de comptes d’abonnés et générer des rapports de données correspondants dans des tableaux et des graphiques. Il existe deux types de segments dans Account IQ :

1. **Segment par défaut** : **Tous les comptes de vos propriétés** est un segment prêt à l’emploi dans le système qui inclut tous les comptes d’abonnés actifs sans conditions spécifiques appliquées.

   >[!NOTE]
   >
   >L’utilisation du segment par défaut peut empêcher l’affichage de certaines tables telles que [Catégories vidéo dans le segment](data-panels.md#video-categories-segment), [Score de partage par canaux et MVPD](data-panels.md#sharin-score-by-channels-and-mvpds) et [Répartition du modèle d’utilisation pour les catégories vidéo](usage-patterns.md#usage-pattern-dis-video-categories). Ces tableaux peuvent uniquement contenir et afficher des données pour 20 lignes au maximum à la fois. Les tableaux, graphiques et rapports restants sont identiques pour les segments par défaut et personnalisés.

1. **Segments personnalisés** : il s’agit de segments personnalisés qui vous permettent de regrouper des comptes d’abonnés de catégories spécifiques, telles que des types de contenu D2C, des programmeurs, des canaux et des distributeurs multicanaux pour analyser le partage des informations d’identification dans des conditions définies par l’utilisateur. Découvrez comment [créer un segment personnalisé](#create-new-segment).

   >[!IMPORTANT]
   >
   >Toutes les procédures décrites dans ce guide sont basées sur des segments personnalisés. Toutefois, les concepts restent les mêmes pour les segments par défaut et personnalisés.

Lorsque vous accédez à l’onglet **Actions** et sélectionnez l’onglet **[!UICONTROL Segments]** dans le panneau de gauche, une liste des segments disponibles dans le système s’affiche. La page de segments vous permet d’évaluer rapidement les détails clés de chaque segment sous la forme d’un tableau. Les détails incluent le nom du segment, le nombre de [catégories de vidéos](product-concepts.md#video-category-def), les mesures, [opérations](product-concepts.md#operation-def) à l’aide du segment actuel, la date et l’heure de dernière modification, ainsi que le nom du créateur du segment.

Vous pouvez exécuter les fonctions suivantes avec les segments :

* [Création d’un segment](#create-new-segment)
* [Gestion des segments](#manage-segments)


## Création d’un segment {#create-new-segment}

Le processus de création d’un nouveau segment est similaire pour les services D2C et TV Everywhere. Les catégories vidéo seront différentes pour chaque version respective d’Account IQ.

+++Services D2C

Pour créer un segment et analyser le comportement de partage des abonnés, sélectionnez **[!UICONTROL Create new segment]** dans l’angle supérieur droit.

![Sélectionner Créer un segment](assets/create-new-segment-d2c.png)

*Sélectionner Créer un segment*

>[!NOTE]
>
>Les catégories vidéo affichées dans l’image précédente, telles que **Régions** et **Types de contenu**, ne sont que des exemples. Lorsque vous vous connectez à Account IQ, ces étiquettes affichent les catégories vidéo spécifiques à votre entreprise.

Elle ouvre une page **Nouveau segment**, qui comprend les éléments suivants :

![Nouvelle page de segment](assets/d2c-new-segment-dialog.png)

*Nouvelle page de segment*

**A.** Composants de segment **B.** Définition de segment **C.** Synthèse de segment

* **Composants de segment** : inventaire des [catégories vidéo](product-concepts.md##video-category-def) et des mesures calculées utilisées pour définir un segment.

  >[!NOTE]
  >
  >Utilisez **[!UICONTROL Show all]** pour développer la liste des composants de segment. Pour trouver rapidement un composant, recherchez son nom dans **composants de segment de recherche** plutôt que de parcourir la liste entière.

* **Définition de segment** : un canevas dans lequel vous pouvez faire glisser et déposer divers composants de segment pour créer un segment.

* **Synthèse du segment** : résumé qui évalue les comptes qualifiés en fonction des composants dans la définition de segment et fournit un bref aperçu du segment pendant la période d’évaluation.

Pour créer un segment, procédez comme suit :

1. Saisissez le nom de votre segment dans **Nom du segment** qui sera visible dans la liste des segments et lors de la sélection du segment.
1. Saisissez une description détaillée de votre segment dans **Description du segment**.
1. Par exemple, faites glisser **Régions et types de contenu** depuis les composants de segment sur le panneau de gauche et déposez-les dans la section **Régions/types de contenu** dans la **Définition de segment**.

   >[!NOTE]
   >
   >Vous pouvez créer un segment en fonction des régions ou des types de contenu. Affichez les types de contenu associés d’une région à partir d’un menu déroulant.

   Si vous commencez par ajouter un **type de contenu** dans la section **Régions/Types de contenu** , vous pouvez uniquement ajouter des types de contenu en tant que composants suivants.

   Si vous commencez par ajouter une **Région** dans la section **Régions/Types de contenu**, une boîte de dialogue de décision s’affiche.

   ![Ajouter un composant de segment en tant que région ou ses types de contenu ](assets/d2c-segment-basis-selector.png){width="550" align="left"}

   *Ajouter un composant de segment comme région ou sa boîte de dialogue de types de contenu*

   Choisissez si vous souhaitez comparer des régions spécifiques ou un segment en fonction des types de contenu associés à une région.

   Sélectionnez **[!UICONTROL As a region]** pour ajouter des régions à la section **Régions/Types de contenu** .

   Sélectionnez **[!UICONTROL As its content types]** pour ajouter les types de contenu d’une région.

1. Faites glisser **Mesures** depuis les composants de segment sur le panneau de gauche et déposez-les dans la section **Mesures** de la **définition de segment**.

   ![Sélectionnez un opérateur et définissez une valeur pour la mesure ajoutée](assets/component-metrics.png)

   *Sélectionnez un opérateur et affectez une valeur pour la mesure ajoutée*

   Après avoir ajouté des mesures dans la définition de segment, sélectionnez un opérateur dans le menu déroulant **[!UICONTROL Select an operator]** et affectez une valeur à l’aide de **[!UICONTROL Select an option]**.

   Ajustez les valeurs de certaines mesures à l’aide de la flèche vers le haut pour augmenter et de la flèche vers le bas pour diminuer.

1. Faites glisser **Mesures calculées** depuis les composants de segment sur le panneau de gauche et déposez-les dans la section **Mesures calculées** de la **définition de segment**.

   ![Sélectionnez un opérateur et définissez une valeur pour la mesure calculée ajoutée](assets/component-calculated-metrics.png)

   *Sélectionnez un opérateur et affectez une valeur pour la mesure calculée ajoutée*

   Après avoir ajouté des mesures calculées dans la définition de segment, **[!UICONTROL Select an operator]** dans le menu déroulant et attribuer une valeur à l’aide de **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Toutes les mesures et mesures calculées que vous déposez sous la définition de segment sont accompagnées des opérateurs appropriés pour affecter des valeurs aux mesures et mesures calculées respectives.

1. Passez en revue les détails du segment dans le **résumé du segment** pour décider des modifications que vous souhaitez mettre en oeuvre dans le segment.
1. Sélectionnez **[!UICONTROL Last week]** ou **[!UICONTROL Last month]** dans le menu déroulant **Période d’évaluation** pour estimer les valeurs de résumé pour la semaine ou le mois écoulé.
1. Sélectionnez **[!UICONTROL Update estimation]** pour calculer le nombre estimé de comptes qualifiés dans le segment actuel en fonction de la période d’évaluation sélectionnée.
1. Sélectionnez **[!UICONTROL Save segment]**.

Le segment que vous avez créé est désormais disponible dans la liste des segments.

+++

+++TV partout

Pour créer un segment et analyser le comportement de partage des abonnés, sélectionnez **[!UICONTROL Create new segment]** dans l’angle supérieur droit.

![Sélectionner Créer un segment](assets/create-new-segment.png)

*Sélectionner Créer un segment*

Elle ouvre une page **Nouveau segment**, qui comprend les éléments suivants :

![Nouvelle page de segment](assets/new-segment-dialog.png)

*Nouvelle page de segment*

**A.** Composants de segment **B.** Définition de segment **C.** Synthèse de segment

* **Composants de segment** : inventaire des programmeurs et des canaux, des distributeurs multicanaux de programmes, des mesures et des mesures calculées utilisés pour définir un segment.

  >[!NOTE]
  >
  >Utilisez **[!UICONTROL Show all]** pour développer la liste des composants de segment. Pour trouver rapidement un composant, recherchez son nom dans **composants de segment de recherche** plutôt que de parcourir la liste entière.

* **Définition de segment** : un canevas dans lequel vous pouvez faire glisser et déposer divers composants de segment pour créer un segment.

* **Synthèse du segment** : résumé qui évalue les comptes qualifiés en fonction des composants dans la définition de segment et fournit un bref aperçu du segment pendant la période d’évaluation.

Pour créer un segment, procédez comme suit :

1. Saisissez le nom de votre segment dans **Nom du segment** qui sera visible dans la liste des segments et lors de la sélection du segment.
1. Saisissez une description détaillée de votre segment dans **Description du segment**.
1. Faites glisser **Programmeurs et Canaux** à partir des composants de segment sur le panneau de gauche et déposez-les dans la section **Programmers/Canaux** de la **définition de segment**.

   >[!NOTE]
   >
   >Vous pouvez créer un segment basé sur des programmeurs ou des canaux. Affichez les canaux associés avec un programmeur à partir d’un menu déroulant.

   Si vous commencez par ajouter un **Canal** dans la section **Programmers/Canaux** , vous pouvez uniquement ajouter des canaux en tant que composants suivants.

   Si vous commencez par ajouter un **programmeur** dans la section **Programmeurs/Canaux** , une boîte de dialogue de décision s’affiche.

   ![Ajouter un composant de segment en tant que programmeur ou ses canaux ](assets/segment-basis-selector.png){width="550" align="left"}


   *Ajouter un composant de segment en tant que programmeur ou sa boîte de dialogue de canaux*

   Choisissez si vous souhaitez comparer des programmeurs spécifiques ou un segment en fonction des canaux associés à un programmeur.

   Sélectionnez **[!UICONTROL As a programmer]** pour ajouter des programmeurs à la section **Programmers/Channels** .

   Sélectionnez **[!UICONTROL As its channels]** pour ajouter tous les canaux d’un programmeur.

1. Faites glisser **MVPDs** depuis les composants de segment sur le panneau de gauche et déposez-les dans la section **MVPDs** dans la **définition de segment**.

   >[!NOTE]
   >
   >Lorsque vous vous connectez en tant que programmeur, un MVPD nommé **xfinity** apparaît comme option autonome dans la section **MVPDs** . Vous ne pouvez pas le combiner avec un autre MVPD.

1. Faites glisser **Mesures** depuis les composants de segment sur le panneau de gauche et déposez-les dans la section **Mesures** de la **définition de segment**.

   ![Sélectionnez un opérateur et définissez une valeur pour la mesure ajoutée](assets/component-metrics.png)

   *Sélectionnez un opérateur et affectez une valeur pour la mesure ajoutée*

   Après avoir ajouté des mesures dans la définition de segment, sélectionnez un opérateur dans le menu déroulant **[!UICONTROL Select an operator]** et affectez une valeur à l’aide de **[!UICONTROL Select an option]**.

   Ajustez les valeurs de certaines mesures à l’aide de la flèche vers le haut pour augmenter et de la flèche vers le bas pour diminuer.

1. Faites glisser **Mesures calculées** depuis les composants de segment sur le panneau de gauche et déposez-les dans la section **Mesures calculées** de la **définition de segment**.

   ![Sélectionnez un opérateur et définissez une valeur pour la mesure calculée ajoutée](assets/component-calculated-metrics.png)

   *Sélectionnez un opérateur et affectez une valeur pour la mesure calculée ajoutée*

   Après avoir ajouté des mesures calculées dans la définition de segment, **[!UICONTROL Select an operator]** dans le menu déroulant et attribuer une valeur à l’aide de **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Toutes les mesures et mesures calculées que vous déposez sous la définition de segment sont accompagnées des opérateurs appropriés pour affecter des valeurs aux mesures et mesures calculées respectives.

1. Passez en revue les détails du segment dans le **résumé du segment** pour décider des modifications que vous souhaitez mettre en oeuvre dans le segment.
1. Sélectionnez **[!UICONTROL Last week]** ou **[!UICONTROL Last month]** dans le menu déroulant **Période d’évaluation** pour estimer les valeurs de résumé pour la semaine ou le mois écoulé.
1. Sélectionnez **[!UICONTROL Update estimation]** pour calculer le nombre estimé de comptes qualifiés dans le segment actuel en fonction de la période d’évaluation sélectionnée.
1. Sélectionnez **[!UICONTROL Save segment]**.

Le segment que vous avez créé est désormais disponible dans la liste des segments.
+++

## Gestion des segments {#manage-segments}

Vous pouvez sélectionner un segment dans la liste de segments, puis effectuer les actions suivantes :

* [Modification d’un segment](#edit-segment)
* [Duplication d’un segment](#duplicate-segment)
* [Suppression d’un segment](#delete-segment)

![Modifier, dupliquer ou supprimer un segment](assets/manage-segments-list.png)

*Sélectionner un segment à modifier, dupliquer ou supprimer*

**A.** [Segment par défaut](#work-with-segments) **B.** [Catégories vidéo](product-concepts.md#video-category-def)

>[!NOTE]
>
>Les catégories vidéo affichées dans cette section, telles que **MVPDs**, **Programmers** et **Canaux** , représentent les étiquettes utilisées dans la version TV Everywhere d’Account IQ. Si vous êtes connecté en tant que service D2C, ces étiquettes affichent les catégories vidéo spécifiques de votre entreprise.

Vous ne pouvez pas modifier, dupliquer ou supprimer le segment par défaut nommé **Tous les comptes dans vos propriétés**.

### Modification d’un segment {#edit-segment}

1. Accédez à l’onglet **[!UICONTROL Segments]** sous **Actions** dans le panneau de gauche pour afficher une liste de segments.
1. Sélectionnez un segment à modifier.
1. Sélectionnez **[!UICONTROL Edit]**.
1. Modifiez les détails du segment, tels que le nom, la description ou les composants du **Segment definition**.

   >[!TIP]
   >
   >Utilisez **[!UICONTROL Clear all]** pour supprimer simultanément tous les composants de segment dans chaque section sous la définition de segment. Vous pouvez également sélectionner le bouton croisé pour supprimer des éléments individuels.

   ![Effacer tous les composants de segment dans chaque section sous la définition de segment ](assets/clear-all-components.png)

   *Sélectionnez Effacer tout pour supprimer tous les composants de segment à la fois*

1. Sélectionnez **[!UICONTROL Update segment]** pour mettre à jour le segment existant ou **[!UICONTROL Save as new segment]** pour créer un nouveau segment avec les modifications.

   >[!NOTE]
   >
   >La mise à jour des segments en cours d’exploitation n’est pas autorisée. L’enregistrement des modifications en tant que nouveau segment est la seule option pour les segments avec des opérations en cours.

### Duplication d’un segment {#duplicate-segment}

1. Accédez à l’onglet **[!UICONTROL Segments]** sous **Actions** dans le panneau de gauche pour afficher une liste de segments.
1. Sélectionnez un segment que vous souhaitez dupliquer.
1. Sélectionnez **[!UICONTROL Duplicate]**.

Une copie du segment sélectionné est générée et placée à la fin de la liste de segments. Vous pouvez modifier les détails nécessaires dans le segment dupliqué, puis mettre à jour le segment dupliqué ou l’enregistrer en tant que nouveau segment.

### Suppression d’un segment {#delete-segment}

1. Accédez à l’onglet **[!UICONTROL Segments]** sous **Actions** dans le panneau de gauche pour afficher une liste de segments.
1. Sélectionnez le segment à supprimer.

   Sélectionnez plusieurs segments pour les supprimer en une seule opération. Vous pouvez également cocher une case à gauche du **nom du segment** pour supprimer tous les segments à la fois.

   >[!NOTE]
   >
   > Vous ne pouvez supprimer que plusieurs segments ou tous les segments si aucun des segments n’est utilisé par les opérations. De plus, la suppression du segment par défaut nommé **Tous les comptes dans vos propriétés** n’est pas autorisée. Elle reste non sélectionnée lorsque vous tentez de supprimer tous les segments à la fois.

   ![Suppression de plusieurs segments](assets/delete-more-than-one-segment.png)

   *Sélectionner plusieurs segments pour supprimer plusieurs segments*

1. Sélectionnez **[!UICONTROL Delete]**.
1. Confirmez en **[!UICONTROL Delete]** dans la boîte de dialogue pour supprimer définitivement le segment.

   >[!NOTE]
   >
   >Le segment est définitivement supprimé du système et vous ne pouvez pas annuler cette action.
