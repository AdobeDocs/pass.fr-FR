---
title: Prise en charge de l’authentification SSO
description: Prise en charge de l’authentification SSO
exl-id: edc3719e-c627-464c-9b10-367a425698c6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 1%

---

# (Hérité) Prise En Charge De L’Authentification Unique

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Vue d’ensemble {#overview-sso-support}

Ce document décrit les types d’authentification unique pris en charge et optimisés par l’authentification Adobe Pass sur différentes plateformes. Le but de ce document est de faire la lumière sur ce qui est pris en charge et ce qui ne l&#39;est pas, sur la couverture MVPD de chaque méthode de connexion unique et sur ce qui est requis des programmeurs pour pouvoir bénéficier de la connexion unique sur chaque plateforme.

Une fois qu’un utilisateur se connecte avec ses informations d’identification MVPD, l’authentification Adobe Pass génère un jeton sécurisé qui représente la session d’authentification MVPD et lie ce jeton à l’appareil de l’utilisateur à l’aide d’un identifiant d’appareil. L’authentification Adobe Pass stocke le jeton/l’ID d’appareil sur un serveur ou sur l’appareil. Cela permet aux utilisateurs de saisir leurs informations d’identification moins fréquemment tout en maintenant la sécurité des transactions.

>[!NOTE]
>
>Les workflows SSO font partie du package de workflow Premium . Contactez votre représentant Adobe Pass si vous souhaitez utiliser cette fonctionnalité.

## Statut actuel de la fonction SSO sur différentes plateformes {#current-sso-status-platforms}

| Plateforme/Appareil | Prise en charge de la fonction SSO | Type de SSO | Couverture MVPD | Notes |
|:-------------------:|:-----------:|:---------------------------------------:|-----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Web (JavaScript) | Oui | Jeton d’authentification partagé (SSO Adobe) | TOUTES | Aucune authentification unique (SSO) entre les navigateurs Veuillez suivre les instructions du Guide d’intégration du programmeur pour JavaScript. Si vous suivez les instructions, la fonction SSO est activée par défaut.  L’activation de l’authentification par demandeur interrompt l’authentification unique |
| iOS | Oui | SSO de Platform - exchange du jeton | Selon la prise en charge d’Apple - la liste est disponible ici | À partir d’iOS 10, Apple et Adobe ont introduit la fonctionnalité SSO pour les programmeurs et les MVPD participants. En utilisant la dernière version d’Adobe iOS SDK ou l’API REST sans client d’Adobe et en implémentant la fonctionnalité de connexion unique d’Apple, vous pouvez bénéficier de la connexion unique sur les appareils iOS. Vous trouverez plus d’informations sur l’implémentation de SDK ici et plus d’informations sur l’implémentation sans client ici. Remarques supplémentaires : - Si vous ne souhaitez pas utiliser l’authentification unique Apple, vous pouvez toujours avoir une authentification unique limitée entre les applications du même fournisseur (même ID de lot) qui peuvent partager le stockage et un identifiant (IDFV). Par conséquent, l’authentification unique est limitée uniquement aux applications du même fournisseur. |
| Android | Oui | Jeton d’authentification partagé (SSO Adobe) | TOUTES | Si l’utilisateur n’accepte pas la demande d’autorisation WRITE_EXTERNAL_STORAGE, la bibliothèque utilise un espace de stockage en sandbox local. Dans ce cas, cela signifie qu’il n’y aura pas de connexion unique entre les différentes applications lors de l’utilisation du stockage local. |
| tvOS - nouveau Apple TV | Oui | SSO de Platform - exchange du jeton | Selon la prise en charge d’Apple - la liste est disponible ici | À partir de tvOS 10, Apple et Adobe ont introduit la fonctionnalité SSO pour les programmeurs et les MVPD participants. En utilisant le dernier SDK tvOS d’Adobe ou l’API REST sans client d’Adobe et en implémentant la fonctionnalité SSO d’Apple, vous pouvez bénéficier de la connexion unique sur les appareils tvOS. Plus d’informations sur tvOS SDK : ici et ici et plus d’informations sur l’implémentation sans client ici. |
| Roku | Oui | Jeton d’authentification partagé (SSO Adobe) | Une liste complète de couverture significative sera bientôt fournie. | L’authentification unique Roku est prête à l’emploi avec l’API cliente pour tous les clients en respectant les directives Roku, aucune implémentation spéciale n’est requise. La fonction SSO repose sur les informations d’identification d’appareil que Roku envoie en toute sécurité à l’Adobe. |
| Amazon FireTV | Oui | Jeton d’authentification partagé (SSO Adobe) | Une liste complète de couverture significative sera bientôt fournie. | FireTV SDK prend en charge l’authentification unique en fonction des fonctionnalités d’Android. La connexion unique sur cette plateforme est possible uniquement entre les applications qui utilisent actuellement Adobe FireTV SDK. Plus d&#39;infos sur le nouveau FireTV SDK ici. Les applications FireTV mises en œuvre sur des API sans client pourront bénéficier de la fonction SSO d&#39;ici 2018. |
| Xbox 360 | Non |                                         |                                                     | Aucun ID d’appareil n’est disponible. Il existe un ID d’application, de sorte que les utilisateurs n’ont pas à s’authentifier à chaque fois. |
| Xbox One | Non |                                         |                                                     | Aucun ID d’appareil n’est disponible. Il existe un ID d’application, de sorte que les utilisateurs n’ont pas à s’authentifier à chaque fois. |
| Windows 8/10 | Non |                                         |                                                     | Aucun ID d’appareil n’est disponible. Il existe un ID d’application, de sorte que les utilisateurs n’ont pas à s’authentifier à chaque fois. |
| Téléviseurs Samsung | Non |                                         |                                                     | Aucun ID d’appareil n’est disponible. Il existe un ID d’application, de sorte que les utilisateurs n’ont pas à s’authentifier à chaque fois. |

### Notes sur Xbox 360 et Xbox One {#notes-xbox-360}

* **Xbox 360**- Xbox 360 s&#39;appuie sur le service Live pour fournir le jeton qui intègre deviceID. Les calques Live Service dans la valeur appID pour deviceID, ce qui le rend accessible uniquement à l’application. Pour Xbox 360, Microsoft a fourni à Adobe une bibliothèque Java pour faciliter l’analyse du jeton.

* **Xbox One**- Un jeton Web JSON est émis, chiffré avec le certificat/la clé de l’éditeur et signé par Microsoft. Adobe extrait deviceID à partir d&#39;un paramètre appelé DPI (Device Pairwise ID), différent du paramètre PDID (Partner Device ID) de la Xbox 360. Le PDID existe également dans Xbox One, mais il est censé être remplacé par ce nouveau paramètre « Identifiant par paires d&#39;appareils » (DPI).


### Désactivation de l’authentification unique {#disable-sso}

Dans certains cas, certaines applications ou certains sites souhaitent désactiver l’authentification unique pour répondre à des cas d’utilisation avancés.

* **Pour les SDK JS et natifs** - L’équipe d’assistance de l’authentification Adobe Pass peut désactiver l’authentification unique pour une paire ID de demandeur/MVPD. Aucun travail n’est nécessaire sur les sites ou dans les applications natives.  Une fois la connexion unique désactivée par l’équipe de support de l’authentification Adobe Pass, les authentifications effectuées à l’aide de la paire RequestorId/MVPD spécifiée ne seront pas partagées avec des sites ou des applications utilisant des ID de demandeur différents. En outre, les authentifications existantes avec des ID de demandeur différents ne seront pas valides pour la combinaison ID de demandeur/MVPD dans laquelle l’authentification unique a été désactivée. Techniquement, la désactivation de l’authentification unique est effectuée en liant le jeton AuthN à la combinaison ID du demandeur/MVPD spécifique.
* **Pour l’API sans client** - Vous pouvez désactiver l’authentification unique dans le flux d’authentification sans client en spécifiant un paramètre appId non vide dans les appels REST. Vous pouvez utiliser n’importe quelle chaîne comme valeur, à condition que cette chaîne soit unique pour l’ID du demandeur. Notez que pour l’API sans client, le programmeur/implémentateur doit modifier le site ou l’application pour ajouter ce paramètre spécifique au demandeur.

>[!IMPORTANT]
>
>REMARQUE IMPORTANTE CONCERNANT L’AUTHENTIFICATION UNIQUE DE L’API CLIENTLESS : certains MVPD exigent que chaque réseau (ID du demandeur) effectue son propre flux d’authentification. Pour les flux basés sur SDK (iOS, etc.), cela est géré automatiquement par le SDK. Toutefois, pour les API sans client, cela doit être géré par le programmeur. Nous conseillons vivement aux programmeurs de ne pas activer les flux SSO pour les API sans client à ce stade et d’utiliser plutôt une combinaison identifiant de l’appareil + identifiant de l’application pour l’identifiant de l’appareil. L’Adobe travaillera également à l’amélioration des flux d’API sans client afin que la connexion unique appropriée puisse être établie.

### Déconnexion {#logout-sso-support}

Les programmeurs doivent savoir que l&#39;action « Déconnexion » dans le contexte de l&#39;authentification SSO, lorsqu&#39;elle est effectuée dans une application/sur un site, supprimera tous les jetons sur l&#39;appareil et l&#39;utilisateur sera déconnecté sur plusieurs applications/sites.

Si les conditions SSO sont remplies (que SSO soit activé ou désactivé ou non), la déconnexion sera effectuée et toutes les informations d&#39;authentification et d&#39;autorisation seront supprimées.
