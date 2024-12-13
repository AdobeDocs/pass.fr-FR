---
title: Chrome de l’évaluation de la prévention du suivi de Google
description: Chrome de l’évaluation de la prévention du suivi de Google
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 0%

---

# Évaluation de la prévention du suivi (hérité) - Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble

Ce document agrège les ressources utiles et évalue les modifications à venir planifiées par Google Chrome dans le cadre de son initiative visant à supprimer progressivement les cookies tiers.

L’évaluation est effectuée pour les applications TV Everywhere (TVE) s’exécutant sur le navigateur Google Chrome et qui utilisent Adobe Pass Access Enabler JavaScript SDK v4 pour s’intégrer aux services principaux d’authentification d’Adobe Pass.

## Ressources publiques

Vous trouverez ci-dessous une liste de ressources agrégées à partir du site web du développeur de Google et également de son blog officiel, que nous recommandons à nos clients de consulter :

* [Étape suivante vers la suppression progressive des cookies tiers dans Chrome](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [Documentation destinée aux développeurs pour Privacy Sandbox](https://developers.google.com/privacy-sandbox)
* [Préparation aux restrictions des cookies tiers](https://developers.google.com/privacy-sandbox/3pcd)
* [Se préparer à l’élimination progressive des cookies tiers](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [Préparation à la fin des cookies tiers](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Les cookies tiers sont restreints par défaut pour 1 % des utilisateurs de Chrome](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## Chronologie

En résumé, Google Chrome a commencé à tester [Protection contre le tracking](https://privacysandbox.com/), une nouvelle fonctionnalité qui limite le tracking intersite affectant tous les cookies tiers.

Dans un premier temps, cela a commencé au début de l’année 2024, affectant environ 1 % de leurs utilisateurs, et leur plan (provisoire) est d’étendre cette mesure à 100 % des utilisateurs à partir du troisième trimestre de 2024.

## Évaluation

Google a publié un document rassemblant son playbook recommandé pour préparer l’élimination progressive des cookies tiers sur le lien suivant : https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

Nous avons adhéré à ce guide pour l’évaluation des applications TV Everywhere (TVE) s’exécutant sur le navigateur Google Chrome et qui utilisent Adobe Pass Access Enabler JavaScript SDK v4 pour s’intégrer aux services principaux d’authentification d’Adobe Pass.

### Conclusions

Sur la base de nos tests, simulant les mises à jour à venir de Google Chrome, les principaux flux commerciaux TVE **continueront à fonctionner comme prévu**.

Cependant, il est essentiel de reconnaître la stratégie plus large de Google, qui implique non seulement l’arrêt des cookies tiers, mais également le partitionnement du stockage tiers.

Par conséquent, les utilisateurs de Chrome subiront des perturbations avec les fonctionnalités d’authentification unique (SSO), de déconnexion unique (SLO) et d’authentification passive, nécessitant des actions de connexion/déconnexion distinctes pour chaque application TVE qu’ils utilisent (conformément à l’expérience actuelle sur Safari).

## Appel à l&#39;auto-évaluation

Nous recommandons vivement à nos clients de mener de manière proactive des évaluations similaires afin de repérer les problèmes potentiels bien à l’avance et de se familiariser avec l’expérience utilisateur Google Chrome révisée.

Cette évaluation doit englober les services propriétaires et les services tiers, en particulier en ce qui concerne l’intégration d’Adobe Pass Access Enabler JavaScript SDK v4.

Si vous rencontrez des difficultés liées aux flux d&#39;affaires TVE, y compris l&#39;authentification, la pré-autorisation, l&#39;autorisation, les métadonnées de l&#39;utilisateur ou la déconnexion, nous vous invitons à déposer un rapport via un ticket Zendesk auprès de notre équipe d&#39;assistance clientèle.

Pour obtenir de l&#39;aide pour élaborer votre plan d&#39;auto-évaluation, veuillez consulter les sections suivantes.

### Audit de l’utilisation des cookies

À partir de Chrome 118, l’onglet [Problèmes DevTools](https://developer.chrome.com/docs/devtools/issues/) met en évidence les cookies potentiellement affectés avec le message suivant : `Cookie sent in cross-site context will be blocked in future Chrome versions`.

Les cookies marqués pour une utilisation par des tiers peuvent être identifiés par leur valeur d’attribut `SameSite=None`.

Suivez ce lien pour en savoir plus : https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### Test de rupture

Pour vérifier s’il y a eu rupture, lancez Chrome à l’aide de l’indicateur de ligne de commande `--test-third-party-cookie-phaseout` ou activez `#test-third-party-cookie-phaseout` dans `chrome://flags/` à partir de Chrome 118.

Cela configurera Google Chrome pour bloquer les cookies tiers et s’assurer que les fonctionnalités futures sont actives afin de simuler au mieux l’état après la suppression progressive.

Il est intéressant d’examiner en détail les spécifications techniques des indicateurs Chrome suivants :

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

Suivez ce lien pour en savoir plus : https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## Autres navigateurs

### Firefox

Firefox a lancé son mécanisme appelé : `Enhanced Tracking Protection` il y a quelques années.

Consultez ci-dessous quelques ressources utiles de Firefox :

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari a lancé son mécanisme appelé : `Intelligent Tracking Prevention` il y a quelques années.

Consultez ci-dessous quelques ressources utiles de Safari :

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Vous trouverez ci-dessous quelques ressources utiles d’Adobe Pass :

* [Évaluation de la prévention du suivi - Apple Safari](tracking-prevention-assessment-apple-safari.md)
