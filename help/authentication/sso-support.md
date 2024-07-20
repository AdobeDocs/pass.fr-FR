---
title: Prise en charge de la connexion unique
description: Prise en charge de la connexion unique
exl-id: edc3719e-c627-464c-9b10-367a425698c6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 1%

---

# Prise en charge de la connexion unique

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview-sso-support}

Ce document décrit les types d’authentification unique pris en charge et optimisés par l’authentification Adobe Pass sur différentes plateformes. La portée de ce document est de mettre en lumière ce qui est pris en charge et ce qui ne l’est pas, quelle est la couverture MVPD pour chaque méthode d’authentification unique et ce qui est requis des programmeurs pour pouvoir bénéficier de l’authentification unique sur chaque plateforme.

Une fois qu’un utilisateur se connecte avec ses informations d’identification MVPD, l’authentification Adobe Pass génère un jeton sécurisé qui représente la session d’authentification du MVPD et lie ce jeton à l’appareil de l’utilisateur à l’aide d’un identifiant de périphérique. L’authentification Adobe Pass stocke le jeton/ID de périphérique sur un serveur ou sur l’appareil. Cela permet aux utilisateurs de saisir leurs informations d’identification moins fréquemment tout en préservant la sécurité des transactions.

>[!NOTE]
>
>Les workflows SSO font partie du package de workflow Premium. Contactez votre représentant commercial Adobe Pass si vous souhaitez utiliser cette fonctionnalité.

## État actuel de la SSO sur diverses plateformes {#current-sso-status-platforms}

| Plateforme / Appareil | Prise en charge SSO | type SSO | Couverture MVPD | Notes |
|:-------------------:|:-----------:|:---------------------------------------:|-----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Web (JavaScript) | Oui | Jeton d’authentification partagé (Adobe SSO) | TOUTES | Aucune authentification unique sur plusieurs navigateurs Veuillez suivre les instructions du Guide d’intégration des programmeurs pour JavaScript. Selon les instructions, SSO est activé par défaut.  L’activation de l’authentification par demandeur interrompt l’authentification unique |
| iOS | Oui | SSO de la plateforme - exchange des jetons | En fonction de la prise en charge d’Apple, la liste se trouve ici. | Depuis iOS 10, Apple et Adobe ont introduit la fonctionnalité SSO pour les programmeurs et les MVPD participants. En utilisant le dernier SDK iOS Adobe ou en utilisant l’API REST sans client d’Adobe et en mettant en oeuvre la fonctionnalité SSO d’Apple, vous pouvez bénéficier de la connexion unique sur les appareils iOS. Plus d’informations sur l’implémentation du SDK ici et plus d’informations sur l’implémentation sans client ici. Remarques supplémentaires : - Si vous ne souhaitez pas utiliser la fonction SSO d’Apple, vous pouvez toujours avoir une fonction SSO limitée entre les applications du même fournisseur (même ID de lot) qui peuvent partager du stockage et un ID (IDFV). Par conséquent, la fonction SSO est limitée uniquement aux applications du même fournisseur. |
| Android | Oui | Jeton d’authentification partagé (Adobe SSO) | TOUTES | Si l’utilisateur n’accepte pas la demande d’autorisation WRITE_EXTERNAL_STORAGE, la bibliothèque utilise un espace de stockage local avec sandbox. Dans ce cas, il n’y aura pas d’authentification unique entre les différentes applications lors de l’utilisation du stockage local. |
| tvOS - nouvelle Apple TV | Oui | SSO de la plateforme - exchange des jetons | En fonction de la prise en charge d’Apple, la liste se trouve ici. | Depuis tvOS 10, Apple et Adobe ont introduit la fonctionnalité SSO pour les programmeurs et MVPD participants. En utilisant le dernier SDK tvOS d’Adobe ou en utilisant l’API REST sans client d’Adobe et en mettant en oeuvre la fonctionnalité SSO d’Apple, vous pouvez bénéficier de la connexion unique sur les appareils tvOS. Plus de détails sur le SDK tvOS : ici et ici et plus de détails sur la mise en oeuvre sans client ici. |
| Roku | Oui | Jeton d’authentification partagé (Adobe SSO) | Une liste complète de couverture importante sera bientôt fournie. | La fonction SSO Roku est prête à l’emploi avec l’API sans client pour tous les clients, en respectant les instructions de Roku. Aucune mise en oeuvre spéciale n’est requise. La connexion unique est basée sur les informations d’identification de périphérique que Roku envoie en toute sécurité à Adobe. |
| Amazon FireTV | Oui | Jeton d’authentification partagé (Adobe SSO) | Une liste complète de couverture importante sera bientôt fournie. | Le SDK FireTV prend en charge l’authentification unique en fonction des fonctionnalités d’Android. La connexion unique sur cette plateforme n’est possible qu’entre les applications qui utilisent pour l’instant le SDK FireTV Adobe. Plus d’informations sur le nouveau SDK FireTV ici. Les applications FireTV mises en oeuvre sur l’API sans client pourront bénéficier de la connexion unique d’ici à EOY 2018. |
| Xbox 360 | Non |                                         |                                                     | Il n’existe aucun identifiant d’appareil que nous pouvons exploiter. Il existe un ID d’application, de sorte que les utilisateurs n’ont pas à s’authentifier à chaque fois. |
| Xbox | Non |                                         |                                                     | Il n’existe aucun identifiant d’appareil que nous pouvons exploiter. Il existe un ID d’application, de sorte que les utilisateurs n’ont pas à s’authentifier à chaque fois. |
| Windows 8/10 | Non |                                         |                                                     | Il n’existe aucun identifiant d’appareil que nous pouvons exploiter. Il existe un ID d’application, de sorte que les utilisateurs n’ont pas à s’authentifier à chaque fois. |
| Télévisions Samsung | Non |                                         |                                                     | Il n’existe aucun identifiant d’appareil que nous pouvons exploiter. Il existe un ID d’application, de sorte que les utilisateurs n’ont pas à s’authentifier à chaque fois. |

### Remarques sur Xbox 360 et Xbox One {#notes-xbox-360}

* **Xbox 360** - Xbox 360 s’appuie sur le Live Service pour fournir le jeton qui incorpore l’ID de périphérique. Les calques Live Service dans la valeur appID pour deviceID, ce qui le limite à l’application. Pour Xbox 360, Microsoft a fourni à Adobe une bibliothèque Java pour vous aider à analyser le jeton.

* **Xbox One** - Un jeton web JSON est émis qui est chiffré avec le certificat/la clé de l’éditeur et signé par Microsoft. Adobe extrait l’deviceID d’un paramètre appelé DPI (Device Pairwise ID), différent du paramètre Xbox 360 PDID (Partner Device ID). Le PDID existe également sur Xbox One, mais il est censé être remplacé par ce nouveau paramètre &quot;Device Pairwise ID&quot; (identifiant par paires de périphérique).


### Désactivation de l’authentification unique {#disable-sso}

Dans certains cas, certaines applications ou certains sites voudront désactiver l’authentification unique pour répondre aux demandes d’analyse avancées.

* **Pour les SDK JS et natifs** - L’équipe d’assistance de l’authentification Adobe Pass peut désactiver l’authentification unique pour une paire ID/MVPD du demandeur. Aucun travail n’est nécessaire sur les sites ou dans les applications natives.  Une fois l’authentification unique désactivée par l’équipe de prise en charge de l’authentification Adobe Pass, les authentifications effectuées à l’aide de la paire RequestorId / MVPD spécifiée ne seront pas partagées avec des sites ou des applications utilisant des identifiants de demandeur différents. En outre, les authentifications existantes avec différents ID de demandeur ne seront pas valides pour la combinaison ID de demandeur/MVPD dans laquelle SSO a été désactivé. Techniquement, la désactivation de l’authentification unique est effectuée en liant le jeton AuthN à la combinaison ID/MVPD du demandeur spécifique.
* **Pour l’API sans client** - Vous pouvez désactiver la connexion unique dans le flux d’authentification sans client en spécifiant un paramètre appId non vide dans les appels REST. Vous pouvez utiliser n’importe quelle chaîne comme valeur, à condition que cette chaîne soit unique pour l’identifiant du demandeur. Notez que pour l’API sans client, le programmeur/implémentateur doit modifier le site ou l’application pour ajouter ce paramètre spécifique au demandeur.

>[!IMPORTANT]
>
>REMARQUE IMPORTANTE POUR LA SSO DE L’API SANS CLIENT : certains MVPD exigent que chaque réseau (ID de demandeur) exécute son propre flux d’authentification. Pour les flux basés sur le SDK (iOS, etc.), ceci est géré automatiquement par le SDK. Toutefois, pour les API sans client, cela doit être géré par le programmeur. Nous conseillons vivement aux programmeurs de ne pas activer les flux d’authentification unique pour les API sans client à ce stade et d’utiliser plutôt une combinaison ID d’appareil + ID d’application pour l’ID d’appareil. Adobe s’efforcera également d’améliorer les flux d’API sans client afin d’établir une authentification unique correcte.

### Déconnexion {#logout-sso-support}

Les programmeurs doivent savoir que l’action &quot;Déconnexion&quot; dans le contexte de l’authentification unique, lorsqu’elle est effectuée dans une application/sur un site, supprime tous les jetons de l’appareil et l’utilisateur est déconnecté sur plusieurs applications/sites.

Si les conditions d’authentification unique sont remplies (que l’authentification unique soit activée ou désactivée), la déconnexion est effectuée et toutes les informations d’authentification et d’autorisation sont supprimées.
