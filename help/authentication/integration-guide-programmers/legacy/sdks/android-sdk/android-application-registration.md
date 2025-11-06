---
title: Enregistrement de l’application Android
description: Enregistrement de l’application Android
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 0%

---

# Enregistrement de l’application Android (hérité) {#android-application-registration}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Introduction {#intro}

À compter de la version 3.0 du SDK AccessEnabler d’Android, nous modifions le mécanisme d’authentification avec les serveurs d’Adobe. Au lieu d’utiliser une clé publique et un système secret pour signer l’ID du demandeur, nous introduisons le concept d’une chaîne d’instruction logicielle qui peut être utilisée pour obtenir un jeton d’accès qui est ensuite utilisé pour tous les appels que le SDK effectue à nos serveurs. Outre une déclaration de logiciel, vous devrez également créer un lien profond pour votre application.

Pour plus d’informations, voir [Présentation de l’enregistrement client dynamique](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Qu’est-ce qu’une déclaration de logiciel ? {#what}

Une instruction logicielle est un jeton JWT contenant des informations sur votre application. Chaque application doit disposer d’une instruction logicielle unique utilisée par nos serveurs pour identifier l’application dans le système Adobe.

L’instruction logicielle doit être transmise lorsque vous initialisez le SDK `AccessEnabler`. Il est utilisé pour enregistrer l’application auprès d’Adobe. Lors de l’enregistrement, le SDK reçoit un identifiant client et un secret client, qui est utilisé pour obtenir un jeton d’accès. Tout appel du SDK aux serveurs Adobe nécessite un jeton d’accès valide. Le SDK est chargé d’enregistrer l’application, d’obtenir et d’actualiser le jeton d’accès.

>[!NOTE]
>
>Les instructions logicielles sont spécifiques à l&#39;application et une instruction logicielle individuelle ne peut pas être utilisée pour plusieurs applications. Notez que les instructions logicielles au niveau du programmeur ont la même contrainte. Elles ne peuvent être utilisées que pour une seule application, que ce soit un canal unique ou multicanal.

## Comment obtenir une déclaration de logiciel {#how-to-get-ss}

Voici quelques moyens d’obtenir une déclaration logicielle.

### Si vous avez accès au tableau de bord Adobe TVE

1. Ouvrez votre navigateur et accédez au tableau de bord Adobe Pass TVE [](https://experience.adobe.com/#/pass/authentication).

1. Accédez à **[!UICONTROL Channels]** section, puis sélectionnez votre canal.

1. Accédez à l’onglet **[!UICONTROL Registered Applications]** .

1. Cliquez sur **[!UICONTROL Add new application]**.

1. Nommez l’application et spécifiez une version.

1. Sélectionnez les plateformes sur lesquelles l&#39;application sera disponible (Android dans ce cas).

1. Fournissez une **[!UICONTROL Domain Name]** en choisissant parmi une liste de domaines déjà configurés pour votre programmeur.

1. Envoyez vos modifications au serveur , puis revenez à l’onglet **[!UICONTROL Registered Applications]** de votre canal.

   Vous devriez voir une liste comportant toutes les applications enregistrées. Sélectionnez **[!UICONTROL Download]** sur l’application que vous avez créée. Vous devrez peut-être attendre quelques minutes avant que votre déclaration logicielle soit prête à être téléchargée.

   Un fichier texte est téléchargé. Utilisez son contenu comme déclaration de logiciel.

Pour plus d’informations, voir [Dynamic Client Registration Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Si vous n’avez pas accès au tableau de bord Adobe TVE

Envoyez un ticket à `tve-support@adobe.com`. Incluez les informations nécessaires telles que le canal, le nom de l’application, la version et les plateformes. Un membre de notre équipe d’assistance va créer un énoncé de logiciel pour vous.

## Utilisation de la déclaration de logiciel {#how-to-use-ss}

Une fois que vous avez obtenu votre instruction logicielle, vous devez la transmettre en tant que paramètre dans le constructeur Access Enabler. Nous vous recommandons d’héberger la déclaration logicielle sur un emplacement distant. De cette façon, vous pouvez facilement révoquer et modifier la déclaration du logiciel sans publier une nouvelle version de votre application.

## Créer et utiliser un lien profond pour votre application {#create}

Sur Android, utilisez comme valeur de lien profond l’inverse du nom de domaine sélectionné lors de la création de l’instruction logicielle

Le lien profond créé doit avoir une valeur unique sur l’appareil Android. Lorsque plusieurs applications utilisent la même valeur de lien profond, les flux d’authentification et de déconnexion interfèrent.

## Utilisation de la déclaration du logiciel et du lien profond {#use-both}

Dans le fichier de ressources de votre application, `strings.xml` ajoutez le code suivant :

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
