---
title: Accéder à l’authentification unique (SSO) Android SDK sur les applications Android 10
description: Accéder à l’authentification unique (SSO) Android SDK sur les applications Android 10
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# (Hérité) Accéder à l’authentification unique (SSO) Android SDK Enabler sur les applications Android 10 {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Vue d’ensemble

L’authentification unique (SSO) entre les applications optimisées par l’authentification Adobe Pass est disponible sur les appareils utilisant le système d’exploitation Android par le biais de l’activateur d’accès Android SDK. Afin d’offrir l’authentification unique (SSO) sur les appareils Android, la version 3.2.1 (la plus récente) d’Access Enabler Android SDK et les versions précédentes utilisent un fichier de base de données partagé enregistré dans une implémentation de stockage Android, accessible par toutes les applications Adobe Pass Authentication.

Cependant, Google dans la dernière version d’Android 10 a produit certaines modifications « pour donner aux utilisateurs plus de contrôle sur leurs fichiers et limiter l’encombrement des fichiers, les applications ciblant Android 10 (niveau API 29) et versions ultérieures bénéficient par défaut d’un accès étendu à un appareil de stockage externe, ou stockage étendu. Ces applications ne peuvent afficher que leur répertoire spécifique `\[...\]` ». Vous trouverez plus d’informations sur ces modifications de stockage Android 10 dans la [documentation sur le stockage de données et de fichiers pour Android](https://developer.android.com/training/data-storage/files/external-scoped).

Suite à ces modifications, l’authentification unique (SSO) proposée par la version **3.2.1 du SDK d’Access Enabler Android (la plus récente)** ainsi que les versions précédentes peuvent être affectées sur les appareils Android 10, comme expliqué dans la section suivante.

## Comportement

En fonction de la **[!UICONTROL target SDK level]** de votre application ou de l’utilisation de l’attribut de manifeste **android:requestLegacyExternalStorage**, l’authentification unique (SSO) proposée par la version 3.2.1 du SDK d’Access Enabler Android (la plus récente) et les versions précédentes se comporteront actuellement comme suit :

- Votre application cible **Android 9 (niveau API 28)** ou une version inférieure **-\>** authentification unique **fonctionnera**
- Votre application cible **Android 10** **(niveau API 29)** et **définit** la valeur de **requestLegacyExternalStorage sur true** dans le fichier manifeste de votre application **-\>** authentification unique (SSO) **fonctionnera**
- Votre application cible **Android 10** **(niveau d’API 29)** et ne définit **pas** la valeur de **requestLegacyExternalStorage sur true** dans le fichier de manifeste de votre application **-\>** authentification unique (SSO) **ne fonctionnera pas**

>[!TIP]
>
> Avant que l&#39;activateur d&#39;accès à l&#39;authentification Adobe Pass Android SDK ne soit entièrement compatible avec le stockage étendu, vous pouvez temporairement vous exclure en fonction du niveau de SDK cible de votre application ou de l&#39;attribut de manifeste requestLegacyExternalStorage, comme expliqué dans la [documentation Android publique](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage).
