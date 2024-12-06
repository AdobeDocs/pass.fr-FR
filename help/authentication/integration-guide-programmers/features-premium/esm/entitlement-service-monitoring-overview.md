---
title: Présentation de la surveillance du service de droits
description: Présentation de la surveillance du service de droits
exl-id: ebd5d650-0a32-4583-9045-5156356494e2
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1303'
ht-degree: 0%

---

# Présentation de la surveillance du service de droits {#entitlement-service-monitoring-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#introduction}

Les sites et applications TVE doivent être disponibles 24/7. Les clients doivent donc disposer d’informations en temps réel sur les événements de droits afin de détecter et de corriger les problèmes le plus rapidement possible. Ils doivent également analyser les données mensuelles afin de déterminer les plateformes qui fournissent la majeure partie du trafic et celles qui peuvent présenter une mauvaise implémentation et des taux de conversion médiocres.

Le service de surveillance des droits (ESM) fournit aux programmeurs et aux MVPD un flux de données qui offre une visibilité en temps réel dans leurs événements d’authentification et d’autorisation. Les données sont collectées à partir des systèmes d’authentification Adobe Pass et fournies via une API RESTful.  Les clients peuvent utiliser les données directement ou depuis leurs propres tableaux de bord opérationnels personnalisés.

Les éléments essentiels du système de gestion de l&#39;environnement sont ses mesures et ses dimensions. ESM génère des rapports qui contiennent des mesures agrégées en fonction de la sélection de dimension. Comme les événements Adobe Pass sont consignés dans le fuseau horaire PST, les rapports ESM sont également disponibles dans le fuseau horaire PST.

L’API ESM n’est pas disponible en général.  Contactez votre représentant Adobe pour toute question concernant la disponibilité.

## ESM pour les programmeurs {#esm-for-programmers}

### Les programmeurs peuvent surveiller les mesures suivantes : {#programmers-monitor-metrics}


| *Nom des mesures* | *Description* |
|-------------------------|--------------------------|
| authn-tries | Nombre de flux d’authentification lancés |
| authn-success | Nombre de jetons d’authentification obtenus avec succès par les clients |
| authn-pending | Nombre de jetons d’authentification générés avec succès (sans tenir compte du fait que le client l’ait réellement obtenu) |
| authn-failed | Nombre d’échecs d’authentification effectués via un système externe. |
| jetons sans client | Nombre de jetons sans client émis avec succès |
| clientless-failure | Nombre de tentatives de réception de jetons par l’API sans client ayant échoué |
| authz-tries | Nombre de tentatives d’autorisation |
| authz-success | Nombre d’autorisations réussies |
| authz-failed | Nombre d’autorisations refusées par les MVPD au niveau de l’application |
| authz-reject | Nombre de tentatives d’autorisation considérées comme malveillantes par le fournisseur de services Adobe et rejetées en raison d’une prévention des attaques par le DoS |
| authz-latency | Nombre total de millisecondes passées sur le point de terminaison du MVPD |
| media-tokens | Nombre de jetons multimédias courts générés (qui s’assimilent au nombre de requêtes de lecture) |
| unique-accounts | Nombre d’utilisateurs uniques qui ont exécuté des actions de droit (AuthN / AuthZ) dans l’intervalle de temps sélectionné. (Cette mesure ne s’affichera que si des valeurs quotidiennes sont demandées.) </br> Ce calcul est effectué pour chaque centre de données individuel. Lorsque la dimension &quot;dc&quot; n’est pas demandée, cette mesure ne s’affiche pas. |
| unique-sessions | Nombre de sessions uniques qui ont effectué des appels de flux d’authentification au service d’authentification Adobe Pass pendant l’intervalle de temps sélectionné. (Cette mesure ne s’affichera que si des valeurs quotidiennes sont demandées.) </br> Ce calcul est effectué pour chaque centre de données individuel. Lorsque la dimension &quot;dc&quot; n’est pas demandée, cette mesure ne s’affiche pas. |
| count | Un compteur simple utilisé dans les rapports axés sur les événements |

</br>

### Les programmeurs peuvent filtrer les mesures répertoriées ci-dessus selon les dimensions suivantes : {#progr-filter-metrics}


| *Nom de la Dimension* | *Description* |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| year | Année à 4 chiffres |
| month | Mois de l’année (1-12) |
| day | Jour du mois (1-31) |
| hour | Heure de la journée |
| minute | La minute de l’heure |
| media-company | La société de médias propriétaire du site web qui a lancé le processus de droits pour l’utilisateur |
| dc | (centre de données) La région d’origine où la demande a été reçue. |
| proxy | Le proxy MVPD (qui sera &quot;Direct&quot; pour les intégrations directes) |
| mvpd | Le MVPD responsable de l’octroi des droits à l’utilisateur |
| requestor-id | Identifiant du demandeur utilisé pour exécuter la demande de droit |
| channel | Le site web du canal, extrait du champ de ressource (extrait de la payload MRSS en tant que canal/titre s&#39;il est fourni, ou mappé à la valeur de la ressource si elle n&#39;est pas au format RSS). |
| resource-id | Titre réel de la ressource impliquée dans la requête d’autorisation (extrait de la charge utile MRSS en tant qu’élément/titre s’il est fourni). |
| appareil | La plateforme de l’appareil (PC, mobile, console, etc.) |
| eap | Fournisseur d’authentification externe lorsque le flux d’authentification est effectué via un système externe. </br> Les valeurs peuvent être : </br> - S/O - l’authentification a été fournie par l’authentification Adobe Pass </br> - Apple - le système externe qui a fourni l’authentification est Apple |
| os-family | Système d’exploitation en cours d’exécution sur le périphérique |
| browser-family | Agent utilisateur utilisé pour accéder à l’authentification Adobe Pass |
| cdt | Plateforme d’appareil (alternative), actuellement utilisée pour les clients sans client. </br> Les valeurs peuvent être : </br> - S/O - l’événement ne provient pas d’un SDK sans client </br> - Inconnu - Puisque le paramètre deviceType d’une API sans client est facultatif, il existe des appels qui ne contiennent aucune valeur. </br> - toute autre valeur envoyée via l’API sans client, par exemple xbox, appletv, roku, etc. </br> |
| platform-version | Version du SDK sans client |
| os-type | Système d’exploitation en cours d’exécution sur l’appareil, alternative (non utilisée actuellement) |
| browser-version | Version de l’agent utilisateur |
| nsdk | Le SDK client utilisé (android, fireTV, js, iOS, tvOS, non-sdk) |
| nsdk-version | Version du SDK client d’authentification Adobe Pass |
| event | Nom de l’événement d’authentification Adobe Pass |
| reason | Raison des échecs, comme indiqué par l’authentification Adobe Pass |
| sso-type | Mécanisme d’authentification unique sous-jacent : platform/passive/adobe. Indique que le jeton d’autorisation a été émis en réutilisant AuthN dans une autre application. |
| platform | L’appareil a identifié la plateforme. Valeurs possibles : </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - etc. |
| application-name | Nom de l’application configuré, dans le tableau de bord TVE, pour l’application enregistrée DCR configurée pour être utilisée. |
| application-version | Version de l’application configurée, dans le tableau de bord TVE, pour l’application enregistrée DCR configurée pour être utilisée. |
| customer-app | L’ID d’application personnalisé transmis via [ Device Information](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md). |
| content-category | La catégorie du contenu demandée par votre application. |

## ESM pour MVPD {#esm-for-mvpds}

### Les MVPD peuvent surveiller les mesures suivantes :

| *Nom de mesure* | *Description* |
|---|---|
| authn-tries | Nombre de flux d’authentification lancés |
| authn-success | Nombre de jetons d’authentification obtenus avec succès par les clients |
| authn-pending | Nombre de jetons d’authentification générés avec succès (sans tenir compte du fait que le client l’ait réellement obtenu) |
| authn-failed | Nombre d’échecs d’authentification effectués via un système externe. |
| authz-tries | Nombre de tentatives d’autorisation |
| authz-success | Nombre d’autorisations réussies |
| authz-failed | Nombre d’autorisations refusées par les MVPD au niveau de l’application |
| authz-reject | Nombre de tentatives d’autorisation considérées comme malveillantes par le fournisseur de services Adobe et rejetées en raison d’une prévention des attaques par le DoS |
| authz-latency | Nombre total de millisecondes passées sur le point de terminaison du MVPD |

### Les MVPD peuvent filtrer les mesures répertoriées ci-dessus selon les dimensions suivantes :

| *Nom de la Dimension* | *Description* |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| year | Année à 4 chiffres |
| month | Mois de l’année (1-12) |
| day | Jour du mois (1-31) |
| hour | Heure de la journée |
| minute | La minute de l’heure |
| mvpd | Identifiant mvpd utilisé pour effectuer la demande de droits. |
| requestor-id | Identifiant du demandeur utilisé pour exécuter la demande de droit |
| eap | Fournisseur d’authentification externe lorsque le flux d’authentification est effectué via un système externe. </br> Les valeurs peuvent être : </br> - S/O - l’authentification a été fournie par l’authentification Adobe Pass </br> - Apple - le système externe qui a fourni l’authentification est Apple |
| cdt | Plateforme d’appareil (alternative), actuellement utilisée pour les clients sans client. </br> Les valeurs peuvent être : </br> - S/O - l’événement ne provient pas d’un SDK sans client </br> - Inconnu - Puisque le paramètre deviceType d’une API sans client est facultatif, il existe des appels qui ne contiennent aucune valeur. </br> - toute autre valeur envoyée via l’API sans client, par exemple xbox, appletv, roku, etc. </br> |
| sdk-type | Le SDK client utilisé (Flash, HTML5, Android natif, iOS, sans client, etc.) |
| platform | L’appareil a identifié la plateforme. Valeurs possibles : </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - etc. |
| nsdk | Le SDK client utilisé (android, fireTV, js, iOS, tvOS, non-sdk) |
| nsdk-version | Version du SDK client d’authentification Adobe Pass |

## Cas d’utilisation {#use-cases}

Vous pouvez utiliser les données ESM pour les cas d’utilisation suivants :

- **Surveillance** - Les équipes d’opérations ou de surveillance peuvent créer un tableau de bord ou un graphique qui appelle l’API toutes les minutes. En utilisant les informations affichées, ils peuvent détecter un problème (avec l’authentification Adobe Pass ou avec un MVPD) à la minute où il apparaît.

- **Débogage/test de qualité** - Comme les données sont également ventilées par plateforme, périphérique, navigateur et système d’exploitation, l’analyse des modèles d’utilisation peut pointer des problèmes sur des combinaisons spécifiques (par exemple, Safari sur OSX).

- **Analytics** - Les données fournies peuvent être utilisées pour compléter/auditer les données côté client collectées via Adobe Analytics ou un autre outil d’analyse.

<!--
## Related Information {#related-information}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Degradation API Overview](/help/authentication/degradation-api-overview.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
