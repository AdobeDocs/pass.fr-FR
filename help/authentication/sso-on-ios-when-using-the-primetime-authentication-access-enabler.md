---
title: SSO sur iOS lors de l’utilisation de l’activateur d’accès à l’authentification Adobe Pass
description: SSO sur iOS lors de l’utilisation de l’activateur d’accès à l’authentification Adobe Pass
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '1121'
ht-degree: 0%

---

# SSO sur iOS lors de l’utilisation de l’activateur d’accès à l’authentification Adobe Pass {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Vue d’ensemble

L’authentification unique (SSO) entre les applications Adobe Pass Authentification fonctionne de différentes manières selon le système d’exploitation sous-jacent.

Ce document traite de la fonction **SSO sur iOS** lors de l’utilisation de l’authentification Adobe Pass **Access Enabler**.

**Access Enabler** **1.10** est la dernière version du SDK natif d’Adobe Pass Authentication iOS. Adobe recommande vivement de passer à cette version plutôt que de conserver une ancienne version. Si vous utilisez une ancienne version de Access Enabler, vous pouvez télécharger la dernière version [ici](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

L’authentification unique sur iOS est dictée par les conditions suivantes :

- Les applications doivent utiliser le même **stockage de jeton** (sous la forme d’un tableau de bord personnalisé créé par Access Enabler).
- Les applications doivent générer le même **ID de périphérique** (Access Enabler calcule l’ID de périphérique en fonction de l’adresse MAC ou de l’IDFV, selon la version du système d’exploitation).

## Comportement

Le comportement de l’authentification unique est le suivant :

- **iOS 6 et versions antérieures** : l’authentification unique fonctionne automatiquement entre les applications qui sont développées par la même équipe ou différentes équipes. L’identifiant de l’appareil est calculé en fonction de l’adresse MAC (la même valeur est générée dans toutes les applications) et la zone de stockage est commune à toutes les applications (le tableau de bord personnalisé est partageable dans toutes les applications sur iOS 6 et versions antérieures).
   - **Important :** Notez que la version 1.9.4 du SDK iOS a [ augmenté la cible de déploiement iOS minimale à iOS 7.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 et supérieur** : SSO fonctionne dans les conditions suivantes :

1. Les applications sont publiées à l’aide du même profil de distribution Apple ou des profils appartenant à la même équipe. C’est la seule façon pour les applications de partager des tableaux de bord personnalisés sur iOS 7 et les versions ultérieures. Dans tous les autres scénarios, le tableau de bord est un environnement de test par application. À partir de [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html) : \+\[`UIPasteboard pasteboardWithName:create:\`] et +\[`UIPasteboard pasteboardWithUniqueName`\], le prénom est désormais unique afin d’autoriser uniquement les applications du même groupe d’applications à accéder au tableau de bord. Si le développeur tente de créer un tableau de bord avec un nom qui existe déjà et qu’il ne fait pas partie de la même suite d’applications, il obtient son propre tableau de bord privé unique. Notez que cela n’a aucune incidence sur les tableaux de bord fournis par le système, ainsi que sur les informations générales et la recherche.

1. Les applications comportent le même préfixe Bundle ID (tous les composants sauf le dernier). Seules les applications qui partagent le même préfixe Bundle ID calculeront le même IDFV. À partir de [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor) : sur IOS 7, tous les composants du lot, à l’exception du dernier composant, sont utilisés pour générer l’identifiant du fournisseur. Si l’ID de lot ne comporte qu’un seul composant, l’ID de lot entier est utilisé.

Concentrons-nous maintenant sur le scénario **&#39;iOS 7 et supérieur&#39;**, car il s’agit du scénario le plus fréquent pour les utilisateurs réels :

Les deux conditions (partager un profil de la même équipe de développement et avoir un préfixe d’identifiant de lot commun) sont des conditions obligatoires pour SSO.

Voici les combinaisons possibles et les résultats obtenus :

- **Profils de la même équipe et du même préfixe d’ID de lot** : les applications partagent le même stockage de tableau de bord et disposent du même ID de périphérique (IDFV). Un utilisateur ne doit s’authentifier qu’une seule fois (dans la première application utilisée) et l’état d’authentification est partagé sur toutes les autres applications. Exemple de flux :

1. L’utilisateur ouvre l’application A (avec l’ID de lot *com.x.y.AppA*) et n’est pas authentifié.
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B (avec l’ID de lot *com.x.y.AppB*) et est automatiquement authentifié en partageant les données de droits de l’application.
A (à partir de l’étape 2)
1. L’utilisateur ouvre l’application A et est toujours authentifié (à l’étape 2).



- **Profils d’une même équipe mais préfixes d’ID de lot différents** : les applications partagent le même stockage de tableau de bord, mais ont des identifiants d’appareil (IDFV) différents. Un utilisateur devra s’authentifier une fois par application. Exemple de flux :

1. L’utilisateur ouvre l’application A (avec l’ID de lot *com.x.y.AppA*) et n’est pas authentifié.
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B (avec l’ID de lot *com.z.AppB*) et l’Activateur d’accès détecte le jeton obtenu par la première application (car le stockage est partagé), mais ne tente pas de l’utiliser via SSO en raison des différents identifiants de périphérique.
1. L’utilisateur effectue une authentification dans l’application B
1. L’utilisateur ouvre l’application A et est toujours authentifié (à l’étape 2).



- **Profils provenant de différentes équipes (l’ID de bundle n’est pas pertinent dans ce scénario)** : les applications auront différents magasins de table de montage et la connexion unique sera désactivée entre elles. Un utilisateur devra s’authentifier une fois par application et les sessions d’authentification resteront persistantes lors du basculement entre les applications. Exemple de flux :


1. L’utilisateur ouvre l’application A et n’est pas authentifié.
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B et n’est pas authentifié.
1. L’utilisateur effectue une authentification dans l’application B
1. L’utilisateur ouvre l’application A et est authentifié (à l’étape 2).
1. L’utilisateur ouvre l’application B et est authentifié (à l’étape 4).

**Remarque :** Notez que les conditions ci-dessus pour la connexion unique s’appliquent lors de l’installation d’applications via **Apple App Store**. Si les applications sont déployées sur un simulateur (où la signature de l’application ne s’applique pas), installées avec Xcode ou distribuées via un profil ad hoc, vous pouvez obtenir des résultats différents.

**Important :** Remarque (**concernant AccessEnabler v1.8**) : le deuxième scénario décrit ci-dessus (profils d’une même équipe mais différents préfixes d’ID de lot) créera une expérience utilisateur très déplaisante pour les utilisateurs de **AccessEnabler v1.8** dans les applications développées par la même équipe (société de médias). L’utilisateur sera automatiquement déconnecté lors de la transition entre les applications d’une même société multimédia. Par conséquent, les développeurs d’applications doivent prendre soin lors du choix de l’ID de bundle et du profil de distribution. Dans ce cas, le scénario exact est présenté ci-dessous :

Les applications partagent le même stockage de tableau de bord, mais elles ont des identifiants d’appareil (IDFV) différents. Un utilisateur devra s’authentifier une fois par application, **mais les sessions d’authentification seront effacées lors du passage d’une application à l’autre**. Exemple de flux :

1. L’utilisateur ouvre l’application A (avec l’ID de lot *com.x.y.AppA*) et n’est pas authentifié.
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B (avec l’ID de bundle *com.z.AppB*) et les données de droits qui ont été créées par l’application A sont automatiquement effacées par l’Activateur d’accès (mécanisme de sécurité qui détecte une collision entre l’ID de périphérique actuellement calculé dans l’application B et celui qui est stocké dans les jetons de droits - créé par l’application A).
1. L’utilisateur effectue une authentification dans l’application B
1. L’utilisateur ouvre l’application A et les données de droits qui ont été créées par l’application B sont automatiquement effacées par l’activateur d’accès (mécanisme de sécurité qui détecte une collision entre l’identifiant de périphérique actuellement calculé dans l’application A et celui qui est stocké dans les jetons de droits - créé par l’application B).
