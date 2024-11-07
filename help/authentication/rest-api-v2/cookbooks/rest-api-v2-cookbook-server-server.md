---
title: Guide pas à pas de l’API REST V2 (serveur à serveur)
description: Guide pas à pas de l’API REST V2 (serveur à serveur)
source-git-commit: c17e52dd52fa14c50d59945598d1913f02be2468
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 0%

---

# Guide pas à pas de l’API REST V2 (serveur à serveur) {#rest-api-v2-cookbook-server-to-server}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


## Vue d’ensemble {#overview}

Ce guide de cuisine a pour but de détailler les bonnes pratiques pour implémenter l’authentification Adobe Pass à l’aide de l’API REST V2 dans une architecture serveur à serveur.  Il fournit des exigences de base, une implémentation de flux détaillée et des considérations générales pour les environnements de production et le fonctionnement.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .


## Composants {#components}

Dans une solution serveur à serveur opérationnelle, les composants suivants sont impliqués :


| Type | Composant | Description |
| --- | --- | --- |
| Appareil de diffusion en continu | Application de diffusion en continu | L’application de programmation qui réside sur l’appareil de diffusion en continu de l’utilisateur et lit la vidéo authentifiée. |
| | \[Facultatif\] Module AuthN | Si le périphérique de diffusion comporte un agent utilisateur (c’est-à-dire un navigateur web), le module AuthN est responsable de l’authentification de l’utilisateur sur le MVPD IdP. |
| \[Facultatif\] Périphérique AuthN | Application AuthN | Si le périphérique de diffusion en continu ne dispose pas d’un agent utilisateur (c’est-à-dire un navigateur web), l’application AuthN est une application web de programmeur accessible à partir d’un périphérique d’un utilisateur distinct à l’aide d’un navigateur web. |
| Infrastructure de programmation | Service de programmation | Service qui relie le périphérique de diffusion en continu au service Adobe Pass pour obtenir les décisions d’authentification et d’autorisation. |
| Infrastructure d’Adobe | Service Adobe Pass | Un service qui s’intègre au service MVPD IdP et AuthZ et qui fournit des décisions d’authentification et d’autorisation. |
| Infrastructure MVPD | MVPD IdP | Un point de terminaison MVPD qui fournit un service d’authentification basé sur les informations d’identification pour valider l’identité de l’utilisateur. |
| | Service MVPD AuthZ | Un point de terminaison MVPD qui fournit des décisions d’autorisation basées sur les abonnements de l’utilisateur, les contrôles parental, etc. |


Les termes supplémentaires utilisés dans le flux sont définis dans la variable
[Glossaire](/help/authentication/glossary.md).

Le diagramme suivant illustre l’ensemble du flux :

![](/help/authentication/assets/rest-api-v2/cookbooks/apass-servertoserver-cookbook.png)

### Procédure de mise en oeuvre de l’API REST V2 dans l’architecture serveur à serveur {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Pour mettre en oeuvre l’API REST Adobe Pass V2, vous devez suivre les étapes ci-dessous regroupées en phases.

## 0. Conditions préalables {#prerequisites}

Dans les mises en oeuvre serveur à serveur, l’application de diffusion en continu et le service de programmeur doivent établir un protocole pour que le service de programmation puisse :
* Identifier de manière unique l’application en flux continu sur l’appareil
* agir au nom de l’application de diffusion en continu et communiquer avec le service Adobe Pass ;
* collecter et stocker des informations sur l’application en flux continu et l’appareil comme l’adresse IP, le port source et les informations sur l’appareil pour les transmettre à Adobe Pass ;
* renvoyer des décisions et des instructions à l’application de diffusion en continu ;

Des paramètres tels que l’identifiant de l’appareil, l’identifiant du client, le secret du client (définis ci-dessous) peuvent être stockés sur l’application en flux continu ou le service de programmation.

## A. Phase d’enregistrement {#registration-phase}

### Étape 1 : Enregistrement de votre application {#step-1-register-your-application}

Pour que l’application puisse appeler l’API REST Adobe Pass V2, elle a besoin d’un jeton d’accès requis par la couche de sécurité de l’API. Pour les implémentations serveur à serveur, le service de programmation peut s’enregistrer au nom d’une instance d’application.
L’identifiant client et les valeurs secrètes du client doivent être obtenus pour chaque appareil de diffusion en continu.

Pour obtenir le jeton d’accès, le service de programmation peut agir au nom d’une application de diffusion en continu et doit suivre les étapes décrites : [Enregistrement du client dynamique](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B. Phase d’authentification {#authentication-phase}

### Étape 2 : recherche des profils authentifiés existants {#step-2-check-for-existing-authenticated-profiles}

Le service de programmation recherche, au nom de l’application de diffusion en continu, les profils authentifiés existants : `/api/v2/{serviceProvider}/profiles` ([Récupérer les profils authentifiés](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md))

* Si aucun profil n’a été trouvé et que l’application de diffusion en continu implémente un flux TempPass
   * Consultez la documentation sur la mise en oeuvre des [flux d’accès temporaires](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Si aucun profil n’a été trouvé, l’application de diffusion en continu implémente un flux d’authentification.
   * <b>Étape 2.a :</b> le service de programmation récupère la liste des MVPD disponibles pour serviceProvider : <b>/api/v2/{serviceProvider}/configuration</b><br>
([Récupérer la liste des MVPD disponibles](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Le service de programmation peut mettre en oeuvre un filtrage sur la liste des MVPD et afficher uniquement les MVPD prévus tout en masquant d’autres (TempPass, test des MVPD, MVPD en cours de développement, etc.)
   * Le service de programmation doit renvoyer une liste MVPD filtrée pour l’application de diffusion en continu à afficher dans le sélecteur. L’utilisateur sélectionne la MVPD.
   * avec le MVPD sélectionné dans l’application de diffusion en continu, le service de programmation crée une session : <b>/api/v2/{serviceProvider}/sessions</b><br>
([Créer une session d’authentification](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * un CODE et une URL à utiliser pour l’authentification sont renvoyés.
      * si un profil est trouvé, le service de programmation peut passer à <a href="#preauthorization-phase">C. Phase de préautorisation</a>
   * Le service de programmation doit renvoyer le code et l’URL à l’application de diffusion en continu.

### Étape 3 : Authentification de l’utilisateur {#step-3-authenticate-the-user}

Utilisation d’un navigateur ou d’une application web de deuxième écran :

* Option 1. L’application de diffusion en continu peut ouvrir un navigateur ou une vue web, charger l’URL pour s’authentifier et l’utilisateur arrive sur la page de connexion MVPD où les informations d’identification doivent être envoyées.
   * l’utilisateur saisit son identifiant/mot de passe, la redirection finale affiche une page de succès.
* Option 2. L’application de diffusion en continu ne peut pas ouvrir un navigateur et simplement afficher le CODE. <b> Une application web distincte, AuthN_APP, doit être développée</b> pour demander à l’utilisateur de saisir le CODE, de créer et d’ouvrir l’URL : <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * l’utilisateur saisit son identifiant/mot de passe, la redirection finale affiche une page de succès.

### Étape 4 : Recherche de profils authentifiés {#step-4-check-for-authenticated-profiles}

Le service de programmation recherche l’authentification avec MVPD à effectuer dans le navigateur ou le deuxième écran.

* L&#39;interrogation toutes les 15 secondes est recommandée sur <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Récupération des profils authentifiés pour une MVPD spécifique](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Si la sélection MVPD n’est pas effectuée dans l’application de diffusion en continu, car le sélecteur MVPD est présenté dans l’application du deuxième écran, l’interrogation doit avoir lieu avec le CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Récupération des profils authentifiés pour un CODE spécifique](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* L’interrogation ne doit pas dépasser 30 minutes, dans le cas où 30 minutes sont atteintes et que l’application de diffusion en continu est toujours active, une nouvelle session doit être lancée et un nouveau CODE et une URL sont renvoyés.
* Une fois l’authentification terminée, le retour est de 200 avec le profil authentifié
* Le service de programmation peut passer à <a href="#preauthorization-phase">C. Phase de préautorisation</a>

## C. Phase de préautorisation {#preauthorization-phase}

### Etape 5 : Recherche des ressources préautorisées {#step-5-check-for-preauthorized-resources}

Avec un profil d’authentification valide pour un utilisateur, le service de programmation peut vérifier la variable
accéder aux vidéos disponibles et transmettre la liste à l’application de diffusion en continu à afficher.

* L’étape est facultative et exécutée si l’application souhaite filtrer les ressources non disponibles dans le package utilisateur authentifié.
* Appelez à <b>/api/v2/{serviceProvider}/requests/preauthorized/{mvpd}</b><br>
([Récupérer la décision de préautorisation à l’aide de MVPD spécifique](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. Phase d’autorisation {#authorization-phase}

### Etape 6 : Recherche des ressources autorisées {#step-6-check-for-authorized-resources}

L’application de diffusion en continu se prépare à lire une vidéo/ressource/ressource sélectionnée par l’utilisateur.

* Une étape est nécessaire pour chaque démarrage de lecture
* L’application de diffusion en continu transmet ces informations au service de programmation.
* Le service de programmation au nom de l’application de diffusion en continu, appelez <b>/api/v2/{serviceProvider}/décision/autoriser/{mvpd}</b><br>
([Récupérer la décision d’autorisation à l’aide de MVPD spécifique](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * décision = &quot;Autoriser&quot; , le service de programmation demande à l’application de diffusion en continu de commencer la diffusion en continu.
   * décision = &quot;Refuser&quot;, le service de programmation demande à l’application de diffusion en continu d’informer l’utilisateur qu’elle n’a pas accès à cette vidéo.
   * Dans le processus, le service de programmation peut évaluer d’autres règles de fonctionnement et renvoyer la décision appropriée à l’application de diffusion en continu.

## E. Phase de déconnexion {#logout-phase}

### Étape 7 : déconnexion {#step-7-logout}

Application de diffusion en continu : l’utilisateur souhaite se déconnecter du MVPD.

* L’application de diffusion en continu informe le service de programmation qu’il doit se déconnecter du MVPD pour cette application spécifique.
* Le service de programmation peut nettoyer les informations qu’il stocke sur l’utilisateur authentifié.
* l’appel du service de programmation <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Lancement de la déconnexion pour MVPD spécifique](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Si la réponse actionType=&#39;interactive&#39; et url est présente, le service de programmation renvoie à l’application de diffusion en continu l’URL .
* En fonction des fonctionnalités existantes, l’application de diffusion en continu peut ouvrir l’URL dans le navigateur (généralement la même que celle utilisée pour l’authentification).
* Si l’application de diffusion en continu ne dispose pas de navigateur ou s’il s’agit d’une instance différente de celle de l’authentification, le flux peut être arrêté, car la session MVPD n’a pas été conservée dans le cache du navigateur.

## Environnements et exigences fonctionnelles{#environments}

Un programmeur doit créer au moins deux environnements : un pour la production et un ou plusieurs pour l’évaluation.


### Production

L’environnement de production doit être hautement disponible et adapté aux pics importants ou inattendus (par exemple, les sports en direct, la rupture
news).



Le service Adobe Pass s’exécute sur plusieurs centres de données géographiquement dispersés aux États-Unis.  Pour obtenir le meilleur temps de réponse (c’est-à-dire la latence la plus faible) à partir du service Adobe Pass, le programmeur doit également créer un service géographiquement dispersé similaire.
infrastructure.


Le service Programmer doit limiter le cache DNS à un maximum de 30 s au cas où l’Adobe aurait besoin d’annuler le trafic. Cela peut se produire si un centre de données n’est plus disponible.


Le programmeur doit fournir la plage IP publique de l’environnement de production. Elles seront entrées dans une liste autorisée d’adresses IP dans l’infrastructure Adobe Pass pour l’accès et gérées par les stratégies d’utilisation de l’API équitables d’Adobe.

### Évaluation

L’environnement d’évaluation peut être minimal, mais doit inclure tous les composants système et la logique métier. Il doit fonctionner de la même manière que la production et permettre le test des versions en dehors de la production. Idéalement, l’environnement d’évaluation peut être connecté aux environnements de test Adobe Pass pour être utilisé par le programmeur et par Adobe si nécessaire afin que nous puissions vous aider à tester et résoudre les problèmes.

### Exigences fonctionnelles

Le service de programmation doit transmettre des informations d’identification précises de l’appareil pour lequel il exécute les flux. De plus, le service Programmer doit transmettre l’adresse IP de l’appareil pour lequel il exécute les flux (dans un en-tête x-transfer-for) avec le port source de la connexion (dans le champ d’informations sur l’appareil) :

Le service de programmation doit envoyer les données et le formatage requis par les MVPD individuels ou les applications intégrées (par exemple, IP de l’appareil, port source, informations sur l’appareil, MRSS, données facultatives telles que l’ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.

Le service de programmation doit respecter le profil d’authentification et la validité des décisions lors de la mise en cache et de l’invalidation des authentifications ou des décisions en cas de notification.

Le service de programmation doit conserver les certificats partagés avec Adobe (pour les métadonnées utilisateur chiffrées).

## Informations connexes {#related}

* [Référence de l’API REST V2](/help/authentication/rest-api-v2/rest-api-v2-overview.md)
