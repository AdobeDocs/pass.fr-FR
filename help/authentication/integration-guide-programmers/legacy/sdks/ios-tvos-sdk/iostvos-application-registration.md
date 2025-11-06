---
title: Enregistrement de l’application iOS/tvOS
description: Enregistrement de l’application iOS/tvOS
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 0%

---


# Enregistrement de l’application iOS/tvOS (hérité) {#iostvos-application-registration}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Introduction {#Intro}

À compter de la version 3.0 du SDK AccessEnabler d’iOS/tvOS, nous modifions le mécanisme d’authentification avec les serveurs Adobe. Au lieu d’utiliser une clé publique et un système secret pour signer l’ID du demandeur, nous introduisons le concept d’une chaîne d’instruction logicielle qui peut être utilisée pour obtenir un jeton d’accès qui est ensuite utilisé pour tous les appels que le SDK effectue à nos serveurs. Outre une déclaration de logiciel, vous aurez également besoin d’un schéma d’URL personnalisé pour votre application.

Pour plus d’informations, voir [Présentation de l’enregistrement client dynamique](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Qu’est-ce qu’une déclaration de logiciel ? {#Soft_state}

Une instruction logicielle est un jeton JWT contenant des informations sur votre application. Chaque application doit avoir une déclaration logicielle unique qui est utilisée par nos serveurs pour identifier l&#39;application dans le système Adobe. L’instruction logicielle doit être transmise lorsque vous initialisez le SDK AccessEnabler et elle sera utilisée pour enregistrer l’application auprès d’Adobe. Lors de l’enregistrement, le SDK recevra un identifiant client et un secret client qui seront utilisés pour obtenir un jeton d’accès. Tout appel du SDK à nos serveurs nécessite un jeton d’accès valide. Le SDK est chargé d’enregistrer l’application, d’obtenir et d’actualiser le jeton d’accès.

**Remarque :** une instruction logicielle est spécifique à l&#39;application et la même instruction logicielle ne peut pas être utilisée sur plusieurs applications. Notez que les instructions logicielles au niveau du programmeur suivent le même principe, c’est-à-dire qu’elles ne peuvent être utilisées que pour une seule application, à canal unique ou multicanal. Cette limitation s’applique également aux schémas personnalisés.

## Comment obtenir une déclaration logicielle ? {#obtain}

### Si vous avez accès au tableau de bord Adobe TVE :

- Ouvrez votre navigateur et accédez à <https://experience.adobe.com/#/pass/authentication>
- Accédez à `Channels` section et sélectionnez votre canal.
- Accédez à l’onglet `Registered Applications` .
- Cliquez sur `Add new application`.
- Attribuez un nom et une version à votre application, puis sélectionnez le   les plateformes sur lesquelles il sera disponible. iOS/tvOS dans notre cas.
- Envoyez vos modifications au serveur, puis revenez à l’onglet Applications enregistrées de votre canal.
- Vous devriez voir une liste comportant toutes les applications enregistrées. Cliquez sur le lien   `Download` bouton sur l&#39;application que vous venez de créer. Vous devrez peut-être attendre quelques minutes avant que votre déclaration logicielle soit prête à être téléchargée.
- Un fichier texte sera téléchargé. Utilisez son contenu comme déclaration logicielle.

Pour plus d&#39;informations, voir [Dynamic Client Registration Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Si vous n’avez pas accès au tableau de bord Adobe TVE :

Envoyez un ticket à <tve-support@adobe.com>. Veuillez indiquer toutes les informations nécessaires telles que le canal, le nom de l’application, la version et les plateformes. Un membre de notre équipe d’assistance créera alors une déclaration logicielle pour vous.

## Comment utiliser la déclaration du logiciel ? {#use}

Une fois que vous avez obtenu votre instruction logicielle, vous devez la transmettre en tant que paramètre dans le constructeur Access Enabler. Nous vous recommandons d’héberger la déclaration logicielle sur un emplacement distant. De cette façon, vous pouvez facilement révoquer et modifier la déclaration logicielle sans publier une nouvelle version de votre application.

## Génération d’un schéma d’URL personnalisé pour votre application {#generating}

### Si vous avez accès au tableau de bord Adobe TVE :

- Ouvrez votre navigateur et accédez à <https://experience.adobe.com/#/pass/authentication>
- Accédez à `Channels` section et sélectionnez votre canal.
- Accédez à l’onglet `Custom Schemes` .
- Cliquez sur `Generate a new custom scheme`.
- Un nouveau schéma personnalisé sera généré pour votre application. Ex : `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- Envoyez vos modifications au serveur.

### Si vous n’avez pas accès au tableau de bord Adobe TVE :

Envoyez un ticket à <tve-support@adobe.com>. Veuillez indiquer l’ID du canal. Une personne de notre équipe d’assistance créera un schéma personnalisé pour vous.

## Utilisation du schéma personnalisé {#use_custom}

Dans le fichier `info.plist` de votre application, ajoutez le code suivant :

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
