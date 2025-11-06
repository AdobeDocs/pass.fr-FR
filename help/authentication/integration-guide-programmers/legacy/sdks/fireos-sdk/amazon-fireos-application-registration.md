---
title: Enregistrement de l’application Amazon FireOS
description: Enregistrement de l’application Amazon FireOS
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# Enregistrement de l’application Amazon FireOS (héritée) {#amazon-fireos-application-registration}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>

## Introduction {#intro}

À compter de la version 3.0 du SDK FireOS AccessEnabler, nous modifions le mécanisme d’authentification avec les serveurs Adobe. Au lieu d’utiliser une clé publique et un système secret pour signer l’ID du demandeur, nous introduisons le concept d’une chaîne d’instruction logicielle qui peut être utilisée pour obtenir un jeton d’accès qui est ensuite utilisé pour tous les appels que le SDK effectue à nos serveurs. En plus d&#39;une déclaration logicielle, vous devrez également créer un lien profond pour votre application.

Pour plus d’informations, voir [Présentation de l’enregistrement client dynamique](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Qu’est-ce qu’une déclaration de logiciel ? {#what}

Une instruction logicielle est un jeton JWT contenant des informations sur votre application. Chaque application doit avoir une déclaration logicielle unique qui est utilisée par nos serveurs pour identifier l&#39;application dans le système Adobe. L’instruction logicielle doit être transmise lorsque vous initialisez le SDK AccessEnabler et elle sera utilisée pour enregistrer l’application auprès d’Adobe. Lors de l’enregistrement, le SDK recevra un identifiant client et un secret client qui seront utilisés pour obtenir un jeton d’accès. Tout appel du SDK à nos serveurs nécessite un jeton d’accès valide. Le SDK est chargé d’enregistrer l’application, d’obtenir et d’actualiser le jeton d’accès.

**Remarque :** les instructions logicielles sont spécifiques à l&#39;application et une instruction logicielle individuelle ne peut pas être utilisée pour plusieurs applications. Notez que cela s’applique également aux applications qui offrent un accès à plusieurs canaux.

## Comment obtenir une déclaration logicielle ? {#how-to}

### Si vous avez accès au tableau de bord Adobe TVE :

1. Ouvrez votre navigateur et accédez à `https://experience.adobe.com/#/pass/authentication`.

1. Accédez à la section **[!UICONTROL Channels]** , puis sélectionnez votre canal.

1. Accédez à l’onglet **[!UICONTROL Registered Applications]** .

1. Cliquez sur **[!UICONTROL Add new application]**.

1. Attribuez un nom et une version à votre application, puis sélectionnez les plateformes sur lesquelles elle sera disponible (par exemple, Android).

1. Fournissez une **[!UICONTROL Domain Name]** en choisissant parmi une liste de domaines déjà configurés pour votre programmeur.

1. Envoyez vos modifications au serveur , puis revenez à l’onglet **[!UICONTROL Registered Applications]** de votre canal.

   Vous devriez voir une liste comportant toutes les applications enregistrées.

1. Cliquez sur **[!UICONTROL Download]** dans l&#39;application que vous venez de créer.

   Vous devrez peut-être attendre quelques minutes avant que votre déclaration logicielle soit prête à être téléchargée.

   Un fichier texte est téléchargé. Utilisez son contenu comme déclaration de logiciel.

Pour plus d’informations, voir [Dynamic Client Registration Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Si vous n’avez pas accès au tableau de bord Adobe TVE :

Envoyez un ticket à [tve-support@adobe.com](mailto:tve-support@adobe.com). Incluez toutes les informations nécessaires, notamment le canal, le nom de l’application, la version et les plateformes. Un membre de notre équipe d’assistance créera alors une déclaration logicielle pour vous.

## Utilisation de la déclaration de logiciel {#use}

Une fois que vous avez obtenu votre instruction logicielle, vous devez la transmettre en tant que paramètre dans le constructeur Access Enabler. Adobe recommande d’héberger la déclaration logicielle sur un emplacement distant. De cette façon, vous pouvez facilement révoquer et modifier la déclaration du logiciel sans publier une nouvelle version de votre application.

## Utilisation de la déclaration de logiciel {#use-both}

Dans le fichier de ressources de votre application, `strings.xml` ajoutez le code suivant :

```XML
<string name="software_statement">softwarestatement value</string>
```
