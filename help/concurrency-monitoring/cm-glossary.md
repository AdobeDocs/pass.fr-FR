---
title: Glossaire
description: Glossaire des termes dans la surveillance de simultanéité
exl-id: 3b3b36fe-9f04-4de9-bd84-9f8d766bbc71
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Glossaire {#glossary}

## ID de compte {#accid-defn}

* Compte MVPD d’un abonné, correspondant généralement au compte de facturation réel. Ce compte doit être identifiable par le MVPD dans son propre système.

## Action {#action-defn}

* Type d’accès demandé par le sujet. Les valeurs possibles pour CM sont ***initier*** ou ***continuer*** une session de streaming.

## Flux actif {#active-stream-defn}

* Un flux qui a reçu au moins 1 événement (pulsation) au cours des 90 dernières secondes.

* ***Remarque :*** si le dernier événement du flux est de type stop (`?event=stop`), il ne sera pas comptabilisé. Il s’agit d’une optimisation qui permet à un lecteur de fermer explicitement un flux afin qu’il ne soit plus considéré comme « actif ».

## Application {#application-defn}

* Développé par le client pour l’accès au contenu vidéo
* prend et applique les décisions concernant l’accès au contenu en fonction des informations fournies par le service de surveillance simultanée (cela est valide dans le cas du [point d’information sur les politiques](/help/concurrency-monitoring/technical/policy-info-pt-versionone.md)) ;
* Disposera d’un **ID d’application** unique fourni par Adobe.

## Service de surveillance de concurrence {#cm-service-defn}

* Agit comme un système de surveillance pour les abonnés, soutenant les MVPD et les programmeurs dans leurs exigences d&#39;application de politique entre applications.
* Reçoit des pulsations qui indiquent l’activité du flux.
* Agit comme un _point de décision de politique_ en évaluant les demandes d’autorisation en fonction de l’activité de l’utilisateur et en fournissant une réponse d’autorisation/de refus.
* Agit comme un _point d’information sur la politique_ en signalant le nombre de flux actifs (et de métadonnées de flux supplémentaires) pour un abonné.

## Environnement {#env-defn}

* Informations supplémentaires pertinentes pour la requête, telles que les paramètres de configuration ou l’heure du système.

## MVPD {#mvpd-defn}

* Distributeur De Programmation Vidéo Multicanal.
* Agit comme fournisseur d’authentification et d’autorisation, mais peut également être un fournisseur de services ou de contenu.

## Politiques {#policy-defn}

* Concept de contrôle d’accès principal dans CM défini comme une cible et une ou plusieurs règles regroupées sous un nom unique.

## Point d’administration des politiques (PAP) {#policy-admin-pt-defn}

* Point qui gère les politiques d’autorisation d’accès. Cela ne sera pas documenté ici, mais nous allons fournir éventuellement une console en libre-service pour que les clients puissent gérer leurs politiques d’accès.

## Point de décision de politique (PDP) {#policy-decn-pt-defn}

* Point qui évalue les demandes d’accès par rapport aux politiques d’autorisation avant d’émettre les décisions d’accès.

## Point d&#39;application des politiques (PEP) {#policy-ef-pt-defn}

* Point qui intercepte la demande d’accès de l’utilisateur à une ressource, adresse une demande de décision au PDP et applique cette décision à la demande. Il s’agit actuellement de l’application cliente et aucun transfert de ce rôle vers la surveillance de simultanéité n’est prévu.

## Point d’information sur les politiques (PIP) {#policy-info-pt-defn}

* Source de valeurs d’attribut. La surveillance de la simultanéité sert de point d’information en fournissant les éléments suivants :
   * métadonnées de flux de transmission.
   * mesures d’activité concernant les flux simultanés.

## Programmeur {#programmer-defn}

* Agit comme un fournisseur de services et de contenu.
* Repose sur l’application cliente déployée qui s’intègre au service de surveillance simultanée pour appliquer les politiques de sécurité définies en fonction des données de service mentionnées ci-dessus.
* Doit prendre en charge le MVPD pour collecter l’activité des abonnés et appliquer les règles de limitation lors de l’utilisation de leurs propriétés.
* Peut également souhaiter limiter l’accès simultané à son contenu sur tous les portails de destination, dans une règle distincte.

  *Q : Pourquoi le programmeur et non l’ID de demandeur comme dans le reste de l’authentification Adobe Pass ?*

  *A : la raison est de permettre aux programmeurs d’utiliser ce paramètre de manière flexible pour transmettre ou isoler des données entre leurs propriétés en fonction de leurs cas d’utilisation.*

## Ressource {#resource-defn}

* Contenu réel qu’un sujet souhaite consommer. La ressource comporte généralement des attributs liés au propriétaire (éditeur) et peut également fournir des informations supplémentaires telles que le genre ou autre chose.

## Règle {#rule-defn}

* Fonction booléenne qui est évaluée par rapport à un flux particulier et à l’activité de l’utilisateur ou de l’utilisatrice concernée afin de déterminer si l’accès doit être autorisé ou refusé pour ce flux.

## Session de streaming {#streaming-session-defn}

* Session vidéo en flux continu lancée par un sujet pour consommer une ressource spécifique.

## Objet {#subj-defn}

* Consommateur du contenu (vidéo) sur Internet. Nous évitons délibérément le terme _**utilisateur**_, car la surveillance de simultanéité traite généralement des ID de compte MVPD (qui impliquent plusieurs utilisateurs réels partageant le même contrat, par exemple des membres de la famille d’un foyer).

* Pour chaque flux, le sujet peut être amélioré avec des attributs liés à la personne réelle utilisant le service, à son appareil connecté au réseau, etc.

## Abonné {#subscriber-defn}

* Client payant d’un MVPD ou personne qui partage les informations d’identification d’un client payant
* Peut être empêché de regarder du contenu par le service de surveillance simultanée, par l’application cliente utilisant le service mentionné ci-dessus.
* Dans le meilleur des cas, il ou elle ne remarque jamais l&#39;existence du Service de surveillance simultanée

## Cible {#target-defn}

* Prédicat de flux qui renvoie une valeur indiquant si la règle est applicable à un flux donné. Dans CM, la cible implicite sera tout flux créé par une application qui référence la politique en question. De plus, des conditions de valeur d’attribut peuvent être ajoutées afin d’affiner le filtrage des activités avant d’appliquer les règles.

## Locataire {#tenant-defn}

* Organisation du client de la surveillance simultanée.
