---
title: Notes de mise à jour de l’authentification Adobe Pass Android 3.7.3
description: Notes de mise à jour de l’authentification Adobe Pass Android 3.7.3
exl-id: f335357e-c209-428d-af2a-2181551447d4
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Notes de mise à jour de l’authentification Adobe Pass Android 3.7.3 {#android-sdk-373-rn}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Numéro de build {#build-number-373}

Authentification Adobe Pass : Android 3.7.3

Date De Publication : **09/19/2023**

## Présentation de la version {#release-overview-373}

* Modifications pour prendre en charge Android 14 et les applications ciblant le niveau API 34
   * Ajoutez l&#39;indicateur requis par les récepteurs de diffusions enregistrés à l&#39;exécution d&#39;Android 14 [](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported).
* Correction de l’absence d’ouverture de ChromeCustomTabs pour la connexion à MVPD sur l’API d’émulateur 32+
   * Remarque : une solution à ce problème sur SDK &lt;3.7.3 consiste à ouvrir l’application Chrome sur l’émulateur et à terminer sa configuration avant d’essayer d’ouvrir une session MVPD

## Package de version {#release-package-373}

Vous pouvez télécharger Android SDK v3.7.3 à partir de [ici](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
