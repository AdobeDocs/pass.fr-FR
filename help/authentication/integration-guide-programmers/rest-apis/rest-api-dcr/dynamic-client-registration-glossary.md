---
title: Glossaire de Dynamic Client Registration (DCR)
description: Glossaire de Dynamic Client Registration (DCR)
exl-id: 4ce67fa5-b0e5-4967-b83d-c682426d9329
source-git-commit: ae02f53afc58b7d31f57bcc1e4dd1328f12abc3e
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Glossaire de Dynamic Client Registration (DCR) {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Ce document fournit des définitions des termes utilisés lors de l’intégration de l’enregistrement client dynamique d’authentification Adobe Pass (DCR).

>[!MORELIKETHIS]
> 
> * [Glossaire de l’API REST v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## Glossaire {#glossary-terms}

### A {#a}

#### Jeton d’accès {#access-token}

Le jeton d’accès est un jeton généré par l’authentification Adobe Pass suite au processus [Enregistrement dynamique du client (DCR)](#dcr) destiné à assurer l’accès aux API protégées.

### C {#c}

#### Informations d’identification client {#client-credentials}

Les informations d’identification du client sont un ensemble de valeurs uniques générées pendant le processus [Enregistrement dynamique du client (DCR)](#dcr) et destinées à être utilisées pour obtenir un [jeton d’accès](#access-token).

#### Custom Scheme {#custom-scheme}

Le schéma personnalisé est une valeur unique référençant une application [Programmeur](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) qui peut être générée et téléchargée à partir d’Adobe Pass [Tableau de bord TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) et qui est destinée à être utilisée comme redirection finale dans les applications s’exécutant sur des appareils iOS.

### D {#d}

#### DCR {#dcr}

L’enregistrement client dynamique (DCR) est un mécanisme d’autorisation défini par la norme [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) et repose sur le cadre d’autorisation OAuth 2.0 décrit par la norme [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

Le DCR est fourni à un [programmeur](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) en tant que service d’authentification Adobe Pass pouvant autoriser l’accès aux API protégées.

Pour plus d’informations, consultez la documentation [ Présentation de l’enregistrement client dynamique ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

### R {#r}

#### Application enregistrée {#registered-application}

L’application enregistrée est un concept d’authentification Adobe Pass qui stocke des informations sur l’application [Programmeur](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) qui nécessite de poursuivre le processus [Enregistrement dynamique du client (DCR)](#dcr).

### S {#s}

#### Instruction de logiciel {#software-statement}

L’instruction du logiciel est un jeton Web JSON (JWT) qui peut être téléchargé à partir d’Adobe Pass [tableau de bord TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) et est destinée à être utilisée dans le cadre du processus [Enregistrement dynamique du client (DCR)](#dcr).
