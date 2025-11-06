---
title: Présentation de l’API REST
description: Présentation des API REST
exl-id: 5533d852-f644-417e-bf80-6f7aa1edd6b2
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1635'
ht-degree: 0%

---

# Présentation de l’API REST (héritée) {#rest-api-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Vue d’ensemble {#over}

L’API REST d’authentification Adobe Pass fournit un accès direct aux services d’authentification et d’autorisation TV Everywhere (TVE). Cette API prend en charge deux architectures principales : les applications serveur à serveur ou les appareils connectés (par exemple, consoles de jeux, téléviseurs intelligents, décodeurs, etc.) qui ne disposent pas de fonctionnalités de navigation web.

### Mécanisme de limitation

L’API REST d’authentification Adobe Pass est régie par un [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).


### Serveur à serveur

Les solutions serveur à serveur impliquent des applications clientes Programmer qui s’intègrent aux services Programmer qui se connectent aux services d’authentification Adobe Pass pour les flux TVE. Cette approche déplace la majeure partie de l’implémentation de TVE du client vers le serveur, où un seul module d’autorisation unifié peut être créé et géré. La principale responsabilité restante des applications clientes est la gestion d’une vue web pour l’authentification des utilisateurs.



### Appareils connectés

Les applications Appareils connectés communiquent directement avec l’authentification Adobe Pass par le biais des API REST pour effectuer la configuration, l’enregistrement, les contrôles de statut d’authentification et les flux d’autorisation, tandis qu’une application à deuxième écran (navigateur) est requise pour le flux d’authentification. Par conséquent, les SDK natifs ne sont pas utilisés.



### Autres architectures

Outre les deux architectures principales basées sur l’API REST, les solutions serveur à serveur et client direct pour les appareils intelligents, il existe d’autres architectures.  Le Principal d’entre eux est l’architecture SDK, qui utilise un composant client appelé Activateur d’accès que l’authentification Adobe Pass fournit aux programmeurs.  L’application utilise les API Access Enabler pour gérer le démarrage, l’authentification, l’autorisation et la déconnexion.  Toutes les communications entre l’application du programmeur et les serveurs d’authentification d’Adobe Pass passent par Access Enabler.  Une autre version d’Access Enabler est disponible pour les plateformes suivantes : JavaScript, iOS, tvOS, Android et FireTV.

Bien qu’il soit possible d’utiliser l’API REST directement sur des plateformes clientes qui prennent en charge les SDK natifs en dehors d’une solution serveur à serveur, cette approche n’est pas recommandée.



## Avantages et inconvénients de l’API REST {#ProsAndCons}

L’API REST d’authentification Adobe Pass a été créée pour fournir une solution TV Everywhere (TVE) pour les appareils qui ne disposent pas de fonctionnalités de navigation web ou de stockage persistant. L’API REST prend en charge tous les flux d’authentification et d’autorisation, mais il lui manque un composant SDK natif. Les SDK fournis et gérés par l’authentification Adobe Pass sont dotés de fonctionnalités prêtes à l’emploi qui implémentent des règles métier qui, dans le cas de l’API REST, doivent être implémentées et gérées par les programmeurs. Dans le tableau des responsabilités du programmeur ci-dessous, nous décrivons les limites de l’API REST actuelle qui doivent être traitées par les programmeurs.



### Avantages et inconvénients de serveur à serveur par rapport à client

Une architecture serveur à serveur permet de consolider la majeure partie de la logique liée à l’authentification et à l’autorisation en une seule unité logique ou implémentation.  Cette approche a des avantages et des inconvénients.  Les avantages sont les suivants :

* Implémentation unique pour la logique commerciale d’authentification et d’autorisation.
* Évitez d’implémenter cette logique sur chaque plateforme prise en charge à l’aide des outils natifs de cette plateforme.
* Possibilité de mettre à jour les fonctionnalités sans avoir à mettre à jour les clients avec toutes les exigences associées (par exemple, mises à jour de la boutique d’applications).
* **Plus facilement** étendez et personnalisez les fonctionnalités authN et authZ (par exemple, ajoutez D2C).
* Gestion directe du trafic associé pour un contrôle, une qualité et une surveillance accrus.



Là encore, les inconvénients sont énumérés dans les responsabilités du programmeur, mais incluent les éléments suivants :

* La connexion unique doit être mise en œuvre pour chaque client pour les plateformes sans connexion unique de Platform.
* Les programmeurs doivent implémenter une logique spécifique à MVPD si nécessaire.
* Toutes les plateformes qui utilisent l’API REST partagent une configuration unique régissant les propriétés telles que les TTL d’authentification.



### Appareils connectés

Pour la plupart des appareils connectés, l’API REST doit être utilisée d’une manière ou d’une autre, car un SDK n’est pas disponible. L’appareil connecté utilisera directement l’API REST ou s’intégrera à une solution serveur à serveur qui utilise l’API REST.

## Responsabilités du programmeur {#programmer-responsibilities}

Les éléments suivants s’appliquent à la fois aux applications serveur à serveur et aux applications d’appareil connecté.

| **fonctionnalité** | **Responsable de la gestion de la fonctionnalité** | **Description des limites de l’API cliente actuelle et des différences par rapport aux SDK natifs** |
| --- | --- | --- |
| Paramètres de configuration appliqués par plateforme | Adobe | Une **limitation majeure** de l’utilisation de l’API REST sur toutes les plateformes (y compris les appareils mobiles tels qu’iOS et Android) est que les paramètres de configuration correspondant à l’API REST dans notre outil de configuration de tableau de bord TVE sont appliqués à tous les appareils (même s’il existe un appareil iOS qui exécute une application native implémentée sur notre API REST). Cette limitation **peut rompre** les TTL convenues et les paramètres de plateforme convenus avec les MVPD, si ceux-ci diffèrent pour chaque plateforme. [1](#1) |
| Authentification Single Sign-On | Programmeurs | En utilisant l’API REST, la connexion unique est disponible uniquement sur les plateformes qui prennent en charge la connexion unique de la plateforme (par exemple, Apple, Roku, Amazon), tandis que la connexion unique ne peut pas être garantie pour les autres plateformes lors de l’utilisation de l’API REST. Les SDK mettent les données en cache de manière intersite/application. Cela signifie que l’utilisateur se connecte une seule fois sur un site/une application et qu’il est déjà connecté sur les sites participants, sans qu’aucune interaction utilisateur ne soit nécessaire. [2](#2) |
| Déconnexion unique | Programmeurs | Dans un scénario SSO natif de SDK, la déconnexion d’une application participante déconnecte l’utilisateur où qu’il se trouve. Sur l’API REST actuelle, nous ne prenons pas en charge les SLO. La déconnexion d’une application entraîne la déconnexion de l’utilisateur uniquement pour cette application spécifique. |
| Mise en cache | Programmeurs | Les implémentations de l’API REST devront mettre en œuvre leur propre mécanisme de mise en cache pour les éléments de données approuvés par l’entreprise. Les SDK mettent automatiquement en cache divers éléments de données, tout en prenant en compte diverses règles métier. Par exemple, les métadonnées de l’utilisateur sont mises en cache avec la même TTL que le jeton d’authentification, tandis que certains éléments peuvent être exclus de la mise en cache par programmation (contrôle en amont). |
| Mécanisme de rapport d’erreurs détaillé | Programmeurs | L’API REST repose principalement sur les codes d’erreur HTTP pour signaler les erreurs d’application, tandis que les SDK disposent d’un mécanisme de rapport d’erreur détaillé qui aide les développeurs d’applications à mieux comprendre ce qui se passe. |
| Récupération des erreurs de l’application (reprise en cas d’erreur, détection de boucle, etc.) | Programmeurs | Les implémentations de l’API REST doivent créer leurs propres systèmes de récupération d’application, tandis que les implémentations qui s’ajoutent aux SDK bénéficient du système de récupération des erreurs des SDK : récupération après des erreurs réseau transitoires, en réessayant certains appels réseau avec une logique en place pour éviter les « boucles ». |
| Authentification par demandeur | Programmeurs | Certains MVPD nécessitent une authentification pour chaque site/application, soit pour des raisons commerciales, soit en raison de défis techniques. Les SDK l’appliquent automatiquement en fonction de la configuration du tableau de bord TVE. Les personnes en charge de la mise en œuvre de l’API REST doivent la mettre en œuvre elles-mêmes, afin de ne pas enfreindre les accords commerciaux ou de pouvoir terminer l’autorisation pour les MVPD qui définissent la portée de leurs données d’authentification par application. |
| Authentification passive | Programmeurs | Certains fichiers MVPD prennent en charge l’authentification « passive », où l’utilisateur n’est pas tenu de saisir les informations d’identification et où une tentative d’authentification de l’utilisateur est effectuée automatiquement. Cela s’avère particulièrement utile pour les MVPD qui ont également comme exigence « Authentification par demandeur ». Dans ce cas, l’expérience utilisateur activée par les SDK est transparente, où l’utilisateur ne s’authentifie qu’une seule fois dans une application et où le SDK effectue une authentification « passive » pour d’autres applications de l’écosystème. L’utilisateur ne voit pas les appels passifs supplémentaires, il sera simplement déjà authentifié, tandis que le MVPD atteint l’objectif d’avoir des sessions d’authentification distinctes pour chaque application. |
| Détection implicite et uniforme des dispositifs | Programmeurs | Les SDK détectent automatiquement le type d’appareil et envoient ces informations de manière standard. Les implémentations de l’API REST sont susceptibles d’envoyer des informations différentes en fonction de l’implémentateur, ce qui fausse les règles métier et les statistiques sur l’ensemble des sites. **Les programmeurs doivent s&#39;assurer qu&#39;ils nous ont envoyé des informations correctes sur les appareils** sur chacune de leurs applications. Pour les implémentations serveur à serveur, l’api REST n’identifie pas correctement l’adresse IP de l’utilisateur final, sauf si d’autres étapes sont effectuées. L’adresse IP est importante dans certains cas d’utilisation, tels que la prévention des fraudes ou les adaptateurs HBA. |
| TempPass inviolable | Programmeurs | L’application de TempPass repose sur un identifiant d’appareil stable. Les SDK utilisent des techniques d’information/d’empreinte digitale pour identifier le périphérique. Ce mécanisme n’est pas public, et est donc plus sécurisé et sans collision. Pour les implémentations d’API REST, les programmeurs doivent mettre en œuvre leurs propres algorithmes d’identification/d’empreinte numérique de l’appareil. |
| Liaison de l’appareil | Programmeurs | Les jetons générés sur un appareil avec une application sur SDK ne sont pas portables, ce qui rend difficile pour un utilisateur malveillant de partager ses jetons et de fournir l’accès à d’autres utilisateurs. Celui-ci est basé sur le même mécanisme d’identifiant d’appareil que le « TempPass inviolable ». Pour l’API REST, les jetons restent dans le cloud, ce qui signifie que deux appareils avec les mêmes ID d’appareil effectuant les mêmes appels obtiendront les mêmes réponses/accès. Les programmeurs doivent s&#39;assurer qu&#39;ils disposent d&#39;un mécanisme robuste, sécurisé et sans collision pour générer/attribuer des deviceID. |
| Un ensemble de fonctionnalités prévisibles et certifiées | Programmeurs | L’ensemble des fonctionnalités de chaque SDK est cohérent, prévisible, entièrement certifié et testé. Certaines exigences de l’entreprise sont appliquées via les SDK, ce qui fait qu’il est risqué pour le programmeur de rompre les contrats lors de l’utilisation de l’API REST. Bien que l’API REST puisse être plus flexible, Adobe garantit que les implémentations de SDK appliquent les décisions et paramètres métier depuis le tableau de bord TVE. Dans le cas de l’absence de client, chaque programmeur met en œuvre son propre sous-ensemble de règles métier qui s’adapte à ses applications à un moment donné. |


1. Dans le cadre de notre nouvelle initiative One API , nous prévoyons de corriger cette limitation et de pouvoir appliquer des règles par plateforme en fonction de l’identification de l’appareil.

2. Adobe continue de travailler avec toutes les principales plateformes pour implémenter l’authentification unique de Platform qui peut être utilisée avec notre API REST. Notre initiative One API offrira une prise en charge de l’authentification unique entre les applications implémentées à l’aide de SDK natifs et les applications implémentées à l’aide de l’API REST.

## Configuration Minimale Requise Pour Les Appareils {#min_reqs}

Pour utiliser l’API REST d’authentification Adobe Pass, les appareils doivent respecter ou dépasser les exigences techniques minimales répertoriées dans la section API REST du document [Exigences en matière de plateforme d’authentification Adobe Pass/appareil/outils](#general_clientless_reqs).
