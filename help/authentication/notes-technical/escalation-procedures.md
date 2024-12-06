---
title: Procédures de réaffectation
description: Procédures de réaffectation
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---

# Procédures de réaffectation {#escalation-procedures}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
> 
>Appelez la hotline : **+1-205-693-9813** et envoyez un email à **tve-support@adobe.com** y compris **URGENT - INCIDENT** dans la ligne d&#39;objet.

## Introduction {#introduction}

Ce document décrit les procédures de prise en charge pour les incidents majeurs (**niveau SEVERITY 1**) affectant l’authentification Adobe Pass, la surveillance de la simultanéité Adobe Pass et ses partenaires.


## Définition d’un incident de niveau SÉVÉRITÉ 1 {#definition-of-a-severity-1-level-incident}

Un incident de niveau **SEVERITY 1** est une situation **LIVE**, **se produisant dans l’environnement de production**, qui ne permet pas la fin des flux d’authentification et/ou d’autorisation pour un canal et un MVPD, affectant un grand nombre d’abonnés du MVPD exécutant le flux.


## Exemples d&#39;incidents de SÉCURITÉ 1 {#examples-of-severity-1-incidentcs}

* L’activateur d’accès en production hébergé à `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js` (ou `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`) n’est pas disponible.

* Pour un MVPD spécifique, l’Adobe ne redirige/n’affiche plus la page de connexion, une fois que l’utilisateur a sélectionné le MVPD (dans l’un des navigateurs pris en charge).

* Le partenaire reçoit un grand nombre de rapports que les utilisateurs ne peuvent pas authentifier/autoriser avec un MVPD spécifique.

* Pendant le processus d’authentification, l’utilisateur est bloqué sur une page d’erreur d’Adobe sans possibilité de relancer le flux d’authentification/d’autorisation.


| Exemples d’incident **NOT** a Severity 1 |
|---|
| Pour les problèmes de ce type, l’Adobe fournira un soutien pour les enquêtes, mais il ne s’agit pas d’incidents de gravité 1 :<ul><li>Un ou plusieurs abonnés ne sont pas en mesure d’effectuer le flux en raison d’un problème de version de Flash (Flash manquant, bloqueurs de Flashs, version de Flash incorrecte).</li><li>Un ou plusieurs abonnés ne peuvent pas s’authentifier et restent sur la page de connexion MVPD.</li><li>Un ou plusieurs abonnés sont authentifiés mais ne peuvent pas lire de vidéos.</li><li>Un/plusieurs/tous les abonnés rencontrent une erreur JavaScript sur le site du programmeur</li></ul> |

## Flux de réaffectation de gravité 1 {#severity-1-escalation-flows}

Les incidents de gravité 1 peuvent être initiés par un Adobe ou un partenaire d’authentification Adobe Pass. Les étapes de chacune d’elles sont présentées ci-dessous.

### Flux initié par le partenaire {#partner-initiated-flow}

1. Le partenaire identifie un incident de gravité 1 (comme décrit ci-dessus) nécessitant une attention immédiate de l’Adobe.
1. Le partenaire envoie un courrier électronique à **tve-support@adobe.com**, y compris **URGENT - INCIDENT** dans la ligne d’objet et en ajoutant les informations suivantes :
   * Titre
   * Description et étapes à reproduire
   * Système d’exploitation/navigateur
   * SDK et version
   * Périphériques concernés
   * % des utilisateurs concernés
   * Suivi HTTP ou journaux d’appareil présentant le problème
   * (Facultatif) Toutes les captures d’écran ou vidéos disponibles démontrant le problème.
1. Si Adobe ne répond pas au ticket dans les 30 minutes, le partenaire appelle le numéro suivant :
   **1-205-693-9813**
   >[!IMPORTANT]
   >Si vous n’incluez pas &quot;URGENT-INCIDENT&quot; dans le titre du ticket, il ne sera pas repris par notre système de notification**.

### Flux initié par l’Adobe {#adobe-initiated-flow}

#### ...pour un problème d’authentification Adobe Pass {#adobe-initiated-flow-authn-issue}

1. Adobe identifie un problème interne et ouvre un ticket avec notre système de suivi.

1. Adobe informe le responsable de programme et le contact technique du partenaire, en indiquant le numéro de ticket et l’impact estimé de la publication.

1. Adobe travaille à la résolution de l&#39;incident et tient informés tous les partenaires concernés.

#### ...pour un problème de partenaire (Programmeur/MVPD) {#adobe-initiated-flow-partner-issue}

1. Adobe identifie un problème lié à l’intégration à un MVPD ou à l’un des sites du programmeur.

1. Adobe avertit le partenaire concerné <u>en suivant les procédures d’assistance en place avec ce partenaire</u> et ouvre un ticket auprès de l’organisation d’assistance du partenaire.

1. Si, lors de l’analyse de l’impact, Adobe identifie que le problème appartient à l’une des décisions prévalant dans les scénarios d’incident, voir la section **Décisions prévalidées sur les scénarios d’incident**, il agira en conséquence sans attendre l’apport du partenaire.

1. Adobe attend les mises à jour du partenaire et une notification du partenaire une fois le service restauré.

## Décisions prises d’avance sur les scénarios d’incident {#pre-agreed-descn}

Dans certains cas, une action par défaut est exécutée dans le cas de l’occurrence de ce scénario :

|   | Scénario | Description | Actions |
|---|---|---|---|
| S1 | Adobe identifie un problème avec l’intégration d’un MVPD lors d’opérations de production normales. | Lors des opérations de production normales, Adobe identifie un problème avec l’un des MVPD qui empêche l’exécution des flux d’authentification/d’autorisation (par exemple, certificats expirés, réponses SAML expirées, ports fermés, paramètres modifiés, etc.). | - L&#39;Adobe informera les programmeurs et MVPD concernés.  </br> </br> - L’Adobe désactivera ce MVPD pour tous les programmeurs affectés. </br> </br> - L’Adobe ouvrira un ticket avec le MVPD suivant la procédure de support convenue avec ce MVPD |
| S2 | Adobe active un nouveau MVPD pour un programmeur, et le programmeur autorise le MVPD avant la date de lancement. | Adobe active un nouveau MVPD pour un site de programmeur, et le site affiche déjà le nouveau MVPD dans le sélecteur, même s’il n’était pas censé le faire. | - Adobe informera le programmeur que le nouveau MVPD apparaîtra dans le sélecteur avant la date planifiée. </br> </br> - Le programmeur prendra des mesures pour le supprimer du sélecteur, si nécessaire. |
| S3 | Adobe active un nouveau MVPD pour un programmeur même si le MVPD n’est pas prêt pour la production | Adobe active un nouveau MVPD pour un programmeur, mais le MVPD n’a pas encore déployé la prise en charge de l’intégration afin que les flux d’authentification/d’autorisation ne puissent pas être effectués. | - L’Adobe effectue le déploiement uniquement si le programmeur </br> le demande. </br> - Le programmeur sera chargé d’assurer l’autorisation du MVPD une fois tous les tests effectués. |

## Attentes En Matière De Réponse Pour Les Incidents De Gravité 1 {#response-expectations-for-severity-one-incidents}

* Réponse initiale : 30 minutes (24/7)
* Plan d’action : 1 heure (24/7)
* Résolution : ASAP (24/7)
