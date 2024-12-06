---
title: Accès à l’authentification unique (SSO) du SDK Android sur les applications Android 10
description: Accès à l’authentification unique (SSO) du SDK Android sur les applications Android 10
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 0%

---

# Accès à l’authentification unique (SSO) du SDK Android sur les applications Android 10 {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble

L’authentification unique (SSO) entre les applications Adobe Pass Authentification est disponible sur les appareils utilisant Android OS via Access Enabler Android SDK. Pour offrir l’authentification unique (SSO) sur les appareils Android, le SDK Access Enabler Android version 3.2.1 (dernière version) et les versions précédentes utilisent un fichier de base de données partagé enregistré dans une mise en oeuvre de stockage Android, accessible par toutes les applications fonctionnant avec l’authentification Adobe Pass.

Cependant, dans la dernière version d’Android 10, Google a produit quelques modifications &quot;pour donner aux utilisateurs plus de contrôle sur leurs fichiers et pour limiter l’encombrement des fichiers, les applications ciblant Android 10 (niveau API 29) et versions ultérieures se voient accorder par défaut un accès limité à un périphérique de stockage externe, ou à un espace de stockage. Ces applications ne peuvent afficher que leur répertoire spécifique à l’application `\[...\]`&quot;. Pour plus d&#39;informations sur ces modifications de stockage dans Android 10, consultez la [documentation sur le stockage de données et de fichiers pour Android](https://developer.android.com/training/data-storage/files/external-scoped).

Suite à ces modifications, l’authentification unique (SSO) proposée par Access Enabler Android version **3.2.1 SDK (dernière version)** et les versions précédentes peuvent être affectées sur les appareils Android 10, comme expliqué dans la section suivante.

## Comportement

Selon l’attribut de manifeste **[!UICONTROL target SDK level]** de votre application ou l’utilisation de **android:requestLegacyExternalStorage**, l’authentification unique (SSO) proposée par le SDK Access Enabler Android version 3.2.1 (dernière version) et les versions précédentes se comportent actuellement comme suit :

- Votre application cible **Android 9 (niveau API 28)** ou inférieur **-\>** L’authentification unique (SSO) **fonctionnera**
- Votre application cible **Android 10** **(niveau API 29)** et **définit** la valeur de **requestLegacyExternalStorage sur true** dans le fichier manifeste de votre application **-\>** L’authentification unique (SSO) **fonctionnera**
- Votre application cible **Android 10** **(niveau API 29)** et **ne définit pas** la valeur de **requestLegacyExternalStorage sur true** dans le fichier manifeste de votre application **-\>** L’authentification unique (SSO) **ne fonctionnera pas**

>[!TIP]
>
> Avant que l’option Adobe Pass Authentication Access Enabler Android SDK ne soit entièrement compatible avec le stockage étendu, vous pouvez temporairement vous exclure en fonction du niveau de SDK cible de votre application ou de l’attribut de manifeste requestLegacyExternalStorage comme expliqué dans la [documentation Android](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage) publique.
