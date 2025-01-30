---
title: Guide de démarrage rapide du programmeur
description: Guide de démarrage rapide du programmeur
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 37858fa83aecbdf443a4a6058c78e4f9246eee42
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Guide de démarrage rapide du programmeur {#programmer-kickstart-guide}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Ce guide de démarrage rapide est destiné aux fournisseurs de contenu (programmeurs) qui prévoient d’intégrer l’authentification Adobe ® Pass à leurs sites web ou applications.

Ce document décrit les principales étapes initiales pour garantir un démarrage fluide et efficace du processus d’intégration. Il vise à clarifier les attentes et à fournir des conseils sur la façon dont nous collaborerons avec les partenaires pour réussir les intégrations.

Adobe fournit diverses ressources pour vous aider à intégrer l’authentification Adobe Pass à votre site web ou application. Veuillez vous reporter aux mentions **« Vous fournirez »** et **« L’Adobe fournira »** de chaque section ci-dessous.

## Processus de configuration {#setup-process}

Le processus de configuration comprend entre autres les étapes suivantes :

![Processus D&#39;Intégration De L&#39;Authentification Adobe ® Pass](../assets/progr-flow-int-lifecycle.png)

*Processus D&#39;Intégration De L&#39;Authentification Adobe ® Pass*

**Vous fournirez** au cours de la phase de lancement :

* **Prestataire (identifiant du demandeur)**

  Il s’agit d’une chaîne qui identifie de manière unique la marque du site web ou l’application qui effectue des requêtes à l’authentification Adobe Pass. La chaîne elle-même est arbitraire, mais doit être acceptée par l’Adobe et le programmeur

* **Informations sur le canal**

  Il s’agit d’un ensemble de chaînes utilisées pour identifier le ou les canaux de contenu demandés par le fournisseur d’accès. Dans de nombreux cas, le canal et le fournisseur de services sont identiques. Cependant, un identifiant unique peut représenter plusieurs canaux de contenu. Ces chaînes de nom de canal doivent s’aligner sur les chaînes de télévision par câble correspondantes. Notez que certains MVPD peuvent valider cette valeur pendant le processus d’authentification et/ou d’autorisation.

* **Noms de domaine**

  Cette liste inclut les noms de domaine réels répertoriés à l’Adobe pour représenter le fournisseur de services. Cela garantit que seuls vos domaines autorisés peuvent accéder à l’authentification Adobe Pass à l’aide de vos métadonnées. Veillez à fournir et à identifier clairement les noms de domaine pour les environnements de production et d’évaluation (tests), car ils peuvent différer.

**Vous fournirez** via MVPD :

* **Jeux d’informations d’identification**

  Il s’agit d’informations d’identification utilisées pour authentifier et autoriser, ou uniquement authentifier, l’utilisateur à l’aide du MVPD. En règle générale, ces informations d’identification se composent d’un nom d’utilisateur et d’un mot de passe que le MVPD vous fournira pour les deux profils (évaluation et production).

* **Identifiants des ressources**

  Il s’agit d’identifiants uniques pour les canaux de contenu, les programmes, les épisodes ou les ressources que le fournisseur de services souhaite protéger. Ces identifiants sont utilisés pour demander des décisions d’autorisation et doivent être convenus avec le MVPD.

>[!IMPORTANT]
>
> Le programmeur est chargé de se coordonner avec le MVPD pour finaliser les accords commerciaux nécessaires. Dans le même temps, l’authentification Adobe Pass collaborera avec MVPD pour s’assurer que l’intégration technique est correctement établie :
>
> * **Nouveau MVPD**
>
>     Si le MVPD n’est pas intégré à Adobe, le code personnalisé doit être développé en fonction des exigences spécifiques à MVPD. Tant que ce développement ne sera pas terminé, le MVPD ne sera pas disponible et les tests de produit avec ce MVPD ne pourront pas continuer.
>
> * **MVPD existant**
>
>     Si le MVPD est déjà intégré à Adobe, le processus de connectivité est considérablement rationalisé. Dans la plupart des cas, la connectivité peut être établie rapidement par des ajustements de configuration plutôt que par un développement étendu.
>
> Toutes les intégrations nécessitent des efforts conjoints d’assurance qualité (QA), y compris des tests MVPD, car l’utilisateur final est finalement un client de MVPD. La coordination des cycles de test dépend souvent de la disponibilité des ressources de MVPD, ce qui peut entraîner des retards potentiels.

## Accès au service clientèle {#access-customer-support}

**Adobe fournira** l&#39;accès à notre système de service clientèle via [Zendesk](https://tve.zendesk.com/home). Pour accéder à Zendesk, vous devez vous enregistrer et créer un compte à l’adresse https://tve.zendesk.com/home. Le nombre d’utilisateurs pouvant être enregistrés n’est pas limité. Une fois enregistré, vous pouvez afficher et partager des commentaires sur n’importe quel ticket envoyé.

L’équipe d’authentification d’Adobe Pass est à votre disposition pour toute question ou problème technique que vous pourriez rencontrer au cours du processus d’intégration. Veuillez nous contacter à [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Accès à la documentation {#access-documentation}

**L’Adobe fournira** l’accès à notre documentation publique via [Adobe Experience League](https://experienceleague.adobe.com/en/docs/pass/authentication/home).

L’équipe d’authentification d’Adobe Pass fournit une documentation complète sur les fonctionnalités et les API disponibles dans la section [ Guide d’intégration pour les programmeurs ](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md). Reportez-vous à la table des matières de cette section pour obtenir des liens vers des informations détaillées sur chaque sujet.

## Accès à l’outil de test {#access-testing-tool}

**L’Adobe donnera accès** via le site web [Adobe Developer](https://developer.adobe.com/adobe-pass/), à notre outil d’exploration des API.

## Accès à l’outil de gestion de la configuration {#access-configuration-management-tool}

**L’Adobe permet d’accéder** par le biais du tableau de bord TVE d’Adobe Pass, à un outil en libre-service de gestion de votre configuration et de vos données](https://experience.adobe.com/pass/authentication).[

L’équipe d’authentification d’Adobe Pass fournit une documentation complète sur l’utilisation du tableau de bord TVE dans la section [ Guide de l’utilisateur pour le tableau de bord TVE ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md). Reportez-vous à la table des matières de cette section pour obtenir des liens vers des informations détaillées sur chaque sujet.
