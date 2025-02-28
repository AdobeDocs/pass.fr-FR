---
title: Environnements du tableau de bord TVE
description: Comprendre l’utilisation et le fonctionnement de différents environnements dans le tableau de bord TVE.
exl-id: 591becb8-2f6c-46e0-b108-c64e6df69f89
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Environnements {#environments}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Le tableau de bord TVE fournit différents environnements personnalisés à des fins spécifiques dans l’authentification Adobe Pass. Il existe deux environnements principaux :

* **Prequal** : l’environnement de préqualification sert de terrain de test pour la préparation et le test de nouvelles versions avant le déploiement en production.

* **Version** : l’environnement de version héberge les versions finalisées et testées pour la production.

Dans chaque environnement, il existe deux profils différents :

* **Staging** : le profil intermédiaire se connecte au serveur intermédiaire du MVPD pour le test et la validation des intégrations avant leur mise en ligne.

* **Production** : le profil de production se connecte au profil de production du MVPD pour les activités de production réelles.

## Cas d’utilisation

Les environnements du tableau de bord TVE présentent divers cas d’utilisation tout au long du cycle de vie de l’application. Ces environnements vous permettent d’effectuer les opérations suivantes :

### Évaluation préquate

* Validez les nouvelles fonctionnalités non publiées du serveur d’authentification Adobe Pass à l’aide des points de terminaison intermédiaires du MVPD.
* Principalement utilisé par l’équipe produit Authentification Adobe Pass pour ajouter et valider de nouvelles intégrations MVPD.

### Production prédéfinie

* Validez les nouvelles fonctionnalités ou configurations non publiées du serveur d’authentification Adobe Pass à l’aide des points de terminaison de production MVPD.
* Validation des nouvelles versions d’application pour chaque canal à l’aide des points de terminaison de production MVPD.
* Validez chaque modification de configuration avant de la mettre en production.

### Évaluation des versions

* Validez les nouvelles versions d’application pour chaque canal à l’aide des points de terminaison intermédiaires du MVPD.
* Effectuez des tests de performance ou de capacité dans cet environnement.

### Version Production

* Représente l’environnement en ligne avec la dernière version d’Adobe Pass disponible en général pour tous les utilisateurs finaux.
* Maintient la stabilité du code et de la configuration et reflète immédiatement les modifications de configuration dans l’application de l’utilisateur final.

## Changement d’environnement {#switch-environments}

Suivez les étapes pour basculer entre les environnements de tableau de bord TVE d’authentification Adobe Pass.

1. Connectez-vous à l’aide de vos informations d’identification de programmeur.

1. Sélectionnez l’environnement d’évaluation ou de production requis dans le menu déroulant **Environnement** en haut du panneau de gauche.

   ![Liste déroulante des environnements de tableau de bord TVE](../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

   *Menu déroulant de l’environnement du tableau de bord TVE d’authentification Adobe Pass*

>[!NOTE]
>
> Les configurations peuvent varier dans chaque environnement en fonction de vos paramètres.
