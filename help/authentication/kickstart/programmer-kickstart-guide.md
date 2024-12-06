---
title: Guide de démarrage rapide du programmeur
description: Guide de démarrage rapide du programmeur
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '972'
ht-degree: 0%

---

# Guide de démarrage rapide du programmeur {#programmer-kickstart-guide}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#prog-kickstart-guide-intro}

Bienvenue dans l’authentification Adobe Pass pour TV partout. Nous nous réjouissons de travailler avec vous.

>[!NOTE]
>
>Il s’agit du Guide de démarrage rapide pour les programmeurs (fournisseurs de contenu). Si vous utilisez un distributeur de programmation vidéo multicanal (MVPD), veillez à consulter le [guide de démarrage rapide MVPD](/help/authentication/kickstart/mvpd-kickstart-guide.md).


Identifiants d’authentification Adobe Pass :

* Assistance : pour toutes les questions, incidents ou demandes de fonctionnalités, `tve-support@adobe.com`
* Un contact d’activation sera affecté à votre projet au moment du lancement de celui-ci.

Les informations suivantes décrivent quelques étapes importantes pour nous permettre de commencer de manière solide et efficace. L’objectif est de fournir une explication et une attente sur la manière dont nous allons travailler avec les partenaires pour obtenir des intégrations. Veuillez noter que les sections &quot;Vous allez fournir&quot; / &quot;Adobe vous fournira&quot; ci-dessous pour chaque élément. Ils sont répertoriés au moyen d’une liste de contrôle ou d’un guide au fur et à mesure que nous travaillons sur le projet.

Ce document suppose que les programmeurs sont inscrits pour travailler avec un partenaire MVPD choisi.

## Calendrier des versions {#release-schedule}

Le cycle de développement du sprint Adobe est planifié afin que vous puissiez voir quand nos versions sont planifiées et comment chaque version est promue par le biais de notre système de développement.

Une fois chaque sprint terminé, il est disponible pour l’assouplissement quantitatif ou pour voir de nouvelles implémentations sur notre serveur UAT.

Environ toutes les 6 semaines, une version est envoyée au serveur de pré-qualification de l’Adobe. (Il s’agit du serveur sur lequel nous conservons la prochaine version proposée lors de l’exécution de l’assouplissement quantitatif final.) Ces versions incluront tous les travaux réalisés en sprints depuis la dernière goutte. Une fenêtre QE de deux semaines est actuellement disponible pour que les partenaires puissent tester cette version.

En supposant qu’aucun problème critique ne soit survenu au cours de la période de test des deux semaines précédentes, la version sera convertie en production réelle. Cela signifie que l’intégration sera disponible dans l’environnement de version d’Adobe, mais les partenaires choisissent lorsqu’ils rendent la version publique.

<!--For the latest release schedule information, see the Release Calendar.-->

## Documentation d’assistance {#supp-doc}

L’Adobe fournira :

* Guide de déploiement : **`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* Accès à notre système d’assistance clientèle Zendesk. C’est également là que vous trouverez des exemples, des informations et des tutoriels vidéo sur certains processus. Pour accéder à ce document sur Zendesk, ainsi qu’aux autres documents qui y sont publiés, vous devez vous enregistrer et créer un compte sur `https://tve.zendesk.com/home`. Le nombre d’utilisateurs que vous pouvez enregistrer n’est pas limité.  Vous pouvez afficher et partager des commentaires sur n’importe quel ticket déposé. Toutes les questions relatives à l&#39;assistance doivent être adressées à `tve-support@adobe.com`.
* [Guide d’intégration de programmeur](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)
* Bibliothèque du vérificateur de jeton multimédia : `https://tve.zendesk.com/entries/471323-media-token-validator-library`.

## Configuration de l’environnement de test {#test-env-setup}

Adobe vous configure tout d’abord avec le site de test d’Adobe, où l’Adobe agit comme MVPD à des fins de test. Votre équipe peut ensuite configurer un site web de test qui appelle l’API Adobe. Utilisez le sélecteur MVPD par défaut et sélectionnez &quot;Adobe&quot; comme idP.

Vous fournirez les éléments suivants :

1. Identifiant du demandeur. Il s’agit d’une chaîne qui identifiera de manière unique la marque du site web ou de l’application qui émet des requêtes vers l’authentification Adobe Pass. La chaîne elle-même est arbitraire mais doit être convenue entre l&#39;Adobe et le programmeur
1. Informations sur le canal. Il s’agit d’un ensemble de chaînes qui identifie le ou les canaux de contenu demandés par l’identifiant demandeur. Dans de nombreux cas, le canal et l’identifiant du demandeur sont identiques. Vous pouvez toutefois disposer de plusieurs canaux de contenu qui peuvent être demandés par le même identifiant. Les chaînes de nom de canal doivent correspondre aux chaînes de télévision par câble. Certains MVPD valideront cette valeur par rapport au protocole AuthN et/ou AuthZ.
1. Noms de domaine (à autoriser pour cet ID de demandeur). Il s’agira d’une liste de noms de domaine réels qui seront répertoriés par Adobe pour accepter l’ID du demandeur. Cela garantit que seuls les domaines approuvés ont accès à l’authentification Adobe Pass avec vos métadonnées. REMARQUE : les noms de domaine valides pour la production peuvent être différents pour le test/l’évaluation et doivent être fournis et identifiés.

Adobe configure le compte et l’Adobe fournit :

* Connexion et mot de passe pour accéder au site de test

## Configuration avec MVPD {#setup-mvpd}

Cette section décrit ce dont vous avez besoin lorsque vous migrez du site de test d’Adobe pour travailler avec un MVPD.

Vous fournirez (via MVPD) :

* **Deux jeux d’informations d’identification** :
   * AuthN + AuthZ : login/mot de passe pour un utilisateur authentifié et autorisé
   * AuthN + Non-AuthZ : login/mot de passe pour un utilisateur authentifié mais non autorisé
* **ID de ressource**. Il s’agit d’un identifiant de contenu spécifique qui sera validé avec un MVPD au-dessus du protocole AuthZ. Il peut s’agir du canal, du spectacle, de l’épisode ou de la ressource ; il doit être convenu avec votre MVPD.

L’authentification Adobe Pass prend en charge un schéma de métadonnées basé sur MRSS, ce qui signifie que les ID de ressource peuvent être aussi spécifiques que nécessaire et peuvent inclure des identifiants qui peuvent être uniques à un MVPD spécifique.

**NOUVELLE intégration MVPD** : il est important de se rappeler que le MVPD de votre choix joue un rôle essentiel dans l’achèvement de toute intégration. Adobe doit écrire du code pour chaque MVPD en fonction de leurs spécifications. Tant que ces étapes ne sont pas terminées, vous ne pourrez pas sélectionner ce MVPD dans la boîte de dialogue ni terminer vos tests de produit. Adobe doit planifier ce travail à l’avance pour l’adapter au prochain sprint disponible. (Pour plus d’informations sur le calendrier actuel, reportez-vous au Calendrier des versions.)

**Intégrations MVPD existantes** : si le MVPD de votre choix est déjà configuré avec l’Adobe, les étapes de connectivité doivent être beaucoup plus simples (plus rapides) et la connectivité peut souvent être réalisée par le biais de modifications de configuration.

>[!NOTE]
>
>Le MVPD devra TOUJOURS activer le programmeur, et signer tous les accords commerciaux pertinents.

**QE avec MVPDs** : toutes les intégrations impliquent un QE commun, et comme l’utilisateur final est finalement client du MVPD, beaucoup ont défini des cycles de test avant de passer à &quot;live&quot;. Comme cela implique la planification des ressources MVPD, il peut y avoir des retards.

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
