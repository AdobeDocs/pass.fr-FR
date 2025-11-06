---
title: Présentation de la surveillance du service de droits
description: Présentation de la surveillance du service de droits
exl-id: ebd5d650-0a32-4583-9045-5156356494e2
source-git-commit: 49a6a75944549dbfb062b1be8a053e6c99c90dc9
workflow-type: tm+mt
source-wordcount: '1303'
ht-degree: 0%

---

# Présentation de la surveillance du service de droits {#entitlement-service-monitoring-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#introduction}

Les sites et applications TVE doivent être disponibles 24h/24 et 7j/7. Les clients ont donc besoin d’insight en temps réel dans les événements de droits afin de détecter et de corriger les problèmes le plus rapidement possible. Ils doivent également analyser les données mensuelles afin de déterminer quelles plateformes fournissent la majeure partie du trafic, et lesquelles sont susceptibles de présenter une mauvaise implémentation et de faibles taux de conversion.

Entitlement Service Monitoring (ESM) fournit aux programmeurs et aux MVPD un flux de données qui offre une visibilité en temps réel sur leurs événements d’authentification et d’autorisation. Les données sont collectées à partir des systèmes d’authentification Adobe Pass et fournies par le biais d’une API RESTful.  Les clients peuvent utiliser les données de manière directe ou à partir de leurs propres tableaux de bord opérationnels personnalisés.

Les principaux éléments du système MES sont ses mesures et ses dimensions. ESM génère des rapports qui contiennent des mesures agrégées en fonction de la sélection de dimension. Comme les événements Adobe Pass sont consignés dans le fuseau horaire PST, les rapports ESM sont également disponibles dans le fuseau horaire PST.

L’API ESM n’est généralement pas disponible.  Contactez votre représentant Adobe pour toute question sur la disponibilité.

## ESM pour les programmeurs {#esm-for-programmers}

### Les programmeurs peuvent surveiller les mesures suivantes : {#programmers-monitor-metrics}


| *Nom des mesures* | *Description* |
|-------------------------|--------------------------|
| authn-attempts | Nombre de flux d’authentification lancés |
| création réussie | Nombre de jetons d’authentification obtenus avec succès par les clients |
| en attente de création | Nombre de jetons d’authentification générés avec succès (que le client les ait obtenus ou non) |
| authn-failed | Nombre d’échecs d’authentification effectués via un système externe. |
| jetons sans client | Nombre de jetons sans client émis avec succès |
| clientless-failure | Nombre de tentatives infructueuses de réception de jetons depuis l’API sans client |
| authz-attempts | Nombre de tentatives d’autorisation |
| création réussie | Nombre d’autorisations réussies |
| authz-failed | Nombre d&#39;autorisations refusées par les MVPD au niveau de l&#39;application |
| authz-failed | Nombre de tentatives d&#39;autorisation considérées comme malveillantes par Adobe Service Provider et rejetées suite à une prévention d&#39;attaque par déni de service |
| aut-latence | Nombre total de millisecondes passées sur le point d’entrée de MVPD |
| media-tokens | Nombre de jetons de médias courts générés (qui sont assimilés au nombre de requêtes de lecture) |
| comptes uniques | Nombre d’utilisateurs uniques ayant effectué des actions de droits (AuthN / AuthZ) dans l’intervalle de temps sélectionné. (Cette mesure s’affiche uniquement si des valeurs quotidiennes sont demandées.) </br> Elle est calculée pour chaque centre de données individuel. Lorsque la dimension « dc » n’est pas demandée, cette mesure ne s’affiche pas. |
| uniques-sessions | Nombre de sessions uniques qui ont effectué des appels de flux d’authentification au service d’authentification Adobe Pass dans l’intervalle de temps sélectionné. (Cette mesure s’affiche uniquement si des valeurs quotidiennes sont demandées.) </br> Elle est calculée pour chaque centre de données individuel. Lorsque la dimension « dc » n’est pas demandée, cette mesure ne s’affiche pas. |
| comptage | Compteur simple utilisé dans les rapports orientés événement |

</br>

### Les programmeurs peuvent filtrer les mesures répertoriées ci-dessus en fonction des dimensions suivantes : {#progr-filter-metrics}


| *Nom du Dimension* | *Description* |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| année | L’année à 4 chiffres |
| mois | Le mois de l&#39;année (1-12) |
| jour | Le jour du mois (1-31) |
| heure | Heure de la journée |
| minute | La minute de l&#39;heure |
| media-company | Société de médias propriétaire du site web qui a lancé le processus de droits pour l’utilisateur |
| dc | (Centre de données) Région d’origine dans laquelle la demande a été reçue. |
| proxy | Le MVPD proxy (qui sera « Direct » pour les intégrations directes) |
| mvpd | MVPD responsable de l’octroi des droits à l’utilisateur |
| requestor-id | Identifiant du demandeur utilisé pour effectuer la demande de droit |
| canal | Site web du canal, extrait du champ de la ressource (extrait de la payload MRSS comme canal/titre si fourni, ou mappé à la valeur de la ressource si elle n’est pas au format RSS). |
| resource-id | Le titre réel de la ressource impliqué dans la demande d’autorisation (extrait de la payload MRSS comme l’élément/le titre, le cas échéant). |
| appareil | Plateforme de l’appareil (PC, mobile, console, etc.) |
| japper | Fournisseur d’authentification externe lorsque le flux d’authentification est effectué via un système externe. </br> Les valeurs peuvent être les suivantes : </br> - S.O. - l’authentification a été fournie par Adobe Pass Authentication </br> - Apple - le système externe qui a fourni l’authentification est Apple |
| famille du système d’exploitation | Système d’exploitation s’exécutant sur l’appareil |
| browser-family | Agent utilisateur utilisé pour accéder à l’authentification Adobe Pass |
| cdt | Plateforme d’appareil (alternative) actuellement utilisée pour le sans client. </br> Les valeurs peuvent être les suivantes : </br> - S/O - l’événement n’a pas été généré à partir d’un </br> SDK sans client - Inconnu - Étant donné que le paramètre deviceType d’une API sans client est facultatif, certains appels ne contiennent aucune valeur. </br> - toute autre valeur envoyée par l’intermédiaire de l’API Clientless, par exemple xbox, appletv, roku, etc. </br> |
| platform-version | Version du SDK sans client |
| type de système d’exploitation | Système d’exploitation s’exécutant sur l’appareil, alternative (non utilisé actuellement) |
| browser-version | Version de l’agent utilisateur |
| nsdk | Le SDK client utilisé (android, fireTV, js, iOS, tvOS, non-sdk) |
| nsdk-version | Version du SDK du client d’authentification Adobe Pass |
| événement | Nom de l’événement d’authentification Adobe Pass |
| raison | La raison des échecs, telle qu’elle est signalée par l’authentification Adobe Pass |
| type sso | Le mécanisme SSO sous-jacent : platform/passive/adobe. Indique que le jeton d’autorisation a été émis en réutilisant l’authentification dans une autre application |
| plate-forme | Plateforme identifiée par l’appareil. Valeurs possibles : </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - etc |
| application-name | Nom de l’application configurée, dans le tableau de bord TVE, pour l’application enregistrée DCR configurée pour être utilisée. |
| application-version | Version de l’application configurée, dans le tableau de bord TVE, pour l’application enregistrée DCR configurée pour être utilisée. |
| customer-app | ID d’application personnalisé transmis via [Informations sur l’appareil](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| content-category | Catégorie du contenu demandée par votre application. |

## ESM pour MVPD {#esm-for-mvpds}

### Les MVPD peuvent surveiller les mesures suivantes :

| *Nom de la mesure* | *Description* |
|---|---|
| authn-attempts | Nombre de flux d’authentification lancés |
| création réussie | Nombre de jetons d’authentification obtenus avec succès par les clients |
| en attente de création | Nombre de jetons d’authentification générés avec succès (que le client les ait obtenus ou non) |
| authn-failed | Nombre d’échecs d’authentification effectués via un système externe. |
| authz-attempts | Nombre de tentatives d’autorisation |
| création réussie | Nombre d’autorisations réussies |
| authz-failed | Nombre d&#39;autorisations refusées par les MVPD au niveau de l&#39;application |
| authz-failed | Nombre de tentatives d&#39;autorisation considérées comme malveillantes par Adobe Service Provider et rejetées suite à une prévention d&#39;attaque par déni de service |
| aut-latence | Nombre total de millisecondes passées sur le point d’entrée de MVPD |

### Les MVPD peuvent filtrer les mesures répertoriées ci-dessus en fonction des dimensions suivantes :

| *Nom du Dimension* | *Description* |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| année | L’année à 4 chiffres |
| mois | Le mois de l&#39;année (1-12) |
| jour | Le jour du mois (1-31) |
| heure | Heure de la journée |
| minute | La minute de l&#39;heure |
| mvpd | Identifiant mvpd utilisé pour effectuer la demande de droits |
| requestor-id | Identifiant du demandeur utilisé pour effectuer la demande de droit |
| japper | Fournisseur d’authentification externe lorsque le flux d’authentification est effectué via un système externe. </br> Les valeurs peuvent être les suivantes : </br> - S.O. - l’authentification a été fournie par Adobe Pass Authentication </br> - Apple - le système externe qui a fourni l’authentification est Apple |
| cdt | Plateforme d’appareil (alternative) actuellement utilisée pour le sans client. </br> Les valeurs peuvent être les suivantes : </br> - S/O - l’événement n’a pas été généré à partir d’un </br> SDK sans client - Inconnu - Étant donné que le paramètre deviceType d’une API sans client est facultatif, certains appels ne contiennent aucune valeur. </br> - toute autre valeur envoyée par l’intermédiaire de l’API Clientless, par exemple xbox, appletv, roku, etc. </br> |
| type-sdk | Le SDK client utilisé (Flash, HTML5, Android natif, iOS, Clientless, etc.) |
| plate-forme | Plateforme identifiée par l’appareil. Valeurs possibles : </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - etc |
| nsdk | Le SDK client utilisé (android, fireTV, js, iOS, tvOS, non-sdk) |
| nsdk-version | Version du SDK du client d’authentification Adobe Pass |

## Cas d’utilisation {#use-cases}

Vous pouvez utiliser les données ESM pour les cas d’utilisation suivants :

- **Surveillance** - Les équipes d’opérations ou de surveillance peuvent créer un tableau de bord ou un graphique qui appelle l’API toutes les minutes. À l’aide des informations affichées, ils peuvent détecter un problème (avec l’authentification Adobe Pass ou avec un MVPD) dès son apparition.

- **Débogage/test de qualité** - Les données étant également réparties par plateforme, appareil, navigateur et système d’exploitation, l’analyse des schémas d’utilisation peut mettre en évidence des problèmes sur des combinaisons spécifiques (par exemple, Safari sur OSX).

- **Analytics** - Les données fournies peuvent être utilisées pour compléter/contrôler les données côté client collectées via Adobe Analytics ou un autre outil d’analyse.
