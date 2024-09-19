---
title: Guide pas à pas de l’API REST V2 (client à serveur)
description: Guide pas à pas de l’API REST V2 (client à serveur)
source-git-commit: 709835276710ec4b92abec3e39aaecae99872e77
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---


# Guide pas à pas de l’API REST V2 (client à serveur) {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

## Procédure de mise en oeuvre de l’API REST V2 dans les applications côté client {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

Pour mettre en oeuvre l’API REST Adobe Pass V2, vous devez suivre les étapes ci-dessous regroupées en phases.

## A. Phase d’enregistrement {#registration-phase}

### Étape 1 : Enregistrement de votre application {#step-1-register-your-application}

Pour que l’application puisse appeler l’API REST Adobe Pass V2, elle a besoin d’un jeton d’accès requis par la couche de sécurité de l’API.

Pour obtenir le jeton d’accès, l’application doit suivre les étapes décrites : [Enregistrement du client dynamique](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B. Phase d’authentification {#authentication-phase}

### Étape 2 : recherche des profils authentifiés existants {#step-2-check-for-existing-authenticated-profiles}

Les vérifications d’applications de diffusion en continu pour les profils authentifiés existants : <b>/api/v2/{serviceProvider}/profiles</b><br>
([Récupération des profils authentifiés](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md))

* Si aucun profil n’a été trouvé et que l’application de diffusion en continu implémente un flux TempPass
   * Consultez la documentation sur la mise en oeuvre des [flux d’accès temporaires](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Si aucun profil n’a été trouvé, l’application de diffusion en continu implémente un flux d’authentification.
   * L’application de diffusion en continu récupère la liste des MVPD disponibles pour serviceProvider : <b>/api/v2/{serviceProvider}/configuration</b><br>
([Récupérer la liste des MVPD disponibles](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * L’application de diffusion en continu peut mettre en oeuvre le filtrage sur la liste des MVPD et afficher uniquement les MVPD prévus tout en masquant d’autres (TempPass, tests MVPD, MVPD en cours de développement, etc.)
   * Sélecteur d’affichage d’application de diffusion en continu, l’utilisateur sélectionne le MVPD
   * L’application de diffusion en continu crée une session : <b>/api/v2/{serviceProvider}/sessions</b><br>
([Créer une session d’authentification](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * un CODE et une URL à utiliser pour l’authentification sont renvoyés.
      * si un profil est trouvé, l’application de diffusion en continu peut passer à <a href="#preauthorization-phase">C. Phase de préautorisation</a> ou <a href="#authorization-phase">D. Phase d’autorisation</a>

### Étape 3 : Authentification de l’utilisateur {#step-3-authenticate-the-user}

Utilisation d’un navigateur ou d’une application web de deuxième écran :

* Option 1. L’application de diffusion en continu peut ouvrir un navigateur ou une vue web, charger l’URL pour s’authentifier et l’utilisateur arrive sur la page de connexion MVPD où les informations d’identification doivent être envoyées.
   * l’utilisateur saisit son identifiant/mot de passe, la redirection finale affiche une page de succès.
* Option 2. L’application de diffusion en continu ne peut pas ouvrir un navigateur et simplement afficher le CODE. <b>Une application web distincte doit être développée</b> pour demander à l’utilisateur de saisir le CODE, de créer et d’ouvrir l’URL : <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * l’utilisateur saisit son identifiant/mot de passe, la redirection finale affiche une page de succès.

### Étape 4 : Recherche de profils authentifiés {#step-4-check-for-authenticated-profiles}

L’application de diffusion en continu recherche une authentification avec MVPD à effectuer dans le navigateur ou le deuxième écran

* L&#39;interrogation toutes les 15 secondes est recommandée sur <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Récupération des profils authentifiés pour une MVPD spécifique](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Si la sélection MVPD n’est pas effectuée dans l’application de diffusion en continu, car le sélecteur MVPD est présenté dans l’application du deuxième écran, l’interrogation doit avoir lieu avec le CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Récupération des profils authentifiés pour un CODE spécifique](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* L’interrogation ne doit pas dépasser 30 minutes, dans le cas où 30 minutes sont atteintes et que l’application de diffusion en continu est toujours active, une nouvelle session doit être lancée et un nouveau CODE et une URL sont renvoyés.
* Une fois l’authentification terminée, le retour est de 200 avec le profil authentifié
* L&#39;application de diffusion en continu peut passer à <a href="#preauthorization-phase">C. Phase de préautorisation</a> ou <a href="#authorization-phase">D. Phase d’autorisation</a>

## C. Phase de préautorisation {#preauthorization-phase}

### Etape 5 : Recherche des ressources préautorisées {#step-5-check-for-preauthorized-resources}

L’application de diffusion en continu se prépare à afficher les vidéos disponibles pour l’utilisateur authentifié et peut vérifier la variable
accès à ces ressources.

* L’étape est facultative et exécutée si l’application souhaite filtrer les ressources non disponibles dans le package utilisateur authentifié.
* Appelez à <b>/api/v2/{serviceProvider}/requests/preauthorized/{mvpd}</b><br>
([Récupérer la décision de préautorisation à l’aide de MVPD spécifique](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. Phase d’autorisation {#authorization-phase}

### Etape 6 : Recherche des ressources autorisées {#step-6-check-for-authorized-resources}

L’application de diffusion en continu se prépare à lire une vidéo/ressource/ressource sélectionnée par l’utilisateur.

* Une étape est nécessaire pour chaque démarrage de lecture
* Appelez <b>/api/v2/{serviceProvider}/décision/autoriser/{mvpd}</b><br>
([Récupérer la décision d’autorisation à l’aide de MVPD spécifique](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * décision = &quot;Permit&quot; , le périphérique de diffusion en continu commence la diffusion en continu
   * décision = &quot;Refuser&quot;, le périphérique de diffusion informe l’utilisateur qu’il n’a pas accès à cette vidéo.

## E. Phase de déconnexion {#logout-phase}

### Étape 7 : déconnexion {#step-7-logout}

Appareil de diffusion en continu : l’utilisateur souhaite se déconnecter du MVPD

* Appelez <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Lancement de la déconnexion pour MVPD spécifique](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Si la réponse actionType=&#39;interactive&#39; et url est présente, ouvrez l’url dans un navigateur/deuxième écran pour terminer la déconnexion avec MVPD.
