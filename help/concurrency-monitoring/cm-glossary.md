---
title: Glossaire
description: Glossaire des termes de la surveillance simultanée
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---


# Glossaire {#glossary}

## Identifiant de compte {#accid-defn}

* Compte MVPD d’un abonné, correspondant généralement au compte de facturation réel. Ce compte doit être identifiable par le MVPD dans son propre système.

## Action {#action-defn}

* Le type d’accès demandé par le sujet ; les valeurs possibles pour CM sont ***lancer*** ou ***continue*** une session de diffusion en continu.

## Flux actif {#active-stream-defn}

* Flux qui a reçu au moins 1 événement (pulsation) au cours des 90 dernières secondes.

* ***Remarque :*** Si le dernier événement du flux est de type stop (`?event=stop`), il ne sera pas comptabilisé. Il s’agit d’une optimisation qui permet à un lecteur de fermer explicitement un flux afin qu’il ne soit plus considéré comme &quot;actif&quot;.

## Application {#application-defn}

* Développé par le tenant pour l’accès au contenu vidéo
* prend et applique des décisions concernant l’accès au contenu en fonction des informations fournies par le service de surveillance de la simultanéité (cette information est valide dans la variable [Point d’informations sur la stratégie](/help/concurrency-monitoring/policy-info-pt-versionone.md) case)
* Comportera une variable **ID d’application** fourni par Adobe.

## Service de surveillance de la simultanéité {#cm-service-defn}

* Agit comme un système de surveillance pour les abonnés, soutenant les programmes multivariés et les programmeurs dans leurs exigences d’application des politiques inter-applications.
* Reçoit les pulsations qui indiquent l’activité du flux.
* Agit comme _Point de décision politique_ en évaluant les demandes d’autorisation en fonction de l’activité de l’utilisateur et en fournissant une réponse d’autorisation/de refus.
* Agit comme _Point d’informations sur la stratégie_ en signalant le nombre de diffusions actives (et les métadonnées de diffusion supplémentaires) pour un abonné.

## Environnement {#env-defn}

* Informations supplémentaires pertinentes pour la requête, telles que les paramètres de configuration ou l’heure du système.

## MVPD {#mvpd-defn}

* Distributeur de programmation vidéo multicanal.
* Agit comme fournisseur d’authentification et d’autorisation, mais peut également être un fournisseur de service ou de contenu.

## Stratégie {#policy-defn}

* Le concept de contrôle d’accès de base dans CM défini comme une cible et une ou plusieurs règles regroupées sous un nom unique.

## Point d’administration des stratégies (PAP) {#policy-admin-pt-defn}

* Point qui gère les stratégies d’autorisation d’accès. Cela ne sera pas documenté ici, mais nous allons éventuellement fournir une console en libre-service permettant aux clients de gérer leurs stratégies d’accès.

## Point de décision politique (PDP) {#policy-decn-pt-defn}

* Point qui évalue les demandes d’accès par rapport aux stratégies d’autorisation avant d’émettre des décisions d’accès.

## Point d’application des stratégies (PEP) {#policy-ef-pt-defn}

* Point qui intercepte la demande d’accès de l’utilisateur à une ressource, envoie une demande de décision au PDP et applique cette décision sur la demande. Il s’agit actuellement de l’application cliente et il n’est pas prévu de transférer ce rôle à la surveillance de la simultanéité.

## Point d’informations sur la stratégie (PIP) {#policy-info-pt-defn}

* Source de valeurs d’attribut. La surveillance de la simultanéité agit comme un point d’information en fournissant :
   * métadonnées de flux de transmission.
   * mesures d’activité concernant les flux simultanés.

## Programmeur {#programmer-defn}

* Agit en tant que fournisseur de service et de contenu.
* S’appuie sur l’application cliente déployée qui s’intègre au service de surveillance de simultanéité pour appliquer les stratégies de sécurité définies en fonction des données de service mentionnées ci-dessus.
* Doit prendre en charge le MVPD pour la collecte de l’activité d’abonné et l’application des règles de limitation lorsque sur leurs propriétés.
* Peut également être intéressé par la limitation de l’accès simultané à leur contenu sur tous les portails de destination, en tant que règle distincte.

  *Q : Pourquoi le programmeur et non l’identifiant du demandeur comme dans le reste de l’authentification Adobe Pass ?*

  *R : La raison est de permettre aux programmeurs d’utiliser ce paramètre de manière flexible pour transmettre ou isoler des données entre leurs propriétés en fonction de leurs cas d’utilisation.*

## Ressource {#resource-defn}

* Contenu réel qu’un sujet souhaite consommer. La ressource contient généralement des attributs liés au propriétaire (éditeur) et peut également fournir des informations supplémentaires telles que le genre ou autre.

## Règle {#rule-defn}

* Fonction booléenne évaluée par rapport à un flux particulier et à l’activité utilisateur correspondante afin de déterminer si l’accès doit être autorisé ou refusé pour ce flux.

## Session de diffusion en continu {#streaming-session-defn}

* Une session de vidéo en continu initiée par un sujet pour utiliser une ressource spécifique.

## Objet {#subj-defn}

* Consommateur du contenu (vidéo) sur Internet. Nous évitons délibérément le terme _**user**_, car la surveillance de la simultanéité traite généralement des identifiants de compte MVPD (qui impliquent plusieurs utilisateurs réels partageant le même contrat, par exemple des membres de la famille pour un foyer).

* Pour chaque flux, l’objet peut être amélioré avec des attributs liés à la personne réelle utilisant le service, son appareil connecté au réseau, etc.

## Abonné {#subscriber-defn}

* Le client payeur d’un MVPD ou une personne qui partage les informations d’identification d’un client payant
* peut être empêché de regarder le contenu par le service de surveillance de simultanéité, par l’application cliente à l’aide du service mentionné ci-dessus.
* Dans le meilleur des cas, il ne remarque jamais l’existence du service de surveillance de la simultanéité.

## Cible {#target-defn}

* Prédicat de flux qui renvoie si la règle s’applique à un flux donné. La cible implicite dans CM est tout flux créé par une application qui référence la stratégie en question. De plus, des conditions de valeur d’attribut peuvent être ajoutées afin d’affiner le filtrage de l’activité avant d’appliquer les règles.

## Tenant {#tenant-defn}

* Une surveillance simultanée de l’organisation du client.
