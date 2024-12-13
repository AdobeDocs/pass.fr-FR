---
title: Prise en charge de WKWebView sur iOS SDK 3.1+
description: Prise en charge de WKWebView sur iOS SDK 3.1+
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# Prise en charge de WKWebView sur iOS SDK 3.1+ {#wkwebview-support-on-ios-sdk-3.1} (héritée)

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>

**En raison de l’abandon par Apple de UIWebView sur iOS, nous avons mis à jour iOS SDK 3.1 avec la prise en charge de WKWebView.**

## Compatibilité {#compatibility}

À partir de la version 3.1 d’iOS SDK, les personnes en charge de l’implémentation peuvent désormais utiliser WKWebView ou UIWebView de manière interchangeable. Comme UIWebView est obsolète par Apple, les applications doivent migrer vers WKWebView afin d’éviter tout problème avec les futures versions d’iOS.

Notez que la migration impliquerait simplement de changer la classe UIWebView avec WKWebView. Il n’y a pas de travail spécifique à effectuer concernant AccessEnabler d’Adobe.

## Problèmes connus {#known-issues}

AccessEnabler d’Adobe a utilisé une instance UIWebView interne masquée pour effectuer une « authentification [ passive »](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-passive-authn.md) pour certains MVPD. Le flux « passif » était utile pour les MVPD qui nécessitent une authentification pour chaque ID de demandeur. De ce flux, les programmeurs qui utilisaient le même ID d’équipe dans plusieurs applications iOS ont bénéficié de ce flux pour simuler une expérience SSO (authentification unique Adobe). Cette fonctionnalité est actuellement utilisée par un nombre limité de MVPD.

Cette fonction utilisait un comportement de l’UIWebView qui permettait à Adobe de capturer les cookies d’authentification et de les relire pendant le flux « passif ». WKWebView offre une sécurité renforcée qui empêche l’Adobe de capturer les cookies définis lors de la connexion et de les relire à l’aide d’une instance masquée de WKWebView. En raison de cette amélioration de la sécurité et compte tenu du fait que le flux « passif » n’a bénéficié qu’à un nombre très limité de MVPD dans un scénario d’implémentation très spécifique (plusieurs applications utilisant le même identifiant d’équipe), l’Adobe a supprimé la fonction d’« authentification passive » pour les MVPD utilisant des vues web pour s’authentifier.

La fonctionnalité est toujours présente pour les MVPD configurés pour utiliser SFSafariViewController, mais notez que dans ce cas, l’authentification « passive » sera visible par l’utilisateur, car SFSafariViewController ne peut pas être utilisé de manière « masquée ».
