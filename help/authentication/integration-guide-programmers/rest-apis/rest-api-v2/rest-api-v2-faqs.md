---
title: FAQ sur l’API REST V2
description: FAQ sur l’API REST V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '9611'
ht-degree: 0%

---

# FAQ sur l’API REST V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Ce document fournit des réponses générales aux questions fréquentes sur l’adoption de l’API REST d’authentification Adobe Pass V2.

Pour plus d’informations sur l’API REST V2 dans son ensemble, consultez la documentation [ Présentation de l’API REST V2 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) .

## Questions fréquentes d’ordre général {#general-faqs}

Commencez par cette section si vous travaillez sur une application qui doit intégrer l’API REST V2, qu’il s’agisse d’une nouvelle application ou d’une application existante qui migre depuis [l’API REST V1](#migration-rest-api-v1-to-rest-api-v2) ou [SDK](#migration-sdk-to-rest-api-v2).

Pour plus d’informations sur les détails et les étapes de la migration, consultez également les sections suivantes.

### FAQ sur la phase d&#39;enregistrement {#registration-phase-faqs-general}

+++FAQ sur la phase d&#39;enregistrement

Reportez-vous à la documentation [FAQ sur l’enregistrement client dynamique (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### FAQ sur la phase de configuration {#configuration-phase-faqs-general}

+++FAQ sur la phase de configuration

#### &#x200B;1. Quel est l’objectif de la phase de configuration ? {#configuration-phase-faq1}

La phase de configuration a pour but de fournir à l’application cliente la liste des fichiers MVPD auxquels elle est activement intégrée, ainsi que les détails de la configuration (par exemple, `id`, `displayName`, `logoUrl`, etc.) enregistrés par l’authentification Adobe Pass pour chaque MVPD.

La phase de configuration fait office d’étape préalable à la phase d’authentification lorsque l’application cliente doit demander à l’utilisateur de sélectionner son fournisseur de télévision.

#### &#x200B;2. La phase de configuration est-elle obligatoire ? {#configuration-phase-faq2}

La phase de configuration n’est pas obligatoire, l’application cliente ne doit récupérer la configuration que lorsque l’utilisateur doit sélectionner son MVPD pour s’authentifier ou s’authentifier à nouveau.

L’application cliente peut ignorer cette phase dans les scénarios suivants :

* L’utilisateur est déjà authentifié.
* L’utilisateur se voit proposer un accès temporaire par le biais de la fonctionnalité de base ou promotionnelle [TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md).
* L’authentification de l’utilisateur a expiré, mais l’application cliente a mis en cache le MVPD précédemment sélectionné en tant que choix motivé par l’expérience utilisateur, et invite simplement l’utilisateur à confirmer qu’il est toujours abonné à ce MVPD.

#### &#x200B;3. Qu’est-ce qu’une configuration et combien de temps est-elle valide ? {#configuration-phase-faq3}

La configuration est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration).

La configuration contient une liste de fichiers MVPD définis par les attributs suivants `id`, `displayName`, `logoUrl`, etc., qui peuvent être récupérés à partir du point d’entrée [Configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

L’application cliente ne doit récupérer la configuration que lorsque l’utilisateur doit sélectionner son MVPD pour s’authentifier ou se réauthentifier.

L’application cliente peut utiliser la réponse de configuration pour présenter un sélecteur d’interface utilisateur avec les options MVPD disponibles chaque fois que l’utilisateur doit sélectionner son fournisseur de télévision.

L’application cliente doit stocker l’identifiant MVPD sélectionné par l’utilisateur, tel que spécifié par l’attribut `id` au niveau de la configuration MVPD, pour procéder aux phases d’authentification, de préautorisation, d’autorisation ou de déconnexion.

Pour plus d’informations, consultez la documentation [Récupérer la configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### &#x200B;4. La configuration est-elle spécifique à un fournisseur, une plateforme ou un utilisateur ? {#configuration-phase-faq4}

La configuration est spécifique à un [prestataire](rest-api-v2-glossary.md#service-provider).

La configuration est spécifique à un type de plateforme.

La configuration n’est pas spécifique à un utilisateur.

Pour les applications clientes utilisant une architecture serveur à serveur, il est recommandé de mettre en cache la réponse de configuration (par exemple, avec une durée de vie de 2 minutes) pour chaque type de plateforme dans le stockage de la mémoire côté serveur. Cela réduit les requêtes inutiles pour chaque utilisateur et améliore l’expérience globale de l’utilisateur.

#### &#x200B;5. L’application cliente doit-elle mettre en cache les informations de réponse de configuration dans un espace de stockage persistant ? {#configuration-phase-faq5}

>[!IMPORTANT]
> 
> Pour les applications clientes utilisant une architecture serveur à serveur, il est recommandé de mettre en cache la réponse de configuration (par exemple, avec une durée de vie de 2 minutes) pour chaque type de plateforme dans le stockage de la mémoire côté serveur. Cela réduit les requêtes inutiles pour chaque utilisateur et améliore l’expérience globale de l’utilisateur.

L’application cliente ne doit récupérer la configuration que lorsque l’utilisateur doit sélectionner son MVPD pour s’authentifier ou se réauthentifier.

L’application cliente doit mettre en cache les informations de réponse de configuration dans un espace de stockage mémoire afin d’éviter les requêtes inutiles et d’améliorer l’expérience utilisateur lorsque :

* L’utilisateur est déjà authentifié.
* L’utilisateur se voit proposer un accès temporaire par le biais de la fonctionnalité de base ou promotionnelle [TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md).
* L’authentification de l’utilisateur a expiré, mais l’application cliente a mis en cache le MVPD précédemment sélectionné en tant que choix motivé par l’expérience utilisateur, et invite simplement l’utilisateur à confirmer qu’il est toujours abonné à ce MVPD.

#### &#x200B;6. L’application cliente peut-elle gérer sa propre liste de fichiers MVPD ? {#configuration-phase-faq6}

L’application cliente peut gérer sa propre liste de fichiers MVPD, mais elle doit synchroniser les identifiants MVPD avec l’authentification Adobe Pass. Par conséquent, il est recommandé d’utiliser la configuration fournie par Authentification Adobe Pass pour vous assurer que la liste est à jour et précise.

L’application cliente recevrait une [erreur](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) de l’API REST d’authentification Adobe Pass V2 si l’identifiant MVPD fourni n’est pas valide ou si elle ne dispose pas d’une intégration active avec le [fournisseur de services](rest-api-v2-glossary.md#service-provider) spécifié.

#### &#x200B;7. L’application cliente peut-elle filtrer la liste des fichiers MVPD ? {#configuration-phase-faq7}

L’application cliente peut filtrer la liste des MVPD fournie dans la réponse de configuration en implémentant un mécanisme personnalisé en fonction de sa propre logique commerciale et de ses exigences, telles que l’emplacement de l’utilisateur ou l’historique de la sélection précédente.

L’application cliente peut filtrer la liste des MVPD [TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md) ou des MVPD dont l’intégration est toujours en cours de développement ou de test.

#### &#x200B;8. Que se passe-t-il si l’intégration à un MVPD est désactivée et marquée comme inactive ? {#configuration-phase-faq8}

Lorsque l’intégration à un MVPD est désactivée et marquée comme inactive, le MVPD est supprimé de la liste des MVPD fournie dans les autres réponses de configuration. Il faut tenir compte de deux conséquences importantes :

* Les utilisateurs non authentifiés de ce MVPD ne pourront plus terminer la phase d’authentification à l’aide de ce MVPD.
* Les utilisateurs authentifiés de ce MVPD ne pourront plus terminer les phases de préautorisation, d’autorisation ou de déconnexion à l’aide de ce MVPD.

L’application cliente recevrait une [erreur](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) de l’API REST d’authentification Adobe Pass V2 si l’utilisateur a sélectionné MVPD ne dispose plus d’une intégration active avec le [fournisseur de services](rest-api-v2-glossary.md#service-provider) spécifié.

#### &#x200B;9. Que se passe-t-il si l’intégration à un MVPD est activée à l’arrière et marquée comme active ? {#configuration-phase-faq9}

Lorsque l’intégration à un MVPD est réactivée et marquée comme active, le MVPD est de nouveau inclus dans la liste des MVPD fournie dans les autres réponses de configuration. Il faut tenir compte de deux conséquences importantes :

* Les utilisateurs non authentifiés de ce MVPD pourront à nouveau terminer la phase d’authentification à l’aide de ce MVPD.
* Les utilisateurs authentifiés de ce MVPD pourront à nouveau terminer les phases de préautorisation, d’autorisation ou de déconnexion à l’aide de ce MVPD.

#### &#x200B;10. Comment activer ou désactiver l’intégration à un MVPD ? {#configuration-phase-faq10}

Cette opération peut être effectuée via le tableau de bord Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre organisation ou par un représentant Adobe Pass Authentication agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Questions fréquentes sur la phase d’authentification {#authentication-phase-faqs-general}

+++Questions fréquentes sur la phase d’authentification

#### &#x200B;1. Quel est l’objectif de la phase d’authentification ? {#authentication-phase-faq1}

La phase d’authentification a pour but de permettre à l’application cliente de vérifier l’identité de l’utilisateur et d’obtenir des informations sur les métadonnées de l’utilisateur.

La phase d’authentification agit comme une étape préalable à la phase de préautorisation ou à la phase d’autorisation lorsque l’application cliente doit lire du contenu.

#### &#x200B;2. La phase d’authentification est-elle obligatoire ? {#authentication-phase-faq2}

La phase d’authentification est obligatoire, l’application cliente doit authentifier l’utilisateur lorsqu’elle ne dispose pas d’un profil valide dans l’authentification Adobe Pass.

L’application cliente peut ignorer cette phase dans les scénarios suivants :

* L’utilisateur est déjà authentifié et le profil est toujours valide.
* L’utilisateur se voit proposer un accès temporaire par le biais de la fonctionnalité de base ou promotionnelle [TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md).

La gestion des erreurs de l’application cliente nécessite de gérer les codes [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) (par exemple, `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated`, etc.), qui indiquent que l’application cliente nécessite que l’utilisateur s’authentifie.

#### &#x200B;3. Qu’est-ce qu’une session d’authentification et combien de temps est-elle valide ? {#authentication-phase-faq3}

La session d’authentification est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session).

La session d’authentification stocke des informations sur le processus d’authentification lancé qui peuvent être récupérées à partir du point d’entrée Sessions.

La session d’authentification est valide pendant une période limitée et courte spécifiée au moment de l’émission par l’horodatage `notAfter`, indiquant le temps dont dispose l’utilisateur pour terminer le processus d’authentification avant de devoir redémarrer le flux.

L’application cliente peut utiliser la réponse de session d’authentification pour savoir comment procéder au processus d’authentification. Notez qu’il existe des cas où l’utilisateur n’est pas tenu de s’authentifier, par exemple lorsqu’il fournit un accès temporaire, un accès dégradé ou lorsqu’il est déjà authentifié.

Pour plus d’informations, consultez les documents suivants :

* [Créer une API de session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API Reprendre la session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;4. Qu’est-ce qu’un code d’authentification et combien de temps est-il valide ? {#authentication-phase-faq4}

Le code d’authentification est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code).

Le code d’authentification stocke une valeur unique générée lorsqu’un utilisateur lance le processus d’authentification et identifie de manière unique la session d’authentification d’un utilisateur jusqu’à ce que le processus soit terminé ou jusqu’à l’expiration de la session d’authentification.

Le code d’authentification est valide pendant une période limitée et courte spécifiée au moment du lancement de la session d’authentification par l’horodatage `notAfter`, indiquant le temps dont dispose l’utilisateur pour terminer le processus d’authentification avant de devoir redémarrer le flux.

L’application cliente peut utiliser le code d’authentification pour vérifier si l’utilisateur a terminé l’authentification avec succès et récupérer les informations de profil de l’utilisateur, généralement par le biais d’un mécanisme d’interrogation.

L’application cliente peut également utiliser le code d’authentification pour permettre à l’utilisateur ou à l’utilisatrice de terminer ou de reprendre le processus d’authentification sur le même appareil ou sur un second (écran), étant donné que la session d’authentification n’a pas expiré.

Pour plus d’informations, consultez les documents suivants :

* [Créer une API de session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API Reprendre la session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;5. Comment l’application cliente peut-elle savoir si l’utilisateur a saisi un code d’authentification valide et si la session d’authentification n’a pas encore expiré ? {#authentication-phase-faq5}

L’application cliente peut valider le code d’authentification saisi par l’utilisateur dans une application secondaire (écran) en envoyant une requête à l’un des points d’entrée Sessions chargés de reprendre la session d’authentification ou de récupérer les informations de session d’authentification associées au code d’authentification.

L’application cliente recevait une [erreur](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) si le code d’authentification fourni était mal saisi ou si la session d’authentification arrivait à expiration.

Pour plus d’informations, reportez-vous aux documents [Reprendre la session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) et [Récupérer la session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### &#x200B;6. Comment l’application cliente peut-elle savoir si l’utilisateur est déjà authentifié ? {#authentication-phase-faq6}

L’application cliente peut interroger l’un des points d’entrée suivants capable de vérifier si un utilisateur est déjà authentifié et de renvoyer des informations de profil :

* [API du point d’entrée des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Point d’entrée des profils pour une API MVPD spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Point d’entrée des profils pour une API de code spécifique (authentification)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Pour plus d’informations, reportez-vous aux documents suivants :

* [Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### &#x200B;7. Qu’est-ce qu’un profil et combien de temps est-il valide ? {#authentication-phase-faq7}

Le profil est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile).

Le profil stocke des informations sur la validité d’authentification de l’utilisateur, des informations de métadonnées et bien d’autres informations qui peuvent être récupérées à partir du point d’entrée des profils.

L’application cliente peut utiliser le profil pour connaître le statut d’authentification de l’utilisateur, accéder aux informations de métadonnées de l’utilisateur, trouver la méthode utilisée pour s’authentifier ou l’entité utilisée pour fournir l’identité.

Pour plus d’informations, reportez-vous aux documents suivants :

* [API du point d’entrée des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Point d’entrée des profils pour une API MVPD spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Point d’entrée des profils pour une API de code spécifique (authentification)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Le profil est valide pendant une période limitée spécifiée lors de l’interrogation de l’horodatage `notAfter`, indiquant la durée pendant laquelle l’utilisateur dispose d’une authentification valide avant de devoir repasser par la phase d’authentification.

Cette période limitée appelée authentification (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) peut être affichée et modifiée via le tableau de bord Adobe Pass [TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre organisation ou par un représentant Adobe Pass Authentication agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### &#x200B;8. L’application cliente doit-elle mettre en cache les informations de profil de l’utilisateur dans un espace de stockage persistant ? {#authentication-phase-faq8}

L’application cliente doit mettre en cache certaines parties des informations de profil de l’utilisateur dans un stockage persistant afin d’éviter les requêtes inutiles et d’améliorer l’expérience utilisateur, en tenant compte des aspects suivants :

| Attribut | Expérience utilisateur |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `mvpd` | L’application cliente peut l’utiliser pour suivre le fournisseur de télévision sélectionné par l’utilisateur et continuer à l’utiliser pendant les phases de préautorisation ou d’autorisation.<br/><br/>À l’expiration du profil utilisateur actuel, l’application cliente peut utiliser la sélection MVPD mémorisée et demander à l’utilisateur de confirmer. |
| `attributes` | L’application cliente peut l’utiliser pour personnaliser l’expérience utilisateur en fonction de différentes clés [métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) (par exemple, `zip`, `maxRating`, etc.).<br/><br/>Les métadonnées utilisateur sont disponibles une fois le flux d’authentification terminé. Par conséquent, l’application cliente n’a pas besoin d’interroger un point d’entrée distinct pour récupérer les informations [métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), car elles sont déjà incluses dans les informations de profil.<br/><br/>Certains attributs de métadonnées peuvent être mis à jour au cours du flux d’autorisation, selon le MVPD et l’attribut de métadonnées spécifique. Par conséquent, l’application cliente peut avoir besoin d’interroger à nouveau les API Profiles pour récupérer les dernières métadonnées de l’utilisateur. |
| `notAfter` | L’application cliente peut l’utiliser pour suivre la date d’expiration du profil utilisateur.<br/><br/>La gestion des erreurs de l’application cliente nécessite de gérer les codes [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) (par exemple, `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated`, etc.), ce qui indique que l’application cliente nécessite que l’utilisateur s’authentifie. |

#### &#x200B;9. L’application cliente peut-elle étendre le profil de l’utilisateur sans nécessiter de réauthentification ? {#authentication-phase-faq9}

Non.

Le profil utilisateur ne peut pas être étendu au-delà de sa validité sans interaction utilisateur, car son expiration est déterminée par la durée de vie d’authentification établie avec les fichiers MVPD.

Par conséquent, l’application cliente doit inviter l’utilisateur à s’authentifier à nouveau et à interagir avec la page de connexion de MVPD pour actualiser son profil sur notre système.

Toutefois, pour les fichiers MVPD qui prennent en charge l’[authentification à domicile](/help/premium-workflow/hba-access/home-based-authentication.md) (HBA), l’utilisateur n’est pas tenu de saisir les informations d’identification.

#### &#x200B;10. Quels sont les cas d’utilisation de chaque point d’entrée de profil disponible ? {#authentication-phase-faq10}

Les points d’entrée de profils de base sont conçus pour permettre à l’application cliente de connaître le statut d’authentification de l’utilisateur, d’accéder aux informations de métadonnées de l’utilisateur, de trouver la méthode utilisée pour s’authentifier ou l’entité utilisée pour fournir l’identité.

Chaque point d’entrée correspond à un cas d’utilisation spécifique, comme suit :

| API | Description | Cas d’utilisation |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [API du point d’entrée des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | Récupérez tous les profils utilisateur. | **L’utilisateur ouvre l’application cliente pour la première fois**<br/><br/> Dans ce scénario, l’application cliente ne dispose pas de l’identifiant MVPD sélectionné par l’utilisateur mis en cache dans le stockage persistant.<br/><br/>Par conséquent, il envoie une seule demande pour récupérer tous les profils utilisateur disponibles. |
| [Point d’entrée des profils pour une API MVPD spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | Récupérez le profil utilisateur associé à un MVPD spécifique. | **L’utilisateur revient à l’application cliente après s’être authentifié lors d’une visite précédente**<br/><br/> Dans ce cas, l’application cliente doit avoir l’identifiant MVPD de l’utilisateur précédemment sélectionné mis en cache dans le stockage persistant.<br/><br/>Par conséquent, il envoie une seule demande pour récupérer le profil de l’utilisateur pour ce MVPD spécifique. |
| [ Point d’entrée des profils pour une API de code (d’authentification) spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Récupérez le profil utilisateur associé à un code d’authentification spécifique. | **L’utilisateur lance le processus d’authentification**<br/><br/> Dans ce scénario, l’application cliente doit déterminer si l’utilisateur a terminé l’authentification avec succès et récupérer ses informations de profil.<br/><br/>Par conséquent, il lance un mécanisme d’interrogation pour récupérer le profil de l’utilisateur associé au code d’authentification. |

Pour plus d’informations, reportez-vous aux documents [Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md) et [Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md).

Le point d’entrée de l’authentification unique des profils a un autre objectif. Il permet à l’application cliente de créer un profil utilisateur à l’aide de la réponse d’authentification du partenaire et de le récupérer en une seule opération unique.

| API | Description | Cas d’utilisation |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [API du point d’entrée de l’authentification unique des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | Créez et récupérez un profil utilisateur à l’aide de la réponse d’authentification du partenaire. | **L’utilisateur autorise l’application à utiliser l’authentification unique du partenaire pour s’authentifier**<br/><br/> Dans ce scénario, l’application cliente doit créer un profil utilisateur après avoir reçu la réponse d’authentification du partenaire et la récupérer en une seule opération unique. |

Pour toute requête ultérieure, les points d’entrée Profils de base doivent être utilisés pour déterminer le statut d’authentification de l’utilisateur, accéder aux informations de métadonnées de l’utilisateur, trouver la méthode utilisée pour l’authentification ou l’entité utilisée pour fournir l’identité.

Pour plus d’informations, reportez-vous aux documents [Authentification unique à l’aide des flux de partenaires](/help/premium-workflow/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md).

#### &#x200B;11. Que doit faire l’application cliente si l’utilisateur dispose de plusieurs profils MVPD ? {#authentication-phase-faq11}

La décision de prendre en charge plusieurs profils dépend des exigences commerciales de l’application cliente.

La plupart des utilisateurs n’auront qu’un seul profil. Cependant, dans les cas où il existe plusieurs profils (comme décrit ci-dessous), l’application cliente est chargée de déterminer la meilleure expérience utilisateur pour la sélection des profils.

L’application cliente peut choisir d’inviter l’utilisateur à sélectionner le profil MVPD souhaité ou d’effectuer la sélection automatiquement, par exemple en choisissant le premier profil utilisateur dans la réponse ou celui dont la période de validité est la plus longue.

L’API REST v2 prend en charge plusieurs profils pour s’adapter aux éléments suivants :

* Les utilisateurs qui peuvent avoir à choisir entre un profil MVPD standard et un profil obtenu par authentification unique (SSO).
* Les utilisateurs se voient proposer un accès temporaire sans avoir à se déconnecter de leur MVPD standard.
* Utilisateurs disposant d’un abonnement MVPD associé à des services de type « Direct-to-Consumer » (DTC).
* Utilisateurs disposant de plusieurs abonnements MVPD.

#### &#x200B;12. Que se passe-t-il lorsque les profils utilisateur expirent ? {#authentication-phase-faq12}

Lorsque les profils utilisateur expirent, ils ne sont plus inclus dans la réponse du point d’entrée Profils .

Si le point d’entrée Profils renvoie une réponse de mappage de profils vide, l’application cliente doit créer une session d’authentification et inviter l’utilisateur à s’authentifier à nouveau.

Pour plus d’informations, consultez la documentation [Créer une API de session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

#### &#x200B;13. À quel moment les profils utilisateur deviennent-ils non valides ? {#authentication-phase-faq13}

Les profils utilisateur ne sont plus valides dans les scénarios suivants :

* Lorsque la TTL d’authentification expire, comme indiqué par la date et l’heure `notAfter` dans la réponse de point d’entrée des profils .
* Lorsque l’application cliente modifie la valeur de l’en-tête [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md).
* Lorsque l’application cliente met à jour les informations d’identification du client utilisées pour récupérer la valeur de l’en-tête [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md).
* Lorsque l’application cliente révoque ou met à jour l’instruction logicielle utilisée pour obtenir les informations d’identification du client.

#### &#x200B;14. Quand l’application cliente doit-elle démarrer le mécanisme d’interrogation ? {#authentication-phase-faq14}

Pour garantir l’efficacité et éviter les requêtes inutiles, l’application cliente doit démarrer le mécanisme d’interrogation dans les conditions suivantes :

**Authentification effectuée dans l’application principale (écran)**

L’application principale (de diffusion en continu) doit commencer à interroger l’utilisateur ou l’utilisatrice lorsqu’il ou elle atteint la page de destination finale, une fois que le composant de navigateur charge l’URL spécifiée pour le paramètre `redirectUrl` dans la requête de point d’entrée [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

**Authentification effectuée dans une application secondaire (écran)**

L’application principale (de diffusion en continu) doit commencer à interroger l’utilisateur dès qu’il initie le processus d’authentification, juste après avoir reçu la réponse de point d’entrée [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) et affiché le code d’authentification à l’utilisateur.

#### &#x200B;15. Quand l’application cliente doit-elle arrêter le mécanisme d’interrogation ? {#authentication-phase-faq15}

Pour garantir l’efficacité et éviter les requêtes inutiles, l’application cliente doit arrêter le mécanisme d’interrogation dans les conditions suivantes :

**Authentification réussie**

Les informations de profil de l’utilisateur sont récupérées avec succès, confirmant leur statut d’authentification. À ce stade, l’interrogation n’est plus nécessaire.

**Session d’authentification et expiration du code**

La session d’authentification et le code expirent, comme indiqué par l’horodatage `notAfter` (par exemple, 30 minutes) dans la réponse de point d’entrée [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). Si cela se produit, l’utilisateur ou l’utilisatrice doit redémarrer le processus d’authentification et l’interrogation à l’aide du code d’authentification précédent doit être arrêtée immédiatement.

**Nouveau code d’authentification généré**

Si l’utilisateur demande un nouveau code d’authentification sur l’appareil principal (écran), la session existante n’est plus valide et l’interrogation à l’aide du code d’authentification précédent doit être arrêtée immédiatement.

#### &#x200B;16. Quel intervalle entre les appels l’application cliente doit-elle utiliser pour le mécanisme d’interrogation ? {#authentication-phase-faq16}

Pour garantir l’efficacité et éviter les requêtes inutiles, l’application cliente doit configurer la fréquence du mécanisme d’interrogation dans les conditions suivantes :

| **Authentification effectuée dans l’application principale (écran)** | **Authentification effectuée dans une application secondaire (écran)** |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| L’application principale (de diffusion en continu) doit effectuer une interrogation toutes les 3 à 5 secondes ou plus. | L’application principale (de diffusion en continu) doit effectuer une interrogation toutes les 3 à 5 secondes ou plus. |

#### &#x200B;17. Quel est le nombre maximal de requêtes d’interrogation que l’application cliente peut envoyer ? {#authentication-phase-faq17}

L’application cliente doit respecter les limites actuelles définies par le [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits) d’authentification d’Adobe Pass.

La gestion des erreurs de l’application cliente doit être en mesure de gérer le code d’erreur [429 Too Many Requests](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response), qui indique que l’application cliente a dépassé le nombre maximal de requêtes autorisées.

Pour plus d’informations, consultez la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

#### &#x200B;18. Comment l’application cliente peut-elle obtenir les informations sur les métadonnées de l’utilisateur ? {#authentication-phase-faq18}

L’application cliente peut interroger l’un des points d’entrée suivants capables de renvoyer des informations [métadonnées de l’utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) dans le cadre des informations de profil :

* [API du point d’entrée des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Point d’entrée des profils pour une API MVPD spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Point d’entrée des profils pour une API de code spécifique (authentification)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Les métadonnées de l’utilisateur sont disponibles une fois le flux d’authentification terminé. Par conséquent, l’application cliente n’a pas besoin d’interroger un point d’entrée distinct pour récupérer les informations [métadonnées de l’utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), car elles sont déjà incluses dans les informations de profil.

Pour plus d’informations, reportez-vous aux documents suivants :

* [Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Certains attributs de métadonnées peuvent être mis à jour pendant le flux d’autorisation, selon le MVPD et l’attribut de métadonnées spécifique. Par conséquent, l’application cliente peut avoir besoin d’interroger à nouveau les API ci-dessus pour récupérer les dernières métadonnées de l’utilisateur.

#### &#x200B;19. Comment l’application cliente doit-elle gérer l’accès dégradé ? {#authentication-phase-faq19}

La [fonctionnalité de dégradation](/help/premium-workflow/degraded-access/degradation-feature.md) permet à l’application cliente de conserver une expérience de diffusion en continu transparente pour les utilisateurs et utilisatrices, même lorsque leurs services d’authentification ou d’autorisation MVPD rencontrent des problèmes.

En résumé, cela peut garantir un accès ininterrompu au contenu malgré les perturbations temporaires des services de MVPD.

Étant donné que votre entreprise a l’intention d’utiliser la fonctionnalité de dégradation Premium, l’application cliente doit gérer les flux d’accès dégradés, qui décrivent le comportement des points d’entrée de l’API REST v2 dans de tels scénarios.

Pour plus d&#39;informations, consultez la documentation [Flux d&#39;accès dégradés](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).

#### &#x200B;20. Comment l’application cliente doit-elle gérer l’accès temporaire ? {#authentication-phase-faq20}

La [fonction TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md) permet à l&#39;application cliente de fournir un accès temporaire à l&#39;utilisateur.

En résumé, cela peut intéresser les utilisateurs et les utilisatrices en leur fournissant un accès limité dans le temps au contenu ou à un nombre prédéfini de titres VOD pendant une période spécifiée.

Étant donné que votre entreprise a l’intention d’utiliser la fonctionnalité TempPass Premium, l’application cliente doit gérer les flux d’accès temporaires, qui décrivent le comportement des points d’entrée de l’API REST v2 dans de tels scénarios.

Dans les versions précédentes de l’API, l’application cliente devait déconnecter un utilisateur authentifié avec son MVPD standard pour offrir un accès temporaire.

Avec l’API REST v2, l’application cliente peut basculer facilement entre un MVPD standard et un MVPD TempPass lors de l’autorisation d’un flux, éliminant ainsi la nécessité pour l’utilisateur d’être déconnecté.

Pour plus d&#39;informations, consultez la documentation [Flux d&#39;accès temporaires](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).

#### &#x200B;21. Comment l’application cliente doit-elle gérer l’accès avec authentification unique sur plusieurs appareils ? {#authentication-phase-faq21}

L’API REST v2 peut activer l’authentification unique entre appareils si l’application cliente fournit un identifiant utilisateur unique cohérent entre les appareils.

Cet identifiant, appelé jeton de service, doit être généré par l’application cliente par l’implémentation ou l’intégration d’un service d’identités externe de votre choix.

Pour plus d’informations, reportez-vous à la documentation [ Authentification unique à l’aide des flux de jeton de service ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

+++

### FAQ sur la phase de préautorisation {#preauthorization-phase-faqs-general}

+++FAQ sur la phase de préautorisation

#### &#x200B;1. Quel est l’objectif de la phase de préautorisation ? {#preauthorization-phase-faq1}

L’objectif de la phase de préautorisation est de permettre à l’application cliente de présenter un sous-ensemble de ressources de son catalogue auquel l’utilisateur ou l’utilisatrice serait autorisé à accéder.

La phase de préautorisation peut améliorer l’expérience de l’utilisateur lorsqu’il ouvre l’application cliente pour la première fois ou accède à une nouvelle section.

#### &#x200B;2. La phase de préautorisation est-elle obligatoire ? {#preauthorization-phase-faq2}

La phase de préautorisation n’est pas obligatoire, l’application cliente peut ignorer cette phase si elle souhaite présenter un catalogue de ressources sans les filtrer au préalable en fonction des droits de l’utilisateur.

#### &#x200B;3. Qu’est-ce qu’une décision de préautorisation ? {#preauthorization-phase-faq3}

La préautorisation est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization), tandis que le terme de décision se trouve également dans le [Glossaire](rest-api-v2-glossary.md#decision).

La décision de préautorisation stocke des informations sur le résultat de la recherche du processus de préautorisation MVPD qui peuvent être récupérées à partir du point d’entrée de préautorisation des décisions.

L’application cliente peut utiliser les décisions de préautorisation pour présenter un sous-ensemble de ressources auxquelles les décisions (informatives) du fournisseur de télévision permettraient à l’utilisateur d’accéder.

Pour plus d’informations, reportez-vous aux documents suivants :

* [Récupération de l’API de décisions de préautorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### &#x200B;4. L’application cliente doit-elle mettre en cache les décisions de préautorisation dans un stockage persistant ? {#preauthorization-phase-faq4}

L’application cliente n’est pas nécessaire pour stocker les décisions de préautorisation dans un stockage persistant. Cependant, il est recommandé de mettre en cache les décisions d’autorisation en mémoire pour améliorer l’expérience de l’utilisateur. Cela permet d’éviter les appels inutiles au point d’entrée de préautorisation des décisions pour les ressources qui ont déjà été préautorisées, ce qui réduit la latence et améliore les performances.

#### &#x200B;5. Comment l’application cliente peut-elle déterminer pourquoi une décision de préautorisation a été refusée ? {#preauthorization-phase-faq5}

L’application cliente peut déterminer la raison d’un refus de décision de préautorisation en examinant le [ code d’erreur et le message ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) inclus dans la réponse à partir du point d’entrée de préautorisation des décisions . Ces détails fournissent à insight la raison spécifique pour laquelle la demande de préautorisation a été refusée, ce qui permet d’informer l’expérience utilisateur ou de déclencher toute gestion nécessaire dans l’application.

Assurez-vous que tout mécanisme de reprise implémenté pour récupérer les décisions de préautorisation ne génère pas de boucle sans fin si la décision de préautorisation est refusée.

Envisagez de limiter les reprises à un nombre raisonnable et de gérer les refus de manière élégante en présentant des commentaires clairs à l’utilisateur.

#### &#x200B;6. Pourquoi un jeton de média est-il manquant dans la décision de préautorisation ? {#preauthorization-phase-faq6}

Il manque un jeton multimédia dans la décision de préautorisation, car la phase de préautorisation ne doit pas être utilisée pour lire les ressources, car c’est l’objectif de la phase d’autorisation.

#### &#x200B;7. La phase d’autorisation peut-elle être ignorée si une décision de préautorisation existe déjà ? {#preauthorization-phase-faq7}

Non.

La phase d’autorisation ne peut pas être ignorée même si une décision de préautorisation est disponible. Les décisions de préautorisation sont uniquement informatives et n’accordent pas de droits de lecture réels. La phase de préautorisation est destinée à fournir des conseils précoces, mais la phase d’autorisation est toujours requise avant la lecture de tout contenu.

#### &#x200B;8. Qu’est-ce qu’une ressource et quels formats sont pris en charge ? {#preauthorization-phase-faq8}

La ressource est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

La ressource est un identifiant unique convenu avec les MVPD et associé à un contenu que l’application cliente peut diffuser.

L’identifiant unique de la ressource peut avoir deux formats :

* Format de chaîne simple, tel qu’un identifiant unique pour un canal (marque).
* Format RSS (Media RSS) contenant des informations supplémentaires telles que le titre, les évaluations et les métadonnées de contrôle parental.

Pour plus d’informations, consultez la documentation [Ressources protégées](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### &#x200B;9. Pour combien de ressources l’application cliente peut-elle obtenir une décision de préautorisation à la fois ? {#preauthorization-phase-faq9}

L’application cliente peut obtenir une décision de préautorisation pour un nombre limité de ressources dans une seule requête API, généralement jusqu’à 5, en raison des conditions imposées par les MVPD.

Ce nombre maximal de ressources peut être consulté et modifié après accord avec les MVPD via le tableau de bord Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre entreprise ou par un représentant Adobe Pass Authentication agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

### FAQ sur la phase d’autorisation {#authorization-phase-faqs-general}

+++FAQ sur la phase d’autorisation

#### &#x200B;1. Quel est l’objectif de la phase d’autorisation ? {#authorization-phase-faq1}

La phase d’autorisation a pour but de permettre à l’application cliente de lire les ressources demandées par l’utilisateur ou l’utilisatrice après validation de ses droits avec MVPD.

#### &#x200B;2. La phase d’autorisation est-elle obligatoire ? {#authorization-phase-faq2}

La phase d’autorisation est obligatoire, l’application cliente ne peut pas ignorer cette phase si elle souhaite lire les ressources demandées par l’utilisateur ou l’utilisatrice, car elle doit vérifier auprès du MVPD que l’utilisateur ou l’utilisatrice a le droit de lire avant de libérer le flux.

#### &#x200B;3. Qu&#39;est-ce qu&#39;une décision d&#39;autorisation et quelle est sa durée de validité ? {#authorization-phase-faq3}

L’autorisation est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization), tandis que le terme de décision se trouve également dans le [Glossaire](rest-api-v2-glossary.md#decision).

La décision d’autorisation stocke des informations sur le résultat de la recherche du processus d’autorisation MVPD qui peuvent être récupérées à partir du point d’entrée Autoriser les décisions .

L’application cliente peut utiliser la décision d’autorisation contenant un jeton multimédia pour lire le flux de ressources au cas où la décision Fournisseur de télévision (faisant autorité) permettrait à l’utilisateur d’y accéder.

Pour plus d’informations, reportez-vous aux documents suivants :

* [Récupérer l’API des décisions d’autorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

La décision d’autorisation est valide pendant une période limitée et courte spécifiée au moment de l’émission, indiquant la durée pendant laquelle elle sera mise en cache par l’authentification Adobe Pass avant de devoir réitérer la requête au MVPD.

Cette période limitée appelée autorisation (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) peut être consultée et modifiée via le tableau de bord Adobe Pass [TVE](rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre organisation ou par un représentant Adobe Pass Authentication agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### &#x200B;4. L’application cliente doit-elle mettre en cache les décisions d’autorisation dans un stockage persistant ? {#authorization-phase-faq4}

L’application cliente n’est pas nécessaire pour stocker les décisions d’autorisation dans un stockage persistant.

#### &#x200B;5. Comment l’application cliente peut-elle déterminer pourquoi une décision d’autorisation a été refusée ? {#authorization-phase-faq5}

L’application cliente peut déterminer la raison d’un refus d’autorisation en examinant le [ code d’erreur et le message ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) inclus dans la réponse à partir du point d’entrée Autoriser les décisions . Ces détails fournissent à insight la raison spécifique pour laquelle la demande d’autorisation a été refusée, ce qui permet d’informer l’expérience utilisateur ou de déclencher toute gestion nécessaire dans l’application.

Assurez-vous que tout mécanisme de reprise implémenté pour récupérer les décisions d’autorisation ne génère pas de boucle sans fin si la décision d’autorisation est refusée.

Envisagez de limiter les reprises à un nombre raisonnable et de gérer les refus de manière élégante en présentant des commentaires clairs à l’utilisateur.

#### &#x200B;6. Qu’est-ce qu’un jeton multimédia et combien de temps est-il valide ? {#authorization-phase-faq6}

Le jeton de média est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token).

Le jeton de média se compose d’une chaîne signée envoyée en texte clair qui peut être récupérée à partir du point d’entrée Autoriser les décisions .

Pour plus d’informations, consultez la documentation du [Vérificateur de jeton multimédia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

Le jeton de média est valide pendant une période limitée et courte spécifiée au moment de l’émission, indiquant le délai avant lequel il doit être vérifié et utilisé par l’application cliente.

L’application cliente peut utiliser le jeton multimédia pour lire un flux de ressources au cas où la décision du fournisseur de télévision (faisant autorité) permettrait à l’utilisateur d’y accéder.

Pour plus d’informations, reportez-vous aux documents suivants :

* [Récupérer l’API des décisions d’autorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### &#x200B;7. L’application cliente doit-elle valider le jeton de média avant de lire le flux de ressources ? {#authorization-phase-faq7}

Oui.

L’application cliente doit valider le jeton de média avant de commencer la lecture du flux de ressources. Cette validation doit être effectuée à l’aide du [Vérificateur de jeton multimédia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier). En vérifiant le `serializedToken` du `token` renvoyé, l’application cliente empêche tout accès non autorisé, tel que l’extraction de flux, et garantit que seuls les utilisateurs correctement autorisés peuvent lire le contenu.

#### &#x200B;8. L’application cliente doit-elle actualiser un jeton de média expiré pendant la lecture ? {#authorization-phase-faq8}

Non.

L’application cliente n’est pas tenue d’actualiser un jeton de média expiré pendant que le flux est en cours de lecture. Si le jeton de média expire pendant la lecture, le flux doit pouvoir continuer sans interruption. Cependant, le client doit demander une nouvelle décision d’autorisation et obtenir un nouveau jeton de média la prochaine fois que l’utilisateur tente de lire une ressource.

#### &#x200B;9. À quoi sert chaque attribut d’horodatage dans la décision d’autorisation ? {#authorization-phase-faq9}

La décision d’autorisation comprend plusieurs attributs d’horodatage qui fournissent un contexte essentiel sur la période de validité de l’autorisation elle-même et du jeton de média associé. Ces horodatages ont des fins différentes, selon qu’ils se rapportent à la décision d’autorisation ou au jeton multimédia.

**Horodatages Au Niveau De La Décision**

Ces dates et heures décrivent la période de validité de la décision d’autorisation globale :

| Attribut | Description | Notes |
|-------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Heure en millisecondes à laquelle la décision d’autorisation a été émise. | Ceci marque le début de la fenêtre de validité de l’autorisation. |
| `notAfter` | Heure en millisecondes à laquelle la décision d’autorisation expire. | La [ durée de vie (TTL) de l’autorisation détermine ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management) durée pendant laquelle l’autorisation reste valide avant d’exiger une nouvelle autorisation. Cette TTL est négociée avec les représentants de MVPD. |

**Horodatages au niveau des jetons**

Ces horodatages décrivent la période de validité du jeton multimédia lié à la décision d’autorisation :

| Attribut | Description | Notes |
|-------------|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Heure en millisecondes d’émission du jeton multimédia. | Cela marque le moment où le jeton devient valide pour la lecture. |
| `notAfter` | Heure en millisecondes à laquelle le jeton de média expire. | Les jetons multimédias ont une durée de vie délibérément courte (généralement 7 minutes) afin de minimiser les risques d’utilisation abusive et de tenir compte des différences d’horloge potentielles entre le serveur de génération de jetons et le serveur de vérification des jetons. |

#### &#x200B;10. Qu’est-ce qu’une ressource et quels formats sont pris en charge ? {#authorization-phase-faq10}

La ressource est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

La ressource est un identifiant unique convenu avec les MVPD et associé à un contenu que l’application cliente peut diffuser.

L’identifiant unique de la ressource peut avoir deux formats :

* Format de chaîne simple, tel qu’un identifiant unique pour un canal (marque).
* Format RSS (Media RSS) contenant des informations supplémentaires telles que le titre, les évaluations et les métadonnées de contrôle parental.

Pour plus d’informations, consultez la documentation [Ressources protégées](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### &#x200B;11. Pour combien de ressources la demande du client peut-elle obtenir une décision d’autorisation à la fois ? {#authorization-phase-faq11}

L’application cliente peut obtenir une décision d’autorisation pour un nombre limité de ressources dans une seule requête API, généralement jusqu’à 1, en raison des conditions imposées par les MVPD.

+++

### FAQ sur la phase de déconnexion {#logout-phase-faqs-general}

+++FAQ sur la phase de déconnexion

#### &#x200B;1. Quel est l’objectif de la phase de déconnexion ? {#logout-phase-faq1}

L’objectif de la phase de déconnexion est de permettre à l’application cliente de mettre fin au profil authentifié de l’utilisateur dans l’authentification Adobe Pass à la demande de l’utilisateur.

#### &#x200B;2. La phase de déconnexion est-elle obligatoire ? {#logout-phase-faq2}

La phase de déconnexion est obligatoire, l’application cliente doit permettre à l’utilisateur de se déconnecter.

+++

### FAQ sur les en-têtes {#headers-faqs-general}

+++FAQ sur les en-têtes

#### &#x200B;1. Comment calculer la valeur de l’en-tête d’autorisation ? {#headers-faq1}

>[!IMPORTANT]
>
> Si l’application cliente migre de l’API REST V1 vers l’API REST V2, elle peut continuer à utiliser la même méthode pour obtenir la valeur du jeton d’accès `Bearer` que précédemment.

L’en-tête de requête [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) contient le jeton d’accès `Bearer` requis par l’application cliente pour accéder aux API protégées d’Adobe Pass.

La valeur de l’en-tête Autorisation doit être obtenue à partir de l’authentification Adobe Pass pendant la phase d’enregistrement.

Pour plus d’informations, reportez-vous aux documents suivants :

* [Présentation de l’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [API de récupération des informations d’identification du client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Récupérer l’API du jeton d’accès](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### &#x200B;2. Comment calculer la valeur de l’en-tête AP-Device-Identifier ? {#headers-faq2}

>[!IMPORTANT]
>
> Si l’application cliente migre de l’API REST V1 vers l’API REST V2, elle peut continuer à utiliser la même méthode pour calculer la valeur de l’identifiant de l’appareil que précédemment.

L’en-tête de requête [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contient l’identifiant de l’appareil de diffusion en continu tel qu’il a été créé par l’application cliente.

La documentation de l’en-tête [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) fournit des exemples pour les principales plateformes sur la manière de calculer la valeur, mais l’application cliente peut choisir d’utiliser une méthode différente en fonction de sa propre logique commerciale et de ses propres exigences.

#### &#x200B;3. Comment calculer la valeur de l’en-tête X-Device-Info ? {#headers-faq3}

>[!IMPORTANT]
>
> Si l’application cliente migre de l’API REST V1 vers l’API REST V2, elle peut continuer à utiliser la même méthode pour calculer la valeur des informations sur l’appareil que précédemment.

L’en-tête de requête [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contient les informations client (appareil, connexion et application) liées à l’appareil de diffusion en continu actuel et est utilisé pour déterminer les règles spécifiques à la plateforme que les MVPD peuvent appliquer.

La documentation de l’en-tête [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) fournit des exemples pour les principales plateformes sur la manière de calculer la valeur, mais l’application cliente peut choisir d’utiliser une autre méthode en fonction de sa propre logique commerciale et de ses propres exigences.

Si l’en-tête [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) est manquant ou contient des valeurs incorrectes, la requête peut être classée comme provenant d’une plateforme `unknown`.

Cela peut entraîner le traitement de la requête comme non sécurisée et soumise à des règles plus restrictives, telles que des TTL d’authentification plus courtes. De plus, certains champs, comme le `connectionIp` et le `connectionPort` de l&#39;appareil de diffusion en continu, sont obligatoires pour des fonctions comme l&#39;authentification de base d&#39;accueil [ de Spectrum](/help/premium-workflow/hba-access/home-based-authentication.md).

Même lorsque la requête provient d’un serveur pour le compte d’un appareil, la valeur de l’en-tête [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) doit refléter les informations réelles de l’appareil de diffusion en continu.

+++

### Questions fréquentes diverses {#misc-faqs-general}

+++Questions fréquentes diverses

#### &#x200B;1. Puis-je explorer les requêtes et réponses de l’API REST V2 et tester l’API ? {#misc-faq1}

Oui.

Vous pouvez explorer l’API REST V2 via notre site web [Adobe Developer](https://developer.adobe.com/adobe-pass/) dédié. Le site Web d’Adobe Developer offre un accès illimité aux éléments suivants :

* [ API DCR ](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Pour interagir avec [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/), vous devez inclure l’en-tête [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) avec un jeton d’accès `Bearer` obtenu via l’API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Pour utiliser l’[API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), une instruction logicielle avec la portée API REST V2 est requise. Pour plus d’informations, consultez le document [FAQ sur l’enregistrement client dynamique (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

#### &#x200B;2. Puis-je explorer les requêtes et réponses de l’API REST V2 à l’aide d’un outil de développement d’API avec prise en charge d’OpenAPI ? {#misc-faq2}

Oui.

Vous pouvez télécharger les fichiers de spécification OpenAPI pour l’API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) et l’API [REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) à partir du site web [Adobe Developer](https://developer.adobe.com/adobe-pass/).

Pour télécharger les fichiers de spécification OpenAPI, cliquez sur les boutons de téléchargement pour enregistrer les fichiers suivants sur votre ordinateur local :

* [JSON DE L’API DCR](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [JSON DE L’API REST V2](https://developer.adobe.com/adobe-pass/restApiV2.json)

Vous pouvez ensuite importer ces fichiers dans votre outil de développement d’API préféré pour explorer les requêtes et réponses de l’API REST V2 et tester l’API.

#### &#x200B;3. Puis-je toujours utiliser l’outil de test d’API existant hébergé sur https://sp.auth-staging.adobe.com/apitest/api.html ? {#misc-faq3}

Non.

Les applications clientes qui migrent vers l’API REST V2 doivent utiliser le nouvel outil de test hébergé sur https://developer.adobe.com/adobe-pass/. Le site Web d’Adobe Developer offre un accès illimité aux éléments suivants :

* [ API DCR ](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Pour interagir avec [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/), vous devez inclure l’en-tête [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) avec un jeton d’accès `Bearer` obtenu via l’API [DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Pour utiliser l’[API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), une instruction logicielle avec la portée API REST V2 est requise. Pour plus d’informations, consultez le document [FAQ sur l’enregistrement client dynamique (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

+++

## FAQ sur la migration {#migration-faqs}

Passez à cette section si vous travaillez sur une application qui doit migrer une application existante vers l’API REST V2.

>[!MORELIKETHIS]
>
> * [FAQ sur Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### Questions fréquentes générales sur la migration {#general-migration-faqs}

+++Questions fréquentes générales sur la migration

#### &#x200B;1. Dois-je déployer une nouvelle application cliente migrée vers l’API REST V2 en une seule fois pour tous les utilisateurs ? {#migration-faq1}

Non.

L’application cliente n’est pas tenue de déployer simultanément une nouvelle version intégrant la version V2 de l’API REST à tous les utilisateurs.

L’authentification Adobe Pass continuera à prendre en charge les anciennes versions d’applications clientes intégrant l’API REST V1 ou SDK jusqu’à la fin de 2025.

#### &#x200B;2. Dois-je déployer simultanément une nouvelle application cliente migrée vers l’API REST V2 sur toutes les API et tous les flux ? {#migration-faq2}

Oui.

L’application cliente est requise pour déployer simultanément une nouvelle version intégrant l’API REST V2 à tous les flux et API d’authentification Adobe Pass.

Dans le cas du flux « deuxième authentification de l’écran », l’application cliente doit déployer simultanément une nouvelle version intégrant l’API REST V2 pour les applications [principales](rest-api-v2-glossary.md#primary-application) et [secondaires](rest-api-v2-glossary.md#secondary-application).

L’authentification Adobe Pass ne prend pas en charge les implémentations « hybrides » qui intègrent à la fois l’API REST V2 et l’API REST V1/SDK entre les API et les flux.

#### &#x200B;3. L’authentification de l’utilisateur sera-t-elle conservée lors de la mise à jour vers une nouvelle application cliente migrée vers l’API REST V2 ? {#migration-faq3}

Non.

L’authentification de l’utilisateur obtenue dans les anciennes versions de l’application cliente intégrant l’API REST V1 ou SDK n’est pas conservée.

Par conséquent, l’utilisateur devra s’authentifier à nouveau dans la nouvelle application cliente migrée vers l’API REST V2.

#### &#x200B;4. Les codes d’erreur améliorés sont-ils activés par défaut dans l’API REST V2 ? {#migration-faq4}

Oui.

Les applications clientes qui migrent vers l’API REST V2 bénéficient automatiquement de cette fonctionnalité par défaut, fournissant des informations d’erreur plus détaillées et plus précises.

Pour plus d’informations, consultez la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

#### &#x200B;5. Les intégrations existantes nécessitent-elles des modifications de configuration lors de la migration vers l’API REST V2 ? {#migration-faq5}

Non.

Les applications clientes qui migrent vers l’API REST V2 ne nécessitent aucune modification de configuration pour les intégrations MVPD existantes. En outre, ils continueront à utiliser les mêmes identifiants pour les [fournisseurs de services](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) et [MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd) existants.

En outre, les protocoles utilisés par l’authentification Adobe Pass pour communiquer avec les points d’entrée MVPD restent inchangés.

+++

### Migration de l’API REST V1 vers l’API REST V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Continuez avec cette sous-section si vous travaillez sur une application qui doit migrer de l’API REST V1 vers l’API REST V2.

#### FAQ sur la phase d&#39;enregistrement {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++FAQ sur la phase d&#39;enregistrement

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’enregistrement ? {#registration-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, il n’y a aucune modification de haut niveau en ce qui concerne la phase d’enregistrement.

L’application cliente peut continuer à utiliser le même flux pour s’enregistrer auprès de l’authentification Adobe Pass par le biais du processus [Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) et obtenir un jeton d’accès.

Pour plus d’informations, consultez les documents suivants :

* [Présentation de l’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [API de récupération des informations d’identification du client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Récupérer l’API du jeton d’accès](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### FAQ sur la phase de configuration {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++FAQ sur la phase de configuration

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase de configuration ? {#configuration-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans le tableau suivant :

| Portée | API REST V1 | API REST V2 | Observations |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec l’intégration active | [<br/> GET /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [<br/> GET /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Questions fréquentes sur la phase d’authentification {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Questions fréquentes sur la phase d’authentification

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’authentification ? {#authentication-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans le tableau suivant :

| Portée | API REST V1 | API REST V2 | Observations |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer le code d’enregistrement (code d’authentification) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le code d’enregistrement (code d’authentification) | [<br/> GET /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [<br/> GET /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reprendre l’authentification (MVPD) sur le deuxième écran (application) | [<br/> GET /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [<br/> GET /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Lancer l’authentification (MVPD) | [<br/> GET /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [<br/> GET /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [<br/> GET /api/v1/checkauthn (premier écran)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (deuxième écran)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupérer le jeton d’authentification de l’utilisateur (profil) | [<br/> GET /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase de préautorisation {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++FAQ sur la phase de préautorisation

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase de préautorisation ? {#preauthorization-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans le tableau suivant :

| Portée | API REST V1 | API REST V2 | Observations |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération des ressources préautorisées (décisions de préautorisation) | [<br/> GET /api/v1/preauthorize (premier écran)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [<br/> GET /api/v1/preauthorize (deuxième écran)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase d’autorisation {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++FAQ sur la phase d’autorisation

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’autorisation ? {#authorization-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans le tableau suivant :

| Portée | API REST V1 | API REST V2 | Observations |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer l’autorisation (MVPD) | [<br/> GET /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Récupération du jeton d’autorisation (décision d’autorisation) | [<br/> GET /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Récupération du jeton d’autorisation court (jeton multimédia) | [<br/> GET /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase de déconnexion {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++FAQ sur la phase de déconnexion

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase de déconnexion ? {#logout-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans le tableau suivant :

| Portée | API REST V1 | API REST V2 | Observations |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer la déconnexion | [<br/> GET /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [<br/> GET /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migration de SDK vers l’API REST V2 {#migration-sdk-to-rest-api-v2}

Continuez avec cette sous-section si vous travaillez sur une application qui doit migrer de SDK vers l’API REST V2.

#### FAQ sur la phase d&#39;enregistrement {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase d&#39;enregistrement

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’enregistrement ? {#registration-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement dynamique du client (DCR) | Transmission d’une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### SDK AccessEnabler iOS/tvOS

| Portée | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement dynamique du client (DCR) | Transmission d’une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### SDK AccessEnabler Android

| Portée | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement dynamique du client (DCR) | Transmission d’une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### SDK FireOS AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement dynamique du client (DCR) | Transmission d’une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### FAQ sur la phase de configuration {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase de configuration

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase de configuration ? {#configuration-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec l’intégration active | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [<br/> GET /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### SDK AccessEnabler iOS/tvOS

| Portée | SDK | API REST V2 | Observations |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec l’intégration active | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [<br/> GET /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### SDK AccessEnabler Android

| Portée | SDK | API REST V2 | Observations |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec l’intégration active | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [<br/> GET /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### SDK FireOS AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec l’intégration active | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [<br/> GET /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Questions fréquentes sur la phase d’authentification {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++Questions fréquentes sur la phase d’authentification

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’authentification ? {#authentication-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [<br/> GET /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK AccessEnabler iOS

| Portée | SDK | API REST V2 | Observations |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [<br/> GET /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK tvOS AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer le code d’enregistrement (code d’authentification) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le code d’enregistrement (code d’authentification) | [<br/> GET /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [<br/> GET /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reprendre l’authentification (MVPD) sur le deuxième écran (application) | [<br/> GET /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [<br/> GET /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Lancer l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [<br/> GET /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK AccessEnabler Android

| Portée | SDK | API REST V2 | Observations |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [<br/> GET /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK FireOS AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer le code d’enregistrement (code d’authentification) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le code d’enregistrement (code d’authentification) | [<br/> GET /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [<br/> GET /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reprendre l’authentification (MVPD) sur le deuxième écran (application) | [<br/> GET /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [<br/> GET /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Lancer l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [<br/> GET /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Utilisez l’une des méthodes suivantes : <br/><br/> [<br/> GET /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [<br/> GET /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase de préautorisation {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase de préautorisation

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase de préautorisation ? {#preauthorization-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération des ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler iOS/tvOS

| Portée | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération des ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler Android

| Portée | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération des ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Portée | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération des ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase d’autorisation {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase d’autorisation

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’autorisation ? {#authorization-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler iOS/tvOS

| Portée | SDK | API REST V2 | Observations |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler Android

| Portée | SDK | API REST V2 | Observations |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK FireOS AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase de déconnexion {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase de déconnexion

##### &#x200B;1. Quelles sont les migrations d’API de haut niveau requises pour la phase de déconnexion ? {#logout-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer la déconnexion | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [<br/> GET /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler iOS/tvOS

| Portée | SDK | API REST V2 | Observations |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer la déconnexion | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [<br/> GET /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler Android

| Portée | SDK | API REST V2 | Observations |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer la déconnexion | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [<br/> GET /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### SDK FireOS AccessEnabler

| Portée | SDK | API REST V2 | Observations |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer la déconnexion | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [<br/> GET /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
