---
title: SSO sur iOS lors de l’utilisation de l’activateur d’accès à l’authentification Adobe Pass
description: SSO sur iOS lors de l’utilisation de l’activateur d’accès à l’authentification Adobe Pass
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1164'
ht-degree: 0%

---

# SSO sur iOS lors de l’utilisation de l’activateur d’accès à l’authentification Adobe Pass {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Présentation

L’authentification unique (SSO) entre les applications Adobe Pass Authentification fonctionne de différentes manières selon le système d’exploitation sous-jacent.

Ce document s&#39;adresse à **SSO sur iOS**, lors de l’utilisation de l’authentification Adobe Pass **Accéder à l’activateur**.

**Accéder à l’activateur** **1,10** est la dernière version du SDK natif d’Adobe Pass Authentication iOS. Adobe recommande vivement de passer à cette version plutôt que de conserver une ancienne version. Si vous utilisez une ancienne version d’Access Enabler, vous pouvez télécharger la dernière version. [here](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

L’authentification unique sur iOS est dictée par les conditions suivantes :

- Les applications doivent utiliser le même **stockage des jetons** (sous la forme d’un tableau de bord personnalisé créé par Access Enabler).
- Les applications doivent générer le même **ID de périphérique** (Access Enabler calcule l’identifiant de l’appareil en fonction de l’adresse MAC ou de l’IDFV, selon la version du système d’exploitation).

## Comportement

Le comportement de l’authentification unique est le suivant :

- **iOS 6 et versions antérieures**: SSO fonctionne automatiquement entre les applications qui sont développées par la même équipe ou différentes équipes. L’identifiant de l’appareil est calculé en fonction de l’adresse MAC (la même valeur est générée dans toutes les applications) et la zone de stockage est commune à toutes les applications (le tableau de bord personnalisé est partageable dans toutes les applications sur iOS 6 et versions antérieures).
   - **Important :** Veuillez noter que la version 1.9.4 du SDK iOS a [augmentation de la cible de déploiement iOS minimale à iOS 7.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 et versions ultérieures**: SSO fonctionnera dans les conditions suivantes :

1. Les applications sont publiées à l’aide du même profil de distribution Apple ou des profils appartenant à la même équipe. C’est la seule façon pour les applications de partager des tableaux de bord personnalisés sur iOS 7 et les versions ultérieures. Dans tous les autres scénarios, le tableau de bord est un environnement de test par application. De [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+\[UIPasteboard pasteboardWithName:create:\] et +\[UIPasteboard pasteboardWithUniqueName\] sont désormais uniques au nom donné pour permettre uniquement aux applications du même groupe d’applications d’accéder au tableau de bord. Si le développeur tente de créer un tableau de bord avec un nom qui existe déjà et qu’il ne fait pas partie de la même suite d’applications, il obtient son propre tableau de bord privé unique. Notez que cela n’a aucune incidence sur les tableaux de bord fournis par le système, ainsi que sur les informations générales et la recherche.

1. Les applications comportent le même préfixe Bundle ID (tous les composants sauf le dernier). Seules les applications qui partagent le même préfixe Bundle ID calculeront le même IDFV. De [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): sur IOS 7, tous les composants du lot, à l’exception du dernier composant, sont utilisés pour générer l’identifiant du fournisseur. Si l’ID de lot ne comporte qu’un seul composant, l’ID de lot entier est utilisé.

Concentrons-nous maintenant sur le **&quot;iOS 7 et versions ultérieures&quot;** , car il s’agit de la plus fréquente pour les utilisateurs réels :

Les deux conditions (partager un profil de la même équipe de développement et avoir un préfixe d’identifiant de lot commun) sont des conditions obligatoires pour SSO.

Voici les combinaisons possibles et les résultats obtenus :

- **Profils de la même équipe et du même préfixe Bundle ID**: les applications partagent le même stockage de tableau de bord et possèdent le même identifiant de périphérique (IDFV). Un utilisateur ne doit s’authentifier qu’une seule fois (dans la première application utilisée) et l’état d’authentification est partagé sur toutes les autres applications. Exemple de flux :

1. L’utilisateur ouvre l’application A (avec le Bundle ID) *com.x.y.AppA*) et n’est pas authentifié
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B (avec le Bundle ID) *com.x.y.AppB*) et est automatiquement authentifié en partageant les données de droits de l’application A (à l’étape 2).
1. L’utilisateur ouvre l’application A et est toujours authentifié (à l’étape 2).



- **Profils de la même équipe mais préfixes d’ID de lot différents**: les applications partagent le même espace de stockage de tableau de bord, mais ont des identifiants d’appareil (IDFV) différents. Un utilisateur devra s’authentifier une fois par application. Exemple de flux :

1. L’utilisateur ouvre l’application A (avec le Bundle ID) *com.x.y.AppA*) et n’est pas authentifié
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B (avec le Bundle ID) *com.z.AppB*) et Access Enabler détecte le jeton obtenu par la première application (car le stockage est partagé), mais il ne tente pas de l’utiliser via SSO en raison des différents identifiants de périphérique.
1. L’utilisateur effectue une authentification dans l’application B
1. L’utilisateur ouvre l’application A et est toujours authentifié (à l’étape 2).



- **Profils provenant de différentes équipes (l’ID de bundle n’est pas pertinent dans ce scénario)**: les applications auront différents magasins de tableau de bord et l’authentification unique sera désactivée entre elles. Un utilisateur devra s’authentifier une fois par application et les sessions d’authentification resteront persistantes lors du basculement entre les applications. Exemple de flux :


1. L’utilisateur ouvre l’application A et n’est pas authentifié.
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B et n’est pas authentifié.
1. L’utilisateur effectue une authentification dans l’application B
1. L’utilisateur ouvre l’application A et est authentifié (à l’étape 2).
1. L’utilisateur ouvre l’application B et est authentifié (à l’étape 4).

**Remarque :** Veuillez noter que les conditions ci-dessus pour l’authentification unique s’appliquent lors de l’installation d’applications via la fonction **Apple App Store**. Si les applications sont déployées sur un simulateur (où la signature de l’application ne s’applique pas), installées avec Xcode ou distribuées via un profil ad hoc, vous pouvez obtenir des résultats différents.

**Important :** Remarque (**à propos de AccessEnabler v1.8**) : le deuxième scénario décrit ci-dessus (profils de la même équipe mais différents préfixes d’ID de lot) crée une expérience utilisateur très déplaisante pour les utilisateurs de la variable **AccessEnabler v1.8** dans les applications développées par la même équipe (société de médias). L’utilisateur sera automatiquement déconnecté lors de la transition entre les applications d’une même société multimédia. Par conséquent, les développeurs d’applications doivent prendre soin lors du choix de l’ID de bundle et du profil de distribution. Dans ce cas, le scénario exact est présenté ci-dessous :

Les applications partagent le même stockage de tableau de bord, mais elles ont des identifiants d’appareil (IDFV) différents. Un utilisateur devra s’authentifier une fois par application, **mais les sessions d’authentification seront effacées lors du changement entre les applications.**. Exemple de flux :

1. L’utilisateur ouvre l’application A (avec le Bundle ID) *com.x.y.AppA*) et n’est pas authentifié
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B (avec le Bundle ID) *com.z.AppB*) et que les données de droits qui ont été créées par l’application A sont automatiquement effacées par l’activateur d’accès (mécanisme de sécurité qui détecte une collision entre l’identifiant de périphérique actuellement calculé dans l’application B et celui qui est stocké dans les jetons de droits - créé par l’application A).
1. L’utilisateur effectue une authentification dans l’application B
1. L’utilisateur ouvre l’application A et les données de droits qui ont été créées par l’application B sont automatiquement effacées par l’activateur d’accès (mécanisme de sécurité qui détecte une collision entre l’identifiant de périphérique actuellement calculé dans l’application A et celui qui est stocké dans les jetons de droits - créé par l’application B).
