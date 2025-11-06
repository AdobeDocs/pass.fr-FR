---
title: Glossaire de l’API REST V2
description: Glossaire de l’API REST V2
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1742'
ht-degree: 0%

---

# Glossaire de l’API REST V2 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Ce document fournit des définitions des termes utilisés lors de l’intégration de l’API REST d’authentification Adobe Pass V2.

>[!MORELIKETHIS]
>
> * [Glossaire de Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## Glossaire {#glossary-terms}

### A {#a}

#### Authentification {#authentication}

L’authentification est un processus qui permet à un utilisateur de prouver son identité à un [programmeur](#programmer), d’accéder au contenu protégé ([ressource](#resource)), après validation de l’abonnement de l’utilisateur avec le [MVPD](#mvpd).

#### Code d’authentification {#code}

Le code d’authentification est un concept d’authentification Adobe Pass qui stocke une valeur unique générée lorsqu’un utilisateur ou une utilisatrice lance le processus d’authentification [authentication](#authentication) et identifie de manière unique la [ session d’authentification d’un utilisateur ou d’une utilisatrice](#session) jusqu’à ce que le processus d’authentification soit terminé.

Le code d’authentification peut être utilisé à la fois par une application (programmeur) de Principal [](#primary-application) ou une application (programmeur) Secondaire [](#secondary-application) pour terminer le processus [authentification](#authentication), récupérer des informations sur la [session d’authentification](#session) ou accéder au [profil](#profile) de l’utilisateur.

Synonyme de l&#39;ancien terme utilisé code d&#39;enregistrement.

#### Session d&#39;authentification {#session}

La session d’authentification est un concept d’authentification Adobe Pass qui stocke des informations sur le processus d’authentification de l’utilisateur démarré (ou poursuivi) à partir d’une application [Programmeur](#programmer) et qui est identifié de manière unique par un [code d’authentification](#code).

La session d’authentification peut également indiquer l’application [Programmeur](#programmer) pour poursuivre le processus [autorisation](#authorization) comme phase suivante du flux [droits](#entitlement) si l’utilisateur est déjà authentifié.

#### Autorisation {#authorization}

L’autorisation est un processus qui permet à un utilisateur d’accéder au contenu protégé ([ressource](#resource)) d’un catalogue [Programmeur](#programmer) basé sur l’abonnement [MVPD](#mvpd) détenu, après validation des droits de l’utilisateur avec le [MVPD](#mvpd).

### C {#c}

#### Configuration {#configuration}

La configuration est un concept d’authentification Adobe Pass qui stocke des informations sur les paramètres d’intégration [Programmeur](#programmer) et [MVPD](#mvpd) et qui peut être utilisé pendant le processus [authentification](#authentication) lorsque l’utilisateur demande à sélectionner son [fournisseur de télévision](#tv-provider) dans une liste d’intégrations actives.

### D {#d}

#### Décision {#decision}

La décision est un concept d’authentification Adobe Pass qui stocke des informations sur la demande de processus [MVPD](#mvpd) [autorisation](#authorization) ou [préautorisation](#preauthorization) pour autoriser ou refuser l’accès d’un utilisateur à un contenu protégé [Programmeur](#programmer).

#### Dégradation {#degradation}

La dégradation est une fonctionnalité d’authentification d’Adobe Pass qui permet à un utilisateur d’accéder au contenu protégé même lorsque son [MVPD](#mvpd) subit une interruption de service.

Pour plus d’informations, consultez la documentation [Fonctionnalité de dégradation](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md).

#### Identifiant de l’appareil {#device-id}

L’ID d’appareil est un identifiant unique lié à l’appareil de l’utilisateur et doit être fourni par l’application [Programmeur](#programmer) sur toutes les phases du flux [droits](#entitlement).

### E {#e}

#### Droit {#entitlement}

Le droit est un concept d’authentification Adobe Pass qui incorpore les flux et fonctionnalités disponibles pour aider un utilisateur à passer par différentes phases, pour accéder au contenu protégé, allant de [authentification](#authentication), [préautorisation](#preauthorization), [autorisation](#authorization) et enfin [déconnexion](#logout).

#### Code d’erreur amélioré {#enhanced-error-code}

Le code d’erreur amélioré est un concept d’authentification Adobe Pass qui fournit des informations supplémentaires sur l’erreur qui s’est produite lors du traitement d’une requête.

Pour plus d’informations, consultez la documentation [Codes d’erreur améliorés](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

### H {#h}

#### HBA {#hba}

L’authentification à domicile (HBA) est le processus par lequel un client se voit automatiquement accorder l’accès au contenu [TV Everywhere (TVE)](#tve) sur certains appareils connectés à son réseau domestique, qui fait partie de l’emplacement du contrat d’abonnement.

### I {#i}

#### Fournisseur d’identité {#identity-provider}

Le fournisseur d’identité est une société qui fournit des services d’identité aux consommateurs par le biais de services par câble, par satellite ou par Internet dans le contexte de [TV Everywhere (TVE)](#tve).

Synonyme de [MVPD](#mvpd) et [Fournisseur TV](#tv-provider).

### L {#l}

#### Déconnexion {#logout}

La déconnexion est un processus qui permet à un utilisateur de mettre fin à son [profil](#profile) authentifié dans l’authentification Adobe Pass et de mettre à jour l’application [Programmeur](#programmer) pour refléter le statut de l’utilisateur.

### M {#m}

#### Jeton multimédia {#media-token}

Le jeton multimédia est un jeton généré par l’authentification Adobe Pass suite à une autorisation [décision](#decision) destinée à fournir l’accès au contenu protégé.

Le jeton de média est transmis au [programmeur](#programmer), qui le valide ensuite pour garantir la sécurité de l’accès pour cette [ressource](#resource).

Synonyme d’ancien terme utilisé jeton d’autorisation court.

#### Vérificateur de jeton de média {#media-token-verifier}

Le vérificateur de jeton multimédia est une bibliothèque distribuée par l’authentification Adobe Pass qui est chargée de vérifier l’authenticité d’un [jeton multimédia](#media-token).

Pour plus d’informations, consultez la documentation du [Vérificateur de jeton multimédia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

#### MVPD {#mvpd}

Le distributeur de programmation vidéo multicanal (MVPD) est une société qui fournit des services de télévision aux consommateurs par câble, par satellite ou par Internet.

Le MVPD est identifié par une valeur unique définie lors du processus d’intégration entre MVPD et Adobe.

Synonyme de [Fournisseur TV](#tv-provider) et [Fournisseur d’identité](#identity-provider).

### P {#p}

#### Partenaire {#partner}

Le partenaire est une société qui fournit un service ou une structure à un [programmeur](#programmer) pour permettre une expérience utilisateur d’authentification unique.

Le partenaire est identifié par une valeur unique (par exemple, « apple ») définie lors du processus d’intégration entre le partenaire et Adobe.

#### Préautorisation {#preauthorization}

La préautorisation est un processus qui permet à un utilisateur de prévisualiser un sous-ensemble de [ressources](#resource) à partir d’un catalogue [Programmeur](#programmer) auquel il aurait accès, après validation des droits de l’utilisateur avec le [MVPD](#mvpd).

Synonyme de [contrôle en amont](#preflight).

#### Contrôle en amont {#preflight}

Le contrôle en amont est un processus qui permet à un utilisateur de prévisualiser un sous-ensemble de [ressources](#resource) à partir d’un catalogue [Programmeur](#programmer) auquel il serait autorisé à accéder, après validation des droits de l’utilisateur avec le [MVPD](#mvpd).

Synonyme de [préautorisation](#preauthorization).

#### Application Principal (programmeur) {#primary-application}

L’application principale fait référence à une application [Programmeur](#programmer) qui lance l’[authentification](#authentication), mais qui peut ne pas être en mesure d’exécuter le processus à l’aide d’un [agent utilisateur](#user-agent) pour accéder à la page de connexion de [MVPD](#mvpd).

#### Profil {#profile}

Le profil est un concept d’authentification Adobe Pass qui stocke des informations sur la date de début et de fin de l’authentification de l’utilisateur, les [métadonnées de l’utilisateur](#user-metadata) ainsi que d’autres champs indiquant la méthode d’obtention de l’authentification (par exemple, « normale », « dégradée », « temporaire », « authentification unique », etc.).

Synonyme de l’ancien terme utilisé jeton d’authentification.

#### Programmeur {#programmer}

Le programmeur est une entreprise qui fournit du contenu aux consommateurs par le biais de canaux détenus (marques) sur différentes plateformes.

Le programmeur regroupe plusieurs canaux détenus (marques) en tant que [fournisseurs de services](#service-provider) dans son intégration à l’authentification Adobe Pass.

#### MVPD du proxy {#proxy-mvpd}

Le proxy MVPD est une société qui fournit des services d’identités pour d’autres MVPD et qui s’intègre directement à l’authentification Adobe Pass.

#### MVPD proxy {#proxied-mvpd}

Le MVPD proxy est une société qui n’a pas d’intégration directe avec l’authentification Adobe Pass, mais qui est intégrée par le biais d’un [proxy MVPD](#proxy-mvpd).

#### Identité de la plateforme {#platform-identity}

L’identité de la plateforme est une payload d’identifiant de plateforme unique générée par un service ou un framework (bibliothèque) lié à l’appareil de l’utilisateur et fournie au [programmeur](#programmer) pour activer une expérience utilisateur d’authentification unique.

Pour plus d&#39;informations, consultez la documentation [Authentification unique à l&#39;aide des flux d&#39;identité de la plateforme](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

### R {#r}

#### Ressource {#resource}

La ressource est un contenu protégé auquel un utilisateur ou une utilisatrice tente d’accéder à partir d’un catalogue [Programmeur](#programmer).

La ressource est identifiée par une valeur unique convenue entre le programmeur et les MVPD.

Pour plus d’informations, consultez la documentation [Ressources protégées](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

### S {#s}

#### SAML {#saml}

Le langage SAML (Security Assertion Markup Language) est une norme ouverte pour l’échange de données d’authentification et d’autorisation entre les parties, en particulier entre un [fournisseur d’identité](#identity-provider) et un [fournisseur de services](#sp).

#### Application Secondaire (programmeur) {#secondary-application}

L’application secondaire fait référence à une application [Programmeur](#programmer) capable d’effectuer le processus [authentification](#authentication) à l’aide d’un [agent utilisateur](#user-agent) pour accéder à la page de connexion de [MVPD](#mvpd).

L’application secondaire peut s’exécuter sur le même appareil que l’application principale ou sur un autre appareil (secondaire), auquel cas l’expérience de connexion est appelée expérience utilisateur d’« authentification sur deuxième écran ».

#### Jeton de service {#service-token}

Le jeton de service est un identifiant utilisateur unique généré par un service ou une structure (bibliothèque) lié à l’utilisateur et fourni au [programmeur](#programmer) pour activer une expérience utilisateur d’authentification unique.

Pour plus d’informations, reportez-vous à la documentation [ Authentification unique à l’aide des flux de jeton de service ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

#### Prestataire {#service-provider}

Le fournisseur est un canal (marque) détenu par un [programmeur](#programmer).

Le fournisseur de services est identifié par une valeur unique définie lors du processus d’intégration entre le programmeur et Adobe.

Synonyme de l’ancien terme utilisé pour l’ID du demandeur.

#### SLO {#slo}

La déconnexion unique (SLO) est un processus qui permet à un utilisateur de se déconnecter de toutes les applications qui faisaient partie de l’authentification unique [SSO)](#sso).

#### SP {#sp}

Le fournisseur de services (SP) fait référence au rôle joué par l’authentification Adobe Pass pour le compte d’un [programmeur](#programmer) dans une intégration à un [MVPD](#mvpd).

#### SSO {#sso}

L’authentification unique (SSO) est un processus qui permet à un utilisateur de s’authentifier une seule fois et d’accéder à plusieurs applications [Programmeur](#programmer) sans avoir à s’authentifier pour chacune d’elles.

### T {#t}

#### TempPass de base {#temp-pass-basic}

Le TempPass de base est une fonction d’authentification d’Adobe Pass qui permet à un utilisateur d’accéder à un contenu protégé pendant une durée limitée sans avoir à s’authentifier avec un [MVPD](#mvpd).

Pour plus d’informations, reportez-vous à la documentation [Basic TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#basic-temp-pass).

#### Promotionnel TempPass {#temp-pass-promotional}

Le TempPass promotionnel est une fonction d’authentification d’Adobe Pass qui permet à un utilisateur d’accéder au contenu protégé pendant un nombre maximal de ressources et une durée limitée sans avoir à s’authentifier avec un [MVPD](#mvpd).

Pour plus d’informations, consultez la documentation [Promotional TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass).

#### TTL {#ttl}

La durée de vie (TTL) est une valeur qui indique la durée de validité d’une entité sous-jacente.

La TTL peut être mentionnée pour un [jeton d’accès](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token), un [profil](#profile), une autorisation [décision](#decision) ou un [jeton de média](#media-token).

#### TVE {#tve}

TV Everywhere (TVE) est un créneau du secteur qui permet aux consommateurs d’accéder à leurs émissions de télévision, films et autres contenus préférés sur plusieurs appareils, tels que les smartphones, les tablettes, les ordinateurs portables et bien d’autres.

#### Tableau de bord TVE {#tve-dashboard}

Le tableau de bord TV Everywhere (TVE) est un outil d’authentification Adobe Pass fourni aux [programmeurs](#programmer) pour gérer leur configuration et leurs données.

Pour plus d’informations, consultez la documentation du [Guide d’utilisation du tableau de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md).

#### Fournisseur TV {#tv-provider}

Le fournisseur de services de télévision est une entreprise qui fournit des services de télévision aux consommateurs par câble, par satellite ou par Internet.

Le fournisseur de télévision est identifié par une valeur unique définie lors du processus d’intégration entre le fournisseur de télévision et Adobe.

Synonyme de [MVPD](#mvpd) et [Fournisseur d&#39;identité](#identity-provider).

### U {#u}

#### Agent utilisateur {#user-agent}

L&#39;agent utilisateur fait référence à un navigateur ou à un composant similaire (spécifique à la plateforme) capable de naviguer sur le Web et de générer la page de connexion de [MVPD](#mvpd).

#### Identifiant utilisateur {#user-id}

L&#39;identifiant utilisateur est un identifiant unique lié à l&#39;utilisateur et provient du processus d&#39;authentification [MVPD](#mvpd).

#### Métadonnées utilisateur {#user-metadata}

Les métadonnées utilisateur font référence à des attributs spécifiques à l&#39;utilisateur (par exemple, codes postaux, évaluations parentales, ID utilisateur, etc.) gérés par le [MVPD](#mvpd) et fournis par l&#39;authentification Adobe Pass dans le cadre d&#39;un [profil](#profile).

Pour plus d’informations, consultez la documentation [Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

### V {#v}

#### VSA {#vsa}

Le compte d’abonné vidéo (VSA) est un framework développé par Apple et fourni au [programmeur](#programmer) pour permettre une expérience utilisateur d’authentification unique.

Pour plus d’informations, reportez-vous à la documentation [Structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) et [Authentification unique à l’aide de flux de partenaires](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).
