---
title: Prise en charge de WKWebView sur le SDK iOS 3.1+
description: Prise en charge de WKWebView sur le SDK iOS 3.1+
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# Prise en charge de WKWebView sur le SDK iOS 3.1+ {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

**En raison de l’abandon d’UIWebView par Apple sur iOS, nous avons mis à jour le SDK iOS 3.1 avec la prise en charge de WKWebView.**

## Compatibilité {#compatibility}

À partir de la version 3.1 du SDK iOS, les implémentateurs peuvent désormais utiliser WKWebView ou UIWebView de manière interchangeable. Etant donné que UIWebView est obsolète par Apple, les applications doivent migrer vers WKWebView, afin d’éviter tout problème lié aux versions futures d’iOS.

Notez que la migration impliquerait simplement de changer la classe UIWebView avec WKWebView. Il n’y a aucun travail spécifique à faire concernant AccessEnabler de l’Adobe.

## Problèmes connus {#known-issues}

AccessEnabler d’Adobe a utilisé une instance UIWebView interne masquée pour effectuer &quot;[l’authentification passive](/help/authentication/sso-passive-authn.md)&quot; pour certains MVPD. Le flux &quot;passif&quot; était utile pour les distributeurs multicanaux de programmes qui nécessitent une authentification pour chaque ID de demandeur, et de ce flux, les programmeurs qui utilisaient le même ID d’équipe sur plusieurs applications iOS afin de simuler une expérience d’authentification unique (SSO Adobe) ont bénéficié de ce flux. Cette fonctionnalité est actuellement utilisée par un nombre limité de MVPD.

La fonction utilisait un comportement de l’UIWebView qui permettait à l’Adobe de capturer les cookies d’authentification et de les lire pendant le flux &quot;passif&quot;. WKWebView introduit une sécurité renforcée qui empêche l’Adobe de capturer les cookies définis lors de la connexion et de les lire à l’aide d’une instance masquée de WKWebView. En raison de cette amélioration de la sécurité et compte tenu du fait que le flux &quot;passif&quot; n’a bénéficié qu’à un très petit nombre de MVPD dans un scénario de mise en oeuvre très spécifique (plusieurs applications utilisant le même ID d’équipe), Adobe a supprimé la fonctionnalité &quot;authentification passive&quot; pour les MVPD utilisant des webviews pour s’authentifier.

La fonctionnalité est toujours présente pour les MVPD configurés pour utiliser SFSafariViewController, mais notez que dans ce cas, l’authentification &quot;passive&quot; sera visible par l’utilisateur car SFSafariViewController ne peut pas être utilisé de manière &quot;cachée&quot;.
