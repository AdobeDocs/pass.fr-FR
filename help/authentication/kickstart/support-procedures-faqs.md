---
title: FAQ sur les procédures d’assistance
description: FAQ sur les procédures d’assistance
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 1b9847d8dcb078755fd68a6363972f8973290e52
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# FAQ sur les procédures d’assistance {#support-procedures-faqs}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Ce document présente les questions fréquentes (FAQ) sur les procédures d’assistance pour les incidents majeurs (niveau de gravité 1) affectant Adobe Pass Authentication et ses partenaires.

## Questions fréquentes {#faqs}

### Qu’est-ce qu’un incident de gravité 1 ? {#support-procedures-faqs-1}

Un incident de niveau de gravité 1 est une situation en direct dans l’environnement de production qui empêche l’achèvement des flux d’authentification ou d’autorisation pour un canal et un MVPD, ce qui a un impact sur un grand nombre d’abonnés.

Exemples d’incidents de GRAVITÉ 1

* Pendant le processus d’authentification, l’utilisateur n’est pas redirigé vers la page de connexion après avoir sélectionné le MVPD dans un navigateur pris en charge.

* Pendant le processus d’authentification, l’utilisateur est bloqué sur une page d’erreur d’Adobe sans pouvoir réinitialiser le flux d’authentification.

* Le partenaire reçoit de nombreux rapports que les utilisateurs ne peuvent pas authentifier ni autoriser avec un MVPD spécifique.

* L’activateur d’accès en production hébergé sur https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js n’est pas disponible.

### Qu’est-ce qu’un incident de niveau 1 sans gravité ?

L’Adobe prend en charge les enquêtes portant sur ces problèmes, mais ils ne sont pas considérés comme des incidents de GRAVITÉ 1 :

* Un ou plusieurs abonnés ne peuvent pas s’authentifier et restent sur la page de connexion de MVPD.

* Un ou plusieurs abonnés sont authentifiés, mais ne peuvent pas lire les vidéos.

* Certains ou tous les abonnés rencontrent une erreur JavaScript sur le site du programmeur.

### Comment les incidents de niveau GRAVITÉ 1 sont-ils gérés ?

Un incident de niveau de gravité 1 peut être déclenché par un Adobe ou un partenaire d’authentification Adobe Pass. Les étapes de chacune d’elles sont décrites ci-dessous.

**Flux initié par le partenaire**

1. Le partenaire identifie un incident de niveau de gravité 1 nécessitant l&#39;intervention immédiate de l&#39;Adobe.

1. Le partenaire envoie un e-mail à **tve-support@adobe.com** en incluant **URGENT - INCIDENT** dans l’objet et en ajoutant les informations suivantes :
   * Titre
   * Description et étapes à reproduire
   * Système d’exploitation/navigateur
   * SDK et version
   * Appareils affectés
   * % utilisateurs affectés
   * Trace HTTP ou journaux des appareils présentant le problème
   * (facultatif) Toutes les captures d’écran ou vidéos disponibles présentant le problème

1. Si l&#39;Adobe ne répond pas au ticket dans un délai donné, le partenaire peut appeler le numéro suivant : **1-205-693-9813**.

>[!IMPORTANT]
>
> Si vous n&#39;incluez pas « URGENT-INCIDENT » dans le titre du billet, il ne sera pas repris par notre système de notification.

**flux initié par l’Adobe**

Pour un problème d’authentification Adobe Pass :

1. Adobe identifie un problème interne et ouvre un ticket dans notre système de tracking.

1. L’Adobe informe le responsable du programme et le contact technique du partenaire, en spécifiant le numéro du ticket et l’impact estimé du problème.

1. Adobe s&#39;efforce de résoudre l&#39;incident et tient tous les partenaires concernés informés.

Pour un problème de partenaire (Programmeur/MVPD) :

1. Adobe identifie un problème lié à l’intégration à un MVPD ou à l’un des sites du programmeur.

1. L’Adobe avertit le partenaire concerné en suivant les procédures d’assistance en place avec ce partenaire et ouvre un ticket auprès de l’organisation d’assistance du partenaire.

1. Si, au cours de l’analyse d’impact, l’Adobe constate que le problème relève de l’une des décisions préconvenues concernant les scénarios d’incident, il agira en conséquence sans attendre l’avis du partenaire.

1. L’Adobe attend les mises à jour du partenaire et une notification une fois le service restauré.

### Quelles sont les décisions préconvenues concernant les scénarios d’incident ?

Certaines situations avec des actions par défaut qui seront effectuées si le scénario se produit :

|    | Scénario | Description | Actions |
|----|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | L’Adobe identifie un problème lié à une intégration de MVPD lors des opérations de production normales. | Lors des opérations de production normales, l’Adobe détecte un problème avec l’un des MVPD qui rend impossible l’exécution des flux d’authentification/autorisation (par exemple, certificats expirés, réponses SAML expirées, ports fermés, paramètres modifiés, etc.) | Adobe avertira le MVPD et les programmeurs concernés.  </br></br> Adobe désactivera ce MVPD pour tous les programmeurs concernés. </br></br>’Adobe ouvrira un ticket auprès du MVPD selon la procédure d’assistance convenue avec ce MVPD |
| S2 | L’Adobe active un nouveau MVPD pour un programmeur et le programmeur autorise le MVPD avant la date de lancement. | Adobe active un nouveau MVPD pour le site d’un programmeur, et le site affiche déjà le nouveau MVPD dans le sélecteur, même s’il n’était pas censé le faire. | Adobe informera le programmeur de la nouvelle MVPD apparaissant dans le sélecteur avant la date planifiée. </br></br> programmeur prendra des mesures pour le supprimer du sélecteur si nécessaire. |
| S3 | L’Adobe active un nouveau MVPD pour un programmeur même si le MVPD n’est pas prêt à passer en production | Adobe active un nouveau MVPD pour un programmeur, mais le MVPD n’a pas encore déployé la prise en charge de l’intégration, de sorte que les flux d’authentification/autorisation ne peuvent pas être effectués | L’Adobe effectue le déploiement uniquement si le programmeur le demande. </br></br> Le programmeur est chargé de s’assurer que l’autorisation du MVPD est obtenue une fois tous les tests effectués. |
