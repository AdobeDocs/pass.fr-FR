---
title: FAQ sur l’API REST V2
description: FAQ sur l’API REST V2
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '7304'
ht-degree: 0%

---

# FAQ sur l’API REST V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Ce document fournit des réponses d’aperçu pour les questions fréquentes sur l’adoption de l’API REST V2 d’authentification Adobe Pass.

Pour plus d’informations sur l’ensemble de l’API REST V2, consultez la documentation [Présentation de l’API REST V2](./rest-api-v2-overview.md).

## Questions fréquentes d’ordre général {#general-faqs}

Commencez par cette section si vous travaillez sur une application qui doit intégrer l&#39;API REST V2, qu&#39;il s&#39;agisse d&#39;une nouvelle application ou d&#39;une application existante qui migre depuis [l&#39;API REST V1](#migrate-rest-api-v1-to-rest-api-v2) ou [SDK](#migrate-sdk-to-rest-api-v2).

Pour plus d’informations sur les étapes et les détails de la migration, reportez-vous aux sections suivantes également.

+++FAQ sur la phase d’enregistrement

### 1. Quel est l’objectif de la phase d’enregistrement ? {#registration-phase-faq1}

L’objectif de la phase d’enregistrement est d’enregistrer l’application client contre l’authentification Adobe Pass par le biais du processus [Enregistrement dynamique du client (DCR)](./rest-api-v2-glossary.md#dcr).

Le processus d’enregistrement de client dynamique (DCR) nécessite que l’application cliente obtienne une paire d’informations d’identification du client et récupère un jeton d’accès en tant qu’objectif final de la phase d’enregistrement.

Pour plus d’informations, consultez la documentation [Présentation de l’enregistrement du client dynamique](/help/authentication/dcr-api/dynamic-client-registration-overview.md) .

### 2. La phase d’enregistrement est-elle obligatoire ? {#registration-phase-faq2}

La phase d’enregistrement est obligatoire, mais l’application cliente peut ignorer cette phase si elle dispose d’une paire d’informations d’identification client en mémoire cache et d’un jeton d’accès toujours valide.

### 3. Qu’est-ce qu’une déclaration logicielle et combien de temps est-elle valide ? {#registration-phase-faq3}

L’instruction logicielle est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#software-statement).

L’instruction logicielle est composée d’un JSON Web Token (JWT) qui peut être généré et téléchargé à partir du [ tableau de bord TVE](./rest-api-v2-glossary.md#tve-dashboard) d’Adobe Pass par l’un des administrateurs de votre organisation ou par un représentant de l’authentification Adobe Pass agissant en votre nom.

L’instruction logicielle est valide pendant une période illimitée, mais vous pouvez choisir de demander à un représentant de l’authentification Adobe Pass de la révoquer à tout moment.

L’application cliente doit stocker l’instruction logicielle et l’utiliser lorsque vous devez récupérer les informations d’identification du client.

Pour plus d’informations, reportez-vous à la documentation [Présentation de l’enregistrement du client dynamique](/help/authentication/dcr-api/dynamic-client-registration-overview.md) .

### 4. Comment générer et télécharger une instruction logicielle ? {#registration-phase-faq4}

Cette opération peut être effectuée par l’intermédiaire du [tableau de bord TVE](./rest-api-v2-glossary.md#tve-dashboard) d’Adobe Pass par l’un des administrateurs de votre organisation ou par un représentant de l’authentification Adobe Pass agissant en votre nom.

Pour plus d’informations, reportez-vous au [Guide de l’utilisateur des canaux de tableau de bord TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) ou au [Guide de l’utilisateur des programmeurs de tableaux de bord TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) .

### 5. Que se passe-t-il si une déclaration logicielle est révoquée ? {#registration-phase-faq5}

Lorsque la déclaration logicielle est révoquée, il y a une conséquence importante à prendre en compte :

* Les applications clientes qui utilisent l’instruction logicielle révoquée ne pourront plus passer par les flux [ de droits](./rest-api-v2-glossary.md#entitlement), ce qui signifie que les utilisateurs ne pourront plus lire le contenu.

### 6. Quelles sont les informations d’identification du client et combien de temps sont-elles valides ? {#registration-phase-faq6}

Les informations d’identification du client sont un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#client-credentials).

Les informations d’identification du client se composent d’un identifiant client et d’une paire de secrets client qui peuvent être récupérés à partir du point de terminaison de l’enregistrement du client.

Les informations d’identification du client sont valides pendant une période illimitée.

L’application cliente doit stocker indéfiniment les informations d’identification du client et les utiliser lorsqu’il est nécessaire de récupérer un jeton d’accès.

Pour plus d&#39;informations, consultez la documentation [Récupérer les informations d&#39;identification du client](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) .

### 7. Comment gérer les informations d’identification du client ? {#registration-phase-faq7}

Nous recommandons à l’application cliente de gérer une paire d’informations d’identification client unique pour chaque instance d’application utilisateur en cas d’intégrations client-à-serveur et serveur-à-serveur avec l’authentification Adobe Pass.

L’application cliente doit stocker indéfiniment les informations d’identification du client et les utiliser lorsqu’il est nécessaire de récupérer un jeton d’accès.

### 8. Que se passe-t-il si les informations d’identification du client mises en cache sont perdues ? {#registration-phase-faq8}

Lorsque les informations d’identification du client mises en cache sont perdues, vous devez tenir compte de trois conséquences importantes :

* L’application cliente doit obtenir une nouvelle paire d’informations d’identification client.
* L’application cliente doit obtenir un nouveau jeton d’accès à l’aide de la nouvelle paire d’informations d’identification du client.
* L’application cliente devra demander à l’utilisateur de se reconnecter, car l’application cliente perdra l’accès aux profils authentifiés obtenus auparavant.

### 9. Qu’est-ce qu’un jeton d’accès et combien de temps est-il valide ? {#registration-phase-faq9}

Le jeton d’accès est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#access-token).

Le jeton d’accès se compose d’un [jeton porteur](./appendix/headers/rest-api-v2-appendix-headers-authorization.md) qui peut être récupéré à partir du point de terminaison du jeton client.

Le jeton d’accès est valide pendant une période limitée et courte spécifiée au moment de l’émission.

L’application cliente doit stocker le jeton d’accès et l’utiliser jusqu’à ce qu’il expire lors du ciblage de l’API REST V2.

L’application cliente doit obtenir un nouveau jeton d’accès avant l’expiration de celui-ci afin d’empêcher les requêtes non autorisées.

Pour plus d’informations, consultez la documentation [Récupérer le jeton d’accès](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) .

### 10. Comment l’application cliente peut-elle actualiser un jeton d’accès ? {#registration-phase-faq10}

L’application cliente doit actualiser un jeton d’accès de la même manière que la récupération d’un nouveau jeton d’accès, mais en utilisant des informations d’identification client mises en cache.

L’application cliente ne doit pas se réenregistrer pour actualiser un jeton d’accès. Elle doit plutôt utiliser les informations d’identification client stockées, sinon les utilisateurs doivent se reconnecter.

Pour plus d’informations, consultez la documentation [Récupérer le jeton d’accès](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) .

+++

Questions fréquentes sur la phase de configuration +++

### 1. Quel est l’objectif de la phase de configuration ? {#configuration-phase-faq1}

L’objectif de la phase de configuration est de fournir à l’application cliente la liste des MVPD avec lesquels elle est activement intégrée, ainsi que les détails de configuration enregistrés par l’authentification Adobe Pass pour chaque MVPD.

La phase de configuration joue le rôle d’étape préalable à la phase d’authentification lorsque l’application client doit demander à l’utilisateur de sélectionner son fournisseur de télévision.

### 2. La phase de configuration est-elle obligatoire ? {#configuration-phase-faq2}

La phase de configuration n’est pas obligatoire, l’application cliente peut ignorer cette phase dans les scénarios suivants :

* L’utilisateur est déjà authentifié.
* L’utilisateur se voit offrir un accès temporaire par le biais de la fonctionnalité TempPass de base ou promotionnelle.
* L’authentification de l’utilisateur a expiré, mais l’application cliente a mis en cache le MVPD précédemment sélectionné comme choix motivé par une expérience utilisateur et invite simplement l’utilisateur à confirmer qu’il est toujours abonné à ce MVPD.

### 3. Quelle est une configuration et combien de temps est-elle valide ? {#configuration-phase-faq3}

La configuration est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#configuration).

La configuration se compose d’une liste de MVPD qui peuvent être récupérés à partir du point de terminaison Configuration .

L’application cliente peut utiliser la configuration pour présenter un composant d’interface utilisateur appelé &quot;Sélecteur&quot; lorsque l’utilisateur doit sélectionner son MVPD.

L’application cliente doit actualiser la configuration avant que l’utilisateur ne passe par la phase d’authentification.

Pour plus d&#39;informations, consultez la documentation [Récupérer la configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) .

### 4. L’application cliente peut-elle gérer sa propre liste de MVPD ? {#configuration-phase-faq4}

L’application cliente peut gérer sa propre liste de MVPD, mais il est recommandé d’utiliser la configuration fournie par l’authentification Adobe Pass pour s’assurer que la liste est à jour et exacte.

L’application cliente recevrait une [erreur](/help/authentication/enhanced-error-codes.md) de l’API REST d’authentification Adobe Pass V2 si l’utilisateur a sélectionné MVPD n’a pas d’intégration active avec le [fournisseur de services](./rest-api-v2-glossary.md#service-provider) spécifié.

### 5. L’application cliente peut-elle filtrer la liste des MVPD ? {#configuration-phase-faq5}

L’application cliente peut filtrer la liste des MVPD fournis dans la réponse de configuration en implémentant un mécanisme personnalisé basé sur sa propre logique commerciale et des exigences telles que l’emplacement de l’utilisateur ou l’historique de l’utilisateur de la sélection précédente.

### 6. Que se passe-t-il si l’intégration avec un MVPD est désactivée et marquée comme inactive ? {#configuration-phase-faq6}

Lorsque l’intégration à un MVPD est désactivée et marquée comme inactive, le MVPD est supprimé de la liste des MVPD fournies dans d’autres réponses de configuration et il y a deux conséquences importantes à prendre en compte :

* Les utilisateurs non authentifiés de ce MVPD ne pourront plus terminer la phase d’authentification à l’aide de ce MVPD.
* Les utilisateurs authentifiés de ce MVPD ne pourront plus terminer les phases de préautorisation, d’autorisation ou de déconnexion à l’aide de ce MVPD.

L’application cliente recevrait une [erreur](/help/authentication/enhanced-error-codes.md) de l’API REST d’authentification Adobe Pass V2 si l’utilisateur a sélectionné MVPD n’a plus d’intégration active avec le [fournisseur de services](./rest-api-v2-glossary.md#service-provider) spécifié.

### 7. Que se passe-t-il si l’intégration avec un MVPD est réactivée et marquée comme active ? {#configuration-phase-faq7}

Lorsque l’intégration avec un MVPD est réactivée et marquée comme active, le MVPD est inclus dans la liste des MVPD fournis dans les réponses de configuration supplémentaires et il y a deux conséquences importantes à prendre en compte :

* Les utilisateurs non authentifiés de ce MVPD pourront à nouveau terminer la phase d’authentification à l’aide de ce MVPD.
* Les utilisateurs authentifiés de ce MVPD pourront à nouveau terminer les phases de préautorisation, d’autorisation ou de déconnexion à l’aide de ce MVPD.

### 8. Comment activer ou désactiver l’intégration avec un MVPD ? {#configuration-phase-faq8}

Cette opération peut être effectuée par l’intermédiaire du [tableau de bord TVE](./rest-api-v2-glossary.md#tve-dashboard) d’Adobe Pass par l’un des administrateurs de votre organisation ou par un représentant de l’authentification Adobe Pass agissant en votre nom.

Pour plus d’informations, consultez la documentation [TVE Dashboard Integrations Guide de l’utilisateur](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#disable-integration) .

+++

Questions fréquentes sur la phase d’authentification +++

### 1. Quel est l’objectif de la phase d’authentification ? {#authentication-phase-faq1}

L’objectif de la phase d’authentification est de fournir à l’application cliente la capacité de vérifier l’identité de l’utilisateur et d’obtenir des informations de métadonnées utilisateur.

La phase d’authentification joue le rôle d’étape préalable à la phase de préautorisation ou d’autorisation lorsque l’application cliente doit lire du contenu.

### 2. Comment l’application cliente peut-elle savoir si l’utilisateur est déjà authentifié ? {#authentication-phase-faq2}

L’application cliente peut interroger l’un des points de terminaison suivants, capable de vérifier si un utilisateur est déjà authentifié et de renvoyer des informations de profil :

* [API Profiles endpoint](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Point de terminaison de profils pour une API MVPD spécifique](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Point de terminaison des profils pour une API de code spécifique (d’authentification)](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Pour plus de détails, reportez-vous aux documents suivants :

* [Flux de profils de base exécuté dans une application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 3. Comment l’application cliente peut-elle obtenir les informations de métadonnées de l’utilisateur ? {#authentication-phase-faq3}

L’application cliente peut interroger l’un des points de terminaison suivants, capable de renvoyer des informations [métadonnées utilisateur](/help/authentication/user-metadata-feature.md) dans le cadre des informations de profil :

* [API Profiles endpoint](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Point de terminaison de profils pour une API MVPD spécifique](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Point de terminaison des profils pour une API de code spécifique (d’authentification)](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Pour plus de détails, reportez-vous aux documents suivants :

* [Flux de profils de base exécuté dans une application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 4. Quelle est une session d’authentification et combien de temps est-elle valide ? {#authentication-phase-faq4}

La session d’authentification est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#session).

La session d’authentification stocke des informations sur le processus d’authentification lancé, qui peuvent être récupérées à partir du point de terminaison sessions .

La session d’authentification est valide pendant une période limitée et courte spécifiée au moment du problème, indiquant le temps nécessaire à l’utilisateur pour terminer le processus d’authentification avant de devoir redémarrer le flux.

L’application cliente peut utiliser la réponse de session d’authentification pour savoir comment procéder. Notez que dans certains cas, l’utilisateur n’est pas tenu de s’authentifier, par exemple en fournissant un accès temporaire, un accès dégradé ou lorsque l’utilisateur est déjà authentifié.

Pour plus d&#39;informations, consultez les documents suivants :

* [Création d’une API de session d’authentification](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API de session d’authentification de reprise](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flux d’authentification de base exécuté sur une application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flux d’authentification de base exécuté sur une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 5. Qu’est-ce qu’un code d’authentification et combien de temps est-il valide ? {#authentication-phase-faq5}

Le code d’authentification est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#code).

Le code d’authentification stocke une valeur unique générée lorsqu’un utilisateur lance le processus d’authentification et identifie de manière unique la session d’authentification d’un utilisateur jusqu’à ce que le processus soit terminé ou jusqu’à l’expiration de la session d’authentification.

Le code d’authentification est valide pendant une période limitée et courte spécifiée au moment du lancement de la session d’authentification, indiquant le temps nécessaire à l’exécution du processus d’authentification par l’utilisateur avant de devoir redémarrer le flux.

L’application cliente peut utiliser le code d’authentification pour permettre à l’utilisateur de terminer ou de reprendre le processus d’authentification soit sur le même appareil, soit sur un second appareil, en considérant que la session d’authentification n’a pas expiré.

Pour plus d&#39;informations, consultez les documents suivants :

* [Création d’une API de session d’authentification](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [API de session d’authentification de reprise](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flux d’authentification de base exécuté sur une application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flux d’authentification de base exécuté sur une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 6. Comment l’application cliente peut-elle savoir si l’utilisateur a saisi un code d’authentification valide et si la session d’authentification n’a pas encore expiré ? {#authentication-phase-faq6}

L’application cliente peut valider le code d’authentification saisi par l’utilisateur dans une application secondaire (écran) en envoyant une requête au point de terminaison sessions responsable de la récupération des informations de session d’authentification associées au code d’authentification.

L’application cliente recevrait une [erreur](/help/authentication/enhanced-error-codes.md) si le code d’authentification fourni était mal saisi ou si la session d’authentification avait expiré.

Pour plus d&#39;informations, consultez la documentation [Récupérer la session d&#39;authentification](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) .

### 7. Qu&#39;est-ce qu&#39;un profil et combien de temps est-il valide ? {#authentication-phase-faq7}

Le profil est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#profile).

Le profil stocke des informations sur la validité de l’authentification de l’utilisateur, des informations de métadonnées, etc., qui peuvent être récupérées à partir du point de terminaison Profils .

L’application cliente peut utiliser le profil pour connaître l’état d’authentification de l’utilisateur, accéder aux informations de métadonnées utilisateur ou trouver la méthode utilisée pour s’authentifier.

Pour plus de détails, reportez-vous aux documents suivants :

* [API Profiles endpoint](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Point de terminaison de profils pour une API MVPD spécifique](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Point de terminaison des profils pour une API de code spécifique (d’authentification)](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Flux de profils de base exécuté dans une application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Le profil est valide pendant une période limitée spécifiée lors de l’interrogation, indiquant le temps pendant lequel l’utilisateur dispose d’une authentification valide avant de devoir passer à nouveau la phase d’authentification.

Cette période limitée appelée authentification (authN) [TTL](./rest-api-v2-glossary.md#ttl) peut être visualisée et modifiée via le [tableau de bord TVE](./rest-api-v2-glossary.md#tve-dashboard) d’Adobe Pass par l’un des administrateurs de votre organisation ou par un représentant de l’authentification Adobe Pass agissant en votre nom.

Pour plus d’informations, consultez la documentation [TVE Dashboard Integrations Guide de l’utilisateur](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) .

+++

Questions fréquentes sur la phase de préautorisation +++

### 1. Quel est l’objectif de la phase de préautorisation ? {#preauthorization-phase-faq1}

L’objectif de la phase de préautorisation est de permettre à l’application cliente de présenter un sous-ensemble de ressources de son catalogue auxquelles l’utilisateur aurait droit.

La phase de préautorisation peut améliorer l’expérience utilisateur lorsque l’utilisateur ouvre l’application cliente pour la première fois ou accède à une nouvelle section.

### 2. La phase de préautorisation est-elle obligatoire ? {#preauthorization-phase-faq2}

La phase de préautorisation n’est pas obligatoire, l’application cliente peut ignorer cette phase si elle souhaite présenter un catalogue de ressources sans d’abord les filtrer en fonction des droits de l’utilisateur.

### 3. Qu’est-ce qu’une décision de préautorisation ? {#preauthorization-phase-faq3}

La préautorisation est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#preauthorization), tandis que le terme de décision se trouve également dans le [Glossaire](./rest-api-v2-glossary.md#decision).

La décision de préautorisation stocke des informations sur le résultat de l’enquête de processus de préautorisation MVPD qui peuvent être récupérées à partir du point de terminaison Decisions Preauthorized .

L’application cliente peut utiliser les décisions de préautorisation pour présenter un sous-ensemble de ressources auxquelles le fournisseur de télévision (informations) aurait accès.

Pour plus de détails, reportez-vous aux documents suivants :

* [Récupération de l’API de décisions de préautorisation](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Flux de préautorisation de base effectué dans une application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

### 4. Pourquoi la décision de préautorisation ne comporte-t-elle pas de jeton média ? {#preauthorization-phase-faq4}

Il manque un jeton multimédia à la décision de préautorisation, car la phase de préautorisation ne doit pas être utilisée pour lire les ressources, car il s’agit de l’objectif de la phase d’autorisation.

### 5. Qu’est-ce qu’une ressource et quels formats sont pris en charge ? {#preauthorization-phase-faq5}

La ressource est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#resource).

La ressource est un identifiant unique qui est convenu avec les MVPD et associé à un contenu que l’application cliente peut diffuser.

L’identifiant unique de la ressource peut avoir deux formats :

* Un format de chaîne simple tel qu’un identifiant unique pour un canal (marque).
* Format Media RSS (MRSS) contenant des informations supplémentaires telles que le titre, les évaluations et les métadonnées de contrôle parental.

Pour plus d’informations, consultez la documentation [Identification des ressources protégées](/help/authentication/identify-protected-resources.md).

### 6. Pour combien de ressources l’application cliente peut-elle obtenir une décision de préautorisation à la fois ? {#preauthorization-phase-faq6}

L’application cliente peut obtenir une décision de préautorisation pour un nombre limité de ressources dans une seule requête API, généralement jusqu’à 5, en raison des conditions imposées par les MVPD.

Ce nombre maximum de ressources peut être consulté et modifié après avoir accepté les MVPD par le biais du [tableau de bord TVE](./rest-api-v2-glossary.md#tve-dashboard) d’Adobe Pass par l’un des administrateurs de votre organisation ou par un représentant de l’authentification Adobe Pass agissant en votre nom.

Pour plus d’informations, consultez la documentation [TVE Dashboard Integrations Guide de l’utilisateur](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) .

+++

+++FAQ sur la phase d’autorisation

### 1. Quel est l’objectif de la phase d’autorisation ? {#authorization-phase-faq1}

L’objectif de la phase d’autorisation est de fournir à l’application cliente la capacité de lire les ressources demandées par l’utilisateur après validation de ses droits avec le MVPD.

### 2. La phase d’autorisation est-elle obligatoire ? {#authorization-phase-faq2}

La phase d’autorisation est obligatoire, l’application cliente ne peut pas ignorer cette phase si elle souhaite lire les ressources demandées par l’utilisateur, car elle doit vérifier auprès du MVPD que l’utilisateur a le droit avant de publier le flux.

### 3. Quelle est une décision d’autorisation et combien de temps est-elle valide ? {#authorization-phase-faq3}

L’autorisation est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#authorization), tandis que le terme de décision se trouve également dans le [Glossaire](./rest-api-v2-glossary.md#decision).

La décision d’autorisation stocke des informations sur le résultat de la requête du processus d’autorisation MVPD qui peuvent être récupérées à partir du point de terminaison Decisions Authorize.

L’application cliente peut utiliser la décision d’autorisation contenant un jeton multimédia pour lire le flux de ressources au cas où la décision du fournisseur de télévision (autorité) permettrait à l’utilisateur d’y accéder.

Pour plus de détails, reportez-vous aux documents suivants :

* [Récupération de l’API des décisions d’autorisation](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flux d’autorisation de base exécuté sur une application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

La décision d’autorisation est valide pendant une période limitée et courte spécifiée au moment de l’émission, indiquant le temps pendant lequel elle sera mise en cache par l’authentification Adobe Pass avant d’avoir à interroger à nouveau le MVPD.

Cette période limitée appelée autorisation (authZ) [TTL](./rest-api-v2-glossary.md#ttl) peut être visualisée et modifiée via le [tableau de bord TVE](./rest-api-v2-glossary.md#tve-dashboard) d’Adobe Pass par l’un des administrateurs de votre organisation ou par un représentant de l’authentification Adobe Pass agissant en votre nom.

Pour plus d’informations, consultez la documentation [TVE Dashboard Integrations Guide de l’utilisateur](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) .

### 4. Qu’est-ce qu’un jeton multimédia et combien de temps est-il valide ? {#authorization-phase-faq4}

Le jeton multimédia est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#media-token).

Le jeton multimédia est constitué d’une chaîne signée envoyée en texte clair qui peut être récupérée à partir du point de terminaison Decisions Authorize.

Pour plus d’informations, consultez la documentation [Intégration du vérificateur de jeton multimédia](/help/authentication/media-token-verifier-int.md) .

Le jeton multimédia est valide pendant une période limitée et courte spécifiée au moment de l’émission, indiquant la durée pendant laquelle il doit être utilisé par l’application cliente avant d’exiger une nouvelle requête du point de terminaison Decisions Authorize.

L’application cliente peut utiliser le jeton multimédia pour lire un flux de ressources, au cas où la décision du fournisseur de télévision (autorité) permettrait à l’utilisateur d’y accéder.

Pour plus de détails, reportez-vous aux documents suivants :

* [Récupération de l’API des décisions d’autorisation](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flux d’autorisation de base exécuté sur une application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

### 5. Qu’est-ce qu’une ressource et quels formats sont pris en charge ? {#authorization-phase-faq5}

La ressource est un terme défini dans la documentation [Glossaire](./rest-api-v2-glossary.md#resource).

La ressource est un identifiant unique qui est convenu avec les MVPD et associé à un contenu que l’application cliente peut diffuser.

L’identifiant unique de la ressource peut avoir deux formats :

* Un format de chaîne simple tel qu’un identifiant unique pour un canal (marque).
* Format Media RSS (MRSS) contenant des informations supplémentaires telles que le titre, les évaluations et les métadonnées de contrôle parental.

Pour plus d’informations, consultez la documentation [Identification des ressources protégées](/help/authentication/identify-protected-resources.md).

### 6. Pour combien de ressources l’application cliente peut-elle obtenir une décision d’autorisation à la fois ? {#authorization-phase-faq6}

L’application cliente peut obtenir une décision d’autorisation pour un nombre limité de ressources dans une seule requête API, généralement jusqu’à 1, en raison des conditions imposées par les MVPD.

+++

Questions fréquentes sur la phase de déconnexion +++

### 1. Quel est l’objectif de la phase de déconnexion ? {#logout-phase-faq1}

L’objectif de la phase de déconnexion est de fournir à l’application cliente la possibilité de mettre fin au profil authentifié de l’utilisateur dans l’authentification Adobe Pass à la demande de l’utilisateur.

+++

Questions fréquentes sur +++Headers

### 1. Comment calculer la valeur de l’en-tête Authorization ? {#headers-faq1}

L’en-tête de requête [Authorization](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) contient le jeton d’accès `Bearer` requis par l’application cliente pour accéder aux API protégées par Adobe Pass.

La valeur de l’en-tête Authorization doit être obtenue à partir de l’authentification Adobe Pass pendant la phase d’enregistrement.

Pour plus de détails, reportez-vous aux documents suivants :

* [Présentation de l’enregistrement du client dynamique](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [Récupération de l’API des informations d’identification client](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Récupération de l’API de jeton d’accès](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flux d’enregistrement de client dynamique](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

Si l’application cliente migre de l’API REST V1 vers l’API REST V2, l’application cliente peut continuer à utiliser la même méthode pour obtenir le jeton d’accès `Bearer` que précédemment.

### 2. Comment calculer la valeur de l’en-tête AP-Device-Identifier ? {#headers-faq2}

L’en-tête de requête [AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contient l’identifiant de l’appareil de diffusion en continu tel qu’il a été créé par l’application cliente.

La documentation de l’en-tête [AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) fournit quelques exemples de calcul de la valeur pour différentes plateformes, mais l’application cliente peut choisir d’utiliser une méthode différente en fonction de sa propre logique commerciale et de ses exigences.

Si l’application cliente migre de l’API REST V1 vers l’API REST V2, l’application cliente peut continuer à utiliser la même méthode pour calculer l’identifiant de l’appareil qu’auparavant.

### 3. Comment calculer la valeur de l’en-tête X-Device-Info ? {#headers-faq3}

L’en-tête de requête [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contient les informations du client (appareil, connexion et application) liées au périphérique de diffusion en continu réel.

La documentation de l’en-tête [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) fournit quelques exemples de calcul de la valeur pour différentes plateformes, mais l’application cliente peut choisir d’utiliser une méthode différente en fonction de sa propre logique commerciale et de ses exigences.

Si l’application cliente migre de l’API REST V1 vers l’API REST V2, l’application cliente peut continuer à utiliser la même méthode pour calculer les informations sur l’appareil qu’auparavant.

+++

## Questions fréquentes sur la migration {#migration-faqs}

Passez à cette section si vous travaillez sur une application qui doit migrer une application existante vers l’API REST V2.

+++Questions fréquentes sur la migration générale

### 1. Dois-je déployer une nouvelle application cliente migrée simultanément vers l’API REST V2 vers tous les utilisateurs ? {#migration-faq1}

Non.

L’application cliente n’est pas requise pour déployer une nouvelle version intégrant l’API REST V2 à tous les utilisateurs simultanément.

L’authentification Adobe Pass continuera à prendre en charge les anciennes versions des applications clientes intégrant l’API REST V1 ou le SDK jusqu’à fin 2025.

### 2. Dois-je déployer une nouvelle application cliente migrée simultanément vers l’API REST V2 sur toutes les API et tous les flux ? {#migration-faq2}

Oui.

L’application cliente est requise pour déployer une nouvelle version intégrant l’API REST V2 dans toutes les API d’authentification Adobe Pass et tous les flux simultanément.

Dans le cas du flux d&#39;authentification du deuxième écran, l&#39;application cliente doit déployer une nouvelle version intégrant l&#39;API REST V2 pour les applications [primary](./rest-api-v2-glossary.md#primary-application) et [secondary](./rest-api-v2-glossary.md#secondary-application) simultanément.

L’authentification Adobe Pass ne prend pas en charge les implémentations &quot;hybrides&quot; qui intègrent à la fois l’API REST V2 et l’API REST V1/SDK entre les API et les flux.

### 3. L’authentification de l’utilisateur sera-t-elle conservée lors de la mise à jour vers une nouvelle application cliente migrée vers l’API REST V2 ? {#migration-faq3}

Non.

L’authentification de l’utilisateur obtenue dans les anciennes versions de l’application cliente intégrant l’API REST V1 ou le SDK ne sera pas conservée.

Par conséquent, l’utilisateur devra se réauthentifier dans la nouvelle application cliente migrée vers l’API REST V2.

### 4. L’application cliente peut-elle utiliser les applications enregistrées existantes (relevés logiciels) ? {#migration-faq4}

L’application cliente ne peut pas réutiliser les applications enregistrées existantes (instructions logicielles). Elle doit donc générer et télécharger de nouvelles applications enregistrées (instructions logicielles) dédiées à l’utilisation de l’API REST V2.

Cette opération peut être effectuée par l’intermédiaire du [tableau de bord TVE](./rest-api-v2-glossary.md#tve-dashboard) d’Adobe Pass par l’un des administrateurs de votre organisation ou par un représentant de l’authentification Adobe Pass agissant en votre nom.

Pour plus d’informations, reportez-vous au [Guide de l’utilisateur des canaux de tableau de bord TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) ou au [Guide de l’utilisateur des programmeurs de tableaux de bord TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) .

Pour le moment, vous devrez demander à un représentant de l’authentification Adobe Pass d’activer l’utilisation de l’API REST V2 pour vos nouvelles applications enregistrées (instructions logicielles), jusqu’à ce que le [tableau de bord TVE](./rest-api-v2-glossary.md#tve-dashboard) d’Adobe Pass soit mis à jour pour permettre la gestion autonome de cette opération.

Afin de distinguer les applications enregistrées (instructions logicielles) utilisées dans les applications clientes utilisant l’API REST V2, nous vous demandons d’ajouter un suffixe spécifique au nom de l’application enregistrée, tel que &quot;RESTV2&quot;.

### 5. L’application cliente peut-elle utiliser les schémas personnalisés existants ? {#migration-faq5}

L’application cliente peut réutiliser les schémas personnalisés existants générés par le biais du [tableau de bord TVE](./rest-api-v2-glossary.md#tve-dashboard) d’Adobe Pass.

Pour plus d’informations, reportez-vous au [Guide de l’utilisateur des canaux de tableau de bord TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#custom-schemes) ou au [Guide de l’utilisateur des programmeurs de tableaux de bord TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#custom-schemes) .

### 6. Les codes d’erreur améliorés sont-ils activés par défaut dans l’API REST V2 ? {#migration-faq6}

Oui.

Les applications clientes intégrant l’API REST V2 bénéficient de la fonctionnalité de codes d’erreur améliorée activée par défaut.

Pour plus d’informations, reportez-vous à la documentation [Codes d’erreur améliorés](/help/authentication/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) .

+++

### Migration de l’API REST V1 vers l’API REST V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Passez à cette sous-section si vous travaillez sur une application qui doit migrer de l’API REST V1 vers l’API REST V2.

+++FAQ sur la phase d’enregistrement

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’enregistrement ? {#registration-phase-v1-to-v2-faq1}

Lors de la migration de l’API REST V1 vers l’API REST V2, aucun changement de niveau élevé n’est apporté concernant la phase d’enregistrement.

L’application cliente peut continuer à utiliser le même flux pour s’enregistrer auprès de l’authentification Adobe Pass par le biais du processus [Enregistrement dynamique du client (DCR)](./rest-api-v2-glossary.md#dcr) et obtenir un jeton d’accès.

Pour plus d&#39;informations, consultez les documents suivants :

* [Présentation de l’enregistrement du client dynamique](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [Récupération de l’API des informations d’identification client](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Récupération de l’API de jeton d’accès](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flux d’enregistrement de client dynamique](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

+++

Questions fréquentes sur la phase de configuration +++

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de configuration ? {#configuration-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans le tableau suivant :

| Champ d’application | API REST V1 | API REST V2 | Observations |
|------------------------------------------------|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec intégration active | [ <br/> /api/v1/config/{serviceProvider}](/help/authentication/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

Questions fréquentes sur la phase d’authentification +++

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’authentification ? {#authentication-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans le tableau suivant :

| Champ d’application | API REST V1 | API REST V2 | Observations |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du code d’enregistrement (code d’authentification) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérification du code d’enregistrement (code d’authentification) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [ <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reprendre l’authentification (MVPD) sur le second écran (application) | [ <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [ <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [ <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Lancement de l’authentification (MVPD) | [ <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [ <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérification de l’état d’authentification des utilisateurs | [GET <br/> /api/v1/checkauthn (premier écran)](/help/authentication/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (deuxième écran)](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération du jeton d’authentification de l’utilisateur (profil) | [GET <br/> /api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [ <br/> /api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

Questions fréquentes sur la phase de préautorisation +++

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de préautorisation ? {#preauthorization-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans le tableau suivant :

| Champ d’application | API REST V1 | API REST V2 | Observations |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer les ressources préautorisées (décisions de préautorisation) | [GET <br/> /api/v1/preautoriser (premier écran)](/help/authentication/retrieve-list-of-preauthorized-resources.md) <br/> [ <br/> /api/v1/preautoriser (deuxième écran)](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/requests/preautoriser/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de préautorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++FAQ sur la phase d’autorisation

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’autorisation ? {#authorization-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans le tableau suivant :

| Champ d’application | API REST V1 | API REST V2 | Observations |
|-------------------------------------------------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancer l’autorisation (MVPD) | [GET <br/> /api/v1/authorized](/help/authentication/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/requests/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupération de la décision d’autorisation</li><li>Récupération du jeton multimédia court</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’autorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Récupération du jeton d’autorisation (décision d’autorisation) | [GET <br/> /api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/requests/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupération de la décision d’autorisation</li><li>Récupération du jeton multimédia court</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’autorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Récupération du jeton d’autorisation court (jeton multimédia) | [GET <br/> /api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/requests/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | L&#39;application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupération de la décision d’autorisation</li><li>Récupération du jeton multimédia court</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’autorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

Questions fréquentes sur la phase de déconnexion +++

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de déconnexion ? {#logout-phase-v1-to-v2-faq1}

Dans la migration de l’API REST V1 vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans le tableau suivant :

| Champ d’application | API REST V1 | API REST V2 | Observations |
|-----------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancement de la déconnexion | [ <br/> /api/v1/logout](/help/authentication/initiate-logout.md) | [ <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migration du SDK vers l’API REST V2 {#migrate-sdk-to-rest-api-v2}

Passez à cette sous-section si vous travaillez sur une application qui doit migrer du SDK vers l’API REST V2.

+++FAQ sur la phase d’enregistrement

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’enregistrement ? {#registration-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans les tableaux suivants :

##### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement du client dynamique (DCR) | Fournir une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Présentation de l’enregistrement du client dynamique](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement de client dynamique](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### SDK AccessEnabler iOS/tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement du client dynamique (DCR) | Fournir une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Présentation de l’enregistrement du client dynamique](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement de client dynamique](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### SDK Android AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement du client dynamique (DCR) | Fournir une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Présentation de l’enregistrement du client dynamique](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement de client dynamique](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### SDK AccessEnabler FireOS

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terminer l’enregistrement du client dynamique (DCR) | Fournir une instruction logicielle au constructeur | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Présentation de l’enregistrement du client dynamique](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Flux d’enregistrement de client dynamique](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

Questions fréquentes sur la phase de configuration +++

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de configuration ? {#configuration-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans les tableaux suivants :

##### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------------------|--------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec intégration active | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### SDK AccessEnabler iOS/tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec intégration active | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### SDK Android AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec intégration active | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### SDK AccessEnabler FireOS

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération de la liste des MVPD avec intégration active | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

Questions fréquentes sur la phase d’authentification +++

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’authentification ? {#authentication-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans les tableaux suivants :

##### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancement de l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérification de l’état d’authentification des utilisateurs | [AccessEnabler.checkAuthentication](/help/authentication/javascript-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/javascript-sdk-api-reference.md#getMeta) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### SDK iOS AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancement de l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérification de l’état d’authentification des utilisateurs | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### SDK AccessEnabler tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du code d’enregistrement (code d’authentification) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérification du code d’enregistrement (code d’authentification) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [ <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reprendre l’authentification (MVPD) sur le second écran (application) | [ <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [ <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [ <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Lancement de l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérification de l’état d’authentification des utilisateurs | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### SDK Android AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lancement de l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérification de l’état d’authentification des utilisateurs | [AccessEnabler.checkAuthentication](/help/authentication/android-sdk-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/android-sdk-api-reference.md#getMetadata) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### SDK AccessEnabler FireOS

| Champ d’application | SDK | API REST V2 | Observations |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupération du code d’enregistrement (code d’authentification) | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérification du code d’enregistrement (code d’authentification) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [ <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reprendre l’authentification (MVPD) sur le second écran (application) | [ <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [ <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [ <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Lancement de l’authentification (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [ <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’authentification de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flux d’authentification de base effectué dans une application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Vérification de l’état d’authentification des utilisateurs | [AccessEnabler.checkAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthN) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Récupération des informations de métadonnées utilisateur | [AccessEnabler.getMetadata](/help/authentication/amazon-fireos-native-client-api-reference.md#getMetadata) | Utilisez l’une des méthodes suivantes : <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [ <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | L&#39;application cliente peut utiliser la réponse de ces API à plusieurs fins à la fois : <br/> <ul><li>Vérification de l’état d’authentification des utilisateurs</li><li>Récupération du profil utilisateur</li><li>Récupération des informations de métadonnées utilisateur</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de profils de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flux de profils de base effectué dans l’application secondaire](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

Questions fréquentes sur la phase de préautorisation +++

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de préautorisation ? {#preauthorization-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans les tableaux suivants :

##### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer les ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorized](/help/authentication/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/requests/preautoriser/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de préautorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### SDK AccessEnabler iOS/tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer les ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorized](/help/authentication/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/requests/preautoriser/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de préautorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### SDK Android AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer les ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorized](/help/authentication/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/requests/preautoriser/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de préautorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Champ d’application | SDK | API REST V2 | Observations |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Récupérer les ressources préautorisées (décisions de préautorisation) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/requests/preautoriser/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de préautorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++FAQ sur la phase d’autorisation

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase d’autorisation ? {#authorization-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans les tableaux suivants :

##### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/requests/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | L&#39;application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupération de la décision d’autorisation</li><li>Récupération du jeton multimédia court</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’autorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### SDK AccessEnabler iOS/tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/requests/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | L&#39;application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupération de la décision d’autorisation</li><li>Récupération du jeton multimédia court</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’autorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### SDK Android AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/requests/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | L&#39;application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupération de la décision d’autorisation</li><li>Récupération du jeton multimédia court</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’autorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### SDK AccessEnabler FireOS

| Champ d’application | SDK | API REST V2 | Observations |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Récupération du jeton d’autorisation court (jeton multimédia) | [AccessEnabler.checkAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/requests/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | L&#39;application cliente peut utiliser la réponse de cette API à plusieurs fins à la fois : <br/> <ul><li>Lancer l’autorisation (MVPD)</li><li>Récupération de la décision d’autorisation</li><li>Récupération du jeton multimédia court</li></ul> <br/> Pour plus de détails, reportez-vous aux documents suivants : <br/> <ul><li>[Flux d’autorisation de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

Questions fréquentes sur la phase de déconnexion +++

#### 1. Quelles sont les migrations d’API de haut niveau requises pour la phase de déconnexion ? {#logout-phase-sdk-to-v2-faq1}

Dans la migration des SDK vers l’API REST V2, des modifications de haut niveau sont à prendre en compte et sont présentées dans les tableaux suivants :

##### SDK JavaScript AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|-----------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lancement de la déconnexion | [AccessEnabler.logout](/help/authentication/javascript-sdk-api-reference.md#logout) | [ <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### SDK AccessEnabler iOS/tvOS

| Champ d’application | SDK | API REST V2 | Observations |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lancement de la déconnexion | [AccessEnabler.logout](/help/authentication/iostvos-sdk-api-reference.md#logout) | [ <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### SDK Android AccessEnabler

| Champ d’application | SDK | API REST V2 | Observations |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lancement de la déconnexion | [AccessEnabler.logout](/help/authentication/android-sdk-api-reference.md#logout) | [ <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### SDK AccessEnabler FireOS

| Champ d’application | SDK | API REST V2 | Observations |
|-----------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Lancement de la déconnexion | [AccessEnabler.logout](/help/authentication/amazon-fireos-native-client-api-reference.md#logout) | [ <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Pour plus d&#39;informations, reportez-vous aux documents suivants : <br/> <ul><li>[Flux de déconnexion de base effectué dans l’application principale](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
