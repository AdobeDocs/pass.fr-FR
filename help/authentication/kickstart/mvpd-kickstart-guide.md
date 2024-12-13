---
title: Plan d’intégration directe de MVPD
description: Plan d’intégration directe de MVPD
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 0%

---

# Guide de démarrage rapide de MVPD : plan d’intégration de MVPD direct {#mvpd-dir-int-plan}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#mvpd-kickstart-intro}

Bienvenue dans l’authentification Adobe Pass pour TV Everywhere.  Nous sommes impatients de travailler avec vous.

>[!NOTE]
>
>Il s&#39;agit du Guide de démarrage rapide pour les distributeurs de programmes vidéo multicanaux (MVPD). Si vous êtes un programmeur (fournisseur de contenu), consultez le [Guide de démarrage rapide des programmeurs](/help/authentication/kickstart/programmer-kickstart-guide.md).

La prise en charge est disponible à tout moment via le système de ticket d’authentification Adobe Pass sur Zendesk. C’est également là que vous trouverez des exemples, de la documentation et des tutoriels vidéo pour nos processus. Pour utiliser [Zendesk](https://adobeprimetime.zendesk.com/), vous devez vous enregistrer et créer un compte sur https://tve.zendesk.com/home. Le nombre d&#39;utilisateurs pouvant s&#39;inscrire et afficher ou poster des commentaires sur un ticket archivé est illimité. Toutes les questions d’assistance doivent être adressées à : tve-support à l’adresse adobe.com

**Contacts de l’équipe** :

**Assistance** - Pour toutes les questions, incidents ou demandes de fonctionnalités **tve-support@adobe.com**.

## 1. Réunions de lancement {#kickoff-meetings}

Ces réunions sont le point de départ de discussions techniques entre l&#39;Adobe et le MVPD. À ce stade du processus, la documentation doit être partagée des deux côtés. Dans le prolongement, Adobe doit ouvrir un ticket dans notre système de billetterie (https://tve.zendesk.com/) pour suivre le statut de l’intégration.

## 2. Caractéristiques {#features}

Adobe configurera un appel de statut hebdomadaire pour discuter et suivre le planning global, les étapes, le calendrier et les détails de mise en œuvre de l’intégration. Au cours de cette phase, Adobe examine les spécifications de MVPD. Le résultat de cette opération doit être une page de spécification qui détaille toutes les fonctionnalités nécessaires au MVPD. Le MVPD enverra à l’Adobe un document de spécification détaillant la mise en œuvre de l’authentification/autorisation MVPD.

Pour plus de précisions, consultez [Fonctionnalités d&#39;intégration de MVPD](/help/authentication/integration-guide-mvpds/mvpd-integr-features.md).

Plusieurs paramètres doivent être décrits en détail à ce stade :

* **URL du logo MVPD** - Il s’agit d’un fichier aux dimensions suivantes : 112 x 33 pixels. Le logo est affiché par les programmeurs sur leurs sites lorsque l&#39;utilisateur clique sur le bouton « Se connecter » pour sélectionner son fournisseur de télévision payante.
* **Valeurs de durée de vie (TTL, time-to-live)** - La durée de vie est généralement définie par le MVPD pendant le processus d’authentification/d’autorisation. Cependant, Adobe peut remplacer ces valeurs de durée de vie et fournir des valeurs différentes en fonction de ce qui est convenu par le programmeur et le MVPD.
* **Nom d’affichage** : il est affiché par les programmeurs sur leurs sites lorsque l’utilisateur clique sur le bouton « Se connecter » pour sélectionner son fournisseur de télévision payante.
* **Informations d’identification de test** - Les deux profils (évaluation et production) doivent disposer d’une liste d’informations d’identification de test.

>[!IMPORTANT]
>
>Chaque fois qu’un utilisateur lancera un flux de droits, il sera associé à un identifiant utilisateur unique, opaque et unique.  L’ID utilisateur est utilisé pour identifier l’utilisateur de l’application d’un programmeur, mais provient du MVPD.

>[!CAUTION]
>
>Remarque importante pour chaque MVPD : l’ID utilisateur ne doit PAS contenir d’informations d’identification personnelles (PII), informations qui peuvent être utilisées seules ou avec d’autres informations pour identifier, contacter ou localiser l’utilisateur.

## 2. exchange des métadonnées {#metadata-ex}

Les deux parties doivent exchange les métadonnées pour tous les environnements impliqués (production, évaluation, etc.).

* **Adobe**
   * Pour l’environnement d’évaluation, les métadonnées du SP de l’Adobe peuvent être récupérées à partir de : [Métadonnées du SP d’évaluation d’authentification](https://sp.auth-staging.adobe.com/sp/metadata)
   * Pour l’environnement de production, les métadonnées de SP de l’Adobe peuvent être récupérées à partir de : [Authentification des métadonnées de SP de production](https://sp.auth.adobe.com/sp/metadata)

* **MVPD**
   * Pour ajouter des métadonnées (évaluation/production).

## 4. Liste d’adresses IP autorisées {#allow-ip-list}

Les adresses IP suivantes doivent être whitelistées dans le pare-feu MVPD. Contactez l’Adobe pour obtenir la liste des adresses IP.

* L’authentification Adobe Pass nécessite l’ouverture de pare-feu sur les ports 80 et 443, afin de permettre l’accès à des ressources limitées.

* Le MVPD doit ajouter une liste d’adresses IP pour les serveurs d’authentification et d’autorisation (si c’est le cas).

## 5. Développement {#deve}

La durée de la phase de développement sera déterminée après avoir examiné les spécifications et pris en considération les éléments que les deux parties approuvent. L’Adobe doit écrire du code personnalisé pour la partie autorisation.

>[!NOTE]
>
>Notez que l’autorisation est effectuée par ressource. La transaction d’autorisation est généralement effectuée avec une chaîne d’identifiant, transmise à partir du site du programmeur, représentant le canal pour lequel l’utilisateur demande une autorisation. Cet identifiant de ressource est établi entre le programmeur et MVPD et peut être aussi granulaire que nécessaire.

## 6. Environnements d’Adobe {#adobe-env}

Adobe fournit différents environnements pour différentes étapes du processus de développement :

* **Préqualification** (PRE-QUAL) : l’environnement PRE-QUAL contient la prochaine version candidate. Adobe intègre initialement de nouveaux partenaires dans cet environnement, avant de mettre à niveau l’intégration vers l’environnement de version. Les partenaires disposent de deux semaines pour effectuer des tests sur l’environnement PRE-QUAL et doivent demander explicitement toute modification de la configuration PRE-QUAL (contactez votre représentant d’Adobe pour plus d’informations sur le processus de demande de modification). Les correctifs de bugs déclenchent de nouveaux déploiements dans cet environnement.
* **Version** (VERSION) : la version de production actuelle d’Adobe est déployée dans un environnement en ligne ici.

Pour plus d’informations sur l’utilisation des environnements Adobe, voir [ Présentation des environnements Adobe ](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md)

## 7. Déploiement intermédiaire {#stag-env}

En fonction des métadonnées reçues du MVPD, Adobe créera et configurera un nouveau MVPD dans le système d’authentification Adobe Pass. Elle sera déployée dans l’environnement d’évaluation préégal d’Adobe et configurée avec notre programmeur de test (TestDistributors).

Le MVPD doit effectuer le même déploiement dans son environnement d’assurance qualité/d’évaluation/de test.

## 8. Test et dépannage {#tes-troubleshoot}

Au cours de cette phase, l’Adobe et le MVPD testent et résolvent les problèmes d’intégration. Pour tester l’intégration, l’équipe d’authentification d’Adobe Pass peut utiliser le site de test de l’API d’Adobe. Pour en savoir plus sur l’utilisation du site de test de l’API Adobe, voir [Test de l’authentification et des flux d’autorisation à l’aide du site de test de l’API Adobe](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).

Une fois le test et le dépannage terminés avec succès, l’intégration est activée dans l’environnement d’évaluation de version d’Adobe. À ce stade, Adobe peut intégrer MVPD à un programmeur réel.

## 9. Déploiement en production {#prod-dep}

* Le MVPD doit d’abord être déployé dans le profil de production pour tester la connectivité.

* Adobe des déploiements dans la production préquale.

* Toutes les parties peuvent désormais effectuer des tests dans le profil de production.

* Si tout est correct à ce stade, Adobe peut déplacer l’intégration vers l’environnement de production de la version (« actif »), disponible pour tous les utilisateurs et utilisatrices.

## 10. Procédures d&#39;escalade {#esc-proc}

Une fois l’intégration en production, il est essentiel de fournir la meilleure expérience client. Afin d&#39;assurer la meilleure réponse possible en cas de problème d&#39;arrêt du serveur, les MVPD doivent fournir une documentation sur la procédure d&#39;escalade pour les problèmes signalés à l&#39;Adobe.

À son tour, Adobe fournit aux MVPD le processus d’escalade de l’authentification Adobe Pass le plus récent.


<!--- [!RELATEDINFORMATION]
>
>* [Programmer Kickstart Guide](/help/authentication/programmer-kickstart-guide.md)
>* [MVPD Integration Guide](/help/authentication/mvpd-integr-features.md)
-->
