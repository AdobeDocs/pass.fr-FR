---
title: Plan d’intégration directe MVPD
description: Plan d’intégration directe MVPD
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 0%

---

# Guide de démarrage rapide MVPD : plan d’intégration directe MVPD {#mvpd-dir-int-plan}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#mvpd-kickstart-intro}

Bienvenue dans l’authentification Adobe Pass pour TV partout.  Nous nous réjouissons de travailler avec vous.

>[!NOTE]
>
>Il s’agit du Guide de démarrage rapide pour les distributeurs de programmes vidéo multicanaux (MVPD). Si vous êtes programmeur (fournisseur de contenu), consultez le [Guide de démarrage rapide des programmeurs](/help/authentication/kickstart/programmer-kickstart-guide.md).

La prise en charge est toujours disponible via le système de ticket d’authentification Adobe Pass sur Zendesk. C’est également là que vous trouverez des exemples, de la documentation et des tutoriels vidéo pour nos processus. Pour utiliser [Zendesk](https://adobeprimetime.zendesk.com/), vous devez vous enregistrer et créer un compte à l’adresse https://tve.zendesk.com/home. Il n’existe aucune limite quant au nombre d’utilisateurs que vous pouvez enregistrer et qui peut voir ou publier des commentaires sur un ticket déposé. Toutes les questions d’assistance doivent être adressées à : tve-support à l’adresse adobe.com

**Contacts d’équipe** :

**Assistance** - Pour toutes les questions, incidents ou demandes de fonctionnalités **tve-support@adobe.com**.

## 1. Rencontres de lancement {#kickoff-meetings}

Ces rencontres ont pour but de lancer des discussions techniques entre l&#39;Adobe et le MVPD. À ce stade du processus, la documentation doit être partagée des deux côtés. Par la suite, l’Adobe doit ouvrir un ticket dans notre système de tickets (https://tve.zendesk.com/) pour tracker le statut de l’intégration.

## 2. Fonctionnalités {#features}

Adobe configure un appel d’état hebdomadaire pour discuter et suivre le planning global, les étapes, la chronologie et les détails de mise en oeuvre de l’intégration. Au cours de cette phase, Adobe effectue une révision des spécifications du MVPD. Le résultat doit être une page de spécification qui détaille toutes les fonctionnalités nécessaires au MVPD. Le MVPD enverra à l’Adobe un document de spécification, détaillant la mise en oeuvre de l’authentification/de l’autorisation du MVPD.

Éléments à clarifier, voir [Fonctionnalités d’intégration MVPD](/help/authentication/integration-guide-mvpds/mvpd-integr-features.md).

À ce stade, plusieurs paramètres doivent être décrits en détail :

* **URL du logo de MVPD** - Il s’agit d’un fichier avec les dimensions suivantes : 112 x 33 pixels. Le logo est affiché par les programmeurs sur leurs sites lorsque l&#39;utilisateur clique sur le bouton &quot;Se connecter&quot; pour sélectionner son opérateur de télévision payante.
* **Valeurs TTL (time-to-live)** : la durée de vie est généralement définie par le MVPD pendant le processus d’authentification/d’autorisation. Cependant, l’Adobe peut remplacer ces valeurs TTL et fournir des valeurs différentes selon ce qui est convenu par le programmeur et le MVPD.
* **Nom d’affichage** : affiché par les programmeurs sur leurs sites lorsque l’utilisateur clique sur le bouton &quot;Se connecter&quot; pour sélectionner son fournisseur de télévision payante.
* **Informations d’identification du test** - Les deux profils (évaluation et production) doivent avoir une liste des informations d’identification du test.

>[!IMPORTANT]
>
>Chaque fois qu’un utilisateur lance un flux de droits, il est associé à un seul identifiant utilisateur, opaque et unique.  L’ID utilisateur est utilisé pour identifier l’utilisateur d’une application de programmeur, mais il provient du MVPD.

>[!CAUTION]
>
>Avis important pour chaque MVPD : l’ID utilisateur ne doit PAS contenir d’informations d’identification personnelles, des informations qui peuvent être utilisées seules ou avec d’autres informations pour identifier, contacter ou localiser l’utilisateur.

## 2. exchange des métadonnées {#metadata-ex}

Les deux parties doivent exchange les métadonnées pour tous les environnements impliqués (production, évaluation, etc.).

* **Adobe**
   * Pour les métadonnées de l’Adobe de l’environnement intermédiaire, les métadonnées de SP peuvent être récupérées à partir de : [Authentification des métadonnées de test sp](https://sp.auth-staging.adobe.com/sp/metadata)
   * Pour les métadonnées de l’Adobe de l’environnement de production, les métadonnées de SP peuvent être récupérées à partir de : [Authentification des métadonnées de production sp](https://sp.auth.adobe.com/sp/metadata)

* **MVPD**
   * Pour ajouter des métadonnées (évaluation/production).

## 4. Autoriser la liste des adresses IP {#allow-ip-list}

Les adresses IP suivantes doivent être whitelistées dans le pare-feu du MVPD. Veuillez contacter Adobe pour obtenir la liste des adresses IP.

* L’authentification Adobe Pass requiert l’ouverture des pare-feu sur les ports 80 et 443, afin d’autoriser l’accès aux ressources restreintes.

* Le MVPD doit ajouter une liste d’adresses IP pour les serveurs d’authentification et d’autorisation (si c’est le cas).

## 5. Développement {#deve}

La durée de la phase de développement sera déterminée après avoir examiné les spécifications et pris en compte les éléments que les deux parties ont signés. Adobe doit écrire du code personnalisé pour la partie autorisation.

>[!NOTE]
>
>Notez que l’autorisation est effectuée par ressource. La transaction d’autorisation est généralement effectuée avec une chaîne d’identifiant, transmise à partir du site du programmeur, représentant le canal pour lequel l’utilisateur demande l’autorisation. Cet identifiant de ressource est établi entre le programmeur et MVPD et peut être aussi granulaire que nécessaire.

## 6. Environnements d’Adobe {#adobe-env}

Adobe fournit différents environnements pour différentes étapes du processus de développement :

* **Préqualification** (PRE-QUAL) : l’environnement PRE-QUAL contient le prochain candidat de version. Adobe intègre initialement de nouveaux partenaires dans cet environnement, avant de mettre à niveau l’intégration vers l’environnement Version . Les partenaires disposent de deux semaines pour tester l’environnement PRE-QUAL et doivent demander explicitement toute modification de la configuration PRE-QUAL (contactez votre représentant Adobe pour plus d’informations sur le processus de demande de modification). Les correctifs de bogues déclenchent de nouveaux déploiements dans cet environnement.
* **Version** (VERSION) : la version de production actuelle de l’Adobe est déployée dans un environnement en ligne ici.

Pour plus d’informations sur l’utilisation des environnements Adobe, voir [Présentation des environnements Adobe](/help/authentication/notes-technical/understanding-the-adobe-environments.md)

## 7. Déploiement intermédiaire {#stag-env}

En fonction des métadonnées reçues du MVPD, Adobe crée et configure un nouveau MVPD dans le système d’authentification Adobe Pass. Il sera déployé dans l’environnement d’évaluation prédéfini d’Adobe et configuré avec notre programmeur de test (TestDistributors).

Le MVPD doit effectuer le même déploiement dans son environnement de contrôle qualité/d’évaluation/de test.

## 8. Test et dépannage {#tes-troubleshoot}

Au cours de cette phase, Adobes et le test MVPD et résolvez les problèmes d’intégration. Pour aider à tester l’intégration, l’équipe d’authentification Adobe Pass peut utiliser le site de test de l’API d’Adobe. Pour en savoir plus sur l’utilisation du site de test d’API d’Adobe, voir [ Test des flux d’authentification et d’autorisation à l’aide du site de test d’API d’Adobe ](/help/authentication/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).

Une fois le test et la résolution des problèmes terminés, l’intégration est activée dans l’environnement d’évaluation de la version d’Adobe. À ce stade, Adobe peut intégrer le MVPD à un programmeur réel.

## 9. Déploiement en production {#prod-dep}

* Le MVPD doit d’abord être déployé dans le profil de production, afin de tester la connectivité.

* Adobe se déploie en production prédéfinie.

* Toutes les parties peuvent désormais tester dans le profil de production.

* Si tout va bien à ce stade, Adobe peut déplacer l’intégration vers l’environnement de production de version (&quot;live&quot;), disponible pour tous les utilisateurs.

## 10. Procédures de réaffectation {#esc-proc}

Une fois l’intégration en production, il est essentiel de fournir la meilleure expérience client. Pour garantir la meilleure réponse en cas de problème de panne du serveur, les distributeurs multicanaux de programmes audiovisuels doivent fournir la documentation sur les procédures de transmission pour les problèmes portés à l’attention de l’Adobe.

Adobe fournit ensuite les MVPD avec le processus de réaffectation d’authentification Adobe Pass le plus récent.


<!--- [!RELATEDINFORMATION]
>
>* [Programmer Kickstart Guide](/help/authentication/programmer-kickstart-guide.md)
>* [MVPD Integration Guide](/help/authentication/mvpd-integr-features.md)
-->
