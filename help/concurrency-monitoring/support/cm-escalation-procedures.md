---
title: Procédures de réaffectation de la surveillance de simultanéité
description: Procédures de réaffectation de la surveillance de simultanéité
exl-id: eb110465-3a74-489e-a521-0e17f5aeecb8
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Procédures de réaffectation de la surveillance de simultanéité {#esc-procedures}

>[!NOTE]
>
>Appelez la ligne d’assistance téléphonique : +1-657-312-4623 et envoyez un e-mail aux `tve-support@adobe.com` en indiquant « URGENT - INCIDENT » dans l’objet.


## Introduction {#cm-escalation-intro}

Ce document décrit les procédures d’assistance pour les incidents majeurs (niveau **GRAVITÉ 1**) affectant l’authentification Adobe Pass, la surveillance simultanée d’Adobe Pass et ses partenaires.

## Définition de la gravité d’escalade 1 niveau {#defn-escl-sevrityone-level}

Un incident de niveau **GRAVITÉ 1** est une situation **EN DIRECT**, **se produisant dans l’environnement de production**, qui ne permet pas l’achèvement des flux d’authentification et/ou d’autorisation pour un canal et un MVPD, affectant un grand nombre d’abonnés du MVPD exécutant le flux.

## Exemples d’incidents de gravité 1 {#exampl-sevone-incident}

* L’activateur d’accès en production hébergé sur <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> n’est pas disponible.

* Pour un MVPD spécifique, Adobe ne redirige plus et n’affiche plus la page de connexion une fois que l’utilisateur a sélectionné le MVPD (dans l’un des navigateurs pris en charge).

* Le partenaire reçoit un grand nombre de rapports que les utilisateurs ne peuvent pas authentifier/autoriser avec un MVPD spécifique.

* Pendant le processus d’authentification, l’utilisateur est bloqué sur une page d’erreur d’Adobe sans avoir la possibilité de relancer le flux d’authentification/d’autorisation.


## Exemples d’incidents de gravité *NOT* 1 {#exampl-not-sev1}

*Pour les problèmes de ce type, Adobe prend en charge les enquêtes, mais il ne s’agit pas d’incidents de gravité 1 :*

* Un ou plusieurs abonnés ne peuvent pas exécuter le flux en raison d&#39;un problème de version Flash (Flash manquant, bloqueurs de Flash, version Flash incorrecte).
* Un ou plusieurs abonnés ne peuvent pas s’authentifier et restent sur la page de connexion de MVPD.
* Un ou plusieurs abonnés sont authentifiés mais ne peuvent pas lire les vidéos.
* Un/quelques/tous les abonnés rencontrent une erreur JavaScript sur le site du programmeur.

## Flux d’escalade de gravité 1 {#sevone-escalation-flows}

Les incidents de gravité 1 peuvent être déclenchés par Adobe ou par un partenaire d’authentification Adobe Pass. Les étapes de chacune d’elles sont présentées ci-dessous.

### Flux initié par le partenaire {#partner-initiated-flow}

1. Le partenaire identifie un incident de gravité 1 (comme décrit ci-dessus) nécessitant une attention immédiate de la part d&#39;Adobe.

1. Le partenaire envoie un e-mail à tve-support@adobe.com en incluant « URGENT - INCIDENT » dans l’objet et en ajoutant les informations suivantes :

   * Titre
   * Description et étapes à reproduire
   * SE
   * Navigateur
   * Version Flash
   * (facultatif) Toutes les captures d’écran ou vidéos disponibles présentant le problème

1. Si Adobe ne répond pas au ticket dans les 30 minutes, le partenaire appelle le numéro ci-dessous :

   * **1-205-693-9813**


**Si vous n&#39;incluez pas « URGENT-INCIDENT » dans le titre du billet, il ne sera pas récupéré par notre système de notification.**

### Flux initié par Adobe {#adobe-initiated-flow}

**...d’un problème d’authentification Adobe Pass**

1. Adobe identifie un problème interne et ouvre un ticket auprès de notre système de tracking.

1. Adobe informe le responsable du programme et le contact technique du partenaire, en spécifiant le numéro du ticket et l’impact estimé du problème.

1. Adobe s’efforce de résoudre l’incident et tient tous les partenaires concernés informés.


**...pour un problème de partenaire (Programmeur/MVPD)**

1. Adobe identifie un problème lié à l’intégration à un MVPD ou à l’un des sites du programmeur.

1. Adobe avertit le partenaire concerné **en suivant les procédures d’assistance en place avec ce partenaire** et ouvre un ticket auprès de l’organisation d’assistance du partenaire.

1. Si, lors de l’analyse d’impact, Adobe identifie que le problème appartient à l’une des décisions prédéfinies relatives aux scénarios d’incident (voir la section « Décisions prédéfinies relatives aux scénarios d’incident » ci-dessous), il agira en conséquence sans attendre le partenaire1. Entrée de .

1. Adobe attend les mises à jour du partenaire et une notification de ce dernier une fois le service restauré.

### Décisions préalablement convenues concernant les scénarios d’incident {#pre-agreed-decisions}

Dans certains cas, une action par défaut est effectuée dans le cas de l’occurrence de ce scénario :

|    | Scénario | Description | Actions |
|:--:|:------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | Adobe identifie un problème lié à une intégration de MVPD lors des opérations de production normales. | Lors des opérations normales de production, Adobe identifie un problème avec l’un des MVPD qui rend impossible l’exécution des flux d’authentification/autorisation (par exemple, certificats expirés, réponses SAML expirées, ports fermés, paramètres modifiés, etc.) | Adobe avertira les MVPD et les programmeurs concernés. Adobe désactivera ce MVPD pour tous les programmeurs concernés. Adobe ouvrira un ticket auprès du MVPD en suivant la procédure d’assistance convenue avec ce MVPD |
| S2 | Adobe active un nouveau MVPD pour un programmeur et le programmeur place le MVPD sur la liste autorisée avant la date de lancement. | Adobe active un nouveau MVPD pour le site d’un programmeur et le site affiche déjà le nouveau MVPD dans le sélecteur, même s’il n’était pas censé le faire. | Adobe avertira le programmeur du nouveau MVPD apparaissant dans le sélecteur avant la date planifiée. Le programmeur prendra des mesures pour le supprimer du sélecteur si nécessaire. |
| S3 | Adobe active un nouveau MVPD pour un programmeur même si le MVPD n’est pas prêt pour la production | Adobe active un nouveau MVPD pour un programmeur, mais le MVPD n’a pas encore déployé la prise en charge de l’intégration, de sorte que les flux d’authentification/autorisation ne peuvent pas être effectués | Adobe effectue le déploiement uniquement si le programmeur le demande. Le programmeur est chargé d’assurer la mise en whiteliste du MVPD une fois tous les tests effectués. |

### Attentes En Matière D’Intervention Pour Les Incidents De Gravité 1 {#response-expectations}

* Réponse Initiale : 30 Minutes (24/7)
* Plan D&#39;Action : 1 Heure (24/7)
* Résolution : ASAP (24/7)
