---
title: Chrome Google d’évaluation de la prévention du suivi
description: Chrome Google d’évaluation de la prévention du suivi
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 0%

---

# Évaluation de la prévention du suivi - Chrome Google {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble

Ce document rassemble des ressources utiles et évalue les modifications à venir prévues par Google Chrome dans le cadre de leur initiative visant à éliminer progressivement les cookies tiers.

L’évaluation est effectuée pour les applications TV Everywhere (TVE) s’exécutant sur le navigateur Google Chrome et qui utilisent le SDK JavaScript d’accès Adobe Pass v4 pour s’intégrer aux services principaux d’authentification Adobe Pass.

## Ressources publiques

Voir ci-dessous une liste des ressources agrégées à partir du site web de développement de Google et aussi de leur blog officiel que nous recommandons à nos clients de consulter :

* [L’étape suivante vers l’élimination progressive des cookies tiers dans Chrome](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [Documentation destinée aux développeurs pour le sandbox de confidentialité](https://developers.google.com/privacy-sandbox)
* [Préparation des restrictions de cookies tiers](https://developers.google.com/privacy-sandbox/3pcd)
* [Préparation de l’expiration du cookie tiers](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [Préparation de la fin des cookies tiers](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Cookies tiers limités par défaut pour 1 % des utilisateurs de Chrome](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## Chronologie

En résumé, Google Chrome a commencé à tester la [protection du suivi](https://privacysandbox.com/), une nouvelle fonctionnalité qui limite le suivi sur plusieurs sites et affecte tous les cookies tiers.

Au départ, cela a commencé au début de 2024, affectant environ 1% de leurs utilisateurs, et leur plan (provisoire) est de l&#39;étendre à 100% des utilisateurs à partir du troisième trimestre de 2024.

## Évaluation

Google a publié un document agrégeant son playbook recommandé afin de préparer l’élimination progressive des cookies tiers à l’aide du lien suivant : https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

Nous avons adhéré à ce manuel d’évaluation des applications TV Everywhere (TVE) s’exécutant sur le navigateur Google Chrome et qui utilisent le SDK JavaScript d’accès Adobe Pass v4 pour s’intégrer aux services principaux d’authentification Adobe Pass.

### Conclusion

Sur la base de nos tests, en simulant les mises à jour à venir de Google Chrome, les principaux flux commerciaux TVE **continueront à fonctionner comme prévu**.

Cependant, il est essentiel de reconnaître une stratégie plus large de Google, qui implique non seulement l’arrêt des cookies tiers, mais également la partitionnement du stockage tiers.

Par conséquent, les utilisateurs de Chrome subiront des perturbations avec les fonctionnalités d’authentification unique (SSO), de connexion unique (SLO) et d’authentification passive, ce qui nécessitera des actions de connexion/déconnexion distinctes pour chaque application TVE qu’ils utilisent (en phase avec l’expérience actuelle sur Safari).

## Appel à l’auto-évaluation

Nous exhortons nos clients à mener de manière proactive des évaluations similaires afin de déterminer les problèmes potentiels bien à l’avance et de se familiariser avec l’expérience utilisateur de Google Chrome révisée.

Cette évaluation doit englober à la fois les services propriétaires et les services tiers, en particulier en ce qui concerne l’intégration du SDK JavaScript Adobe Pass Access Enabler v4.

Si vous rencontrez des difficultés liées aux flux d’activités TVE, notamment l’authentification, la pré-autorisation, l’autorisation, les métadonnées utilisateur ou la déconnexion, nous vous invitons à soumettre un rapport au moyen d’un ticket Zendesk auprès de notre équipe d’assistance clientèle.

Pour obtenir de l’aide sur l’élaboration de votre plan d’auto-évaluation, reportez-vous aux sections ci-dessous.

### Contrôle de l’utilisation des cookies

À partir de Chrome 118, l’onglet [Problèmes liés aux outils de développement](https://developer.chrome.com/docs/devtools/issues/) met en évidence les cookies potentiellement affectés avec le message suivant : `Cookie sent in cross-site context will be blocked in future Chrome versions`.

Les cookies marqués pour une utilisation tierce peuvent être identifiés par leur valeur d’attribut `SameSite=None` .

Pour en savoir plus, suivez ce lien : https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### Test de la rupture

Pour tester l’arrêt, lancez Chrome à l’aide de l’indicateur de ligne de commande `--test-third-party-cookie-phaseout` ou activez `#test-third-party-cookie-phaseout` dans `chrome://flags/` à partir de Chrome 118.

Cette action configure Google Chrome pour bloquer les cookies tiers et garantir que les futures fonctionnalités sont actives afin de simuler au mieux l’état après l’abandon de la phase.

Les spécifications techniques des indicateurs Chrome suivants méritent d’être approfondies :

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

Pour en savoir plus, suivez ce lien : https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## Autres navigateurs

### Firefox

Firefox a eu un déploiement de son mécanisme appelé : `Enhanced Tracking Protection` il y a quelques années.

Voir ci-dessous quelques ressources utiles de Firefox :

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari a eu un déploiement de son mécanisme appelé : `Intelligent Tracking Prevention` il y a quelques années.

Voir ci-dessous quelques ressources utiles de Safari :

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Voir ci-dessous quelques ressources utiles provenant d’Adobe Pass :

* [Évaluation de la prévention du suivi - Apple Safari](tracking-prevention-assessment-apple-safari.md)
