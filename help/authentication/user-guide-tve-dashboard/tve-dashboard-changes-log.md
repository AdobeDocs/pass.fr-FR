---
title: Log des modifications
description: Savoir comment un administrateur peut surveiller les modifications de configuration dans le tableau de bord TVE.
exl-id: 9b53a61b-679f-491e-90f3-5d827e21b32c
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Log des modifications {#changes-log}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

La section **Journal des modifications** du tableau de bord TVE vous permet d’afficher les modifications de configuration transmises à l’environnement d’authentification Adobe Pass via le tableau de bord TVE. Vous pouvez également comparer deux modifications de configuration différentes.

L’onglet **Log des modifications** dans le panneau de gauche affiche une liste de toutes les modifications de configuration apportées par l’intermédiaire d’un compte spécifique du tableau de bord TVE. Cette liste de modifications contient les détails suivants :

* **Description des modifications** : brève description de la portée des modifications de configuration.
* **poussé par** : ID d’e-mail de l’utilisateur responsable de la modification.
* **Date push** : date de modification de la configuration.
* **Statut des notifications push** : indique si l’opération push a été réussie, en attente ou en échec.

## Comparer les modifications {#compare-changes}

Pour comparer les modifications, procédez comme suit :

1. Sélectionnez dans la liste deux modifications de configuration à comparer.

   ![Comparer les modifications de configuration](/help/authentication/assets/tve-dashboard/new-tve-dashboard/review/review-changes-compare-button.png)

   *Comparer les modifications de configuration*

1. Sélectionnez **Comparer** dans le coin supérieur droit de l’écran.

   La section **Modifications de configuration** affiche le type d’entité, l’ID d’entité, la propriété et le statut de l’opération de modification pour chaque modification.

1. Pointez sur la modification de configuration que vous souhaitez afficher.

1. Sélectionnez **Affichage** pour accéder aux valeurs modifiées.

   ![Afficher les modifications de configuration](/help/authentication/assets/tve-dashboard/new-tve-dashboard/review/review-changes-view-button.png)

   *Afficher les modifications de configuration*

Voici un exemple de modification apportée à la configuration sélectionnée. Vous pouvez voir la différence entre l’ancienne et la nouvelle valeur au sein de la modification.

![ancienne et nouvelle valeur](/help/authentication/assets/tve-dashboard/new-tve-dashboard/review/review-change-modal-view.png)

*ancienne et nouvelle valeur*
