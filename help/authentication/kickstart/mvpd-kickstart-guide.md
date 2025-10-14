---
title: Guide de démarrage rapide de MVPD
description: Guide de démarrage rapide de MVPD
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 2b9a8ce374f7a73cd815e9735d672e5c9ba285cc
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# Guide de démarrage rapide de MVPD {#mvpd-kickstart-guide}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Ce guide de démarrage rapide est destiné aux distributeurs de programmes vidéo multicanaux (MVPD) qui prévoient de s’intégrer à l’authentification Adobe® Pass.

Ce document décrit les principales étapes initiales pour garantir un démarrage fluide et efficace du processus d’intégration. Il vise à clarifier les attentes et à fournir des conseils sur la façon dont nous collaborerons avec les partenaires pour réussir les intégrations.

Adobe fournit toute une gamme de ressources pour vous aider à l’intégrer à l’authentification Adobe Pass. Veuillez vous reporter aux mentions **« Vous fournirez »** et **« L’Adobe fournira »** de chaque section ci-dessous.

>[!CAUTION]
>
> Chaque fois qu’un utilisateur ou une utilisatrice lance un flux de droits, un seul identifiant utilisateur opaque et unique lui est affecté. Cet identifiant, provenant du MVPD, est utilisé pour identifier l’utilisateur dans l’application d’un programmeur.
>
> <br/>
>
> L’ID utilisateur ne doit contenir aucune information d’identification personnelle (PII) ni aucune donnée pouvant être utilisée seule ou en combinaison avec d’autres détails pour identifier, contacter ou localiser l’utilisateur.

## Processus de configuration {#setup-process}

Le processus de configuration comprend entre autres les étapes suivantes :

![Processus D&#39;Intégration De L&#39;Authentification Adobe ® Pass](../assets/mvpd-int-lifecycle.png)

*Processus D&#39;Intégration De L&#39;Authentification Adobe ® Pass*

### Démarrage {#kickoff}

**Vous fournirez** au cours de la phase de lancement :

* **Nom d’affichage**

  Il s&#39;agit d&#39;une chaîne affichée sur les sites Web ou les applications du programmeur lorsque vous demandez aux utilisateurs de sélectionner leur fournisseur de télévision payante.

* **URL du logo**

  Il s&#39;agit d&#39;un fichier de 112 pixels sur 33, contenant le logo affiché sur les sites Web ou applications du programmeur lors de la sélection de son fournisseur de télévision payante.

* **Durée de vie (TTL)**

  La TTL est une valeur généralement définie par le MVPD dans le cadre des processus d’authentification ou d’autorisation. Cependant, l’Adobe peut remplacer ces valeurs de durée de vie et fournir des valeurs différentes en fonction de ce qui est convenu par le programmeur et le MVPD.

* **Jeux d’informations d’identification**

  Il s’agit d’informations d’identification utilisées pour authentifier et autoriser, ou uniquement authentifier, l’utilisateur à l’aide du MVPD. En règle générale, ces informations d’identification se composent d’un nom d’utilisateur et d’un mot de passe, qui doivent être fournis pour les deux profils (évaluation et production).

### Exchange des métadonnées (SAML) {#metadata-exchange-saml}

**L’Adobe fournira** pendant la phase d’exchange des métadonnées :

* **Métadonnées de l’environnement d’évaluation**

  Les métadonnées SP d’Adobe peuvent être récupérées à partir de https://sp.auth-staging.adobe.com/sp/metadata.

* **Métadonnées de l’environnement de production**

  Les métadonnées SP d’Adobe peuvent être récupérées à partir de https://sp.auth.adobe.com/sp/metadata.

**Pendant la phase d’exchange des métadonnées** vous fournirez les éléments suivants :

* **Métadonnées intermédiaires**

  Métadonnées de MVPD pour l’environnement d’évaluation.

* **Métadonnées de production**

  Métadonnées de MVPD pour l’environnement de production.

### Connectivité {#connectivity}

placer sur la liste autorisée **Vous allez fournir** un moyen de les adresses IP de l’Adobe, car l’authentification Adobe Pass nécessite des pare-feu pour autoriser le trafic via les ports 80 et 443 afin de permettre l’accès à des ressources restreintes pendant les processus d’authentification et d’autorisation.

**Vous fournirez** un déploiement dans le profil d’évaluation pour tester la connectivité.

### Développement {#development}

**L&#39;Adobe fournira** temps d&#39;ingénierie pour travailler en étroite collaboration avec le MVPD afin de s&#39;assurer que l&#39;intégration technique est correctement établie. Ce processus implique le développement de code personnalisé adapté aux exigences spécifiques de MVPD.

### Déploiement dans l’évaluation {#deployment-staging}

**L&#39;Adobe fournira** une version avec les mises à jour de code requises qui seront d&#39;abord déployées dans l&#39;environnement d&#39;évaluation PRE-QUAL. Au cours de cette phase, les modifications de configuration nécessaires seront également mises en œuvre pour intégrer MVPD au fournisseur de services `TestDistributors` à des fins de test.

**Vous et l’Adobe fournirez** du temps d’assurance qualité (QA) pour vous assurer que l’intégration est testée avec succès dans l’environnement d’évaluation PRE-QUAL. Après cette phase, le MVPD est déplacé vers l’environnement d’évaluation RELEASE pour des tests supplémentaires avec un programmeur réel.

### Déploiement en production {#deployment-production}

**Vous allez fournir** un déploiement dans le profil de production pour tester la connectivité.

**L&#39;Adobe fournira** une version avec les mises à jour de code requises qui seront déployées dans l&#39;environnement de production PRE-QUAL.

**Vous et l’Adobe fournirez** temps d’assurance qualité pour vous assurer que l’intégration est testée avec succès à l’aide du profil de production. Si tout est correct à ce stade, Adobe peut déplacer l’intégration vers l’environnement de production RELEASE (« actif »), disponible pour tous les utilisateurs.

>[!IMPORTANT]
>
> Une fois l’intégration en ligne dans l’environnement de production RELEASE, il est essentiel de maintenir une expérience client optimale. Pour gérer efficacement les scénarios de serveur en panne, les MVPD doivent fournir une documentation détaillée sur la procédure d’escalade à l’Adobe pour gérer ces problèmes.
>
> En retour, Adobe garantit que les MVPD reçoivent la dernière version du processus d’escalade de l’authentification Adobe Pass pour une résolution rationalisée des problèmes.

## Accès aux environnements {#access-environments}

**L’Adobe permet d’accéder** environnements à différentes étapes du processus de développement :

* **Préqualification (PRE-QUAL)**

  L’environnement PRE-QUAL héberge le prochain candidat de version et sert de plateforme d’intégration initiale pour les nouveaux partenaires. Avant de passer à l&#39;environnement RELEASE, les partenaires ont le temps de tester leur intégration dans PRE-QUAL.

* **VERSION (RELEASE)**

  L’environnement de publication héberge la version de production actuelle (stable).

Pour plus d’informations sur l’utilisation de ces environnements, reportez-vous à la documentation [&#x200B; Présentation des environnements d’Adobe &#x200B;](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md).

>[!IMPORTANT]
> 
> Les modifications de configuration apportées à ces environnements doivent être explicitement demandées par l’intermédiaire de votre représentant d’Adobe, en suivant le processus de demande de modification établi.

## Accès au service clientèle {#access-customer-support}

**Adobe fournira** l&#39;accès à notre système de service clientèle via [Zendesk](https://tve.zendesk.com/home). Pour accéder à Zendesk, vous devez vous enregistrer et créer un compte à l’adresse https://tve.zendesk.com/home.

L’équipe d’authentification d’Adobe Pass est disponible pour répondre à toutes les questions ou problèmes techniques que nous pouvons rencontrer pendant le processus d’intégration. Veuillez nous contacter à [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Accès à la documentation {#access-documentation}

**L’Adobe fournira** l’accès à notre documentation publique via [Adobe Experience League](https://experienceleague.adobe.com/fr/docs/pass/authentication/home).

L’équipe d’authentification d’Adobe Pass fournit une documentation complète sur les fonctionnalités et les workflows disponibles dans la section [&#x200B; Guide d’intégration pour les fichiers MVPD &#x200B;](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md). Reportez-vous à la table des matières de cette section pour obtenir des liens vers des informations détaillées sur chaque sujet.

## Accès à l’outil de test {#access-testing-tool}

**L’Adobe donnera accès** via le site web [Adobe Developer](https://developer.adobe.com/adobe-pass/), à notre outil d’exploration des API.
