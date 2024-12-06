---
title: En-tête - AP-Device-Identifier
description: API REST V2 - En-tête - AP-Device-Identifier
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 1%

---

# En-tête - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b>AP-Device-Identifier</b> contient l’identifiant de l’appareil de diffusion en continu tel qu’il a été créé par l’application cliente.

## Syntaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b> : &lt;type&gt; &lt;identifiant&gt;</td>
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

<b>&lt;type></b>

Type d’identifiant de l’appareil.

Il n’existe qu’un seul type pris en charge, comme illustré ci-dessous.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Type</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>empreinte</td>
      <td>
            L’identifiant de l’appareil est un identifiant stable et unique créé et géré par l’application cliente.
            <br/>
            L’application cliente doit empêcher les modifications de valeur provoquées par des actions de l’utilisateur telles que la désinstallation, la réinstallation ou les mises à niveau de l’application.
      </td>
   </tr>
</table>


<b>&lt;identifier></b>

La valeur `Base64-encoded` de l’identifiant de l’appareil.

## Exemple {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## Cookbooks {#cookbooks}

>[!IMPORTANT]
>
> Les ressources de documentation sont fournies à titre de référence.
>
> Les ressources de documentation ne sont pas exhaustives et peuvent nécessiter des modifications supplémentaires dans votre projet.
> 
> Quelle que soit votre mise en oeuvre, l’en-tête `AP-Device-Identifier` doit contenir une valeur formatée comme décrit dans la section [Directives](#directives) .

### Navigateurs {#browsers}

Pour créer l’en-tête `AP-Device-Identifier` pour les appareils s’exécutant dans un navigateur, votre application cliente doit calculer un identifiant stable et unique en fonction des données disponibles, telles que les données de navigateur, d’appareil ou spécifiques à l’utilisateur.

_(*) Nous vous recommandons d’intégrer une bibliothèque ou un service qui fournit un mécanisme d’empreinte digitale de navigateur ou d’appareil._

### Périphériques mobiles {#mobile-devices}

#### iOS et iPadOS {#ios-ipados}

Pour créer l’en-tête `AP-Device-Identifier` pour les appareils exécutant [iOS ou iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes), vous pouvez vous référer aux documents suivants :

* Documentation destinée aux développeurs Apple pour [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Nous vous recommandons d’appliquer une fonction de hachage SHA-256 sur la valeur fournie par le système d’exploitation._

#### Android {#android}

Pour créer l’en-tête `AP-Device-Identifier` pour les appareils exécutant [Android](https://developer.android.com/about/versions), vous pouvez vous référer aux documents suivants :

* Documentation destinée aux développeurs Android pour [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Nous vous recommandons d’appliquer une fonction de hachage SHA-256 sur la valeur fournie par le système d’exploitation._

### Périphériques connectés à la télévision {#tv-connected-devices}

#### tvOS {#tvos}

Pour créer l’en-tête `AP-Device-Identifier` pour les appareils exécutant [tvOS](https://developer.apple.com/documentation/tvos-release-notes), vous pouvez vous référer aux documents suivants :

* Documentation destinée aux développeurs Apple pour [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Nous vous recommandons d’appliquer une fonction de hachage SHA-256 sur la valeur fournie par le système d’exploitation._

#### SE Fire {#fireos}

Pour créer l’en-tête `AP-Device-Identifier` pour les appareils exécutant [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html), vous pouvez vous référer aux documents suivants :

* Documentation destinée aux développeurs Android pour [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Nous vous recommandons d’appliquer une fonction de hachage SHA-256 sur la valeur fournie par le système d’exploitation._

#### Roku OS {#rokuos}

Pour créer l’en-tête `AP-Device-Identifier` pour les appareils exécutant [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md), vous pouvez vous référer aux documents suivants :

* Documentation destinée aux développeurs Roku pour [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string).

_(*) Nous vous recommandons d’appliquer une fonction de hachage SHA-256 sur la valeur fournie par le système d’exploitation._
