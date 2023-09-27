---
title: Accéder à l’authentification unique (SSO) du SDK Android sur les applications Android 10
description: Accéder à l’authentification unique (SSO) du SDK Android sur les applications Android 10
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Accéder à l’authentification unique (SSO) du SDK Android sur les applications Android 10 {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Présentation

L’authentification unique (SSO) entre les applications Adobe Pass Authentification est disponible sur les appareils utilisant Android OS via Access Enabler Android SDK. Pour offrir l’authentification unique (SSO) sur les appareils Android, le SDK Access Enabler Android version 3.2.1 (dernière version) et les versions précédentes utilisent un fichier de base de données partagé enregistré dans une mise en oeuvre de stockage Android, accessible par toutes les applications Adobe Pass Authentication.

Toutefois, Google de la dernière version d’Android 10 a produit quelques modifications &quot;pour donner aux utilisateurs plus de contrôle sur leurs fichiers et pour limiter la concentration de fichiers, les applications ciblant Android 10 (niveau API 29) et les versions ultérieures se voient accorder par défaut un accès limité à un appareil de stockage externe, ou à un espace de stockage. Ces applications ne peuvent afficher que leur répertoire spécifique à l’application. `\[...\]`&quot;. Vous trouverez plus d’informations sur ces modifications du stockage Android 10 dans la section [Documentation sur le stockage des données et des fichiers pour Android](https://developer.android.com/training/data-storage/files/external-scoped).

Suite à ces modifications, l’authentification unique (SSO) proposée par la version Android d’Access Enabler **SDK 3.2.1 (dernière version)** et les versions précédentes peuvent être affectées sur les appareils Android 10, comme expliqué dans la section suivante.

Voir [Présentation de Roku SSO](/help/authentication/roku-sso-overview.md).

## Comportement

Selon le **niveau du SDK cible** ou l’utilisation de **android:requestLegacyExternalStorage** L’attribut manifest est l’authentification unique (SSO) proposée par le SDK Access Enabler Android version 3.2.1 (dernière version) et les versions précédentes se comportent actuellement comme suit :

- Vos cibles d’application **Android 9 (niveau API 28)** ou inférieur **-\>** Authentification unique (SSO) **fonctionnera**
- Vos cibles d’application **Android 10** **(niveau API 29)** et fait **set** la valeur de **requestLegacyExternalStorage sur true** dans le fichier manifeste de votre application **-\>** Authentification unique (SSO) **fonctionnera**
- Vos cibles d’application **Android 10** **(niveau API 29)** et fait **non défini** la valeur de **requestLegacyExternalStorage sur true** dans le fichier manifeste de votre application **-\>** Authentification unique (SSO) **ne fonctionnera pas**


>[!TIP]
>
> Avant que le SDK Adobe Pass Authentication Access Enabler Android soit entièrement compatible avec le stockage étendu, vous pouvez temporairement vous exclure en fonction du niveau de SDK cible de votre application ou de l’attribut de manifeste requestLegacyExternalStorage comme expliqué dans la rubrique publique [Documentation Android](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage).
