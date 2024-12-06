---
title: Guide de migration d’iOS/tvOS v3.x
description: Guide de migration d’iOS/tvOS v3.x
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 0%

---

# Guide de migration d’iOS/tvOS v3.x {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!TIP]
> 
> **Notes :**
>
> - À partir de la version 3.1 du sdk iOS, les implémentateurs peuvent désormais utiliser WKWebView ou UIWebView de manière interchangeable. Etant donné que UIWebView est obsolète, les applications doivent migrer vers WKWebView, afin d’éviter tout problème lié aux futures versions d’iOS.
> - Notez que la migration impliquerait simplement de changer la classe UIWebView avec WKWebView. Il n’y a aucun travail spécifique à faire concernant AccessEnabler de l’Adobe.

</br>

## Mise à jour des paramètres de build {#update}

Cette version contient des fonctionnalités écrites en langage SWIFT. Si votre application est entièrement Objective-C, vous devez cocher la case &quot;Always Embed Swift Standard Libraries&quot; dans les paramètres de création de votre cible sur &quot;Yes&quot;. Lorsque cette option est définie, Xcode analyse les structures regroupées dans votre application et, si l’une d’elles contient du code Swift, il copie les bibliothèques pertinentes dans le lot de votre application. Si vous ne mettez pas à jour les paramètres de génération, votre application risque de se bloquer avec des erreurs indiquant qu’elle ne peut pas charger AccessEnabler.framework ou différentes bibliothèques `ibswift*`.

</br>

## Ajout de votre instruction logicielle {#add}

> Pour plus d’informations sur la manière d’obtenir votre relevé logiciel, voir
> page :
> [Enregistrement de l’application ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

Une fois que vous disposez de l’instruction logicielle, nous vous recommandons de l’héberger sur un serveur distant afin que vous puissiez facilement la révoquer ou la modifier sans déployer une nouvelle version de l’application dans App Store. Lorsque l’application démarre, obtenez l’instruction logicielle à partir de l’emplacement distant et transmettez-la dans le constructeur AccessEnabler :

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> Informations sur l’API ici : [Référence de l’API iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Ajout du schéma d’URL personnalisé {#add-custom}

> Pour plus d’informations sur la manière d’obtenir un modèle d’URL personnalisé, accédez à cette page : [Obtenir un modèle d’URL de client](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

Après avoir obtenu le modèle d’URL personnalisé, vous devez l’ajouter au fichier info.plist de l’application. Le schéma personnalisé a le format suivant : `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. Vous devez omettre les deux-points et les barres obliques avant lors de son ajout au fichier. L’exemple ci-dessus deviendra `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

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

## Interception d’appels sur le modèle d’URL personnalisé {#intercept}

Cela s’applique uniquement si votre application a activé la gestion manuelle du contrôleur de vue Safari (SVC) via l’appel [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) et pour des MVPD spécifiques nécessitant le contrôleur de vue Safari (SVC), nécessitant donc le chargement des URL des points de terminaison d’authentification et de déconnexion par un contrôleur SFSafariViewController au lieu d’un UIWeb. Contrôleur View/WKWebView.

Pendant les flux d&#39;authentification et de déconnexion, votre application doit surveiller l&#39;activité du contrôleur `SFSafariViewController `lorsqu&#39;elle passe par plusieurs redirections. Votre application doit détecter le moment où elle charge une URL personnalisée spécifique définie par votre `application's custom URL scheme` (par exemple,`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer `SFSafariViewController` et appeler la méthode d&#39;API `handleExternalURL:url ` d&#39;AccessEnabler.

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

Le nouveau SDK utilisant un nouveau mécanisme d’authentification, il n’est pas nécessaire d’utiliser le paramètre signedRequestId ni la clé publique ni le secret (pour tvOS). La méthode `setRequestor` est simplifiée et ne nécessite que le requestorID.

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

> Informations sur l’API ici : [Set Requestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Remplacez la méthode getAuthenticationToken par la méthode handleExternalURL . {#replace}

La méthode `getAuthentication` a été utilisée par le passé pour terminer le flux d’authentification. Comme son nom était trompeur, il a été renommé `handleExternalURL` et prend l’URL comme paramètre.

Modifiez toutes les occurrences de :

```swift
    accessEnabler.getAuthenticationToken()
```

en ceci :

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> Informations sur l’API ici : [Gérer l’URL externe](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
