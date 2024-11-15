---
title: Glossaire de l’API REST V2
description: Glossaire de l’API REST V2
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '1964'
ht-degree: 0%

---

# Glossaire de l’API REST V2 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Ce document fournit des définitions des termes utilisés lors de l’intégration de la documentation de l’API REST Adobe Pass Authentication V2 et sert de remplacement à notre [Glossaire](/help/authentication/glossary.md) hérité.

## Termes du glossaire {#glossary-terms}

### A {#a}

#### Jeton d’accès {#access-token}

Le jeton d’accès est un jeton généré par l’authentification Adobe Pass à la suite du processus [Enregistrement dynamique du client (DCR)](#dcr) destiné à assurer l’accès aux API protégées.

#### Authentification {#authentication}

L’authentification est un processus qui permet à un utilisateur de prouver son identité à un [programmeur](#programmer), afin d’accéder au contenu protégé ([ressource](#resource)), après avoir validé l’abonnement de l’utilisateur avec le [MVPD](#mvpd).

#### Code d’authentification {#code}

Le code d’authentification est un concept d’authentification Adobe Pass qui stocke une valeur unique générée lorsqu’un utilisateur lance le processus [authentication](#authentication) et identifie de manière unique la [session d’authentification](#session) d’un utilisateur jusqu’à la fin du processus d’authentification.

Le code d&#39;authentification peut être utilisé par une [application par Principal (programmeur)](#primary-application) ou une [application Secondaire (programmeur)](#secondary-application) pour terminer le processus [authentication](#authentication), récupérer des informations sur la [session d&#39;authentification](#session) ou accéder à l&#39;utilisateur [profile](#profile).

Synonyme de l’ancien terme utilisé comme code d’enregistrement.

#### Session d’authentification {#session}

La session d’authentification est un concept d’authentification Adobe Pass qui stocke des informations sur le processus d’authentification de l’utilisateur qui a démarré (ou s’est poursuivi) à partir d’une application [Programmer](#programmer) et qui est identifié de manière unique par un [code d’authentification](#code).

La session d&#39;authentification peut également indiquer l&#39;application [Programmer](#programmer) pour poursuivre le processus [authorization](#authorization) comme phase suivante du flux [titlement](#entitlement) si l&#39;utilisateur est déjà authentifié.

#### Autorisation {#authorization}

L’autorisation est un processus qui permet à un utilisateur d’accéder au contenu protégé ([resource](#resource)) à partir d’un catalogue [Programmer](#programmer) basé sur l’abonnement [MVPD](#mvpd) détenu, après avoir validé les droits de l’utilisateur avec le [MVPD](#mvpd).

### C {#c}

#### Informations d’identification client {#client-credentials}

Les informations d’identification du client sont un ensemble de valeurs uniques qui sont générées pendant le processus [Enregistrement dynamique du client (DCR)](#dcr) et qui sont destinées à être utilisées pour obtenir un [jeton d’accès](#access-token).

#### Configuration {#configuration}

La configuration est un concept d’authentification Adobe Pass qui stocke des informations sur les paramètres d’intégration [Programmer](#programmer) et [MVPD](#mvpd) et peut être utilisé pendant le processus [authentification](#authentication) lorsque l’utilisateur demande de sélectionner son [fournisseur de télévision](#tv-provider) dans une liste d’intégrations actives.

#### Schéma personnalisé {#custom-scheme}

Le schéma personnalisé est une valeur unique qui fait référence à une application [Programmer](#programmer) qui peut être générée et téléchargée à partir du [tableau de bord TVE](#tve-dashboard) d’Adobe Pass et qui est destinée à être utilisée comme redirection finale dans les applications s’exécutant sur des périphériques iOS.

### D {#d}

#### DCR {#dcr}

L’enregistrement du client dynamique (DCR) est un mécanisme d’autorisation défini par le [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591), et il est basé sur le framework d’autorisation OAuth 2.0 décrit par le [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

Le DCR est fourni à un [programmeur](#programmer) en tant que service d’authentification Adobe Pass qui peut permettre un accès supplémentaire aux API protégées.

Pour plus d’informations, consultez la documentation [Présentation de l’enregistrement du client dynamique](/help/authentication/dcr-api/dynamic-client-registration-overview.md) .

#### Décision {#decision}

La décision est un concept d’authentification Adobe Pass qui stocke des informations sur la demande de processus [MVPD](#mvpd) [authorization](#authorization) ou [preauthorization](#preauthorization) pour autoriser ou refuser l’accès de l’utilisateur à un contenu protégé [Programmer](#programmer).

#### Dégradation {#degradation}

La dégradation est une fonctionnalité d’authentification Adobe Pass qui permet à un utilisateur d’accéder au contenu protégé même lorsque son [MVPD](#mvpd) subit une interruption de service.

Pour plus d’informations, consultez la documentation [Présentation de l’API de dégradation](/help/authentication/degradation-api-overview.md).

### E {#e}

#### Droit {#entitlement}

Le droit est un concept d’authentification Adobe Pass qui intègre les flux et fonctionnalités disponibles qui aident un utilisateur à passer par différentes phases, afin d’accéder à un contenu protégé, allant de [authentication](#authentication), [preauthorization](#preauthorization), [authorization](#authorization), et enfin [logout](#logout).

#### Code d’erreur amélioré {#enhanced-error-code}

Le code d’erreur amélioré est un concept d’ authentification Adobe Pass qui fournit des informations supplémentaires sur l’erreur qui s’est produite lors du traitement d’une requête.

Pour plus d’informations, reportez-vous à la documentation [Codes d’erreur améliorés](/help/authentication/enhanced-error-codes.md) .

### H {#h}

#### Adaptateur {#hba}

L’authentification par domicile (adaptateur de bus hôte) est le processus par lequel un consommateur se voit automatiquement accorder l’accès au contenu [TV Everywhere (TVE)](#tve) sur certains appareils connectés à son réseau domestique, qui fait partie de l’emplacement dans le contrat d’abonnement.

### I {#i}

#### Fournisseur d’identité {#identity-provider}

Le fournisseur d’identité est une entreprise qui fournit des services d’identité aux consommateurs par câble, satellite ou sur Internet dans le contexte de [TV Everywhere (TVE)](#tve).

Synonyme de [MVPD](#mvpd) et [fournisseur de télévision](#tv-provider).

### L {#l}

#### Déconnexion {#logout}

La déconnexion est un processus qui permet à un utilisateur de mettre fin à son [profil](#profile) authentifié dans l’authentification Adobe Pass et de mettre à jour l’application [Programmer](#programmer) pour refléter l’état de l’utilisateur.

### M {#m}

#### Jeton de média {#media-token}

Le jeton multimédia est un jeton généré par l’authentification Adobe Pass suite à une autorisation [décision](#decision) destinée à fournir l’accès au contenu protégé.

Le jeton multimédia est transmis à [Programmer](#programmer), qui le valide ensuite pour garantir la sécurité de l&#39;accès pour cette [ressource](#resource).

Synonyme avec ancien terme utilisé comme jeton d’autorisation court.

#### Vérificateur de jeton multimédia {#media-token-verifier}

Le outil de vérification de jeton multimédia est une bibliothèque distribuée par l’authentification Adobe Pass qui est chargée de vérifier l’authenticité d’un [jeton multimédia](#media-token).

Pour plus d’informations, consultez la documentation [Intégration du vérificateur de jeton multimédia](/help/authentication/media-token-verifier-int.md) .

#### MVPD {#mvpd}

Le distributeur multicanal de programmes vidéo (MVPD) est une société qui fournit des services de télévision aux consommateurs par câble, satellite ou Internet.

Le MVPD est identifié par une valeur unique définie lors du processus d’intégration entre le MVPD et l’Adobe.

Synonyme de [fournisseur de télévision](#tv-provider) et [fournisseur d’identité](#identity-provider).

### P {#p}

#### Partenaire {#partner}

Le partenaire est une entreprise qui fournit un service ou une structure à un [programmeur](#programmer) afin d’activer une expérience utilisateur de connexion unique.

Le partenaire est identifié par une valeur unique (par exemple, &quot;pomme&quot;) définie lors du processus d’intégration entre le partenaire et l’Adobe.

#### Préautorisation {#preauthorization}

La préautorisation est un processus qui permet à un utilisateur de prévisualiser un sous-ensemble de [ressources](#resource) à partir d’un catalogue de [programmeur](#programmer) auquel il aurait droit, après avoir validé les droits de l’utilisateur avec le [MVPD](#mvpd).

Synonyme de [Preflight](#preflight).

#### Contrôle en amont {#preflight}

Le contrôle en amont est un processus qui permet à un utilisateur de prévisualiser un sous-ensemble de [ressources](#resource) à partir d’un catalogue de [programmeur](#programmer) auquel il aurait droit, après avoir validé les droits de l’utilisateur avec le [MVPD](#mvpd).

Synonyme de [Preauthorization](#preauthorization).

#### Application de Principal (programmeur) {#primary-application}

L’application principale fait référence à une application [Programmer](#programmer) qui initie l’ [authentification](#authentication), mais qui peut ne pas être en mesure d’achever le processus en utilisant un [agent utilisateur](#user-agent) pour accéder à la page de connexion [MVPD](#mvpd).

#### Profil {#profile}

Le profil est un concept d’authentification Adobe Pass qui stocke des informations sur la date de début et de fin de l’authentification de l’utilisateur, les [métadonnées de l’utilisateur](#user-metadata) ainsi que d’autres champs qui indiquent la méthode d’obtention de l’authentification (par exemple, &quot;normal&quot;, &quot;dégradé&quot;, &quot;temporaire&quot;, &quot;authentification unique&quot;, etc.).

Synonyme avec l’ancien terme utilisé comme jeton d’authentification.

#### Programmeur {#programmer}

Le programmeur est une entreprise qui fournit du contenu aux consommateurs par le biais de canaux (marques) détenus sur différentes plateformes.

Le programmeur regroupe plusieurs canaux détenus (marques) en [fournisseurs de services](#service-provider) dans son intégration avec l’authentification Adobe Pass.

#### Proxy MVPD {#proxy-mvpd}

Le proxy MVPD est une entreprise qui fournit des services d’identité pour d’autres MVPD et qui s’intègre directement à l’authentification Adobe Pass.

#### MVPD proxy {#proxied-mvpd}

Le MVPD proxy est une entreprise qui n’a pas d’intégration directe avec l’authentification Adobe Pass, mais qui est intégrée par le biais d’un [proxy MVPD](#proxy-mvpd).

#### Identité de la plateforme {#platform-identity}

L’identité de plateforme est une payload d’identifiant de plateforme unique générée par un service ou une structure (bibliothèque) lié à l’appareil de l’utilisateur et fournie au [programmeur](#programmer) afin d’activer une expérience utilisateur de connexion unique.

Pour plus d&#39;informations, consultez la documentation [Connexion unique à l&#39;aide des flux d&#39;identités de la plateforme](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) .

### R {#r}

#### Application enregistrée {#registered-application}

L’application enregistrée est un concept d’authentification Adobe Pass qui stocke des informations sur l’application [Programmer](#programmer) qui nécessite de poursuivre le processus [Enregistrement dynamique du client (DCR)](#dcr).

#### Ressource {#resource}

La ressource est un contenu protégé auquel un utilisateur tente d’accéder à partir d’un catalogue [Programmer](#programmer).

La ressource est identifiée par une valeur unique qui est convenue entre le programmeur et les MVPD.

Pour plus d’informations, consultez la documentation [Identification des ressources protégées](/help/authentication/identify-protected-resources.md).

### S {#s}

#### SAML {#saml}

Le langage SAML (Security Assertion Markup Language) est une norme ouverte permettant l’échange de données d’authentification et d’autorisation entre les parties, notamment entre un [fournisseur d’identité](#identity-provider) et un [fournisseur de services](#sp).

#### Application Secondaire (programmeur) {#secondary-application}

L’application secondaire fait référence à une application [Programmer](#programmer) capable d’effectuer le processus [authentification](#authentication) en utilisant un [agent utilisateur](#user-agent) pour accéder à la page de connexion [MVPD](#mvpd).

L’application secondaire peut s’exécuter sur le même appareil que l’application principale ou sur un autre appareil (secondaire), auquel cas l’expérience de connexion est appelée expérience utilisateur &quot;authentification par deuxième écran&quot;.

#### Jeton de service {#service-token}

Le jeton de service est un identifiant utilisateur unique généré par un service ou une structure (bibliothèque) lié à l’utilisateur et fourni au [programmeur](#programmer) afin d’activer une expérience utilisateur de connexion unique.

Pour plus d’informations, reportez-vous à la documentation [Authentification unique à l’aide des flux de jetons de service](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) .

#### Fournisseur de services {#service-provider}

Le fournisseur de services est un canal (marque) détenu par un [programmeur](#programmer).

Le fournisseur de services est identifié par une valeur unique définie lors du processus d&#39;intégration entre le programmeur et l&#39;Adobe.

Synonyme avec l&#39;ancien terme utilisé [id du demandeur](/help/authentication/glossary.md#requestor-id).

#### Instruction logicielle {#software-statement}

L’instruction logicielle est un jeton Web JSON (JWT) qui peut être téléchargé à partir du [tableau de bord TVE](#tve-dashboard) d’Adobe Pass et qui est destiné à être utilisé dans le cadre du processus [Enregistrement du client dynamique (DCR)](#dcr).

#### SLO {#slo}

La déconnexion unique (SLO) est un processus qui permet à un utilisateur de se déconnecter de toutes les applications qui faisaient partie de l’ [authentification unique (SSO)](#sso).

#### SP {#sp}

Le fournisseur de services (SP) fait référence au rôle joué par l’authentification Adobe Pass au nom d’un [programmeur](#programmer) dans une intégration à un [MVPD](#mvpd).

#### SSO {#sso}

L’authentification unique (SSO) est un processus qui permet à un utilisateur de s’authentifier une seule fois et d’accéder à plusieurs applications [Programmer](#programmer) sans avoir à s’authentifier pour chacune d’elles.

### T {#t}

#### TempPass Basic {#temp-pass-basic}

TempPass est une fonction d’authentification Adobe Pass qui permet à un utilisateur d’accéder à un contenu protégé pendant une durée limitée sans avoir à s’authentifier avec un [MVPD](#mvpd).

Pour plus d’informations, consultez la documentation [Temp Pass](/help/authentication/temp-pass.md).

#### Promotionnel TempPass {#temp-pass-promotional}

TempPass est une fonctionnalité d’authentification Adobe Pass qui permet à un utilisateur d’accéder à du contenu protégé pour un nombre maximal de ressources et une durée limitée sans avoir à s’authentifier avec un [MVPD](#mvpd).

Pour plus d’informations, reportez-vous à la documentation [Passe temporaire promotionnelle](/help/authentication/promotional-temp-pass.md) .

#### TTL {#ttl}

La durée de vie (TTL) est une valeur qui indique la durée de validité d’une entité sous-jacente.

Le TTL peut être mentionné pour un [jeton d’accès](#access-token), un [profil](#profile), une autorisation [décision](#decision) ou un [jeton multimédia](#media-token).

#### TVE {#tve}

TV Everywhere (TVE) est un créneau du secteur qui permet aux consommateurs d’accéder à leurs programmes télévisés, films et autres contenus préférés sur plusieurs appareils, tels que smartphones, tablettes, ordinateurs portables, etc.

#### Tableau de bord TVE {#tve-dashboard}

Le tableau de bord TV Everywhere (TVE) est un outil d’authentification Adobe Pass fourni à [Programmers](#programmer) pour gérer leur configuration et leurs données.

Pour plus d’informations, consultez la documentation [TVE Dashboard User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md) .

#### Fournisseur TV {#tv-provider}

Le fournisseur de télévision est une entreprise qui fournit des services de télévision aux consommateurs par câble, satellite ou par Internet.

Le fournisseur de télévision est identifié par une valeur unique définie lors du processus d&#39;intégration entre le fournisseur de télévision et l&#39;Adobe.

Synonyme de [MVPD](#mvpd) et [Fournisseur d’identités](#identity-provider).

### U {#u}

#### Agent utilisateur {#user-agent}

L’agent utilisateur fait référence à un navigateur ou à un composant similaire (spécifique à la plate-forme) capable de naviguer sur le web et d’afficher la page de connexion [MVPD](#mvpd).

#### Métadonnées utilisateur {#user-metadata}

Les métadonnées utilisateur font référence à des attributs spécifiques à l’utilisateur (par exemple, des codes postaux, des évaluations parentales, des ID utilisateur, etc.) qui sont gérés par le [MVPD](#mvpd) et fournis par l’authentification Adobe Pass dans le cadre d’un [profil](#profile).

Pour plus d’informations, consultez la documentation [Métadonnées utilisateur](/help/authentication/user-metadata-feature.md).

### V {#v}

#### VSA {#vsa}

Le compte d’abonné vidéo (VSA) est une structure développée par Apple fournie au [programmeur](#programmer) afin d’activer une expérience utilisateur de connexion unique.

Pour plus d’informations, reportez-vous à la documentation [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) et [Connexion unique à l’aide de flux de partenaires](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) .
