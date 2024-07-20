---
title: Présentation de l’API REST
description: Présentation des API REST
exl-id: 5533d852-f644-417e-bf80-6f7aa1edd6b2
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 0%

---

# Présentation de l’API REST {#rest-api-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


## Vue d’ensemble {#over}

L’API REST d’authentification Adobe Pass permet d’accéder directement aux services d’authentification et d’autorisation de TV Everywhere (TVE). Cette API prend en charge deux architectures principales : Serveur à serveur ou Périphériques connectés (par exemple, consoles de jeux, télévisions dynamiques, décodeurs, etc.). applications qui ne disposent pas de fonctionnalités de navigation web.

### Mécanisme de ralentissement

L’API REST d’authentification Adobe Pass est régie par un [mécanisme de limitation](/help/authentication/throttling-mechanism.md).


### Serveur à serveur

Les solutions serveur à serveur impliquent des applications clientes de programmeur qui s’intègrent aux services de programmeur qui se connectent aux services d’authentification Adobe Pass pour les flux TVE. Cette approche déplace la plupart de l’implémentation de TVE du client vers le serveur où un seul module d’autorisation unifié peut être créé et géré. La principale responsabilité restante des applications clientes est la gestion d’un affichage web pour l’authentification des utilisateurs.



### Périphériques connectés

Les applications Périphériques connectés communiquent directement avec l’authentification Adobe Pass via les API REST pour effectuer la configuration, l’enregistrement, les vérifications d’état d’authentification et les flux d’autorisation, tandis qu’un second écran (navigateur) est requis pour le flux d’authentification. Par conséquent, les SDK natifs ne sont pas utilisés.



### Autres architectures

Outre les deux principales architectures basées sur les API REST, les solutions serveur à serveur et client direct pour les appareils intelligents, il existe d’autres architectures.  L’architecture du SDK, qui utilise un composant client appelé Access Enabler fourni par l’authentification Adobe Pass aux programmeurs, est l’un des Principal de cette architecture.  L’application utilise les API Access Enabler pour gérer le démarrage, l’authentification, l’autorisation et la déconnexion.  Toute communication entre l’application du programmeur et les serveurs d’authentification Adobe Pass se fait via l’Access Enabler.  Une version différente de Access Enabler est disponible pour les plateformes suivantes : JavaScript, iOS, tvOS, Android et FireTV.

Bien qu’il soit possible d’utiliser l’API REST directement sur les plateformes clientes qui prennent en charge les SDK natifs en dehors d’une solution serveur à serveur, cette approche n’est pas recommandée.



## Avantages et inconvénients de l’API REST {#ProsAndCons}

L’API REST d’authentification Adobe Pass a été créée pour fournir une solution TV Everywhere (TVE) pour les périphériques qui ne disposent pas de fonctionnalités de navigation web ni de stockage persistant. L’API REST prend en charge tous les flux d’authentification et d’autorisation, mais elle manque d’un composant SDK natif. Les SDK fournis et gérés par l’authentification Adobe Pass sont fournis avec des fonctionnalités prêtes à l’emploi qui implémentent des règles de fonctionnement qui, dans le cas de l’API REST, doivent être implémentées et gérées par les programmeurs. Dans le tableau des responsabilités du programmeur ci-dessous, nous décrivons les limites de l’API REST actuelle qui doivent être traitées par les programmeurs.



### Avantages et inconvénients serveur à serveur par rapport aux avantages et aux inconvénients basés sur le client

Une architecture serveur à serveur permet de consolider la majeure partie de la logique liée à l’authentification et à l’autorisation en une seule unité logique ou implémentation.  Cette approche a ses avantages et ses inconvénients.  Les avantages incluent :

* Mise en oeuvre unique pour la logique métier d’authentification et d’autorisation.
* Évitez d’implémenter cette logique sur chaque plateforme prise en charge à l’aide de ces plateformes avec des outils natifs.
* La possibilité de mettre à jour les fonctionnalités sans avoir à mettre à jour les clients avec toutes les exigences associées (par exemple, les mises à jour de la boutique d’applications).
* **Plus facilement** étendez et personnalisez les fonctionnalités authN et authZ (par exemple, ajoutez D2C).
* Gestion directe du trafic associé pour un meilleur contrôle, une meilleure qualité et une meilleure surveillance.



Encore une fois, les icônes sont répertoriées dans les responsabilités du programmeur, mais incluent les éléments suivants :

* SSO doit être mis en oeuvre pour chaque client pour les plateformes sans SSO Platform.
* Si nécessaire, les programmeurs doivent implémenter une logique spécifique au MVPD.
* Toutes les plateformes qui utilisent l’API REST partagent une configuration unique qui régit les propriétés telles que les TTL d’authentification.



### Périphériques connectés

Pour la plupart des appareils connectés, l’API REST doit être utilisée d’une manière ou d’une autre, car un SDK n’est pas disponible. L’appareil connecté utilise directement l’API REST ou s’intègre à une solution serveur à serveur qui utilise l’API REST.

## Responsabilités du programmeur {#programmer-responsibilities}

Les points suivants s’appliquent à la fois aux applications Serveur à Serveur et Appareil connecté.

| **Fonctionnalité** | **Responsable de la gestion de la fonctionnalité** | **Description des limites de l’API sans client actuelle et des différences par rapport aux SDK natifs** |
| --- | --- | --- |
| Paramètres de configuration appliqués par Platform | Adobe | L’une des **limitations majeures** sur l’utilisation de l’API REST sur toutes les plateformes (y compris les appareils mobiles comme iOS et Android) est que les paramètres de configuration correspondant à l’API REST dans notre outil de configuration de tableau de bord TVE sont appliqués sur tous les appareils (même s’il existe un appareil iOS qui exécute une application native implémentée en plus de notre API REST). Cette limitation **peut rompre** les TTL et les paramètres de plateforme convenus avec les MVPD - si ceux-ci diffèrent pour chaque plateforme. [1](#1) |
| Authentification unique | Programmeurs | Grâce à l’API REST, la connexion unique est disponible uniquement sur les plateformes qui prennent en charge la connexion unique (par exemple Apple, Roku, Amazon), tandis que la connexion unique ne peut pas être garantie pour les autres plateformes lors de l’utilisation de l’API REST. Les SDK mettent en cache les données de manière intersite/application. Cela signifie que l’utilisateur se connecte une fois sur un site/une application et qu’il est déjà connecté aux sites participants, sans intervention de l’utilisateur nécessaire. [2](#2) |
| Connexion unique | Programmeurs | Dans un scénario d’authentification unique du SDK natif, la déconnexion d’une application participante déconnecte l’utilisateur de partout. Sur l’API REST actuelle, nous ne prenons pas en charge SLO, la déconnexion d’une application entraîne celle de l’utilisateur uniquement pour cette application. |
| Mise en cache | Programmeurs | Les implémentations de l’API REST devront mettre en oeuvre leur propre mécanisme de mise en cache pour les éléments de données approuvés par l’entreprise. Les SDK mettent automatiquement en cache divers éléments de données, tout en prenant en compte diverses règles de fonctionnement. Par exemple, les métadonnées utilisateur sont mises en cache avec le même TTL que le jeton d’authentification, tandis que certains éléments peuvent être exclus par programmation de la mise en cache (contrôle en amont). |
| Mécanisme détaillé de reporting des erreurs | Programmeurs | L’API REST repose principalement sur des codes d’erreur HTTP pour signaler les erreurs de l’application, tandis que les SDK disposent d’un mécanisme de rapport d’erreurs détaillé qui aide les développeurs d’applications à mieux comprendre ce qui se passe. |
| Récupération des erreurs de l’application (reprise en cas d’erreur, détection de boucle, etc.) | Programmeurs | Les mises en oeuvre de l’API REST doivent créer leurs propres systèmes de récupération des applications, tandis que les mises en oeuvre sur les SDK bénéficient du système de récupération des erreurs des SDK : récupération des erreurs réseau transitoires, en réessayant certains appels réseau avec une logique mise en place pour empêcher les &quot;boucles&quot;. |
| Authentification par demandeur | Programmeurs | Certains MVPD nécessitent une authentification pour chaque site/application, soit pour des raisons commerciales, soit en raison de problèmes techniques. Les SDK appliquent automatiquement cette règle en fonction de la configuration du tableau de bord TVE. Les implémentateurs de l’API REST doivent l’implémenter eux-mêmes, afin de ne pas enfreindre les accords commerciaux ou de pouvoir terminer l’autorisation pour les MVPD qui étendent leurs données d’authentification par application. |
| Authentification passive | Programmeurs | Certains MVPD prennent en charge l’authentification &quot;passive&quot;, où l’utilisateur n’est pas tenu de saisir les informations d’identification, et une tentative d’authentification de l’utilisateur est effectuée automatiquement. Ceci est particulièrement utile pour les MVPD qui ont également une exigence &quot;Authentification par demandeur&quot;. Dans ce cas, l’expérience utilisateur activée par les SDK est transparente, où l’utilisateur s’authentifie une seule fois dans une application, et le SDK effectue une authentification &quot;passive&quot; pour d’autres applications de l’écosystème. L’utilisateur ne voit pas les appels passifs supplémentaires, il sera simplement déjà authentifié, tandis que le MVPD atteint l’objectif d’avoir des sessions d’authentification distinctes pour chaque application. |
| Détection implicite et uniforme de périphériques | Programmeurs | Les SDK détectent automatiquement le type d’appareil et envoient ces informations d’une manière standard. Les mises en oeuvre de l’API REST sont sujettes à l’envoi d’informations différentes en fonction de l’implémentateur, faussant ainsi/rompant les règles commerciales et les statistiques sur tous les sites. **Les programmeurs doivent s&#39;assurer qu&#39;ils nous ont envoyé les informations correctes sur les appareils** sur chacune de leurs applications. Pour les implémentations serveur à serveur, l’api REST n’identifie pas correctement l’adresse IP de l’utilisateur final, sauf si des étapes supplémentaires sont effectuées. L’adresse IP est importante dans certains cas d’utilisation tels que la prévention de la fraude ou l’adaptateur de bus hôte. |
| Tamper BAT TempPass | Programmeurs | L’application de TempPass est basée sur un identifiant d’appareil stable. Les SDK utilisent des techniques d’identification/d’empreinte digitale du matériel pour identifier l’appareil. Ce mécanisme n’est pas public, donc plus sécurisé et sans collision. Pour les implémentations de l’API REST, les programmeurs doivent mettre en oeuvre leurs propres algorithmes pour l’identification/l’empreinte digitale des appareils. |
| Liaison d’appareil | Programmeurs | Les jetons générés sur un appareil avec une application via SDK ne sont pas portables, ce qui rend difficile pour un utilisateur malveillant de partager ses jetons et de permettre l’accès à d’autres utilisateurs. Il est basé sur le même mécanisme d’identifiant d’appareil que &quot;tamper BAT TempPass&quot;. Pour l’API REST, les jetons restent dans le cloud, ce qui signifie que deux appareils avec les mêmes deviceID effectuant les mêmes appels obtiendront les mêmes réponses/accès. Les programmeurs doivent s’assurer qu’ils disposent d’un mécanisme robuste, sécurisé et sans collision pour générer/attribuer des deviceID. |
| Ensemble de fonctionnalités prédicables et certifiées | Programmeurs | L’ensemble existant de fonctionnalités de chaque SDK est cohérent, prévisible, entièrement certifié et testé. Certaines exigences de l’entreprise sont appliquées via les SDK, ce qui rend risqué pour le programmeur de rompre les contrats lors de l’utilisation de l’API REST. Bien que l’API REST puisse être plus flexible, Adobe garantit que les mises en oeuvre du SDK appliquent les décisions commerciales et les paramètres du tableau de bord TVE. Dans le cas de Clientless, chaque programmeur met en oeuvre son propre sous-ensemble de règles de fonctionnement qui suites ses applications à un moment donné. |


1. Dans le cadre de notre nouvelle initiative Une API , nous prévoyons de corriger cette limitation et de pouvoir appliquer des règles par plateforme en fonction de l’identification de l’appareil.

2. Adobe continue de travailler avec toutes les principales plateformes pour mettre en oeuvre la connexion unique à Platform qui peut être utilisée avec notre API REST. Notre initiative d’une seule API offre une prise en charge SSO entre les applications implémentées à l’aide de SDK natifs et des applications implémentées à l’aide de l’API REST.

## Configuration minimale requise pour les périphériques {#min_reqs}

Pour utiliser l’API REST d’authentification Adobe Pass, les périphériques doivent satisfaire ou dépasser les exigences techniques minimales répertoriées dans la section API REST du document [ Plateforme d’authentification Adobe Pass / Exigences en matière d’appareils / outils](#general_clientless_reqs).
