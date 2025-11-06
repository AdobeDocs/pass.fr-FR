---
title: Rapports
description: Découvrez comment les données sont agrégées dans les rapports du tableau de bord TVE.
exl-id: d8ba48de-d743-4dc2-866c-7d6e3ff94773
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Rapports {#Reports}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

La section **Rapports** du tableau de bord TVE permet d’accéder aux données agrégées pour les rapports AuthN TTL, AuthZ TTL et SSO. Ces rapports incluent vos intégrations de canaux avec différents MVPD sur toutes les [ plateformes ](#platforms).

Les rapports vous permettent de filtrer les données et de collecter des informations sur [des canaux ou MVPD spécifiques](#selecting-specific-channels-mvpds). Vous pouvez également exporter des rapports dans un fichier CSV pour une analyse plus approfondie.

## Afficher les rapports {#view-reports}

Pour afficher un rapport spécifique, procédez comme suit.

1. Sélectionnez l’onglet **Rapports** dans le panneau de gauche.
1. Sélectionnez l’un des onglets suivants pour afficher et exporter les données agrégées des canaux et MVPD inclus :
   * [Rapports de durée de vie AuthN](#authn-ttl-reports)
   * [Rapports de durée de vie AuthZ](#authz-ttl-reports)
   * [Rapports SSO](#sso-reports)

   ![Type de rapports](../assets/tve-dashboard/new-tve-dashboard/reports/reports-tabs-view.png)

   *Type de rapports*

### Rapports de durée de vie AuthN {#authn-ttl-reports}

Les rapports AuthN TTL, également appelés Durée de vie (TTL) de l’authentification, affichent la durée pendant laquelle les jetons d’authentification sont configurés pour vos intégrations de canaux avec divers MVPD sur toutes les [plateformes](#platforms). Ces rapports vous permettent d’examiner la durée d’authentification d’un utilisateur pour un MVPD et une plateforme spécifiques. Les valeurs de durée sont présentées dans des formats conviviaux tels que **jours**, **heures**, **minutes** et **secondes**. Le tableau Rapports de durée de vie AuthN dispose d’un défilement horizontal et vertical pour s’adapter à différentes tailles d’écran.

Vous pouvez également afficher et télécharger des données pour [des canaux spécifiques ou des MVPD](#selecting-specific-channels-mvpds).

![Exporter des rapports de durée de vie AuthN](../assets/tve-dashboard/new-tve-dashboard/reports/reports-authn-ttl-export-button.png)

*Exporter des rapports de durée de vie AuthN*

>[!IMPORTANT]
>
> L’espace réservé **Défini par MVPD** est utilisé lorsque le MVPD applique la valeur de TTL AuthN plutôt que la configuration d’authentification Adobe Pass.

Sélectionnez **Exporter des rapports** pour enregistrer les données au format CSV sur votre ordinateur local.

### Rapports TTL AuthZ {#authz-ttl-reports}

Les rapports de durée de vie (TTL) AuthZ, également appelés durée de vie (TTL) de l’autorisation, affichent la durée du jeton d’autorisation configuré pour vos intégrations de canaux avec divers MVPD sur toutes les [plateformes](#platforms). Ces rapports vous permettent d’examiner la durée pendant laquelle un utilisateur reste autorisé à surveiller le contenu d’un MVPD et d’une plateforme spécifiques. Les valeurs de durée sont présentées dans des formats conviviaux tels que **jours**, **heures**, **minutes** et **secondes**. Le tableau Rapports de durée de vie AuthZ comporte un défilement horizontal et vertical pour s’adapter à différentes tailles d’écran.

Vous pouvez également afficher et télécharger les données pour [des canaux spécifiques ou des MVPD](#selecting-specific-channels-mvpds).

![Exporter des rapports de durée de vie AuthZ](../assets/tve-dashboard/new-tve-dashboard/reports/reports-authz-ttl-export-button.png)

*Exporter des rapports de durée de vie AuthZ*

>[!IMPORTANT]
>
> L’espace réservé **Défini par MVPD** est utilisé lorsque le MVPD applique la valeur de TTL AuthZ plutôt que la configuration d’authentification Adobe Pass.

Sélectionnez **Exporter des rapports** pour enregistrer les données au format CSV sur votre ordinateur local.

### Rapports SSO {#sso-reports}

Les rapports SSO, également appelés authentification unique, affichent le statut d’authentification unique configuré pour vos intégrations de canaux avec divers MVPD sur toutes les [plateformes](#platforms). Ces rapports vous permettent d’examiner l’expérience SSO d’authentification de l’utilisateur attendue pour un MVPD et une plateforme spécifiques. Les valeurs sont présentées dans des formats conviviaux tels que **SSO désactivée**, **SSO activée** et **SSO incertaine**. Le tableau Rapports SSO comporte un défilement horizontal et vertical pour s’adapter à différentes tailles d’écran.

Vous pouvez également afficher et télécharger des données pour [des canaux spécifiques ou des MVPD](#selecting-specific-channels-mvpds).

![Exporter des rapports SSO](../assets/tve-dashboard/new-tve-dashboard/reports/reports-sso-export-button.png)

*Exporter des rapports SSO*

>[!IMPORTANT]
>
> L’espace réservé **SSO incertaine** indique que l’authentification unique (SSO) est activée et potentiellement opérationnelle. Toutefois, les paramètres répertoriés ci-dessous peuvent inhiber l’authentification SSO, comme expliqué dans les exemples suivants :
>
> * Paramètres de la plateforme utilisateur : la possibilité de bloquer les cookies tiers.
> * Décisions des utilisateurs : les utilisateurs refusent à la plateforme l’accès à leur abonnement de fournisseur de télévision.
> * Paramètres de MVPD : MVPD demande une authentification pour chaque canal.

Sélectionnez **Exporter des rapports** pour enregistrer les données au format CSV sur votre ordinateur local.

## Plateformes {#platforms}

Les rapports [AuthN TTL](#authn-ttl-reports), [AuthZ TTL Reports](#authz-ttl-reports) et [SSO Reports](#sso-reports) présentent des données sur différentes plateformes, telles que :

* **Desktop** : affiche les valeurs appliquées aux implémentations du programmeur via le SDK Adobe Pass Authentication JavaScript.

* **Mobile**

  **iOS** : affiche les valeurs appliquées à l’aide de l’authentification Adobe Pass iOS SDK.

  **Android** : affiche les valeurs appliquées par l’intermédiaire de l’authentification Adobe Pass Android SDK.

  **Autres** : affiche les valeurs appliquées à l’aide de l’API REST d’authentification Adobe Pass développée pour les appareils mobiles.

* **TVCD**

  **Roku** : affiche les valeurs appliquées via l’API REST d’authentification Adobe Pass, identifiant Roku comme type d’appareil.

  **FireTV** : affiche les valeurs appliquées par le biais du SDK FireTV d’authentification Adobe Pass.

  **AppleTV** : affiche les valeurs appliquées via le SDK tvOS d’authentification Adobe Pass.

  **Autres** : affiche les valeurs appliquées à l’aide de l’API REST d’authentification Adobe Pass pour les appareils connectés à la télévision.

* **Plateforme non identifiée** : affiche les valeurs appliquées aux implémentations du programmeur lorsque les services d’authentification d’Adobe Pass détectent un type d’appareil inconnu.

Pour en savoir plus sur le partage du type d’appareil souhaité, tel que **Roku** avec les API REST d’authentification Adobe Pass ou les SDK, consultez le mécanisme de [transmission des informations client](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

>[!IMPORTANT]
>
> Les données agrégées sont basées sur la configuration spécifique de chaque environnement d’authentification Adobe Pass. Lorsque vous basculez entre différents environnements de tableaux de bord TVE, attendez-vous à des variations dans les données entre les rapports. Pour en savoir plus[ consultez ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)Environnements d’authentification Adobe Pass .

## Sélection de canaux et de MVPD spécifiques {#selecting-specific-channels-mvpds}

Les rapports [AuthN TTL](#authn-ttl-reports), [AuthZ TTL Reports](#authz-ttl-reports) et [SSO Reports](#sso-reports) présentent par défaut des données pour les intégrations **Tous les canaux** avec **Tous les MVPD**.

>[!NOTE]
>
> Si vous désélectionnez **Tous les canaux** ou **Tous les MVPD** dans les menus déroulants respectifs, un message s’affiche pour effectuer une sélection afin d’afficher des rapports significatifs.

Pour générer un rapport pour des canaux spécifiques, procédez comme suit :

1. Sélectionnez le menu déroulant **Canaux inclus** en haut du rapport sélectionné.

   ![Menu déroulant Canaux inclus](../assets/tve-dashboard/new-tve-dashboard/reports/reports-included-channels-menu.png)

   *Menu déroulant Canaux inclus*

1. Désélectionnez **Tous les canaux**.

1. Sélectionnez les canaux requis pour lesquels vous souhaitez générer des données dans le menu déroulant **Canaux inclus**.

>[!NOTE]
>
> Pour que les options soient disponibles dans le menu déroulant **MVPD inclus**, vous devez sélectionner au moins un canal dans le menu déroulant **Canaux inclus**.

Pour générer un rapport pour des fichiers MVPD spécifiques, procédez comme suit :

1. Sélectionnez le menu déroulant **MVPD inclus** en haut du rapport sélectionné.

   ![Menu déroulant MVPD inclus](../assets/tve-dashboard/new-tve-dashboard/reports/reports-included-mvpds-menu.png)

   *Menu déroulant MVPD inclus*

1. Désélectionnez **Tous les fichiers MVPD**.

1. Sélectionnez les fichiers MVPD requis dans le menu déroulant **MVPD inclus** pour lesquels vous souhaitez générer des données.
