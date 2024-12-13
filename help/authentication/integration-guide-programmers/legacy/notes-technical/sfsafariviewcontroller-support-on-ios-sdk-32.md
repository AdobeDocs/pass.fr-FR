---
title: Prise en charge de SFSafariViewController sur iOS SDK 3.2+
description: Prise en charge de SFSafariViewController sur iOS SDK 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# Prise en charge de SFSafariViewController sur iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2} (héritée)

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>


**En raison d’exigences de sécurité, certaines pages de connexion de MVPD DOIVENT être présentées dans un SFSafariViewController, au lieu de vues web.**

Certains MVPD exigent que leurs pages de connexion soient présentées dans un contrôle de navigateur sécurisé comme SFSafariViewController. Ils bloquent activement les webviews, donc pour être en mesure de s&#39;authentifier avec eux nous devons utiliser SVC.

## Compatibilité {#compatiblity}

À partir de la version 3.1 d’iOS SDK, le SDK AccessEnabler affiche automatiquement la page de connexion à un MVPD spécifique dans un SFSafariViewController, en fonction de la configuration du serveur.

La version 3.1 de SDK présente automatiquement SFSafariViewController à partir du contrôleur de vue racine de l&#39;application. Bien que cela simplifie la gestion des pages de connexion pour les personnes responsables de l’implémentation, il existe des cas où la présentation de SFSafariViewController à partir du contrôleur de vue racine n’est pas possible, en raison de l’implémentation spécifique de l’application (comme un contrôleur modal déjà visible).

Dans ce cas, la version 3.2 permet au programmeur de gérer manuellement le SVC.

## Gestion manuelle du SVC {#manual-svc-management}

Pour gérer manuellement le SVC, l’implémentateur doit effectuer les étapes suivantes :


1. appelez **setOptions([« handleSVC »:true])** après l&#39;initialisation d&#39;AccessEnabler (assurez-vous que cet appel est effectué avant le début de l&#39;authentification). Cela permettra une gestion « manuelle » du SVC, le SDK ne présentera pas automatiquement le SVC, mais à la place, lorsque cela sera nécessaire     appel **navigate(toUrl:*{url}* useSVC:true)**.

1. implémentez le **`navigateToUrl:useSVC:`** de rappel facultatif dans l’implémentation. Vous devez créer une instance svc à l’aide de l’instance SFSafariViewController à l’aide de l’url fournie et la présenter à l’écran :

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Remarques :***

   - *Vous pouvez personnaliser SFSafariViewController comme vous le souhaitez. Par exemple, sur iOS 11+, vous pouvez remplacer le libellé « Terminé » par « Annuler ».*
   - *pour pouvoir ignorer le svc, vous avez besoin d’une référence à celui-ci, ne le créez pas dans la portée de **navigateToUrl:useSVC***
   - *utilisez votre propre contrôleur de vue pour « myController »*


1. Dans l’implémentation déléguée de votre application de **application(\_app: UIApplication, ouvrir l’URL : URL, options : \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool**, ajoutez le code pour fermer le svc. Vous devriez déjà y avoir du code qui appelle **accessEnabler.handleExternalURL()**. Ajouter juste en dessous :

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Encore une fois, svc est une référence au SFSafariViewController que vous avez créé à l&#39;étape 2.


1. Implémentez **safariViewControllerDIDFinish(\_ contrôleur : SFSafariViewController)** à partir de **SFSafariViewControllerDelegate** afin de capturer le moment où l’utilisateur a annulé le svc à l’aide du bouton « Terminé ». Dans cette fonction, pour informer le SDK que l’authentification a été annulée, vous devez appeler :

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
