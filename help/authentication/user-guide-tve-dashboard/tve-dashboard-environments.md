---
title: Environnements de tableaux de bord TVE
description: Découvrez l’utilisation et le fonctionnement des différents environnements dans le tableau de bord TVE.
exl-id: 591becb8-2f6c-46e0-b108-c64e6df69f89
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Environnements {#environments}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Le tableau de bord TVE fournit différents environnements personnalisés pour répondre à des objectifs spécifiques dans l’authentification Adobe Pass. Il existe deux environnements principaux :

* **Prequal** : l’environnement de préqualification sert de terrain d’essai pour la préparation et le test de nouvelles versions avant le déploiement en production.

* **Version** : l’environnement de version héberge les versions finalisées et testées pour la production.

Dans chaque environnement, il existe deux profils différents :

* **Évaluation** : le profil d’évaluation se connecte au serveur d’évaluation MVPD pour tester et valider les intégrations avant leur mise en ligne.

* **Production** : le profil de production se connecte au profil de production MVPD pour les activités de production réelles.

## Cas d’utilisation

Les environnements du tableau de bord TVE servent divers cas d’utilisation tout au long du cycle de vie de l’application. Ces environnements vous permettent :

### Évaluation préalable

* Validez les nouvelles fonctionnalités non publiées du serveur d’authentification Adobe Pass à l’aide des points d’entrée d’évaluation MVPD.
* Principalement utilisé par l’équipe du produit Authentification Adobe Pass pour ajouter et valider de nouvelles intégrations MVPD.

### Production Préalable

* Validez les nouvelles fonctionnalités ou configurations non publiées du serveur d’authentification Adobe Pass à l’aide des points d’entrée de production MVPD.
* Validez de nouvelles versions d’application pour chaque canal à l’aide des points d’entrée de production MVPD.
* Validez chaque modification de configuration avant de la diffuser en production.

### Évaluation des versions

* Validez de nouvelles versions d’application pour chaque canal à l’aide des points d’entrée d’évaluation MVPD.
* Effectuez des tests de performance ou de capacité dans cet environnement.

### Version de production

* Représente l’environnement en ligne avec la dernière version d’Adobe Pass, disponible pour tous les utilisateurs finaux.
* Maintient la stabilité du code et de la configuration et reflète immédiatement les modifications de configuration dans l’application de l’utilisateur final.

## Changement d’environnements {#switch-environments}

Suivez les étapes pour basculer entre les environnements de tableau de bord TVE d’authentification Adobe Pass.

1. Connectez-vous avec vos informations d’identification de programmeur.

1. Sélectionnez l’environnement d’évaluation ou de production requis dans le menu déroulant **Environnement** en haut du panneau de gauche.

   ![Liste déroulante Environnements de tableau de bord TVE](../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

   *Menu déroulant Environnement de tableau de bord TVE d’authentification Adobe Pass*

>[!NOTE]
>
> Les configurations peuvent varier dans chaque environnement en fonction de vos paramètres.
