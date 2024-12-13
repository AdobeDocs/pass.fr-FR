---
title: Procédures d’escalade
description: Procédures d’escalade
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---

# Procédures d’escalade {#escalation-procedures}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
> 
>Appelez la ligne d’assistance téléphonique : **+1-205-693-9813** et envoyez un e-mail à **tve-support@adobe.com** en incluant **URGENT - INCIDENT** dans l’objet.

## Introduction {#introduction}

Ce document décrit les procédures d’assistance pour les incidents majeurs (niveau **GRAVITÉ 1**) affectant l’authentification Adobe Pass, la surveillance simultanée d’Adobe Pass et ses partenaires.


## Définition d’un incident de gravité 1 {#definition-of-a-severity-1-level-incident}

Un incident de niveau **GRAVITÉ 1** est une situation **EN DIRECT**, **se produisant dans l’environnement de production**, qui ne permet pas l’achèvement des flux d’authentification et/ou d’autorisation pour un canal et un MVPD, affectant un grand nombre d’abonnés du MVPD exécutant le flux.


## Exemples d’incidents de GRAVITÉ 1 {#examples-of-severity-1-incidentcs}

* L’activateur d’accès en production hébergé sur `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js` (ou `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`) n’est pas disponible.

* Pour un MVPD spécifique, Adobe ne redirige/n’affiche plus la page de connexion une fois que l’utilisateur a sélectionné le MVPD (dans l’un des navigateurs pris en charge).

* Le partenaire reçoit un grand nombre de rapports que les utilisateurs ne peuvent pas authentifier/autoriser avec un MVPD spécifique.

* Pendant le processus d’authentification, l’utilisateur est bloqué sur une page d’erreur d’Adobe sans avoir la possibilité de relancer le flux d’authentification/d’autorisation.


| Exemples d’incidents de gravité **NOT** 1 |
|---|
| Pour les problèmes de ce type, l’Adobe fournira un soutien pour les enquêtes, mais il ne s’agit pas d’incidents de gravité 1 :<ul><li>Un ou plusieurs abonnés ne peuvent pas exécuter le flux en raison d’un problème de version de Flash (Flash manquant, bloqueurs de Flash, version de Flash incorrecte).</li><li>Un ou plusieurs abonnés ne peuvent pas s’authentifier et restent sur la page de connexion de MVPD.</li><li>Un ou plusieurs abonnés sont authentifiés mais ne peuvent pas lire les vidéos.</li><li>Un/quelques/tous les abonnés rencontrent une erreur JavaScript sur le site du programmeur</li></ul> |

## Flux d’escalade de gravité 1 {#severity-1-escalation-flows}

Les incidents de gravité 1 peuvent être déclenchés par un Adobe ou un partenaire d’authentification Adobe Pass. Les étapes de chacune d’elles sont présentées ci-dessous.

### Flux initié par le partenaire {#partner-initiated-flow}

1. Le partenaire identifie un incident de gravité 1 (tel que décrit ci-dessus) nécessitant l&#39;intervention immédiate de l&#39;Adobe.
1. Le partenaire envoie un e-mail à **tve-support@adobe.com** en incluant **URGENT - INCIDENT** dans l’objet et en ajoutant les informations suivantes :
   * Titre
   * Description et étapes à reproduire
   * Système d’exploitation/navigateur
   * SDK et version
   * Appareils affectés
   * % utilisateurs affectés
   * Trace HTTP ou journaux des appareils présentant le problème
   * (facultatif) Toutes les captures d’écran ou vidéos disponibles présentant le problème
1. Si l’Adobe ne répond pas au ticket dans les 30 minutes, le partenaire appelle le numéro suivant :
   **1-205-693-9813**
   >[!IMPORTANT]
   >Si vous n&#39;incluez pas « URGENT-INCIDENT » dans le titre du billet, il ne sera pas récupéré par notre système de notification**.

### Flux initié par l’Adobe {#adobe-initiated-flow}

#### ...pour un problème d’authentification Adobe Pass {#adobe-initiated-flow-authn-issue}

1. Adobe identifie un problème interne et ouvre un ticket auprès de notre système de tracking.

1. L’Adobe informe le responsable du programme et le contact technique du partenaire, en spécifiant le numéro du ticket et l’impact estimé du problème.

1. Adobe travaille à la résolution de l&#39;incident et tient tous les partenaires touchés informés.

#### ...pour un problème de partenaire (Programmeur/MVPD) {#adobe-initiated-flow-partner-issue}

1. Adobe identifie un problème lié à l’intégration à un MVPD ou à l’un des sites du programmeur.

1. L’Adobe avertit le partenaire concerné <u>en suivant les procédures d’assistance en place avec ce partenaire</u> et ouvre un ticket auprès de l’organisation d’assistance du partenaire.

1. Si, lors de l&#39;analyse d&#39;impact, Adobe identifie que le problème appartient à l&#39;une des décisions pré-convenues sur les scénarios d&#39;incident, voir **Décisions pré-convenues sur les scénarios d&#39;incident**, il agira en conséquence sans attendre la contribution du partenaire.

1. L’Adobe attend les mises à jour du partenaire et une notification du partenaire une fois le service restauré.

## Décisions préalablement convenues concernant les scénarios d’incident {#pre-agreed-descn}

Dans certains cas, une action par défaut est effectuée dans le cas de l’occurrence de ce scénario :

|   | Scénario | Description | Actions |
|---|---|---|---|
| S1 | L’Adobe identifie un problème lié à une intégration de MVPD lors des opérations de production normales. | Lors des opérations de production normales, l’Adobe détecte un problème avec l’un des MVPD qui rend impossible l’exécution des flux d’authentification/autorisation (par exemple, certificats expirés, réponses SAML expirées, ports fermés, paramètres modifiés, etc.) | - L’Adobe avertira le MVPD et les programmeurs concernés.  </br> </br> - L’Adobe désactivera ce MVPD pour tous les programmeurs concernés. </br> </br> - L’Adobe ouvrira un ticket auprès du MVPD selon la procédure d’assistance convenue avec ce MVPD |
| S2 | L’Adobe active un nouveau MVPD pour un programmeur et le programmeur autorise le MVPD avant la date de lancement. | Adobe active un nouveau MVPD pour le site d’un programmeur, et le site affiche déjà le nouveau MVPD dans le sélecteur, même s’il n’était pas censé le faire. | - L’Adobe informera le programmeur de l’apparition du nouveau MVPD dans le sélecteur avant la date planifiée. </br> </br> - Le programmeur prendra des mesures pour le supprimer du sélecteur si nécessaire. |
| S3 | L’Adobe active un nouveau MVPD pour un programmeur même si le MVPD n’est pas prêt à passer en production | Adobe active un nouveau MVPD pour un programmeur, mais le MVPD n’a pas encore déployé la prise en charge de l’intégration, de sorte que les flux d’authentification/autorisation ne peuvent pas être effectués | - L’Adobe effectue le déploiement uniquement si le </br> du programmeur le lui demande </br> - Le programmeur sera chargé d&#39;assurer l&#39;autorisation du MVPD une fois tous les essais effectués. |

## Attentes En Matière D’Intervention Pour Les Incidents De Gravité 1 {#response-expectations-for-severity-one-incidents}

* Réponse Initiale : 30 Minutes (24/7)
* Plan D&#39;Action : 1 Heure (24/7)
* Résolution : ASAP (24/7)
