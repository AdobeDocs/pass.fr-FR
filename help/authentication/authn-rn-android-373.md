---
title: Notes de mise à jour d’Adobe Pass Authentication Android 3.7.3
description: Notes de mise à jour d’Adobe Pass Authentication Android 3.7.3
source-git-commit: 284ae59837fd42bc6b711b3041eb6b7ef79aaed3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Notes de mise à jour d’Adobe Pass Authentication Android 3.7.3 {#android-sdk-373-release-notes}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Numéro de build {#build-no-android-sdk-373}

Authentification Adobe Pass : Android 3.7.3

Date de publication : 09/19/2023



## Présentation des versions {#overview-android-sdk-373}

* Modifications apportées à la prise en charge d’Android 14 et des applications ciblant l’API niveau 34
   * Ajout de l’indicateur requis par [Récepteurs de diffusions enregistrés pour l’exécution Android 14](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported).
* Correction de ChromeCustomTabs qui ne s’ouvre pas pour la connexion MVPD sur l’API d’émulateur 32+
   * Remarque : Une solution à ce problème sur le SDK &lt;3.7.3 consiste à ouvrir l’application Chrome sur l’émulateur et à terminer sa configuration avant de tenter de se connecter au MVPD.


## Package de version {#rel-pkg-android373}

Vous pouvez télécharger le SDK Android v3.7.3 à partir de [here](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
