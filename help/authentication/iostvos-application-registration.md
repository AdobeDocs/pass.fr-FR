---
title: Enregistrement de l’application iOS/tvOS
description: Enregistrement de l’application iOS/tvOS
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---

# Enregistrement de l’application iOS/tvOS {#iostvos-application-registration}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#Intro}

À compter de la version 3.0 du SDK iOS/tvOS AccessEnabler, nous sommes en train de modifier le mécanisme d’authentification avec les serveurs d’Adobe. Au lieu d’utiliser une clé publique et un système secret pour signer le requestorID, nous introduisons le concept d’une chaîne d’instruction logicielle qui peut être utilisée pour obtenir un jeton d’accès utilisé ultérieurement pour tous les appels effectués par le SDK vers nos serveurs. Outre une instruction logicielle, vous aurez également besoin d’un modèle d’URL personnalisé pour votre application.

Pour plus d’informations, voir [Enregistrement du client dynamique](/help/authentication/dynamic-client-registration.md)

## Qu’est-ce qu’une déclaration logicielle ? {#Soft_state}

Une instruction logicielle est un jeton JWT qui contient des informations sur votre application. Chaque application doit avoir une instruction logicielle unique utilisée par nos serveurs pour identifier l’application dans le système d’Adobe. L’instruction logicielle doit être transmise lorsque vous initialisez le SDK AccessEnabler et elle sera utilisée pour enregistrer l’application avec Adobe. Lors de l’enregistrement, le SDK reçoit un identifiant client et un secret client qui seront utilisés pour obtenir un jeton d’accès. Tout appel du SDK à nos serveurs nécessite un jeton d’accès valide. Le SDK est chargé d’enregistrer l’application, d’obtenir et d’actualiser le jeton d’accès.

**Remarque :** Une instruction logicielle est spécifique à l’application et la même instruction logicielle ne peut pas être utilisée sur plusieurs applications. Notez que les instructions logicielles au niveau du programmeur suivent également la même chose, c’est-à-dire qu’elles ne peuvent être utilisées que pour une seule application, qu’il s’agisse d’un seul canal ou d’un multicanal. Cette limitation s’applique également au schéma personnalisé.

## Comment obtenir un relevé logiciel ? {#obtain}

### Si vous avez accès au tableau de bord TVE d’Adobe :

- Ouvrez votre navigateur et accédez à <https://console.auth.adobe.com>
- Accédez à `Channels` et sélectionnez votre canal.
- Accédez à `Registered Applications` Onglet.
- Cliquez sur `Add new application`.
- Attribuez un nom et une version à votre application et sélectionnez les plateformes sur lesquelles elle sera disponible. iOS/tvOS dans notre cas.
- Envoyez vos modifications au serveur, puis revenez à l’onglet Applications enregistrées de votre canal.
- Vous devriez voir une liste contenant toutes les applications enregistrées. Cliquez sur le bouton   `Download` sur l’application que vous venez de créer. Vous devrez peut-être attendre quelques minutes avant que votre déclaration logicielle ne soit prête à être téléchargée.
- Un fichier texte sera téléchargé. Utilisez son contenu comme déclaration logicielle.

Pour plus d’informations, voir [Gestion dynamique de l&#39;enregistrement des clients](/help/authentication/dynamic-client-registration-management.md).

### Si vous n’avez pas accès au tableau de bord TVE d’Adobe :

Envoyer un ticket à <tve-support@adobe.com>. Veuillez inclure toutes les informations nécessaires, telles que le canal, le nom de l’application, la version et les plateformes, et quelqu’un de notre équipe d’assistance créera une déclaration logicielle pour vous.

## Comment utiliser l’instruction logicielle ? {#use}

Après avoir obtenu votre instruction logicielle, vous devez la transmettre en tant que paramètre dans le constructeur Access Enabler. Nous vous recommandons d’héberger l’instruction logicielle sur un emplacement distant. Ainsi, vous pouvez facilement révoquer et modifier l’instruction logicielle sans publier une nouvelle version de votre application.

## Génération d’un modèle d’URL personnalisé pour votre application {#generating}

### Si vous avez accès au tableau de bord TVE d’Adobe :

- Ouvrez votre navigateur et accédez à <https://console.auth.adobe.com>
- Accédez à `Channels` et sélectionnez votre canal.
- Accédez à `Custom Schemes` Onglet.
- Cliquez sur `Generate a new custom scheme`.
- Un nouveau modèle personnalisé sera généré pour votre application. Ex : `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- Envoyez vos modifications au serveur.

### Si vous n’avez pas accès au tableau de bord TVE d’Adobe :

Envoyer un ticket à <tve-support@adobe.com>. Veuillez inclure l’ID de canal et quelqu’un de notre équipe d’assistance créera un schéma personnalisé pour vous.

## Utilisation du schéma personnalisé {#use_custom}

Dans le de votre application `info.plist` ajoutez le code suivant :

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>adbe.u-XFXJeTSDuJiIQs0HVRAg</string> // replace this with your custom scheme
            </array>
        </dict>
    </array>
```
