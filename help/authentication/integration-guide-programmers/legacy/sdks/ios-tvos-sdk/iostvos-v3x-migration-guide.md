---
title: Guide de migration d’iOS/tvOS v3.x
description: Guide de migration d’iOS/tvOS v3.x
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# Guide de migration d’iOS/tvOS v3.x (hérité) {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

>[!TIP]
> 
> **Remarques :**
>
> - À partir de la version 3.1 du sdk iOS, les personnes en charge de l’implémentation peuvent désormais utiliser WKWebView ou UIWebView de manière interchangeable. Comme UIWebView est obsolète, les applications doivent migrer vers WKWebView afin d’éviter tout problème avec les futures versions d’iOS.
> - Notez que la migration impliquerait simplement de changer la classe UIWebView avec WKWebView. Il n’y a pas de travail spécifique à effectuer concernant AccessEnabler d’Adobe.

</br>

## Mettre à jour les paramètres de build {#update}

Cette version contient des fonctionnalités écrites en langage SWIFT. Si votre application est entièrement Objective-C, vous devez définir la case à cocher « Toujours incorporer les bibliothèques Swift Standard » dans les paramètres de build de votre cible sur « Oui ». Lorsque cette option est définie, Xcode analyse les frameworks regroupées dans votre application et, si l’une d’elles contient du code Swift, il copie les bibliothèques pertinentes dans le bundle de votre application. Si vous ne mettez pas à jour les paramètres de build, votre application peut se bloquer avec des erreurs indiquant qu’elle ne peut pas charger AccessEnabler.framework ou d’autres bibliothèques `ibswift*`.

</br>

## Ajout de la déclaration de logiciel {#add}

> Pour plus d&#39;informations sur la façon d&#39;obtenir votre relevé de logiciel, rendez-vous sur
> page :
> [Enregistrement de la demande &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

Une fois que vous disposez de votre déclaration logicielle, nous vous recommandons de l’héberger sur un serveur distant afin de pouvoir facilement la révoquer ou la modifier sans déployer une nouvelle version de l’application dans App Store. Lorsque l’application démarre, obtenez votre instruction logicielle à partir de l’emplacement distant et transmettez-la dans le constructeur AccessEnabler :

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> Informations sur l’API ici : [Référence de l’API iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Ajouter le schéma d’URL personnalisé {#add-custom}

> Pour plus d’informations sur l’obtention d’un schéma d’URL personnalisé, consultez cette page : [Obtenir un schéma d’URL client](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

Une fois que vous avez obtenu le schéma d’URL personnalisé, vous devez l’ajouter au fichier info.plist de l’application. Le schéma personnalisé présente le format suivant : `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. Vous devez omettre les deux-points et les barres obliques lors de l’ajout au fichier. L’exemple ci-dessus devient `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>CUSTOM_URL_SCHEME_HERE</string>
            </array>
        </dict>
    </array>
```

</br>

## Interception des appels sur le schéma d’URL personnalisé {#intercept}

Cela s’applique uniquement au cas où votre application a précédemment activé la gestion manuelle de Safari View Controller (SVC) via l’appel [setOptions(\[« handleSVC »:true »\])](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) et pour les MVPD spécifiques nécessitant Safari View Controller (SVC), nécessitant donc le chargement des URL des points d’entrée d’authentification et de déconnexion par un contrôleur SFSafariViewController au lieu d’un contrôleur UIWebView/WKWebView.

Pendant les flux d&#39;authentification et de déconnexion, votre application doit surveiller l&#39;activité du `SFSafariViewController `contrôleur qui passe par plusieurs redirections. Votre application doit détecter le moment où elle charge une URL personnalisée spécifique définie par votre `application's custom URL scheme` (par ex`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer le `SFSafariViewController` et appeler la méthode `handleExternalURL:url `API d&#39;AccessEnabler.

Dans votre `AppDelegate`, ajoutez la méthode suivante :

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> Informations sur l’API ici : [Gérer l’URL externe](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Mettre à jour la signature de la méthode setRequestor {#update-setreq}

Le nouveau SDK utilisant un nouveau mécanisme d’authentification, le paramètre signedRequestId ou la clé publique et le secret (pour tvOS) ne sont pas nécessaires. La méthode `setRequestor` est simplifiée et ne nécessite que l’ID de demandeur.

### iOS

Ce code :

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

devient :

```swift
    accessEnabler.setRequestor(requestorId)
```

</br>

### tvOS

Ce code :

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId,
                    secret: "secret", publicKey: "public_key")
```

devient :

```swift
    accessEnabler.setRequestor(requestorId)
```

> Informations sur l’API ici : [Définir le demandeur](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Remplacer la méthode getAuthenticationToken par la méthode handleExternalURL {#replace}

`getAuthentication` méthode était utilisée par le passé pour terminer le flux d’authentification. Comme son nom était trompeur, il a été renommé `handleExternalURL` et prend l’URL comme paramètre.

Modifier toutes les occurrences de ce(tte) :

```swift
    accessEnabler.getAuthenticationToken()
```

dans ce contexte :

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> Informations sur l’API ici : [Gérer l’URL externe](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
