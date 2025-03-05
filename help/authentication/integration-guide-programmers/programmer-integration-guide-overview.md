---
title: Guide d’intégration du programmeur
description: Guide d’intégration du programmeur
exl-id: 51461caf-08ef-459e-b284-8f317f45e7b1
source-git-commit: d8097b8419aa36140e6ff550714730059555fd14
workflow-type: tm+mt
source-wordcount: '2073'
ht-degree: 0%

---

# Guide d’intégration du programmeur {#programmer-integration-guide}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Ce guide d’intégration est destiné aux fournisseurs de contenu (programmeurs) qui prévoient de s’intégrer à l’authentification Adobe® Pass.

Dans le paysage numérique d’aujourd’hui, les internautes peuvent accéder à Internet n’importe où et à tout moment, et demander l’accès à votre contenu protégé. Il peut s&#39;agir d&#39;un événement ponctuel ou d&#39;une demande de droits de diffusion d&#39;une série télévisée complète que vous diffusez.

Avant d’accorder l’accès à un contenu protégé, vous devez déterminer si la visionneuse y a droit. Les questions clés sont les suivantes :

* **La visionneuse dispose-t-elle d’un abonnement actif à un distributeur de programmation vidéo multicanal (MVPD) ?**
* **Cet abonnement inclut-il votre programmation ?**

## Authentification Adobe Pass pour TV Everywhere {#adobe-pass-authentication-for-tv-everywhere}

Pour les programmeurs, déterminer les droits n’est pas toujours simple. Les MVPD sont les dépositaires des données d’identification et des privilèges d’accès de leurs clients. Pour compliquer encore les choses, les téléspectateurs de Programmers peuvent s&#39;abonner à une grande variété de MVPD, chacun fonctionnant avec des systèmes uniques. Ces complexités rendent la vérification des droits à la fois techniquement difficile et gourmande en ressources.

![Droit De L’Utilisateur Déterminé Directement Par Le Programmeur](../assets/user-ent-by-progr.png){align="center"}

*Droit De L’Utilisateur Déterminé Directement Par Le Programmeur*

L’authentification Adobe Pass facilite en toute sécurité les transactions de droits entre les programmeurs et les MVPD, ce qui rend rapide, facile et sécurisé la fourniture de contenu protégé aux visiteurs et visiteuses éligibles.

![Droits d’utilisateur arbitrés par l’authentification Adobe Pass](../assets/user-ent-mediatedby-authn.png){align="center"}

*Droits d’utilisateur arbitrés par l’authentification Adobe Pass*

L’authentification Adobe Pass agit comme un proxy et facilite le flux de droits entre les programmeurs et les MVPD en offrant des interfaces sécurisées et cohérentes aux deux parties.

Pour les programmeurs, l’authentification Adobe Pass fournit des API dans le cadre d’un niveau **Standard** ou **Premium** :

* API d’authentification Adobe Pass standard :
   * [DCR DE L’API REST](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* API d’authentification Premium Adobe Pass :
   * [Réinitialiser l’API Temp Pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
   * [API de dégradation](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
   * [API de surveillance du service de droit](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

### Cas d’utilisation {#use-cases}

Cette section décrit plus en détail les cas d’utilisation de l’intégration du programmeur pris en charge par l’authentification Adobe Pass :

* Application programmeur (TVE) avec un réseau à canal unique

  Cela permet au programmeur de fournir aux téléspectateurs l&#39;accès au contenu à partir d&#39;un réseau de chaînes monomarques au sein d&#39;une application TVE.

* Application programmeuse (TVE) avec réseaux à canaux multiples

  Cela permet au programmeur de fournir aux téléspectateurs un accès au contenu à partir de plusieurs réseaux de canaux dans une seule application TVE.

* Application Programmeur (TVE) pour événements spéciaux

  Cela permet au programmeur de fournir aux observateurs l’accès au contenu d’événements spéciaux qui peuvent ne pas être des ressources qui se trouvent dans la base de données de droits de MVPD comme les canaux normaux.

| **Phase** | **Priorité** | **Cas d’utilisation** | **Documents** |
|----------------------|--------------|-------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Authentification** | **Élevé** | Authentification | Pour plus d’informations, reportez-vous aux documents agrégés dans la section [Phase d’authentification](#authentication-phase). |
|                      | **Élevé** | Authentification à domicile (HBA) | Pour plus d’informations, reportez-vous à la section [Authentification basée sur l’accueil](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md). |
|                      | **Élevé** | Authentification unique (SSO) | Pour plus d&#39;informations, reportez-vous aux documents agrégés sous la section [ Authentification unique (SSO)](#sso). |
|                      | **Élevé** | Sélectionner le MVPD | Pour plus d’informations, reportez-vous aux documents agrégés sous la section [Phase de configuration](#configuration-phase). |
|                      | **Medium** | Page de connexion à Brand MVPD | Permet aux MVPD de fournir aux pages de connexion des marques spécifiques au programmeur ou au fournisseur de services, y compris la prise en charge des préférences linguistiques par défaut. |
|                      | **Élevé** | Configurer les valeurs de durée de vie (TTL) par plateforme | Pour plus d’informations, consultez le [Guide de l’utilisateur des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows). |
| **Préautorisation** | **Faible** | Autorisation préalable (autorisation de contrôle en amont) | Pour plus d’informations, reportez-vous aux documents agrégés dans la section [Phase de préautorisation](#preauthorization-phase). |
|                      | **Medium** | Codes d’erreur améliorés | Pour plus d’informations, consultez la section [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md). |
| **Autorisation** | **Élevé** | Autorisation | Pour plus d’informations, reportez-vous aux documents agrégés dans la section [Phase d’autorisation](#authorization-phase). |
|                      | **Élevé** | Autorisation de canal distinct | Permet aux utilisateurs d’accéder au contenu de plusieurs réseaux à travers une seule application TVE. Les programmeurs peuvent effectuer des appels d’autorisation spécifiques au canal pour vérifier les droits. |
|                      | **Faible** | Autorisation au niveau des ressources | Permet aux MVPD de collecter des analyses détaillées pour des ressources de contenu individuelles pendant l’autorisation. |
|                      | **Medium** | Codes d’erreur améliorés | Pour plus d’informations, consultez la section [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md). |
|                      | **Élevé** | Programmer Federated Player - Avec Autorisation Au Niveau De La Page | Pour plus d’informations, reportez-vous à la section [Jetons de média](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Medium** | Programmer Federated Player - Avec Autorisation Interne Du Lecteur | Pour plus d’informations, reportez-vous à la section [Jetons de média](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Élevé** | Lecteur syndiqué : hébergé sur le portail MVPD avec une autorisation au niveau de la page | Pour plus d’informations, reportez-vous à la section [Jetons de média](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Faible** | Contrôle parental - Notation du contenu dans les demandes d’autorisation | Permet au programmeur d’inclure des évaluations de contenu dans le cadre de la demande d’autorisation au MVPD qui sont utiles pour l’autorisation au niveau des ressources. |
|                      | **Faible** | Contrôle parental - Filtrage du contenu en fonction des attributs de l’utilisateur | Permet au programmeur de vérifier la note maximale autorisée pour un utilisateur et de filtrer le contenu disponible en conséquence. |
| **Déconnexion** | **Medium** | Déconnexion | Pour plus d’informations, reportez-vous aux documents agrégés sous la section [Phase de déconnexion](#logout-phase). |

## Flux de droits {#entitlement-flow}

Le flux de droits est une série d’étapes qu’une application de programmation (TVE) doit suivre pour diffuser du contenu protégé. Le flux se compose des phases suivantes :

* [Phase d’enregistrement](#registration-phase)
* [Phase de configuration](#configuration-phase)
* [Phase d’authentification](#authentication-phase)
* [(Facultatif) Phase De Préautorisation](#preauthorization-phase)
* [Phase d’autorisation](#authorization-phase)
* [Phase de déconnexion](#logout-phase)

Lors de la visite initiale d’un utilisateur ou d’une utilisatrice dans une application de programmation (TVE), le flux des droits suit la séquence décrite. Cependant, lors de visites ultérieures, l’application peut contourner certaines étapes en fonction du statut de l’enregistrement ou de l’authentification et des politiques d’affichage applicables.

Pour une exploration détaillée du flux de droits et de ses phases, poursuivez la lecture de ce document, puis reportez-vous aux guides du guide pas à pas pour obtenir plus d’informations :

* [Guide pas à pas API REST V2 (client à serveur)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
* [Guide pas à pas API REST V2 (serveur à serveur)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)

>[!NOTE]
>
> L’application Programmeur (TVE) est utilisée dans ce document pour faire référence collectivement aux types d’applications s’exécutant sur différentes plateformes (navigateurs, appareils mobiles, appareils connectés à la télévision, etc.) prises en charge par l’authentification Adobe Pass.

### Phase d’enregistrement {#registration-phase}

La phase d’enregistrement a pour but d’enregistrer l’application cliente par rapport à l’authentification Adobe Pass par le biais du processus [Enregistrement client dynamique (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

Le processus d’enregistrement client dynamique (DCR) nécessite que l’application cliente obtienne une paire d’informations d’identification client et récupère un jeton d’accès en tant qu’objectif final de la phase d’enregistrement.

**API**

* [Récupérer les informations d’identification du client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Récupérer le jeton d’accès](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Flux**

* [Flux d’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**FAQ**

* [FAQ sur la phase d’enregistrement](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#registration-phase-faqs-general).

### Phase de configuration {#configuration-phase}

La phase de configuration a pour but de fournir à l’application cliente la liste des fichiers MVPD auxquels elle est activement intégrée, ainsi que les détails de configuration enregistrés par l’authentification Adobe Pass pour chaque MVPD.

La phase de configuration fait office d’étape préalable à la phase d’authentification lorsque l’application cliente doit demander à l’utilisateur de sélectionner son fournisseur de télévision.

**API**

* [Récupération de la configuration pour un fournisseur de services spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

**FAQ**

* [FAQ sur la phase de configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#configuration-phase-faqs-general).

>[!TIP]
>
> L&#39;application TVE devrait comprendre une interface de sélection MVPD permettant aux utilisateurs d&#39;identifier et de sélectionner facilement leur fournisseur de télévision.

### Phase d’authentification {#authentication-phase}

La phase d’authentification a pour but de permettre à l’application cliente de vérifier l’identité de l’utilisateur à l’aide du MVPD et d’obtenir des informations sur les métadonnées de l’utilisateur.

La phase d’authentification agit comme une étape préalable à la phase de préautorisation ou à la phase d’autorisation lorsque l’application cliente doit lire du contenu.

Une authentification réussie génère un profil lié à l’application, à l’appareil et au fournisseur de services, contenant également les informations de métadonnées de l’utilisateur.

**Étapes de haut niveau**

Les étapes suivantes décrivent les étapes de haut niveau dans le cas d’une intégration SAML :

1. **Chargement de l’application du programmeur (site web)**\
   L’utilisateur accède à l’application du programmeur (site web), qui intègre l’authentification Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Demande de contenu protégé**\
   Lorsque l’utilisateur tente d’accéder au contenu protégé, l’application du programmeur affiche une liste de fichiers MVPD dans laquelle il peut sélectionner des fichiers.

1. **Initialisation de la demande d’authentification**\
   Lorsque MVPD est sélectionné, l’utilisateur est redirigé vers un serveur d’authentification Adobe Pass. Ici, une demande d’authentification SAML chiffrée pour le MVPD sélectionné est générée, en cas d’intégration SAML. Cette demande est envoyée au MVPD au nom du programmeur. Selon le système de MVPD, le navigateur de l’utilisateur est redirigé vers la page de connexion de MVPD ou un iFrame de connexion est intégré à l’application du programmeur.

1. **Connexion à MVPD**\
   Le MVPD accepte la demande et présente son interface de connexion, soit par redirection, soit par iFrame.

1. **Connexion utilisateur et validation**\
   L’utilisateur se connecte avec ses informations d’identification MVPD. Le MVPD valide le statut d’abonnement de l’utilisateur et établit sa propre session HTTP.

1. **Réponse de MVPD à l&#39;authentification Adobe Pass**\
   Une fois la validation terminée, le MVPD génère une réponse SAML (chiffrée) et la renvoie vers l’authentification Adobe Pass.

1. **Génération de profil**\
   L’authentification Adobe Pass vérifie la réponse SAML, génère un profil utilisateur qui est mis en cache et redirige l’utilisateur vers l’application du programmeur (site web).

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

* [FAQ sur la phase d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

>[!TIP]
>
> L’application TVE doit communiquer clairement le statut d’authentification de l’utilisateur, par exemple en affichant son logo MVPD à côté d’icônes « verrouillées » ou « déverrouillées » pour indiquer l’accessibilité du contenu protégé.

#### Authentification unique (SSO) {#single-sign-on}

**API**

* [Récupérer la demande d’authentification du partenaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [Créer et récupérer un profil à l’aide de la réponse d’authentification du partenaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

**Flux**

* [Authentification unique à l’aide des flux de partenaires](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
* [Authentification unique à l’aide des flux d’identité de plateforme](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
* [Authentification unique à l’aide des flux de jeton de service](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)

### (Facultatif) Phase De Préautorisation {#preauthorization-phase}

L’objectif de la phase de préautorisation est de permettre à l’application cliente de présenter un sous-ensemble de ressources de son catalogue auquel l’utilisateur ou l’utilisatrice serait autorisé à accéder.

La phase de préautorisation peut améliorer l’expérience de l’utilisateur lorsqu’il ouvre l’application cliente pour la première fois ou accède à une nouvelle section.

**API**

* [Récupération des décisions de préautorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Flux**

* [Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**FAQ**

* [FAQ sur la phase de préautorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general).

>[!TIP]
>
> L’application TVE doit clairement différencier le contenu restreint du contenu autorisé à l’aide d’indicateurs visuels, tels qu’une icône « verrouillée » pour le contenu restreint et une icône « déverrouillée » pour le contenu autorisé.

### Phase d’autorisation {#authorization-phase}

La phase d’autorisation a pour but de permettre à l’application cliente de lire les ressources demandées par l’utilisateur ou l’utilisatrice après validation de ses droits avec MVPD.

Une autorisation réussie génère une décision, contenant également un jeton multimédia fourni à l’application du programmeur (TVE) à des fins de sécurité.

**Étapes de haut niveau**

Les étapes suivantes décrivent les étapes de haut niveau :

1. **Gestion des identifiants de ressource**\
   Le contenu protégé est identifié par un [identifiant de ressource](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier), qui peut être une simple chaîne ou une structure plus complexe. Cet identifiant est prédéfini et accepté par le programmeur et le MVPD. L’application du programmeur envoie l’identifiant de la ressource à l’API REST [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) d’Adobe Pass Authentication.

1. **Vérification De L’Autorisation MVPD**\
   Le serveur d’authentification Adobe Pass communique avec le point d’entrée d’autorisation MVPD à l’aide de protocoles normalisés.

1. **Réponse de MVPD à l&#39;authentification Adobe Pass**\
   Une fois la validation terminée, le MVPD confirme que l’utilisateur a le droit (ou non) d’accéder au contenu et renvoie une réponse à Authentification Adobe Pass.

1. **Génération de jetons de décision et de média**\
   L’authentification Adobe Pass vérifie la réponse, génère une [décision](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) qui est mise en cache et renvoie la décision contenant un jeton de média à l’application du programmeur (site web).

1. **Vérification de l’accès au contenu**\
   L’application du programmeur utilise le [vérificateur de jeton multimédia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) pour confirmer que l’utilisateur ou l’utilisatrice approprié(e) accède au contenu correct. Une fois la validation effectuée, l’utilisateur se voit accorder l’accès à l’affichage du contenu protégé.

**API**

* [Récupérer les décisions d’autorisation](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Flux**

* [Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**FAQ**

* [ FAQ sur la phase d’autorisation ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general).

>[!TIP]
>
> L’application TVE doit clairement différencier le contenu restreint du contenu autorisé à l’aide d’indicateurs visuels, tels qu’une icône « verrouillée » pour le contenu restreint et une icône « déverrouillée » pour le contenu autorisé.

### Phase de déconnexion {#logout-phase}

L’objectif de la phase de déconnexion est de permettre à l’application cliente de mettre fin au profil authentifié de l’utilisateur dans l’authentification Adobe Pass à la demande de l’utilisateur.

**API**

* [Lancer la déconnexion pour un fichier mvpd spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Flux**

* [Flux de déconnexion de base effectué dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**FAQ**

* [FAQ sur la phase de déconnexion](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general).

#### Déconnexion unique (SLO) {#single-logout}

**Flux**

* [Flux de déconnexion unique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)

## Comprendre les droits {#understanding-entitlements}

La solution Authentification Adobe Pass s’articule autour de la création de droits, c’est-à-dire d’éléments de données spécifiques générés lors de la réussite des workflows d’authentification et d’autorisation. Ces droits donnent accès à du contenu protégé, mais leur durée de vie est limitée. Une fois qu’un droit expire, il doit être renouvelé en réinitialisant les processus d’authentification ou d’autorisation.

Pour plus d’informations sur les droits, reportez-vous aux documents suivants :

* **Profils**

  Une fois l’authentification réussie, l’authentification Adobe Pass crée un profil authentifié (« de longue durée ») associé à l’application, à l’appareil et à l’identifiant du fournisseur de services demandeur (identifiant du demandeur).

* **[Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Une fois l’authentification réussie (et dans certains cas après autorisation également), l’authentification Adobe Pass reçoit des métadonnées de l’utilisateur de la part de MVPD qui peuvent l’exposer à l’application demandeuse.

* **[Décisions](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Une fois l’autorisation réussie, l’authentification Adobe Pass crée une décision d’autorisation (« de longue durée ») associée à l’application, à l’appareil, à l’identifiant du fournisseur de services (identifiant du demandeur) et à une ressource protégée spécifique (identifiant de ressource).

* **[Jetons multimédia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Une fois l’autorisation réussie, l’authentification Adobe Pass crée un jeton multimédia (« de courte durée ») associé à une requête de lecture réussie.
