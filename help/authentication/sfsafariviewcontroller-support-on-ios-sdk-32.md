---
title: Prise en charge de SFSafariViewController sur le SDK iOS 3.2+
description: Prise en charge de SFSafariViewController sur le SDK iOS 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# Prise en charge de SFSafariViewController sur le SDK iOS 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>


**En raison des exigences de sécurité, certaines pages de connexion du MVPD doivent être présentées dans un SFSafariViewController, au lieu de vues web.**

Certains MVPD exigent que leurs pages de connexion soient présentées dans un contrôle de navigateur sécurisé comme SFSafariViewController. Ils bloquent activement les webviews, donc pour pouvoir s’authentifier avec eux, nous devons utiliser SVC.

## Compatibilité {#compatiblity}

À compter de la version 3.1 du SDK iOS, le SDK AccessEnabler affiche automatiquement la page de connexion de MVPD spécifiques dans un SFSafariViewController, en fonction de la configuration du serveur.

La version 3.1 du SDK présente automatiquement le SFSafariViewController du contrôleur d’affichage racine de l’application. Bien que cela simplifie la gestion des pages de connexion pour les implémentateurs, il arrive que la présentation de SFSafariViewController à partir du contrôleur de vue racine ne soit pas possible en raison de l’implémentation spécifique de l’application (comme un contrôleur modal déjà visible).

Dans ce cas, la version 3.2 offre au programmeur la possibilité de gérer manuellement le SVC.

## Gestion SVC manuelle {#manual-svc-management}

Pour gérer manuellement le SVC, l’implémentateur doit effectuer les étapes suivantes :


1. appelez **setOptions([&quot;handleSVC&quot;:true])** après l’initialisation d’AccessEnabler (assurez-vous que cet appel est effectué avant le début de l’authentification). Cela permet une gestion &quot;manuelle&quot; du SVC. Le SDK ne présente pas automatiquement le SVC, mais, au besoin,     appelez **navigate(toUrl:*{url}* useSVC:true)**.

1. implémentez le rappel facultatif **`navigateToUrl:useSVC:`** dans l’implémentation. Vous devez créer une instance svc à l’aide de l’instance SFSafariViewController à l’aide de l’URL fournie et la présenter à l’écran :

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Notes :***

   - *Vous pouvez personnaliser SFSafariViewController comme vous le souhaitez. Par exemple, sur iOS 11+, vous pouvez remplacer l’étiquette &quot;Terminé&quot; par &quot;Annuler&quot;.*
   - *pour pouvoir ignorer le svc, vous avez besoin d’une référence à ce dernier. Ne le créez pas dans le cadre de **navigateToUrl:useSVC***
   - *utilisez votre propre contrôleur de vue pour &quot;myController&quot;*


1. Dans l’implémentation déléguée de votre application de **application(\_app: UIApplication, ouvrez url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool**, ajoutez le code pour fermer le svc. Vous devriez déjà y trouver du code qui appelle **accessEnabler.handleExternalURL()**. Ajoutez ci-dessous :

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Là encore, svc est une référence au SFSafariViewController que vous avez créé à l’étape 2.


1. Mettez en oeuvre **safariViewControllerDidFinish(\_ controller: SFSafariViewController)** à partir de **SFSafariViewControllerDelegate** afin de capturer lorsque l’utilisateur a annulé le svc à l’aide du bouton &quot;Terminé&quot;. Dans cette fonction, pour informer le SDK que l’authentification a été annulée, vous devez appeler :

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
