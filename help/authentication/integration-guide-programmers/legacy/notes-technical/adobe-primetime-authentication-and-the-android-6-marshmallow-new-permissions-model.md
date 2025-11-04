---
title: Authentification Adobe Pass et nouveau modèle d’autorisations Android 6 « Marshmallow »
description: Authentification Adobe Pass et nouveau modèle d’autorisations Android 6 « Marshmallow »
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# (Hérité) Authentification Adobe Pass et nouveau modèle d’autorisations Android 6 « Marshmallow » {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>

La nouvelle version d’Android 6 Marshmallow introduit certaines mises à jour du modèle d’autorisations, ce qui peut affecter le comportement des applications qui utilisent la version 1.8 et ultérieure du SDK d’authentification Adobe Pass.

Nouvelle fonctionnalité, le nouveau système d’exploitation Android offre un contrôle granulaire [ sur les autorisations dont les applications ont besoin au moment de l’installation et de l’exécution](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html).

>[!IMPORTANT]
>
>Les modifications décrites ci-dessous **affectent uniquement les applications développées spécifiquement pour Android 6.0** (targetSdkVersion=23). Ils n’affectent pas les applications plus anciennes déjà installées sur l’appareil de l’utilisateur lors de la mise à niveau vers Android 6.0.


En particulier, pour les applications développées dans Android Studio à l’aide de [niveau d’API 23](http://developer.android.com/sdk/api_diff/23/changes.html) et qui utilisent le SDK d’authentification Adobe Pass, le développeur ou la développeuse devra écrire du code personnalisé (voir le fragment de code ci-dessous) [pour déclencher la boîte de dialogue Autoriser/Refuser les autorisations](https://developer.android.com/training/permissions/requesting.html).

Voici l’extrait de code utilisé pour demander l’accès en écriture au stockage externe du périphérique :

```java
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.WRITE_EXTERNAL_STORAGE)
        != PackageManager.WRITE_EXTERNAL_STORAGE) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.WRITE_EXTERNAL_STORAGE)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE);

        // MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```




**Du point de vue de l’utilisateur** lors de l’installation, une fenêtre l’invite à confirmer les autorisations de lecture/écriture pour les fichiers (voir la figure 2 ci-dessous). Cela aboutit à l’un des deux résultats suivants :

1. Si l’utilisateur **confirme** les autorisations, le flux d’authentification normal est conservé et les jetons sont stockés dans le stockage global. Les utilisateurs restent authentifiés dans l’application et entre les applications à l’aide de l’authentification Adobe Pass tant que les jetons sont valides.
1. Si l’utilisateur **refuse** les autorisations, les actions d’écriture dans le stockage échouent et les utilisateurs ne sont authentifiés que lorsqu’ils quittent l’application. Notez que certaines applications se réinitialisent lors du basculement entre le premier plan et l’arrière-plan, de sorte que les utilisateurs seront déconnectés lors de cette action. Les jetons ne sont PAS stockés et les utilisateurs devront s’authentifier à chaque fois qu’ils utiliseront l’application.


>[!TIP]
>
>Une fonctionnalité qui introduit la résilience du stockage est actuellement en développement pour Adobe Pass Authentication SDK 1.9. La nouvelle version de SDK devrait sortir **au cours de la dernière semaine d’octobre**. L’application revient à écrire dans le stockage sandbox de l’application chaque fois que le stockage général ne peut pas être utilisé. Cela couvre le cas où, pour les applications développées au niveau API 23, les utilisateurs n’acceptent PAS les autorisations de lecture/écriture dans le stockage global. Les jetons sont stockés individuellement par application, ce qui signifie que l’authentification unique entre les applications à l’aide de l’authentification Adobe Pass sera désactivée.


![](/help/authentication/assets/android-permissions-request.png)

*Figure : boîte de dialogue de demande d’autorisation pour les applications écrites ciblant le niveau d’API 23*

>[!IMPORTANT]
>
> Adobe conseille à **ses partenaires de développer des applications à l’aide d’API de niveau 22 (targetSdkVersion=22) ou supérieur, afin de garantir la meilleure expérience utilisateur possible dans le processus d’authentification**.
