---
title: Guide de démarrage rapide du programmeur
description: Guide de démarrage rapide du programmeur
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '969'
ht-degree: 0%

---

# Guide de démarrage rapide du programmeur {#programmer-kickstart-guide}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#prog-kickstart-guide-intro}

Bienvenue dans l’authentification Adobe Pass pour TV Everywhere. Nous sommes impatients de travailler avec vous.

>[!NOTE]
>
>Il s’agit du Guide de démarrage rapide pour les programmeurs (fournisseurs de contenu). Si vous utilisez un distributeur de programmation vidéo multicanal (MVPD), veillez à consulter le guide de démarrage rapide de [MVPD](/help/authentication/kickstart/mvpd-kickstart-guide.md).


Contacts d’authentification Adobe Pass :

* Assistance - pour toutes les questions, incidents ou demandes de fonctionnalités, `tve-support@adobe.com`
* Un contact d’activation sera affecté à votre projet au moment du lancement de votre projet.

Les informations suivantes présentent quelques premières étapes importantes pour nous permettre de démarrer de manière solide et efficace. L’objectif est de fournir une explication et des attentes sur la façon dont nous travaillerons avec les partenaires pour réaliser les intégrations. Veuillez noter les sections « vous fournirez » / « l’Adobe fournira » ci-dessous pour chaque élément. Ils sont répertoriés sous la forme d’une liste de contrôle ou d’un guide tout au long du projet.

Ce document suppose que les programmeurs sont inscrits pour travailler avec un partenaire MVPD choisi.

## Calendrier des versions {#release-schedule}

Le cycle de développement du sprint d’Adobe est planifié afin que vous puissiez voir quand nos versions sont planifiées et comment chaque version est promue via notre système de développement.

Une fois chaque sprint terminé, il est disponible pour l’assouplissement quantitatif ou pour afficher de nouvelles mises en œuvre sur notre serveur UAT.

Environ toutes les 6 semaines, une version sera envoyée au serveur de préqualification d’Adobe. (Il s’agit du serveur sur lequel nous conservons la prochaine version proposée lors de l’assouplissement quantitatif final.) Ces versions incluront tous les travaux effectués dans les sprints depuis la dernière chute. Un créneau d’assouplissement quantitatif de deux semaines est actuellement disponible pour permettre aux partenaires de tester cette version.

En supposant qu’aucun problème critique ne se soit produit au cours de la période de test de deux semaines précédente, la version sera promue à la production en direct. Cela signifie que l’intégration sera disponible dans l’environnement de publication d’Adobe, mais les partenaires choisissent quand ils rendent la version publique.

<!--For the latest release schedule information, see the Release Calendar.-->

## Documentation d’assistance {#supp-doc}

L’Adobe fournira :

* Guide de déploiement : **`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* Accès à notre système de service clientèle Zendesk. C’est également là que vous trouverez des exemples, des informations et des tutoriels vidéo sur certains des processus. Pour accéder à ce document sur Zendesk, ainsi qu&#39;à d&#39;autres documents postés là-bas, vous devrez vous enregistrer et créer un compte sur `https://tve.zendesk.com/home`. Le nombre d’utilisateurs pouvant être enregistrés n’est pas limité.  Vous pouvez afficher et partager des commentaires sur n’importe quel ticket archivé. Toutes les questions d’assistance doivent être adressées à `tve-support@adobe.com`.
* Bibliothèque du vérificateur de jeton de média : `https://tve.zendesk.com/entries/471323-media-token-validator-library`.

## Configuration de l’environnement de test {#test-env-setup}

Adobe vous installera d’abord sur le site de test Adobe, où Adobe agit comme un MVPD à des fins de test. Votre équipe peut ensuite configurer un site web de test qui appelle l’API d’Adobe. Utilisez le sélecteur MVPD par défaut, puis sélectionnez « Adobe » comme IDp.

Vous fournirez :

1. ID du demandeur. Il s’agit d’une chaîne qui identifie de manière unique la marque du site web ou l’application qui effectue des requêtes à l’authentification Adobe Pass. La chaîne elle-même est arbitraire, mais doit être acceptée par l’Adobe et le programmeur
1. Informations sur le canal. Il s’agit d’un ensemble de chaînes qui identifie le ou les canaux de contenu demandés par l’ID du demandeur. Dans de nombreux cas, le canal et l’ID du demandeur sont identiques. Vous pouvez toutefois disposer de plusieurs canaux de contenu qui peuvent être demandés par le même identifiant. Les chaînes de nom de canal doivent correspondre aux chaînes de télévision par câble. Certains MVPD valident cette valeur sur le protocole AuthN et/ou AuthZ.
1. Noms de domaine (à autoriser pour cet ID de demandeur). Il s’agit d’une liste de noms de domaine réels qui seront répertoriés par Adobe pour accepter l’ID du demandeur. Cela permet de s’assurer que seuls vos domaines approuvés ont accès à l’authentification Adobe Pass avec vos métadonnées. REMARQUE : les noms de domaine valides pour la production peuvent être différents pour les tests/l’évaluation. Ils doivent tous deux être fournis et identifiés.

L’Adobe configurera le compte et l’Adobe fournira :

* Connexion et mot de passe pour accéder au site de test

## Configuration d’avec MVPD {#setup-mvpd}

Cette section décrit ce dont vous avez besoin lorsque vous migrez depuis le site de test Adobe pour travailler avec un MVPD.

Vous fournirez (via MVPD) :

* **Deux ensembles d’informations d’identification** :
   * AuthN + AuthZ : nom d’utilisateur/mot de passe pour un utilisateur authentifié et autorisé
   * AuthN + Non-AuthZ : nom d’utilisateur/mot de passe pour un utilisateur authentifié mais non autorisé
* **ID de ressource**. Il s’agit d’un identifiant de contenu spécifique qui sera validé avec un MVPD sur le protocole AuthZ. Cela peut être au niveau du canal, de l’émission, de l’épisode ou de la ressource ; cela doit être convenu avec votre MVPD.

L’authentification Adobe Pass prend en charge un schéma de métadonnées basé sur MRSS, ce qui signifie que les identifiants de ressource peuvent être aussi spécifiques que nécessaire et peuvent inclure des identifiants qui peuvent être propres à un MVPD spécifique.

**NOUVELLE intégration MVPD** : il est important de se rappeler que le MVPD que vous avez choisi fait partie intégrante de l’exécution de toute intégration. L’Adobe doit écrire le code de chaque MVPD en fonction de ses spécifications. Tant que ces étapes ne sont pas terminées, vous ne pourrez pas sélectionner ce MVPD dans la boîte de dialogue ni terminer les tests de votre produit. L’Adobe doit planifier ce travail à l’avance pour l’adapter au prochain sprint disponible. (Pour obtenir des informations sur la planification actuelle, reportez-vous au Calendrier des versions.)

**Intégrations MVPD existantes** : si le MVPD de votre choix est déjà configuré avec Adobe, les étapes de connectivité doivent être beaucoup plus simples (plus rapides) et la connectivité peut souvent être obtenue par le biais de modifications de configuration.

>[!NOTE]
>
>Le MVPD devra TOUJOURS activer le programmeur et signer toutes les transactions commerciales pertinentes.

**QE avec MVPD** : toutes les intégrations impliquent un QE conjoint. Comme l’utilisateur final est en fin de compte client du MVPD, un grand nombre d’entre eux ont défini des cycles de test avant de publier le « live ». Étant donné que cela implique la planification des ressources MVPD, il s’agit d’une zone potentiellement retardée.

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
