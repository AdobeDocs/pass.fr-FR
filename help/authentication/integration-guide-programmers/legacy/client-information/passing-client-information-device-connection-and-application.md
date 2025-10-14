---
title: Transmission des informations client (appareil, connexion et application)
description: Transmission des informations client (appareil, connexion et application)
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1666'
ht-degree: 3%

---

# (Hérité) Transmission des informations client (appareil, connexion et application) {#pass-client-info}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Champ d’application {#pass-client-info-scope}

Ce document agrège les détails et les cookies pour transmettre les informations du client (appareil, connexion et application) d’une application de programmation aux API REST d’authentification Adobe Pass ou aux SDK.

Les avantages de fournir des renseignements sur les clients sont les suivants :

* Possibilité d’activer correctement l’authentification de base d’accueil (HBA) dans le cas de certains types d’appareils et MVPD qui peuvent prendre en charge les HBA.
* La possibilité d’appliquer correctement des TTL dans le cas de certains types d’appareils (par exemple, configurer des TTL plus longues pour les sessions d’authentification sur les appareils connectés à la télévision).
* Possibilité d’agréger correctement les mesures commerciales dans des rapports répartis entre les types d’appareils à l’aide de la surveillance du service de droit (ESM).
* Débloque la possibilité d’appliquer correctement diverses règles métier (par exemple, dégradation) sur des types d’appareils spécifiques.

## Vue d’ensemble {#pass-client-info-overview}

Les informations sur le client se composent des éléments suivants :

* **Appareil** informations sur les attributs matériels et logiciels de l’appareil d’où l’utilisateur tente de consommer le contenu du programmeur.
* **Connexion** informations sur les attributs de connexion de l’appareil à partir duquel l’utilisateur se connecte aux services d’authentification Adobe Pass et/ou aux services de programmation (implémentations serveur à serveur, par exemple).
* **Application** informations sur l’application enregistrée à partir de laquelle l’utilisateur tente d’utiliser le contenu du programmeur.

Les informations du client sont un objet JSON créé avec les clés présentées dans le tableau suivant.

>[!NOTE]
>
>Les **clés** suivantes sont **obligatoires** à envoyer dans l’objet JSON d’informations client : **model**, **osName**.
>
>Les clés suivantes ont des valeurs **restreintes** : `primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure`.

|   | Clé | Restricted | Description | Valeurs possibles |
|---|---|---|---|---|
|            | primaryHardwareType | # Oui | Type de matériel principal de l’appareil. | # Les valeurs sont restreintes :                                                                     Appareil Photo                                                      DataCollectionTerminal                                                      Ordinateur de bureau                                                      EmbeddedNetworkModule                                                      Reader                                                      Console Jeux                                                      GeolocationTracker                                                      Lunettes                                                      MediaPlayer                                                      Téléphone portable                                                      PaymentTerminal                                                      PluginModem                                                      SetTopBox                                                      TV                                                      Tablette                                                      Zone réactive sans fil                                                      Montre-bracelet                                                      Inconnu |
| #mandatory | modèle | Non | Nom du modèle de l’appareil. | par ex. iPhone, SM-G930V, AppleTV, etc. |
|            | version | Non | Version de l’appareil. | par ex. 2.0.1, etc. |
|            | fabricant | Non | Entreprise/organisation de fabrication de l’appareil. | par ex. Samsung, LG, ZTE, Huawei, Motorola, Apple, etc. |
|            | marchand | Non | Société/organisation de vente de l’appareil. | par ex. Apple, Samsung, LG, Google, etc. |
| #mandatory | osName | # Oui | Nom du système d’exploitation (SE) de l’appareil. | # Les valeurs sont restreintes :                                                   Android                   SE CHROME                   Linux                   SE MAC                   OS X                   OpenBSD                   Roku OS                   Windows                   iOS                   tvOS                   webOS |
|            | osFamily | Oui | Nom du groupe du système d’exploitation (SE) de l’appareil. | # Les valeurs sont restreintes :                                                   Android                   BSD                   Linux                   Système d’exploitation PlayStation                   Roku OS                   Symbien                   Tizen                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | osVendor | Non | Fournisseur du système d’exploitation (SE) de l’appareil. | Amazon                   Apple                   Google                   LG                   Microsoft                   Mozilla                   Nintendo                   Nokia                   Roku                   Samsung                   Sony                   Projet Tizen |
|            | osVersion | Non | Version du système d’exploitation (SE) de l’appareil. | par ex. 10.2, 9.0.1, etc. |
|            | browserName | # Oui | Nom du navigateur. | # Les valeurs sont restreintes :                                                   Navigateur Android                   Chrome                   Edge                   Firefox                   Internet Explorer                   Opéra                   Safari                   SeaMonkey                   Navigateur Symbian |
|            | browserVendor | # Oui | Société/organisation de création du navigateur. | # Les valeurs sont restreintes :                                                   Amazon                   Apple                   Google                   Microsoft                   Motorola                   Mozilla                   Netscape                   Nintendo                   Nokia                   Samsung                   Sony Ericsson |
|            | browserVersion | Non | Version du navigateur de l’appareil. | par ex. 60.0.3112 |
|            | userAgent | Non | User Agent de l’appareil. | par ex. Mozilla/5.0 (Macintosh ; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, comme Gecko) Version/10.0.3 Safari/602.4.8 |
|            | displayWidth | Non | Largeur de l’écran physique de l’appareil. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | Non | Hauteur d’écran physique de l’appareil. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | Non | Densité en pixels de l’écran physique de l’appareil. | par ex. 294 |
|            | diagonalScreenSize | Non | Dimension diagonale de l’écran physique de l’appareil en pouces. | par ex. 5.5, 10.1 |
|            | connectionIp | Non | Adresse IP de l’appareil utilisée pour envoyer les requêtes HTTP. | par ex. 8.8.4.4 |
|            | connectionPort | Non | Port de l’appareil utilisé pour l’envoi de requêtes HTTP. | par ex. 53124 |
|            | connectionType | Non | Type de connexion réseau. | par ex. WiFi, LAN, 3G, 4G, 5G |
|            | connectionSecure | # Oui | Statut de sécurité de la connexion réseau. | # Les valeurs sont restreintes :                                                   true - dans le cas d’un réseau sécurisé                   false - dans le cas d’un point chaud public |
|            | applicationId | Non | Identifiant unique de l’application. | par ex. CNN |

## Références d’API {#api-ref}

Cette section présente l’API chargée de gérer les informations client lors de l’utilisation des API REST d’authentification Adobe Pass ou des SDK.

### API REST {#rest-api}

Les services d’authentification Adobe Pass prennent en charge la réception des informations du client des manières suivantes :

* En tant qu’en-tête **: « X-Device-Info »**
* En tant que **paramètre de requête : « device_info »**
* En tant que paramètre **post : « device_info »**

>[!IMPORTANT]
>
>Dans les trois scénarios, la payload de l’en-tête ou du paramètre doit être codée en **Base64 et en URL**.

**SDK**

#### JavaScript SDK {#js-sdk}

Le SDK JavaScript AccessEnabler crée par défaut un objet JSON d’informations client, qui sera transmis aux services d’authentification Adobe Pass, sauf si vous le remplacez.

Le SDK JavaScript AccessEnabler prend en charge **en remplaçant uniquement** la clé « applicationId » de l’objet JSON d’informations client via le paramètre d’options *applicationId* de [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options)).

>[!CAUTION]
>
>La valeur du paramètre `applicationId` doit être une valeur de chaîne en texte brut.
>Si l’application de programmation décide de transmettre l’application applicationId, le reste des clés d’informations client sera toujours calculé par le SDK JavaScript AccessEnabler.

#### SDK iOS/tvOS {#ios-tvos-sdk}

Le SDK AccessEnabler iOS/tvOS crée par défaut un objet JSON d’informations client, qui sera transmis aux services d’authentification Adobe Pass, sauf si vous le remplacez.

Le SDK AccessEnabler iOS/tvOS prend en charge **en remplaçant l’ensemble** de l’objet JSON d’informations client par le paramètre device_info de [setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setoptions).

>[!CAUTION]
>
>La valeur du paramètre *device_info* doit être une valeur **encodée en Base64** *NSString*.
>
>Si l’application Programmer décide de transmettre le *device_info*, toutes les clés d’informations client calculées par le SDK AccessEnabler iOS/tvOS seront remplacées. Par conséquent, il est très important de calculer et de transmettre les valeurs pour autant de clés que possible. Pour plus d’informations sur l’implémentation, consultez le tableau [Présentation](#pass-client-info-overview) et le guide pas à pas [iOS/tvOS](#ios-tvos).

#### SDK Android/FireOS {#and-fire-os-sdk}

Le SDK Android/FireOS `AccessEnabler` crée par défaut un objet JSON d’informations client, qui sera transmis aux services d’authentification Adobe Pass, sauf si vous le remplacez.

Le SDK Android/FireOS `AccessEnabler` prend en charge **le remplacement de l’ensemble** de l’objet JSON d’informations client par le paramètre `device_info` de [setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setOptions)&#39;s/[setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#fire_setOption).

>[!NOTE]
>
>La valeur du paramètre `device_info` doit être une valeur de chaîne codée en **Base64**.

>[!IMPORTANT]
>
>Si l’application Programmer décide de transmettre le `device_info`, toutes les clés d’informations client calculées par le SDK Android/FireOS `AccessEnabler` seront remplacées. Par conséquent, il est très important de calculer et de transmettre les valeurs pour autant de clés que possible. Pour plus d’informations sur la mise en œuvre, consultez le tableau [Présentation](#pass-client-info-overview) et le guide pas à pas [Android](#android) et [FireOS](#fire-tv).

## Livres de cuisine {#cookbooks}

Cette section présente un guide pas à pas pour la création de l’objet JSON d’informations client dans le cas de différents types d’appareils.

>[!IMPORTANT]
>
>Les clés qui sont marquées avec **!L’envoi des** est obligatoire.

### Android {#android}

Les informations sur le périphérique peuvent être structurées de la manière suivante :

|   | Clé | Source | Valeur (exemple) |
|---|---------------|-----------------------------|---------------|
| ! | modèle | Build.MODEL | GT-I9505 |
|   | marchand | Build.BRAND | samsung |
|   | fabricant | Build.MANUFACTURER | samsung |
| ! | version | Build.DEVICE | fusiller |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | codé en dur | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1 |

Les informations de connexion peuvent être construites comme suit :

|   | Clé | Source | Valeur (exemple) |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

Les informations d’application peuvent être structurées comme suit :

|   | Clé | Source | Valeur (exemple) |
|---|---------------|-----------|--------------|
|   | applicationId | codé en dur | CNN |

>[!IMPORTANT]
>
>Les informations relatives à l’appareil, à la connexion et à l’application doivent être ajoutées au même objet JSON. Par la suite, l’objet résultant doit être codé en **Base64**. En outre, dans le cas des API REST d’authentification Adobe Pass, la valeur doit être **encodée en URL**.

**Exemple de code**

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model",Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer",Build.MANUFACTURER);
          clientInformation.put("version",Build.DEVICE);
          clientInformation.put("osName","Android");
          clientInformation.put("osVersion",Build.VERSION.RELEASE);
          clientInformation.put("connectionType",connectionType);
          clientInformation.put("applicationId","CNN");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

>[!NOTE]
>
>**Ressources:**
>* classe publique [build](https://developer.android.com/reference/android/os/Build.html){target=_blank} dans la documentation destinée aux développeurs Java.

### FireTV {#fire-tv}

Les informations sur le périphérique peuvent être structurées de la manière suivante :

|   | Clé | Source | Valeur (par exemple) |
|---|---------------|-----------------------------|--------------|
| ! | modèle | Build.MODEL | AFTM |
|   | marchand | Build.BRAND | Amazon |
|   | fabricant | Build.MANUFACTURER | Amazon |
| ! | version | Build.DEVICE | montoya |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | codé en dur | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

Les informations de connexion peuvent être construites comme suit :

|   | Clé | Source | Valeur (exemple) |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

Les informations d’application peuvent être structurées comme suit :

|   | Clé | Source | Valeur (exemple) |
|---|---------------|-----------|--------------|
|   | applicationId | codé en dur | CNN |

>[!IMPORTANT]
>
>Les informations relatives à l’appareil, à la connexion et à l’application doivent être ajoutées au même objet JSON. Par la suite, l’objet résultant doit être codé en **Base64**. En outre, dans le cas des API REST d’authentification Adobe Pass, la valeur doit être **encodée en URL**.

>[!NOTE]
>
>**Ressources:**
>* classe publique [Build](https://developer.android.com/reference/android/os/Build.html){target=_blank} dans la documentation destinée aux développeurs Android.
>* [Identification des appareils FireTV](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

Les informations sur le périphérique peuvent être structurées de la manière suivante :

|   | Clé | Source | Valeur (exemple) |
|---|---------------|------------------------|--------------|
| ! | modèle | uname.machine | iPhone |
|   | marchand | codé en dur | Apple |
|   | fabricant | codé en dur | Apple |
| ! | version | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10,2 |

Les informations de connexion peuvent être construites comme suit :

|   | Clé | Source | Valeur (exemple) |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Reachability currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


Les informations d’application peuvent être structurées comme suit :

|   | Clé | Source | Valeur (exemple) |
|---|---------------|-----------|--------------|
|   | applicationId | codé en dur | CNN |

>[!IMPORTANT]
>
>Les informations relatives à l’appareil, à la connexion et à l’application doivent être ajoutées au même objet JSON. Par la suite, l’objet résultant doit être codé en Base64. En outre, dans le cas des API REST d’authentification Adobe Pass, la valeur doit être encodée en URL.

**Exemple de code**

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"CNN" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

>[!NOTE]
>
>**Ressources:**
>* [UIDevice &#x200B;](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
>* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
>* [À propos de l’accessibilité](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

Les informations sur le périphérique peuvent être structurées de la manière suivante :

| Clé | Source | Valeur (exemple) |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | modèle | codé en dur | « Roku » |
|     | marchand | ifDeviceInfo.GetModelDetails().VendorName | « Sharp », « Roku » |
|     | fabricant | ifDeviceInfo.GetModelDetails().VendorName | « Sharp », « Roku » |
| ! | version | ifDeviceInfo.GetModelDetails().ModelNumber |  »5303X » |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | codé en dur | « Roku » |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

Les informations de connexion peuvent être construites comme suit :

|   | Clé | Source | Valeur (exemple) |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | « WifiConnection », « WiredConnection » |
|   | connectionSecure | codé en dur | true si la connexion est câblée |

Les informations d’application peuvent être structurées comme suit :

|   | Clé | Source | Valeur (exemple) |
|---|---------------|-----------|--------------|
|   | applicationId | codé en dur | CNN |

>[!IMPORTANT]
>
>Les informations relatives à l’appareil, à la connexion et à l’application doivent être ajoutées au même objet JSON. Par la suite, l’objet résultant doit être codé en **Base64**. En outre, dans le cas des API REST d’authentification Adobe Pass, la valeur doit être encodée en URL.

>[!NOTE]
>
>Pour plus d’informations, voir [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

Les informations sur le périphérique peuvent être structurées de la manière suivante :

|   | Clé | Source | Valeur (exemple) |
|---|---|---|---|
| ! | modèle | EasClientDeviceInformation.SystemProductName |                 |
|   | marchand | codé en dur | Microsoft |
|   | fabricant | codé en dur | Microsoft |
| ! | version | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

Les informations de connexion peuvent être construites comme suit :

|   | Clé | Source | Exemple |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | « Aucune », « Spa », etc |

Les informations d’application peuvent être structurées comme suit :

| Clé | Source | Valeur (exemple) |
|---|---|---|
| applicationId | codé en dur | CNN |

>[!IMPORTANT]
>
>Les informations relatives à l’appareil, à la connexion et à l’application doivent être ajoutées au même objet JSON. Par la suite, l’objet résultant doit être codé en **Base64**. En outre, dans le cas des API REST d’authentification Adobe Pass, la valeur doit être **encodée en URL**.

**Ressources**

* [EasClientDeviceInformation, classe](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [Classe DisplayInformation](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
