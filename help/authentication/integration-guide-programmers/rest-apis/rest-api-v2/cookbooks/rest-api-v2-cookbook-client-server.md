---
title: Guide pas à pas API REST V2 (client à serveur)
description: Guide pas à pas API REST V2 (client à serveur)
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '1833'
ht-degree: 0%

---

# Guide pas à pas API REST V2 (client à serveur) {#rest-api-v2-cookbook-client-to-server}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

Ce document est destiné aux développeurs qui intègrent l’API REST d’authentification Adobe Pass V2[ ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) dans leurs applications de streaming avec une architecture client à serveur (C2S).

## Conditions préalables {#prerequisites}

Pour connaître les termes et définitions, reportez-vous à la documentation [Glossaire de l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md).

Pour connaître les exigences obligatoires et les pratiques recommandées, reportez-vous à la documentation de la [Liste de contrôle de l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md).

Pour les questions fréquentes, reportez-vous à la documentation [FAQ sur l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md) .

## A. Phase d&#39;enregistrement {#registration-phase}

L’objectif de la phase d’enregistrement est d’enregistrer l’application de diffusion en continu par rapport à l’authentification Adobe Pass par le biais du processus d’enregistrement client dynamique (DCR).

Le processus d’enregistrement client dynamique (DCR) nécessite que l’application de diffusion en continu obtienne une paire d’informations d’identification client et récupère un jeton d’accès en tant qu’objectif final de la phase d’enregistrement.

La phase d’enregistrement est obligatoire, mais l’application de diffusion en continu peut ignorer cette phase si elle dispose d’une paire d’informations d’identification client mises en cache et d’un jeton d’accès toujours valides.

+++Articles connexes

**API:**

* [Récupérer les informations d’identification du client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Récupérer le jeton d’accès](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Flux:**

* [Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**FAQ :**

* [FAQ sur la phase d&#39;enregistrement](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### Étape 1 : enregistrer votre application {#step-1-register-your-application}

* **Récupérer les informations d’identification du client :** l’application de diffusion en continu récupère les informations d’identification du client en appelant le point d’entrée [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

   * L’application de diffusion en continu doit stocker les informations d’identification du client et les utiliser indéfiniment lorsque vous devez récupérer un jeton d’accès.


* **Récupérer le jeton d’accès :** l’application de diffusion en continu récupère le jeton d’accès en appelant le point d’entrée [**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

   * L’application de diffusion en continu doit stocker et utiliser le jeton d’accès jusqu’à son expiration, puis le supprimer et en obtenir un nouveau.

## B. Phase d’authentification {#authentication-phase}

La phase d’authentification a pour but de permettre à l’application de diffusion en continu de vérifier l’identité de l’utilisateur et d’obtenir des informations sur les métadonnées de l’utilisateur.

La phase d’authentification agit comme une étape préalable à la phase de préautorisation ou à la phase d’autorisation lorsque l’application de streaming doit lire du contenu.

+++Articles connexes

**API**

* [Créer une session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Reprendre la session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Récupérer la session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Authentification dans l’agent utilisateur](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Récupération des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Récupération du profil pour un fichier mvpd spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Récupération du profil pour un code spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**Flux**

* [Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**FAQ**

* [Questions fréquentes sur la phase d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### Étape 2 : vérifier les profils authentifiés existants {#step-2-check-for-existing-authenticated-profiles}

* **Récupérer les profils :** l’application de diffusion en continu vérifie les profils existants en appelant le point d’entrée [**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md).


* **Scénario 1 :** il existe des profils existants, l’application de diffusion en continu peut passer à la [phase de préautorisation](#preauthorization-phase) ou à la [phase d’autorisation](#authorization-phase).


* **Scénario 2 :** il n’existe aucun profil, l’application de diffusion en continu peut passer à l’étape suivante pour [Authentifier l’utilisateur](#step-3-authenticate-the-user).


* **Scénario 3 :** il n’existe aucun profil, l’application de diffusion en continu peut procéder à la fourniture d’un accès temporaire à l’utilisateur par le biais de la fonctionnalité [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md).

   * Ce scénario n’entre pas dans le cadre de ce document. Pour plus d’informations[ consultez la documentation ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) Flux d’accès temporaires .

### Étape 3 : Authentifier l’utilisateur {#step-3-authenticate-the-user}

* **Récupérer la configuration :** l’application de diffusion en continu récupère la liste des MVPD disponibles en appelant le point d’entrée [**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

   * L’application de diffusion en continu peut mettre en œuvre un mécanisme de filtrage personnalisé pour affiner la liste des MVPD à partir de la réponse de configuration, en affichant uniquement les fournisseurs prévus tout en masquant les autres (par exemple, les MVPD en cours de développement, les MVPD de test, TempPass). Cela permet de s’assurer que les utilisateurs disposent d’une sélection organisée lors du choix de leur fournisseur de télévision.


* **Créer une session d’authentification :** l’application de diffusion en continu lance une session d’authentification en appelant le point d’entrée [**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).


* **Scénario 1 :** l’application de diffusion en continu peut ouvrir un navigateur ou une vue web. Elle doit donc charger le `url` d’authentification.

   * L’utilisateur envoie son nom d’utilisateur et son mot de passe dans la page de connexion de MVPD. Une fois l’authentification réussie, la redirection finale affiche une page de réussite.


* **Scénario 2 :** l’application de diffusion en continu ne peut pas ouvrir de navigateur. Elle doit donc afficher le `code` d’authentification. Une application web distincte est nécessaire pour inviter l’utilisateur à saisir le `code`, à créer le `url` d’authentification et à l’ouvrir : [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

   * L’utilisateur envoie son nom d’utilisateur et son mot de passe dans la page de connexion de MVPD. Une fois l’authentification réussie, la redirection finale affiche une page de réussite.

### Étape 4 : rechercher des profils authentifiés {#step-4-check-for-authenticated-profiles}

* **Récupérer le profil pour un code spécifique :** l’application de diffusion en continu doit implémenter un mécanisme d’interrogation à l’aide de l’`code` pour vérifier si le profil a été généré et enregistré avec succès en appelant le point d’entrée [**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).

   * L’application de diffusion en continu doit **démarrer le mécanisme d’interrogation** dans les conditions suivantes :

      * **Authentification effectuée dans l’application principale (écran) :** l’application principale (streaming) doit commencer à interroger l’utilisateur ou l’utilisatrice lorsqu’il ou elle atteint la page de destination finale, une fois que le composant de navigateur charge l’URL spécifiée pour le paramètre `redirectUrl` dans la requête de point d’entrée [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

      * **Authentification effectuée dans une application secondaire (écran) :** l’application principale (streaming) doit commencer à interroger les utilisateurs et utilisatrices dès qu’ils ou elles lancent le processus d’authentification, juste après avoir reçu la réponse du point d’entrée [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) et affiché le code d’authentification à l’utilisateur ou à l’utilisatrice.

   * L’application en flux continu doit **arrêter le mécanisme d’interrogation** dans les conditions suivantes :

      * **Authentification réussie :** les informations de profil de l’utilisateur sont récupérées avec succès, confirmant leur statut d’authentification. À ce stade, l’interrogation n’est plus nécessaire.

      * **Session d’authentification et expiration du code :** la session d’authentification et le code expirent, comme indiqué par la date et l’heure `notAfter` (par exemple, 30 minutes) dans la réponse de point d’entrée [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). Si cela se produit, l’utilisateur ou l’utilisatrice doit redémarrer le processus d’authentification et l’interrogation à l’aide du code d’authentification précédent doit être arrêtée immédiatement.

      * **Nouveau code d’authentification généré :** si l’utilisateur demande un nouveau code d’authentification sur l’appareil principal (écran), la session existante n’est plus valide et l’interrogation à l’aide du code d’authentification précédent doit être arrêtée immédiatement.

   * L’application de diffusion en continu doit **configurer la fréquence du mécanisme d’interrogation** dans les conditions suivantes :

      * **Authentification effectuée dans l’application principale (écran) :** l’application principale (streaming) doit interroger les utilisateurs toutes les 3 à 5 secondes ou plus.

      * **Authentification effectuée dans une application secondaire (écran) :** l’application principale (streaming) doit interroger les utilisateurs toutes les 3 à 5 secondes ou plus.

   * L’application de diffusion en continu doit mettre en cache certaines parties des informations de profil de l’utilisateur dans un stockage persistant afin d’éviter les requêtes inutiles et d’améliorer l’expérience de l’utilisateur.

## C. Phase De Préautorisation (Facultatif) {#preauthorization-phase}

L’objectif de la phase de préautorisation est de fournir à l’application de streaming la possibilité de présenter un sous-ensemble de ressources de son catalogue auxquelles l’utilisateur ou l’utilisatrice serait autorisé à accéder.

La phase de préautorisation peut améliorer l’expérience de l’utilisateur lorsqu’il ouvre l’application de diffusion en continu pour la première fois ou accède à une nouvelle section.

La phase de préautorisation n’est pas obligatoire, l’application de diffusion en continu peut ignorer cette phase si elle souhaite présenter un catalogue de ressources sans les filtrer au préalable en fonction des droits de l’utilisateur.

+++Articles connexes

**API**

* [Récupération des décisions de préautorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Flux**

* [Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**FAQ**

* [FAQ sur la phase de préautorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### Étape 5 : rechercher des ressources préautorisées {#step-5-check-for-preauthorized-resources}

* **Récupérer les décisions de préautorisation :** l’application de diffusion en continu récupère les décisions de préautorisation pour une liste de ressources en appelant le point d’entrée [**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md).

   * L’application de diffusion en continu n’est pas nécessaire pour stocker les décisions de préautorisation dans le stockage persistant. Cependant, il est recommandé de mettre en cache les décisions d’autorisation en mémoire pour améliorer l’expérience de l’utilisateur. Cela permet d’éviter les appels inutiles de ressources déjà préautorisées, ce qui réduit la latence et améliore les performances.

   * L’application de diffusion en continu peut déterminer la raison d’un refus de décision de préautorisation en examinant le [ code d’erreur et le message ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) inclus dans la réponse à partir du point d’entrée de préautorisation des décisions . Ces détails fournissent à insight la raison spécifique pour laquelle la demande de préautorisation a été refusée, ce qui permet d’informer l’expérience utilisateur ou de déclencher toute gestion nécessaire dans l’application. Assurez-vous que tout mécanisme de reprise implémenté pour récupérer les décisions de préautorisation ne génère pas de boucle sans fin si la décision de préautorisation est refusée. Envisagez de limiter les reprises à un nombre raisonnable et de gérer les refus de manière élégante en présentant des commentaires clairs à l’utilisateur.

   * L’application de diffusion en continu peut obtenir une décision de préautorisation pour un nombre limité de ressources dans une seule requête API, généralement jusqu’à 5, en raison des conditions imposées par les MVPD. Ce nombre maximal de ressources peut être consulté et modifié après accord avec les MVPD via le tableau de bord Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre entreprise ou par un représentant Adobe Pass Authentication agissant en votre nom.


## D. Phase d’autorisation {#authorization-phase}

La phase d’autorisation a pour but de permettre à l’application de diffusion en continu de lire les ressources demandées par l’utilisateur après validation de ses droits avec MVPD.

La phase d’autorisation est obligatoire, l’application de diffusion en continu ne peut pas ignorer cette phase si elle souhaite lire les ressources demandées par l’utilisateur, car elle doit vérifier auprès du MVPD que l’utilisateur a bien le droit de lire le flux avant de le publier.

+++Articles connexes

**API**

* [Récupérer les décisions d’autorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Flux**

* [Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**FAQ**

* [FAQ sur la phase d’autorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### Étape 6 : vérifier les ressources autorisées {#step-6-check-for-authorized-resources}

* **Récupérer la décision d’autorisation :** l’application de diffusion en continu récupère la décision d’autorisation pour une ressource spécifique en appelant le point d’entrée [**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md).

   * L’application de diffusion en continu n’est pas nécessaire pour stocker les décisions d’autorisation dans un stockage persistant.

   * L’application de diffusion en continu peut déterminer la raison d’un refus de décision d’autorisation en examinant le [ code d’erreur et le message ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) inclus dans la réponse à partir du point d’entrée Autoriser les décisions . Ces détails fournissent à insight la raison spécifique pour laquelle la demande d’autorisation a été refusée, ce qui permet d’informer l’expérience utilisateur ou de déclencher toute gestion nécessaire dans l’application. Assurez-vous que tout mécanisme de reprise implémenté pour récupérer les décisions d’autorisation ne génère pas de boucle sans fin si la décision d’autorisation est refusée. Envisagez de limiter les reprises à un nombre raisonnable et de gérer les refus de manière élégante en présentant des commentaires clairs à l’utilisateur.

   * L’application de diffusion en continu n’est pas nécessaire pour actualiser un jeton de média expiré pendant la lecture du flux. Si le jeton de média expire pendant la lecture, le flux doit pouvoir continuer sans interruption. Cependant, le client doit demander une nouvelle décision d’autorisation et obtenir un nouveau jeton de média la prochaine fois que l’utilisateur tente de lire une ressource.

   * L’application de diffusion en continu peut obtenir une décision d’autorisation pour un nombre limité de ressources dans une seule requête API, généralement jusqu’à 1, en raison des conditions imposées par les MVPD.

## E. Phase de déconnexion {#logout-phase}

L’objectif de la phase de déconnexion est de permettre à l’application de diffusion en continu de mettre fin au profil authentifié de l’utilisateur dans l’authentification Adobe Pass à la demande de l’utilisateur.

La phase de déconnexion est obligatoire, l’application de diffusion en continu doit permettre à l’utilisateur de se déconnecter.

+++Articles connexes

**API**

* [Lancer la déconnexion pour un fichier mvpd spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Flux**

* [Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**FAQ**

* [FAQ sur la phase de déconnexion](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### Étape 7 : Déconnexion {#step-7-logout}

* **Lancer la déconnexion d’Adobe Pass :** l’application de diffusion en continu lance le flux de déconnexion en appelant le point d’entrée [**/api/v2/{serviceProvider}/logout/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md).

   * L’application de diffusion en continu doit suivre les instructions fournies dans les attributs `actionName` et `actionType` de la réponse de point d’entrée de déconnexion pour s’assurer que le processus de déconnexion est correctement terminé.

      * Si l’attribut `actionType` de la réponse est défini sur « interactif » :

         * **Scénario 1 :** l’application de diffusion en continu peut ouvrir un navigateur ou une vue web. Elle doit donc charger le `url` de déconnexion.

         * **Scénario 2 :** l’application de diffusion en continu ne peut pas ouvrir de navigateur. Par conséquent, le processus de déconnexion peut être arrêté, car la session MVPD n’a pas été conservée dans le cache du navigateur d’un appareil de diffusion en continu.
