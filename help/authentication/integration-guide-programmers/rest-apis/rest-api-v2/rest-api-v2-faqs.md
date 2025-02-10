---
title: FAQ sur l’API REST V2
description: FAQ sur l’API REST V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '6460'
ht-degree: 0%

---

# FAQ sur l’API REST V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Ce document fournit des réponses générales aux questions fréquentes sur l’adoption de l’API REST d’authentification Adobe Pass V2.

Pour plus d’informations sur l’API REST V2 dans son ensemble, consultez la documentation [ Présentation de l’API REST V2 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) .

## Questions fréquentes d’ordre général {#general-faqs}

Commencez par cette section si vous travaillez sur une application qui doit intégrer l’API REST V2, qu’il s’agisse d’une nouvelle application ou d’une application existante qui migre depuis [l’API REST V1](#migration-rest-api-v1-to-rest-api-v2) ou [SDK](#migration-sdk-to-rest-api-v2).

Pour plus d’informations sur les détails et les étapes de la migration, consultez également les sections suivantes.

>[!MORELIKETHIS]
>
> * [FAQ sur Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#general-faqs)

### FAQ sur la phase d&#39;enregistrement {#registration-phase-faqs-general}

+++FAQ sur la phase d’enregistrement

Reportez-vous à la documentation [FAQ sur l’enregistrement client dynamique (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### FAQ sur la phase de configuration {#configuration-phase-faqs-general}

+++FAQ sur la phase de configuration

#### 1. Quel est l’objectif de la phase de configuration ? {#configuration-phase-faq1}

La phase de configuration a pour but de fournir à l’application cliente la liste des fichiers MVPD auxquels elle est activement intégrée, ainsi que les détails de configuration enregistrés par l’authentification Adobe Pass pour chaque MVPD.

La phase de configuration fait office d’étape préalable à la phase d’authentification lorsque l’application cliente doit demander à l’utilisateur de sélectionner son fournisseur de télévision.

#### 2. La phase de configuration est-elle obligatoire ? {#configuration-phase-faq2}

La phase de configuration n’est pas obligatoire, l’application cliente peut ignorer cette phase dans les scénarios suivants :

* L’utilisateur est déjà authentifié.
* L’utilisateur se voit proposer un accès temporaire par le biais de la fonctionnalité TempPass de base ou promotionnelle.
* L’authentification de l’utilisateur a expiré, mais l’application cliente a mis en cache le MVPD précédemment sélectionné en tant que choix motivé par l’expérience utilisateur, et invite simplement l’utilisateur à confirmer qu’il est toujours abonné à ce MVPD.

#### 3. Qu’est-ce qu’une configuration et combien de temps est-elle valide ? {#configuration-phase-faq3}

La configuration est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration).

La configuration se compose d’une liste de fichiers MVPD qui peuvent être récupérés à partir du point d’entrée de configuration .

L’application cliente peut utiliser la configuration pour présenter un composant d’interface utilisateur appelé « Sélecteur » lorsque l’utilisateur doit sélectionner son MVPD.

L’application cliente doit actualiser la configuration avant que l’utilisateur ne passe par la phase d’authentification.

Pour plus d’informations, consultez la documentation [Récupérer la configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### 4. L’application cliente peut-elle gérer sa propre liste de fichiers MVPD ? {#configuration-phase-faq4}

L’application cliente peut gérer sa propre liste de fichiers MVPD, mais il est recommandé d’utiliser la configuration fournie par Authentification Adobe Pass pour s’assurer que la liste est à jour et précise.

L’application cliente recevrait une [erreur](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) de l’API REST d’authentification Adobe Pass V2 si l’utilisateur a sélectionné MVPD ne dispose pas d’une intégration active avec le [fournisseur de services](rest-api-v2-glossary.md#service-provider) spécifié.

#### 5. L’application cliente peut-elle filtrer la liste des fichiers MVPD ? {#configuration-phase-faq5}

L’application cliente peut filtrer la liste des fichiers MVPD fournie dans la réponse de configuration en implémentant un mécanisme personnalisé basé sur sa propre logique commerciale et ses exigences, telles que l’emplacement de l’utilisateur ou l’historique de la sélection précédente.

#### 6. Que se passe-t-il si l’intégration à un MVPD est désactivée et marquée comme inactive ? {#configuration-phase-faq6}

Lorsque l’intégration à un MVPD est désactivée et marquée comme inactive, le MVPD est supprimé de la liste des MVPD fournie dans les autres réponses de configuration. Il faut tenir compte de deux conséquences importantes :

* Les utilisateurs non authentifiés de ce MVPD ne pourront plus terminer la phase d’authentification à l’aide de ce MVPD.
* Les utilisateurs authentifiés de ce MVPD ne pourront plus terminer les phases de préautorisation, d’autorisation ou de déconnexion à l’aide de ce MVPD.

L’application cliente recevrait une [erreur](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) de l’API REST d’authentification Adobe Pass V2 si l’utilisateur a sélectionné MVPD ne dispose plus d’une intégration active avec le [fournisseur de services](rest-api-v2-glossary.md#service-provider) spécifié.

#### 7. Que se passe-t-il si l’intégration à un MVPD est activée à l’arrière et marquée comme active ? {#configuration-phase-faq7}

Lorsque l’intégration à un MVPD est réactivée et marquée comme active, le MVPD est de nouveau inclus dans la liste des MVPD fournie dans les autres réponses de configuration. Il existe deux conséquences importantes à prendre en compte :

* Les utilisateurs non authentifiés de ce MVPD pourront à nouveau terminer la phase d’authentification à l’aide de ce MVPD.
* Les utilisateurs authentifiés de ce MVPD pourront à nouveau terminer les phases de préautorisation, d’autorisation ou de déconnexion à l’aide de ce MVPD.

#### 8. Comment activer ou désactiver l’intégration à MVPD ? {#configuration-phase-faq8}

Cette opération peut être effectuée via le tableau de bord Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre organisation ou par un représentant Adobe Pass Authentication agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Questions fréquentes sur la phase d’authentification {#authentication-phase-faqs-general}

+++FAQ sur la phase d’authentification

#### 1. Quel est l’objectif de la phase d’authentification ? {#authentication-phase-faq1}

La phase d’authentification a pour but de permettre à l’application cliente de vérifier l’identité de l’utilisateur et d’obtenir des informations sur les métadonnées de l’utilisateur.

La phase d’authentification agit comme une étape préalable à la phase de préautorisation ou à la phase d’autorisation lorsque l’application cliente doit lire du contenu.

#### 2. Comment l’application cliente peut-elle savoir si l’utilisateur est déjà authentifié ? {#authentication-phase-faq2}

L’application cliente peut interroger l’un des points d’entrée suivants capable de vérifier si un utilisateur est déjà authentifié et de renvoyer des informations de profil :

* [API du point d’entrée des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Point d’entrée des profils pour une API MVPD spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Point d’entrée des profils pour une API de code spécifique (authentification)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Pour plus d’informations, reportez-vous aux documents suivants :

* [Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 3. Comment l’application cliente peut-elle obtenir les informations sur les métadonnées de l’utilisateur ? {#authentication-phase-faq3}

L’application cliente peut interroger l’un des points d’entrée suivants capables de renvoyer des informations [métadonnées de l’utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) dans le cadre des informations de profil :

* [API du point d’entrée des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Point d’entrée des profils pour une API MVPD spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Point d’entrée des profils pour une API de code spécifique (authentification)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Pour plus d’informations, reportez-vous aux documents suivants :

* [Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 4. Qu’est-ce qu’une session d’authentification et combien de temps est-elle valide ? {#authentication-phase-faq4}

La session d’authentification est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session).

La session d’authentification stocke des informations sur le processus d’authentification lancé qui peuvent être récupérées à partir du point d’entrée Sessions.

La session d’authentification est valide pendant une période limitée et courte spécifiée au moment de l’émission, indiquant le temps dont dispose l’utilisateur pour terminer le processus d’authentification avant de devoir redémarrer le flux.

L’application cliente peut utiliser la réponse de session d’authentification pour savoir comment procéder au processus d’authentification. Notez qu’il existe des cas où l’utilisateur n’est pas tenu de s’authentifier, par exemple lorsqu’il fournit un accès temporaire, un accès dégradé ou lorsqu’il est déjà authentifié.

Pour plus d’informations, consultez les documents suivants :

* [Créer une API de session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API Reprendre la session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 5. Qu’est-ce qu’un code d’authentification et combien de temps est-il valide ? {#authentication-phase-faq5}

Le code d’authentification est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code).

Le code d’authentification stocke une valeur unique générée lorsqu’un utilisateur lance le processus d’authentification et identifie de manière unique la session d’authentification d’un utilisateur jusqu’à ce que le processus soit terminé ou jusqu’à l’expiration de la session d’authentification.

Le code d’authentification est valide pendant une période limitée et courte spécifiée au moment du lancement de la session d’authentification, indiquant le temps dont dispose l’utilisateur pour terminer le processus d’authentification avant de devoir redémarrer le flux.

L’application cliente peut utiliser le code d’authentification pour permettre à l’utilisateur d’effectuer ou de reprendre le processus d’authentification sur le même appareil ou sur un second, étant donné que la session d’authentification n’a pas expiré.

Pour plus d’informations, consultez les documents suivants :

* [Créer une API de session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API Reprendre la session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 6. Comment l’application cliente peut-elle savoir si l’utilisateur a saisi un code d’authentification valide et si la session d’authentification n’a pas encore expiré ? {#authentication-phase-faq6}

L’application cliente peut valider le code d’authentification saisi par l’utilisateur dans une application secondaire (écran) en envoyant une requête au point d’entrée Sessions responsable de la récupération des informations de session d’authentification associées au code d’authentification.

L’application cliente recevait une [erreur](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) si le code d’authentification fourni était mal saisi ou si la session d’authentification avait expiré.

Pour plus d’informations, consultez la documentation [Récupérer la session d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### 7. Qu’est-ce qu’un profil et combien de temps est-il valide ? {#authentication-phase-faq7}

Le profil est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile).

Le profil stocke des informations sur la validité d’authentification de l’utilisateur, des informations de métadonnées et bien d’autres informations qui peuvent être récupérées à partir du point d’entrée des profils.

L’application cliente peut utiliser le profil pour connaître le statut d’authentification de l’utilisateur, accéder aux informations de métadonnées de l’utilisateur ou trouver la méthode utilisée pour s’authentifier.

Pour plus d’informations, reportez-vous aux documents suivants :

* [API du point d’entrée des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Point d’entrée des profils pour une API MVPD spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Point d’entrée des profils pour une API de code spécifique (authentification)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Le profil est valide pendant une période limitée spécifiée lors de l’interrogation, indiquant la durée pendant laquelle l’utilisateur dispose d’une authentification valide avant de devoir repasser par la phase d’authentification.

Cette période limitée appelée authentification (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) peut être affichée et modifiée via le tableau de bord Adobe Pass [TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre organisation ou par un représentant Adobe Pass Authentication agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

+++

### FAQ sur la phase de préautorisation {#preauthorization-phase-faqs-general}

+++FAQ sur la phase de préautorisation

#### 1. Quel est l’objectif de la phase de préautorisation ? {#preauthorization-phase-faq1}

L’objectif de la phase de préautorisation est de permettre à l’application cliente de présenter un sous-ensemble de ressources de son catalogue auquel l’utilisateur ou l’utilisatrice serait autorisé à accéder.

La phase de préautorisation peut améliorer l’expérience de l’utilisateur lorsqu’il ouvre l’application cliente pour la première fois ou accède à une nouvelle section.

#### 2. La phase de préautorisation est-elle obligatoire ? {#preauthorization-phase-faq2}

La phase de préautorisation n’est pas obligatoire, l’application cliente peut ignorer cette phase si elle souhaite présenter un catalogue de ressources sans les filtrer au préalable en fonction des droits de l’utilisateur.

#### 3. Qu’est-ce qu’une décision de préautorisation ? {#preauthorization-phase-faq3}

La préautorisation est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization), tandis que le terme de décision se trouve également dans le [Glossaire](rest-api-v2-glossary.md#decision).

La décision de préautorisation stocke des informations sur le résultat de la recherche du processus de préautorisation MVPD qui peuvent être récupérées à partir du point d’entrée de préautorisation des décisions.

L’application cliente peut utiliser les décisions de préautorisation pour présenter un sous-ensemble de ressources auxquelles les décisions (informatives) du fournisseur de télévision permettraient à l’utilisateur d’accéder.

Pour plus d’informations, reportez-vous aux documents suivants :

* [Récupération de l’API de décisions de préautorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### 4. Pourquoi un jeton de média est-il manquant dans la décision de préautorisation ? {#preauthorization-phase-faq4}

Il manque un jeton multimédia dans la décision de préautorisation, car la phase de préautorisation ne doit pas être utilisée pour lire les ressources, car c’est l’objectif de la phase d’autorisation.

#### 5. Qu’est-ce qu’une ressource et quels formats sont pris en charge ? {#preauthorization-phase-faq5}

La ressource est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

La ressource est un identifiant unique convenu avec les MVPD et associé à un contenu que l’application cliente peut diffuser.

L’identifiant unique de la ressource peut avoir deux formats :

* Format de chaîne simple, tel qu’un identifiant unique pour un canal (marque).
* Format RSS (Media RSS) contenant des informations supplémentaires telles que le titre, les évaluations et les métadonnées de contrôle parental.

Pour plus d’informations, consultez la documentation [Ressources protégées](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 6. Pour combien de ressources l’application cliente peut-elle obtenir une décision de préautorisation à la fois ? {#preauthorization-phase-faq6}

L’application cliente peut obtenir une décision de préautorisation pour un nombre limité de ressources dans une seule requête API, généralement jusqu’à 5, en raison des conditions imposées par les MVPD.

Ce nombre maximal de ressources peut être consulté et modifié après accord avec les MVPD via le tableau de bord Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre entreprise ou par un représentant Adobe Pass Authentication agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

### FAQ sur la phase d’autorisation {#authorization-phase-faqs-general}

+++FAQ sur la phase d’autorisation

#### 1. Quel est l’objectif de la phase d’autorisation ? {#authorization-phase-faq1}

La phase d’autorisation a pour but de permettre à l’application cliente de lire les ressources demandées par l’utilisateur ou l’utilisatrice après validation de ses droits avec MVPD.

#### 2. La phase d’autorisation est-elle obligatoire ? {#authorization-phase-faq2}

La phase d’autorisation est obligatoire, l’application cliente ne peut pas ignorer cette phase si elle souhaite lire les ressources demandées par l’utilisateur ou l’utilisatrice, car elle doit vérifier auprès du MVPD que l’utilisateur ou l’utilisatrice a le droit de lire avant de libérer le flux.

#### 3. Qu&#39;est-ce qu&#39;une décision d&#39;autorisation et quelle est sa durée de validité ? {#authorization-phase-faq3}

L’autorisation est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization), tandis que le terme de décision se trouve également dans le [Glossaire](rest-api-v2-glossary.md#decision).

La décision d’autorisation stocke des informations sur le résultat de la recherche du processus d’autorisation MVPD qui peuvent être récupérées à partir du point d’entrée Autoriser les décisions .

L’application cliente peut utiliser la décision d’autorisation contenant un jeton multimédia pour lire le flux de ressources au cas où la décision Fournisseur de télévision (faisant autorité) permettrait à l’utilisateur d’y accéder.

Pour plus d’informations, reportez-vous aux documents suivants :

* [Récupérer l’API des décisions d’autorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

La décision d’autorisation est valide pendant une période limitée et courte spécifiée au moment de l’émission, indiquant la durée pendant laquelle elle sera mise en cache par l’authentification Adobe Pass avant de devoir réitérer la requête au MVPD.

Cette période limitée appelée autorisation (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) peut être consultée et modifiée via le tableau de bord Adobe Pass [TVE](rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre organisation ou par un représentant Adobe Pass Authentication agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### 4. Qu’est-ce qu’un jeton multimédia et combien de temps est-il valide ? {#authorization-phase-faq4}

Le jeton de média est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token).

Le jeton de média se compose d’une chaîne signée envoyée en texte clair qui peut être récupérée à partir du point d’entrée Autoriser les décisions .

Pour plus d’informations, consultez la documentation du [Vérificateur de jeton multimédia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

Le jeton multimédia est valide pendant une période limitée et courte spécifiée au moment de l’émission, indiquant la durée pendant laquelle il doit être utilisé par l’application cliente avant d’avoir à interroger à nouveau le point d’entrée d’autorisation des décisions.

L’application cliente peut utiliser le jeton multimédia pour lire un flux de ressources au cas où la décision du fournisseur de télévision (faisant autorité) permettrait à l’utilisateur d’y accéder.

Pour plus d’informations, reportez-vous aux documents suivants :

* [Récupérer l’API des décisions d’autorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 5. Qu’est-ce qu’une ressource et quels formats sont pris en charge ? {#authorization-phase-faq5}

La ressource est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

La ressource est un identifiant unique convenu avec les MVPD et associé à un contenu que l’application cliente peut diffuser.

L’identifiant unique de la ressource peut avoir deux formats :

* Format de chaîne simple, tel qu’un identifiant unique pour un canal (marque).
* Format RSS (Media RSS) contenant des informations supplémentaires telles que le titre, les évaluations et les métadonnées de contrôle parental.

Pour plus d’informations, consultez la documentation [Ressources protégées](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 6. Pour combien de ressources la demande du client peut-elle obtenir une décision d’autorisation à la fois ? {#authorization-phase-faq6}

L’application cliente peut obtenir une décision d’autorisation pour un nombre limité de ressources dans une seule requête API, généralement jusqu’à 1, en raison des conditions imposées par les MVPD.

+++

### FAQ sur la phase de déconnexion {#logout-phase-faqs-general}

+++FAQ sur la phase de déconnexion

#### 1. Quel est l’objectif de la phase de déconnexion ? {#logout-phase-faq1}

L’objectif de la phase de déconnexion est de permettre à l’application cliente de mettre fin au profil authentifié de l’utilisateur dans l’authentification Adobe Pass à la demande de l’utilisateur.

+++

### FAQ sur les en-têtes {#headers-faqs-general}

+++FAQ sur les en-têtes

#### 1. Comment calculer la valeur de l’en-tête d’autorisation ? {#headers-faq1}

L’en-tête de requête [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) contient le jeton d’accès `Bearer` requis par l’application cliente pour accéder aux API protégées d’Adobe Pass.

La valeur de l’en-tête Autorisation doit être obtenue à partir de l’authentification Adobe Pass pendant la phase d’enregistrement.

Pour plus d’informations, reportez-vous aux documents suivants :

* [Présentation de l’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [API de récupération des informations d’identification du client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Récupérer l’API du jeton d’accès](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

Si l’application cliente migre de l’API REST V1 vers l’API REST V2, elle peut continuer à utiliser la même méthode pour obtenir le jeton d’accès `Bearer` que précédemment.

#### 2. Comment calculer la valeur de l’en-tête AP-Device-Identifier ? {#headers-faq2}

L’en-tête de requête [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contient l’identifiant de l’appareil de diffusion en continu tel qu’il a été créé par l’application cliente.

La documentation de l’en-tête [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) fournit quelques exemples de la manière de calculer la valeur pour différentes plateformes, mais l’application cliente peut choisir d’utiliser une méthode différente en fonction de sa propre logique commerciale et de ses propres exigences.

Si l’application cliente migre de l’API REST V1 vers l’API REST V2, elle peut continuer à utiliser la même méthode pour calculer l’identifiant de l’appareil que précédemment.

#### 3. Comment calculer la valeur de l’en-tête X-Device-Info ? {#headers-faq3}

L’en-tête de requête [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contient les informations du client (appareil, connexion et application) liées à l’appareil de diffusion en continu actuel.

La documentation de l’en-tête [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) fournit quelques exemples de la manière de calculer la valeur pour différentes plateformes, mais l’application cliente peut choisir d’utiliser une autre méthode en fonction de sa propre logique commerciale et de ses propres exigences.

Si l’application cliente migre de l’API REST V1 vers l’API REST V2, elle peut continuer à utiliser la même méthode pour calculer les informations sur l’appareil que précédemment.

+++

## FAQ sur la migration {#migration-faqs}

Passez à cette section si vous travaillez sur une application qui doit migrer une application existante vers l’API REST V2.

>[!MORELIKETHIS]
>
> * [FAQ sur Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### Questions fréquentes générales sur la migration {#general-migration-faqs}

+++Questions fréquentes générales sur la migration

#### 1. Dois-je déployer une nouvelle application cliente migrée vers l’API REST V2 en une seule fois pour tous les utilisateurs ? {#migration-faq1}

Non.

L’application cliente n’est pas tenue de déployer simultanément une nouvelle version intégrant la version V2 de l’API REST à tous les utilisateurs.

L’authentification Adobe Pass continuera à prendre en charge les anciennes versions d’applications clientes intégrant l’API REST V1 ou SDK jusqu’à la fin de 2025.

#### 2. Dois-je déployer simultanément une nouvelle application cliente migrée vers l’API REST V2 sur toutes les API et tous les flux ? {#migration-faq2}

Oui.

L’application cliente est requise pour déployer simultanément une nouvelle version intégrant l’API REST V2 à tous les flux et API d’authentification Adobe Pass.

Dans le cas du flux « deuxième authentification de l’écran », l’application cliente doit déployer simultanément une nouvelle version intégrant l’API REST V2 pour les applications [principales](rest-api-v2-glossary.md#primary-application) et [secondaires](rest-api-v2-glossary.md#secondary-application).

L’authentification Adobe Pass ne prend pas en charge les implémentations « hybrides » qui intègrent à la fois l’API REST V2 et l’API REST V1/SDK entre les API et les flux.

#### 3. L’authentification de l’utilisateur sera-t-elle conservée lors de la mise à jour vers une nouvelle application cliente migrée vers l’API REST V2 ? {#migration-faq3}

Non.

L’authentification de l’utilisateur obtenue dans les anciennes versions de l’application cliente intégrant l’API REST V1 ou SDK n’est pas conservée.

Par conséquent, l’utilisateur devra s’authentifier à nouveau dans la nouvelle application cliente migrée vers l’API REST V2.

#### 4. Les codes d’erreur améliorés sont-ils activés par défaut dans l’API REST V2 ? {#migration-faq4}

Oui.

Les applications clientes intégrant l’API REST V2 bénéficient de la fonctionnalité de codes d’erreur améliorée activée par défaut.

Pour plus d’informations, consultez la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

+++

### Migration de l’API REST V1 vers l’API REST V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Continuez avec cette sous-section si vous travaillez sur une application qui doit migrer de l’API REST V1 vers l’API REST V2.

#### FAQ sur la phase d&#39;enregistrement {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++FAQ sur la phase d’enregistrement

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’enregistrement ? {#registration-phase-v1-to-v2-faq1}

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

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de configuration ? {#configuration-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans le tableau suivant :

| Champ d’application | API REST V1 | API REST V2 | Observations |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec l’intégration active | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | GET [<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Questions fréquentes sur la phase d’authentification {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++FAQ sur la phase d’authentification

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’authentification ? {#authentication-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans le tableau suivant :

| Champ d’application | API REST V1 | API REST V2 | Observations |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer le code d’enregistrement (code d’authentification) | [<br/> POST /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [<br/> POST /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le code d’enregistrement (code d’authentification) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reprendre l’authentification (MVPD) sur le deuxième écran (application) | GET [<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [<br/> POST /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Lancer l’authentification (MVPD) | GET [<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [GET <br/> /api/v1/checkauthn (premier écran)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (deuxième écran)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupérer le jeton d’authentification de l’utilisateur (profil) | GET [<br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | GET [<br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase de préautorisation {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++FAQ sur la phase de préautorisation

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de préautorisation ? {#preauthorization-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans le tableau suivant :

| Champ d’application | API REST V1 | API REST V2 | Observations |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération des ressources préautorisées (décisions de préautorisation) | [GET <br/> /api/v1/preauthorize (premier écran)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (deuxième écran)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [<br/> de POST /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase d’autorisation {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++FAQ sur la phase d’autorisation

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’autorisation ? {#authorization-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans le tableau suivant :

| Champ d’application | API REST V1 | API REST V2 | Observations |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer l’autorisation (MVPD) | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [<br/> de POST /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Récupération du jeton d’autorisation (décision d’autorisation) | GET [<br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [<br/> de POST /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Récupération du jeton d’autorisation court (jeton multimédia) | GET [<br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [<br/> de POST /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase de déconnexion {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++FAQ sur la phase de déconnexion

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de déconnexion ? {#logout-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans le tableau suivant :

| Champ d’application | API REST V1 | API REST V2 | Observations |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer la déconnexion | GET [<br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | GET [<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migration de SDK vers l’API REST V2 {#migration-sdk-to-rest-api-v2}

Continuez avec cette sous-section si vous travaillez sur une application qui doit migrer de SDK vers l’API REST V2.

#### FAQ sur la phase d&#39;enregistrement {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase d’enregistrement

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’enregistrement ? {#registration-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement dynamique du client (DCR) | Transmission d’une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> GET [<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Présentation de l’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### SDK AccessEnabler iOS/tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement dynamique du client (DCR) | Transmission d’une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> GET [<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Présentation de l’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### SDK AccessEnabler Android

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement dynamique du client (DCR) | Transmission d’une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> GET [<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Présentation de l’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### SDK FireOS AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement dynamique du client (DCR) | Transmission d’une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> GET [<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Présentation de l’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### FAQ sur la phase de configuration {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase de configuration

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de configuration ? {#configuration-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec l’intégration active | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | GET [<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### SDK AccessEnabler iOS/tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec l’intégration active | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | GET [<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### SDK AccessEnabler Android

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec l’intégration active | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | GET [<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### SDK FireOS AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec l’intégration active | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | GET [<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Questions fréquentes sur la phase d’authentification {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase d’authentification

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’authentification ? {#authentication-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [<br/> POST /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK AccessEnabler iOS

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [<br/> POST /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK tvOS AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer le code d’enregistrement (code d’authentification) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [<br/> POST /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le code d’enregistrement (code d’authentification) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reprendre l’authentification (MVPD) sur le deuxième écran (application) | GET [<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [<br/> POST /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Lancer l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [<br/> POST /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK AccessEnabler Android

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [<br/> POST /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### SDK FireOS AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer le code d’enregistrement (code d’authentification) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [<br/> POST /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le code d’enregistrement (code d’authentification) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reprendre l’authentification (MVPD) sur le deuxième écran (application) | GET [<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [<br/> POST /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Lancer l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [<br/> POST /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérifier le statut d’authentification de l’utilisateur | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L’application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérifier le statut d’authentification de l’utilisateur</li><li>Récupérer le profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase de préautorisation {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase de préautorisation

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de préautorisation ? {#preauthorization-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération des ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [<br/> de POST /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler iOS/tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération des ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [<br/> de POST /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler Android

| Champ d’application | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération des ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [<br/> de POST /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Champ d’application | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération des ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [<br/> de POST /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase d’autorisation {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase d’autorisation

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’autorisation ? {#authorization-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [<br/> de POST /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler iOS/tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [<br/> de POST /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler Android

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [<br/> de POST /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### SDK FireOS AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [<br/> de POST /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L’application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupérer la décision d’autorisation</li><li>Récupérer un jeton de média court</li></ul> <br/> Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### FAQ sur la phase de déconnexion {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++FAQ sur la phase de déconnexion

##### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de déconnexion ? {#logout-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau doivent être prises en compte et sont présentées dans les tableaux suivants :

###### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer la déconnexion | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | GET [<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler iOS/tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer la déconnexion | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | GET [<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### SDK AccessEnabler Android

| Champ d’application | SDK | API REST V2 | Observations |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer la déconnexion | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | GET [<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### SDK FireOS AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer la déconnexion | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | GET [<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d’informations, consultez les documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
