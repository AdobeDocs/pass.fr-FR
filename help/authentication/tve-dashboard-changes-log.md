---
title: Journal des modifications
description: Découvrez comment un administrateur peut surveiller les modifications de configuration dans le tableau de bord TVE.
source-git-commit: 06c2e1e54515a2ec47722ba1f360467dadd1f73b
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---


# Journal des modifications {#changes-log}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

La variable **Journal des modifications** La section du tableau de bord TVE vous permet d’afficher les modifications de configuration transmises à l’environnement d’authentification Adobe Pass par le biais du tableau de bord TVE. Vous pouvez également comparer deux modifications de configuration différentes.

La variable **Journal des modifications** dans le panneau de gauche affiche la liste de toutes les modifications de configuration effectuées via un compte spécifique du tableau de bord TVE. Cette liste de modifications contient les détails suivants :

* **Modification de la description**: brève description de l’étendue du changement de configuration.
* **Poussé par**: ID de courrier électronique de l’utilisateur responsable de la modification.
* **Date push**: date de la modification de la configuration.
* **État push**: indique si l’opération push a réussi, en attente ou échoué.

## Comparaison des modifications {#compare-changes}

Pour comparer des modifications, procédez comme suit :

1. Sélectionnez deux modifications de configuration dans la liste que vous souhaitez comparer.

   ![Comparaison des modifications de configuration](assets/select-changes.png)

   *Comparaison des modifications de configuration*

1. Sélectionner **Comparer** dans le coin supérieur droit de l’écran.

   La variable **Modifications de configuration** affiche le type d’entité, l’ID d’entité, la propriété et l’état de l’opération de modification pour chaque modification.

1. Pointez sur la modification de configuration que vous souhaitez afficher.
1. Sélectionner **Affichage** pour accéder aux valeurs modifiées.

   ![Afficher les modifications de configuration](assets/view-changes.png)

   *Afficher les modifications de configuration*

Voici un exemple de modification apportée à la configuration sélectionnée. Vous pouvez afficher la différence entre les anciennes et les nouvelles valeurs dans la modification.

![Ancienne et nouvelle valeur](assets/change.png)

*Ancienne et nouvelle valeur*


