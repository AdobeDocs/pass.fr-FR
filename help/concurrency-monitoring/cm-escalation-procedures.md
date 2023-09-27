---
title: Procédures de réaffectation de la surveillance de la simultanéité
description: Procédures de réaffectation de la surveillance de la simultanéité
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '823'
ht-degree: 0%

---


# Procédures de réaffectation de la surveillance de la simultanéité {#esc-procedures}

>[!NOTE]
>
>Appelez la hotline : +1-205-693-9813 et envoyez un email à `tve-support@adobe.com` y compris &quot;URGENT - INCIDENT&quot; dans la ligne d’objet.


## Introduction {#cm-escalation-intro}

Ce document décrit les procédures d’assistance pour les incidents majeurs (**SEVERITY 1** niveau) affectant l’authentification Adobe Pass, la surveillance de la simultanéité Adobe Pass et ses partenaires.

## Définition du niveau de gravité 1 de réaffectation {#defn-escl-sevrityone-level}

A **SEVERITY 1** un incident de niveau **LIVE** situation, **survenant dans l’environnement de production**, qui ne permet pas l’achèvement des flux d’authentification et/ou d’autorisation pour un canal et un MVPD, affectant un grand nombre d’abonnés du MVPD exécutant le flux.

## Exemples d’incidents de gravité 1 {#exampl-sevone-incident}

* L’activation d’accès en production hébergée à l’adresse <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> n’est pas disponible.

* Pour un MVPD spécifique, l’Adobe ne redirige/n’affiche plus la page de connexion, une fois que l’utilisateur a sélectionné le MVPD (dans l’un des navigateurs pris en charge).

* Le partenaire reçoit un grand nombre de rapports que les utilisateurs ne peuvent pas authentifier/autoriser avec un MVPD spécifique.

* Pendant le processus d’authentification, l’utilisateur est bloqué sur une page d’erreur d’Adobe sans possibilité de relancer le flux d’authentification/d’autorisation.


## Exemples de *NOT* un incident de gravité 1 {#exampl-not-sev1}

*Pour les problèmes de ce type, l’Adobe fournira un soutien pour les enquêtes, mais il ne s’agit pas d’incidents de gravité 1 :*

* Un ou plusieurs abonnés ne sont pas en mesure d’effectuer le flux en raison d’un problème de version de Flash (Flash manquant, bloqueurs de Flashs, version de Flash incorrecte).
* Un ou plusieurs abonnés ne peuvent pas s’authentifier et restent sur la page de connexion MVPD.
* Un ou plusieurs abonnés sont authentifiés mais ne peuvent pas lire de vidéos.
* Un/plusieurs/tous les abonnés rencontrent une erreur JavaScript sur le site du programmeur.

## Flux de réaffectation de gravité 1 {#sevone-escalation-flows}

Les incidents de gravité 1 peuvent être initiés par un Adobe ou un partenaire d’authentification Adobe Pass. Les étapes de chacune d’elles sont présentées ci-dessous.

### Flux initié par le partenaire {#partner-initiated-flow}

1. Le partenaire identifie un incident de gravité 1 (comme décrit ci-dessus) nécessitant une attention immédiate de l’Adobe.

1. Le partenaire envoie un email à tve-support@adobe.com comprenant &quot;URGENT - INCIDENT&quot; dans la ligne d’objet et ajoutant les informations suivantes :

   * Titre
   * Description et étapes à reproduire
   * SE
   * Navigateur
   * Version du Flash
   * (Facultatif) Toutes les captures d’écran ou vidéos disponibles démontrant le problème.

1. Si Adobe ne répond pas au ticket dans les 30 minutes, le partenaire appelle le numéro ci-dessous :

   * **1-205-693-9813**


**Si vous n’incluez pas &quot;URGENT-INCIDENT&quot; dans le titre du ticket, il ne sera pas repris par notre système de notification.**

### Flux initié par l’Adobe {#adobe-initiated-flow}

**...pour un problème d’authentification Adobe Pass**

1. Adobe identifie un problème interne et ouvre un ticket avec notre système de suivi.

1. Adobe informe le responsable de programme et le contact technique du partenaire, en indiquant le numéro de ticket et l’impact estimé de la publication.

1. Adobe travaille à la résolution de l&#39;incident et tient informés tous les partenaires concernés.


**...pour un problème de partenaire (Programmeur/MVPD)**

1. Adobe identifie un problème lié à l’intégration à un MVPD ou à l’un des sites du programmeur.

1. Adobe informe le partenaire concerné **suivre les procédures d&#39;assistance en place avec ce partenaire ;** et ouvre un ticket auprès de l’organisation d’assistance du partenaire.

1. Si, lors de l’analyse de l’impact, l’Adobe identifie que la question appartient à l’une des décisions prévalant dans les scénarios d’incident (voir la section &quot;Décisions prévalidées dans les scénarios d’incident&quot; ci-dessous), il agira en conséquence sans attendre le partenaire1. Entrée de .

1. Adobe attend les mises à jour du partenaire et une notification du partenaire une fois le service restauré.

### Décisions prises d’avance sur les scénarios d’incident {#pre-agreed-decisions}

Dans certains cas, une action par défaut est exécutée dans le cas de l’occurrence de ce scénario :

|    | Scénario | Description | Actions |
|:---:|:---|:---|:---|
| S1 | Adobe identifie un problème avec l’intégration d’un MVPD lors d’opérations de production normales. | Lors des opérations de production normales, Adobe identifie un problème avec l’un des MVPD qui empêche l’exécution des flux d’authentification/d’autorisation (par exemple, certificats expirés, réponses SAML expirées, ports fermés, paramètres modifiés, etc.). | Adobe avisera les programmeurs et MVPD concernés. Adobe désactivera ce MVPD pour tous les programmeurs affectés. Adobe ouvrira un ticket avec le MVPD suivant la procédure de support convenue avec ce MVPD |
| S2 | Adobe active un nouveau MVPD pour un programmeur et le programmeur met le MVPD en liste blanche avant la date de lancement. | Adobe active un nouveau MVPD pour un site de programmeur, et le site affiche déjà le nouveau MVPD dans le sélecteur, même s’il n’était pas censé le faire. | Adobe informera le programmeur que le nouveau MVPD apparaîtra dans le sélecteur avant la date planifiée. Si nécessaire, le programmeur prendra des mesures pour le supprimer du sélecteur. |
| S3 | Adobe active un nouveau MVPD pour un programmeur même si le MVPD n’est pas prêt pour la production | Adobe active un nouveau MVPD pour un programmeur, mais le MVPD n’a pas encore déployé la prise en charge de l’intégration afin que les flux d’authentification/d’autorisation ne puissent pas être effectués. | Adobe effectue le déploiement uniquement si le programmeur le demande. Le programmeur est chargé de s’assurer que la liste blanche du MVPD est whitelistée une fois tous les tests effectués. |

### Attentes En Matière De Réponse Pour Les Incidents De Gravité 1 {#response-expectations}

* Réponse initiale : 30 minutes (24/7)
* Plan d’action : 1 heure (24/7)
* Résolution : ASAP (24/7)
