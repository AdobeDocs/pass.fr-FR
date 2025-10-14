---
title: En-Tête - X-Device-Info
description: API REST V2 - En-tête - X-Device-Info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 42df16e34783807e1b5eb1a12ca9db92f4e4c161
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 3%

---

# En-Tête - X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b>X-Device-Info</b> contient les informations client (appareil, connexion et application) liées à l’appareil de diffusion en continu actuel et est utilisé pour déterminer les règles spécifiques à la plateforme que les MVPD peuvent appliquer.

## Syntaxe {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b> : &lt;device_information&gt;</td>
   </tr>
   <tr>
      <td>Type d’en-tête</td>
      <td>En-tête de requête</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Non</td>
   </tr>
</table>

## Directives {#directives}

<b>&lt;device_information></b>

Valeur `Base64-encoded` de l’élément JSON contenant au moins les attributs marqués comme requis par le tableau suivant.

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Présence</th>
        <th style="background-color: #EFF2F7; width: 15%;">Clé</th>
        <th style="background-color: #EFF2F7;">Description</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Restricted</th>
        <th style="background-color: #EFF2F7;">Valeurs possibles</th>
    </tr>
    <tr>
        <td></td>
        <td>primaryHardwareType</td>
        <td>Type de matériel principal de l’appareil.</td>
        <td>&check;</td>
        <td>
            Les valeurs sont limitées :
            <ul>
                <li>Appareil Photo</li>
                <li>DataCollectionTerminal</li>
                <li>Bureau</li>
                <li>EmbeddedNetworkModule</li>
                <li>Reader</li>
                <li>Console Jeux</li>
                <li>GeolocationTracker</li>
                <li>Lunettes</li>
                <li>MediaPlayer</li>
                <li>Téléphone portable</li>
                <li>PaymentTerminal</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>Tablette</li>
                <li>Zone réactive sans fil</li>
                <li>Montre-bracelet</li>
                <li>Inconnu</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>obligatoire</i></td>
        <td>modèle</td>
        <td>Nom du modèle de l’appareil.</td>
        <td></td>
        <td>par ex. iPhone, SM-G930V, AppleTV, etc.</td>
    </tr>
    <tr>
        <td><i>obligatoire</i></td>
        <td>version</td>
        <td>Version de l’appareil.</td>
        <td></td>
        <td>par ex. 2.0.1, etc.</td>
    </tr>
    <tr>
        <td></td>
        <td>fabricant</td>
        <td>Entreprise/organisation de fabrication de l’appareil.</td>
        <td></td>
        <td>par ex. Samsung, LG, ZTE, Huawei, Motorola, Apple, etc.</td>
    </tr>
    <tr>
        <td></td>
        <td>marchand</td>
        <td>Société/organisation de vente de l’appareil.</td>
        <td></td>
        <td>par ex. Apple, Samsung, LG, Google, etc.</td>
    </tr>
    <tr>
        <td><i>obligatoire</i></td>
        <td>osName</td>
        <td>Nom du système d’exploitation (SE) de l’appareil.</td>
        <td>&check;</td>
        <td>
            Les valeurs sont limitées :
            <ul>
                <li>Android</li>
                <li>SE CHROME</li>
                <li>Linux</li>
                <li>Mac OS</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osFamily</td>
        <td>Nom du groupe du système d’exploitation (SE) de l’appareil.</td>
        <td>&check;</td>
        <td>
            Les valeurs sont limitées :
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>Système d’exploitation PlayStation</li>
                <li>Roku OS</li>
                <li>Symbien</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osVendor</td>
        <td>Fournisseur du système d’exploitation de l’appareil.</td>
        <td>&check;</td>
        <td>
            Les valeurs sont limitées :
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Projet Tizen</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>obligatoire</i></td>
        <td>osVersion</td>
        <td>Version du système d’exploitation (SE) de l’appareil.</td>
        <td></td>
        <td>par ex. 10.2, 9.0.1, etc.</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>Nom du navigateur.</td>
        <td>&check;</td>
        <td>
            Les valeurs sont limitées :
            <ul>
                <li>Navigateur Android</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opéra</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Navigateur Symbian</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>Société/organisation de création du navigateur.</td>
        <td>&check;</td>
        <td>
            Les valeurs sont limitées :
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>Version du navigateur de l’appareil.</td>
        <td></td>
        <td>par ex. 60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>User agent de l’appareil.</td>
        <td></td>
        <td>par ex. Mozilla/5.0 (Macintosh ; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, comme Gecko) Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displayWidth</td>
        <td>Largeur de l’écran physique de l’appareil.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayHeight</td>
        <td>Hauteur d’écran physique de l’appareil.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPpi</td>
        <td>Densité en pixels de l’écran physique de l’appareil.</td>
        <td></td>
        <td>par ex. 294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>Dimension diagonale de l’écran physique de l’appareil en pouces.</td>
        <td></td>
        <td>par ex. 5.5, 10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>Adresse IP de l’appareil utilisée pour envoyer les requêtes HTTP.</td>
        <td></td>
        <td>par ex. 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>Port de l’appareil utilisé pour l’envoi de requêtes HTTP.</td>
        <td></td>
        <td>par ex. 53124</td>
    </tr>
    <tr>
        <td><i>obligatoire</i></td>
        <td>connectionType</td>
        <td>Type de connexion réseau.</td>
        <td></td>
        <td>par ex. WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>Statut de sécurité de la connexion réseau.</td>
        <td>&check;</td>
        <td>
            Les valeurs sont limitées :
            <ul>
                <li>true - dans le cas d’un réseau sécurisé</li>
                <li>false - dans le cas d’un point chaud public</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>Identifiant unique de l’application.</td>
        <td></td>
        <td>par ex. REF30</td>
    </tr>
</table>


## Exemples {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```

## Livres de cuisine {#cookbooks}

>[!IMPORTANT]
> 
> Les fragments de code et les ressources de documentation sont fournis à des fins de référence.
> 
> Les fragments de code ne sont pas exhaustifs et peuvent nécessiter des modifications supplémentaires pour fonctionner dans votre projet.
>
> Quelle que soit votre implémentation actuelle, l’en-tête de `X-Device-Info` doit contenir une valeur formatée comme décrit dans la section [&#x200B; Directives &#x200B;](#directives).

### Navigateurs {#browsers}

Pour les applications clientes s’exécutant dans un navigateur, l’en-tête `X-Device-Info` peut être omis, car le navigateur envoie automatiquement un ensemble minimal d’informations requises dans l’en-tête `User-Agent`.

Vous pouvez toujours utiliser l’en-tête `X-Device-Info` pour fournir des informations supplémentaires sur l’appareil, la connexion et l’application, au cas où votre application cliente intègre une bibliothèque ou un service qui fournit un mécanisme d’identification de l’appareil.

### Appareils mobiles {#mobile-devices}

#### iOS et iPadOS {#ios-ipados}

Pour créer l’en-tête `X-Device-Info` pour les appareils exécutant [iOS ou iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes), vous pouvez vous reporter aux documents suivants et au fragment de code ci-dessous :

* Documentation Apple destinée aux développeurs pour [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Documentation Apple destinée aux développeurs pour [accessibilité](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Documentation manuelle Linux pour [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

Les informations sur le périphérique peuvent être structurées de la manière suivante :

| Clé | Source | Valeur (exemple) |
|---------------|------------------------|-----------------|
| modèle | uname.machine | iPhone |
| marchand | codé en dur | Apple |
| fabricant | codé en dur | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10,2 |

Les informations de connexion peuvent être construites comme suit :

| Clé | Source | Valeur (exemple) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Reachability currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |


Les informations d’application peuvent être structurées comme suit :

| Clé | Source | Valeur (exemple) |
|---------------|-----------|-----------------|
| applicationId | codé en dur | REF30 |

#### Android {#android}

Pour créer l’en-tête `X-Device-Info` pour les appareils exécutant [Android](https://developer.android.com/about/versions), vous pouvez vous reporter aux documents suivants et au fragment de code ci-dessous :

* Documentation Android destinée aux développeurs pour la classe [Build](https://developer.android.com/reference/android/os/Build.html).

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
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

Les informations sur le périphérique peuvent être structurées de la manière suivante :

| Clé | Source | Valeur (exemple) |
|---------------|-----------------------------|-----------------|
| modèle | Build.MODEL | GT-I9505 |
| marchand | Build.BRAND | samsung |
| fabricant | Build.MANUFACTURER | samsung |
| version | Build.DEVICE | fusiller |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | codé en dur | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1 |

Les informations de connexion peuvent être construites comme suit :

| Clé | Source | Valeur (exemple) |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

Les informations d’application peuvent être structurées comme suit :

| Clé | Source | Valeur (exemple) |
|---------------|-----------|-----------------|
| applicationId | codé en dur | REF30 |

### Appareils connectés au téléviseur {#tv-connected-devices}

#### tvOS {#tvos}

Pour créer l’en-tête `X-Device-Info` pour les appareils exécutant [tvOS](https://developer.apple.com/documentation/tvos-release-notes), vous pouvez vous reporter aux documents suivants et au fragment de code ci-dessous :

* Documentation Apple destinée aux développeurs pour [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Documentation Apple destinée aux développeurs pour [accessibilité](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Documentation manuelle Linux pour [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

Les informations sur le périphérique peuvent être structurées de la manière suivante :

| Clé | Source | Valeur (exemple) |
|---------------|------------------------|-----------------|
| modèle | uname.machine | AppleTV |
| marchand | codé en dur | Apple |
| fabricant | codé en dur | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10,2 |

Les informations de connexion peuvent être construites comme suit :

| Clé | Source | Valeur (exemple) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Reachability currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |

Les informations d’application peuvent être structurées comme suit :

| Clé | Source | Valeur (exemple) |
|---------------|-----------|-----------------|
| applicationId | codé en dur | REF30 |

#### Système d’exploitation Fire {#fireos}

Pour créer l’en-tête `X-Device-Info` pour les appareils exécutant [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html), vous pouvez vous reporter aux documents suivants :

* Documentation Android destinée aux développeurs pour la classe [Build](https://developer.android.com/reference/android/os/Build.html).
* Documentation Amazon destinée aux développeurs pour [Identification des périphériques Fire TV](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html).

Les informations sur le périphérique peuvent être structurées de la manière suivante :

| Clé | Source | Valeur (exemple) |
|---------------|-----------------------------|-----------------|
| modèle | Build.MODEL | AFTM |
| marchand | Build.BRAND | Amazon |
| fabricant | Build.MANUFACTURER | Amazon |
| version | Build.DEVICE | montoya |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | codé en dur | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

Les informations de connexion peuvent être construites comme suit :

| Clé | Source | Valeur (exemple) |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

Les informations d’application peuvent être structurées comme suit :

| Clé | Source | Valeur (exemple) |
|---------------|-----------|-----------------|
| applicationId | codé en dur | REF30 |

#### Roku OS {#rokuos}

Pour créer l’en-tête `X-Device-Info` pour les appareils exécutant [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md), vous pouvez vous reporter aux documents suivants :

* Documentation Roku destinée aux développeurs pour [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md).

Les informations sur le périphérique peuvent être structurées de la manière suivante :

| Clé | Source | Valeur (exemple) |
|---------------|--------------------------------------------|-----------------|
| modèle | codé en dur | « Roku » |
| marchand | ifDeviceInfo.GetModelDetails().VendorName | « Sharp », « Roku » |
| fabricant | ifDeviceInfo.GetModelDetails().VendorName | « Sharp », « Roku » |
| version | ifDeviceInfo.GetModelDetails().ModelNumber |  »5303X » |
| displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | codé en dur | « Roku » |
| osVersion | ifDeviceInfo.getVersion() |                 |

Les informations de connexion peuvent être construites comme suit :

| Clé | Source | Valeur (exemple) |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | « WifiConnection », « WiredConnection » |
| connectionSecure | codé en dur | true si la connexion est câblée |

Les informations d’application peuvent être structurées comme suit :

| Clé | Source | Valeur (exemple) |
|---------------|-----------|-----------------|
| applicationId | codé en dur | REF30 |

### Autres {#others}

Pour les plateformes d’appareils non couvertes dans la documentation, les informations client (appareil, connexion et application) doivent être liées à tous les attributs matériels et de système d’exploitation disponibles, généralement spécifiés dans les manuels relatifs au matériel et au système d’exploitation de l’appareil.
