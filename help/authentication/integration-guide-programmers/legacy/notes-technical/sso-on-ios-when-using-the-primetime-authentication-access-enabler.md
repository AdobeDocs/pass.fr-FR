---
title: SSO sur iOS lors de l’utilisation d’Adobe Pass Authentication Access Enabler
description: SSO sur iOS lors de l’utilisation d’Adobe Pass Authentication Access Enabler
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 0%

---

# Authentification unique (héritée) sur iOS lors de l’utilisation d’Adobe Pass Authentication Access Enabler {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Vue d’ensemble

L’authentification unique (SSO) entre les applications optimisées par l’authentification Adobe Pass fonctionne de différentes manières en fonction du système d’exploitation sous-jacent.

Ce document traite de la **SSO sur iOS** lors de l’utilisation de l’activateur d’accès **Access** d’authentification Adobe Pass.

**Access Enabler** **1.10** est la dernière version du SDK natif Adobe Pass Authentication iOS. Adobe recommande vivement de passer à cette version plutôt que de rester avec une version plus ancienne. Si vous utilisez une ancienne version d&#39;Access Enabler, vous pouvez télécharger la dernière version [ici](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

La connexion unique sur iOS est déterminée par les conditions suivantes :

- Les applications doivent utiliser le même **stockage de jeton** (sous la forme d’une table de montage personnalisée créée par le programme d’activation d’Access).
- Les applications doivent générer le même **ID d’appareil** (Access Enabler calcule l’ID d’appareil en fonction de l’adresse MAC ou de l’IDFV, selon la version du système d’exploitation).

## Comportement

Le comportement de la fonction SSO est le suivant :

- **iOS 6 et versions antérieures** : la connexion unique fonctionne automatiquement entre les applications développées par la même équipe ou par des équipes différentes. L’identifiant de l’appareil est calculé en fonction de l’adresse MAC (la même valeur est générée dans toutes les applications) et la zone de stockage est commune à toutes les applications (la table de montage personnalisée peut être partagée entre les applications sur iOS 6 et les versions antérieures).
   - **Important :** notez que la version 1.9.4 d’iOS SDK a [augmenté la cible de déploiement minimale d’iOS à iOS 7.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 et versions ultérieures** : l’authentification unique fonctionnera dans les conditions suivantes :

1. Les applications sont publiées à l’aide du même profil de distribution Apple ou de profils appartenant à la même équipe. Il s’agit de la seule façon pour les applications de partager des tableaux de bord personnalisés sur iOS 7 et versions ultérieures. Dans tous les autres scénarios, la table de montage est en sandbox par application. Depuis [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html) : \+\[`UIPasteboard pasteboardWithName:create:\`] et +\[`UIPasteboard pasteboardWithUniqueName`\] attribuent désormais un nom unique au prénom pour permettre uniquement aux applications du même groupe d’applications d’accéder à la table de montage. Si le développeur tente de créer une table de montage avec un nom qui existe déjà et qu’il ne fait pas partie de la même suite d’applications, il obtiendra sa propre table de montage privée et unique. Notez que cela n’a aucune incidence sur les tableaux de bord fournis par le système, General, et Find.

1. Les applications ont le même préfixe d’ID de lot (tous les composants sauf le dernier). Seules les applications partageant le même préfixe d’ID de lot calculeront le même ID. À partir de [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor) : sur IOS 7, tous les composants du lot, à l’exception du dernier composant, sont utilisés pour générer l’ID de fournisseur. Si l’ID de lot ne comporte qu’un seul composant, l’ID de lot entier est utilisé.

Concentrons-nous maintenant sur le scénario **’iOS 7 et versions ultérieures** car il est le plus fréquent pour les utilisateurs réels :

Les deux conditions (partage d’un profil de la même équipe de développement et d’un préfixe d’identifiant de lot commun) sont des conditions obligatoires pour la connexion unique.

Voici les combinaisons possibles et leurs résultats :

- **Profils d’une même équipe et même préfixe d’ID de bundle** : les applications partageront le même stockage dans la table de montage et auront le même ID d’appareil (IDFV). Un utilisateur ne devra s’authentifier qu’une seule fois (dans la première application utilisée) et l’état d’authentification sera partagé dans toutes les autres applications. Exemple de flux :

1. L’utilisateur ouvre l’application A (avec l’ID de bundle *com.x.y.AppA*) et n’est pas authentifié
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B (avec l’ID de lot *com.x.y.AppB*) et est automatiquement authentifié en partageant les données de droits de l’application
A (depuis l’étape 2)
1. L’utilisateur ouvre l’application A et est toujours authentifié (étape 2)



- **Profils d’une même équipe, mais avec des préfixes d’ID de bundle différents** : les applications partageront le même stockage dans la table de collage, mais auront des ID d’appareil (IDFV) différents. Un utilisateur devra s’authentifier une fois par application. Exemple de flux :

1. L’utilisateur ouvre l’application A (avec l’ID de bundle *com.x.y.AppA*) et n’est pas authentifié
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B (avec l’ID de bundle *com.z.AppB*) et Access Enabler détecte le jeton obtenu par la première application (car le stockage est partagé), mais il ne tentera pas de l’utiliser via l’authentification unique en raison des différents ID d’appareil
1. L’utilisateur effectue l’authentification dans l’application B
1. L’utilisateur ouvre l’application A et est toujours authentifié (étape 2)



- **Profils de différentes équipes (l’ID de bundle n’est pas pertinent dans ce scénario)** : les applications disposeront de différents stockages dans la table de montage et l’authentification unique sera désactivée entre elles. Un utilisateur devra s’authentifier une fois par application et les sessions d’authentification resteront persistantes lors du changement d’application. Exemple de flux :


1. L’utilisateur ouvre l’application A sans s’authentifier
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B sans s’authentifier
1. L’utilisateur effectue l’authentification dans l’application B
1. L’utilisateur ouvre l’application A et est authentifié (à partir de l’étape 2)
1. L’utilisateur ouvre l’application B et est authentifié (étape 4)

**Remarque :** notez que les conditions ci-dessus pour la connexion unique s’appliquent lors de l’installation d’applications via **Apple App Store**. Si les applications sont déployées sur un simulateur (où la signature d’application ne s’applique pas), installées avec Xcode ou distribuées via un profil Ad Hoc, vous pouvez obtenir des résultats différents.

**Important :** Remarque (**concernant AccessEnabler v1.8**) : le deuxième scénario décrit ci-dessus (profils d’une même équipe, mais préfixes d’ID de bundle différents) créera une expérience utilisateur très désagréable pour les utilisateurs d’**AccessEnabler v1.8** dans les applications développées par la même équipe (entreprise de médias). L’utilisateur sera automatiquement déconnecté lors de la transition entre les applications d’une même société de médias. Par conséquent, les développeurs d’applications doivent faire attention lors du choix de l’ID de bundle et du profil de distribution. Le scénario exact dans ce cas est présenté ci-dessous :

Les applications partagent le même espace de stockage de la table de montage, mais avec des ID d’appareil (IDFV) différents. Un utilisateur devra s’authentifier une fois par application, **mais les sessions d’authentification seront effacées lors du changement d’application**. Exemple de flux :

1. L’utilisateur ouvre l’application A (avec l’ID de bundle *com.x.y.AppA*) et n’est pas authentifié
1. L’utilisateur effectue l’authentification dans l’application A
1. L’utilisateur ouvre l’application B (avec l’ID de bundle *com.z.AppB*) et les données de droits créées par l’application A sont automatiquement effacées par Access Enabler (mécanisme de sécurité qui détecte un conflit entre l’ID d’appareil actuellement calculé dans l’application B et celui stocké dans les jetons de droits, créé par l’application A)
1. L’utilisateur effectue l’authentification dans l’application B
1. L’utilisateur ouvre l’application A et les données de droits créées par l’application B sont automatiquement effacées par Access Enabler (mécanisme de sécurité détectant un conflit entre l’ID d’appareil actuellement calculé dans l’application A et celui stocké dans les jetons de droits, créés par l’application B)
