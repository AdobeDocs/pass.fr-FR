---
title: Enregistrement d’une application Android
description: Enregistrement d’une application Android
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Enregistrement d’une application Android {#android-application-registration}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

À compter de la version 3.0 du SDK Android AccessEnabler, nous sommes en train de modifier le mécanisme d’authentification avec les serveurs d’Adobe. Au lieu d’utiliser une clé publique et un système secret pour signer le requestorID, nous introduisons le concept d’une chaîne d’instruction logicielle qui peut être utilisée pour obtenir un jeton d’accès utilisé ultérieurement pour tous les appels effectués par le SDK vers nos serveurs. Outre une instruction logicielle, vous devez également créer un lien profond pour votre application.

Pour plus d’informations, voir [Présentation de l’enregistrement du client dynamique](./dcr-api/dynamic-client-registration-overview.md).

## Qu’est-ce qu’une déclaration logicielle ? {#what}

Une instruction logicielle est un jeton JWT qui contient des informations sur votre application. Chaque application doit disposer d’une instruction logicielle unique utilisée par nos serveurs pour identifier l’application dans le système d’Adobe.

L’instruction logicielle doit être transmise lorsque vous initialisez le SDK `AccessEnabler`. Il est utilisé pour enregistrer la demande auprès de Adobe. Lors de l’enregistrement, le SDK reçoit un ID client et un secret client, qui est utilisé pour obtenir un jeton d’accès. Tout appel du SDK aux serveurs Adobe nécessite un jeton d’accès valide. Le SDK est responsable de l’enregistrement de l’application, de l’obtention et de l’actualisation du jeton d’accès.

>[!NOTE]
>
>Les instructions de logiciel sont spécifiques à l’application et une instruction logicielle individuelle ne peut pas être utilisée pour plusieurs applications. Notez que les instructions logicielles au niveau du programmeur ont la même contrainte. Elles ne peuvent être utilisées que pour une seule application, qu’il s’agisse d’un seul canal ou d’un multicanal.

## Comment obtenir un relevé logiciel {#how-to-get-ss}

Vous trouverez ci-dessous des moyens d’obtenir un relevé logiciel.

### Si vous avez accès au tableau de bord TVE d’Adobe

1. Ouvrez votre navigateur et accédez à [Tableau de bord Adobe Pass TVE](https://console.auth.adobe.com).

1. Accédez à la section **[!UICONTROL Channels]**, puis sélectionnez votre canal.

1. Accédez à l’onglet **[!UICONTROL Registered Applications]** .

1. Cliquez sur **[!UICONTROL Add new application]**.

1. Nommez l’application et spécifiez une version.

1. Sélectionnez les plateformes sur lesquelles l’application sera disponible (dans le cas présent, Android).

1. Fournissez un **[!UICONTROL Domain Name]** en choisissant parmi une liste de domaines déjà configurés pour votre programmeur.

1. Poussez vos modifications sur le serveur, puis revenez à l’onglet **[!UICONTROL Registered Applications]** de votre canal.

   Vous devriez voir une liste contenant toutes les applications enregistrées. Sélectionnez **[!UICONTROL Download]** sur l’application que vous avez créée. Vous devrez peut-être attendre quelques minutes avant que votre déclaration logicielle ne soit prête à être téléchargée.

   Téléchargements d’un fichier texte. Utilisez son contenu comme déclaration logicielle.

Pour plus d’informations, voir [Dynamic Client Registration Management](./dcr-api/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Si vous n’avez pas accès au tableau de bord TVE d’Adobe

Envoyez un ticket à `tve-support@adobe.com`. Incluez les informations nécessaires telles que le canal, le nom de l’application, la version et les plateformes. Une personne de notre équipe de support créera une déclaration logicielle pour vous.

## Utilisation de l’instruction logicielle {#how-to-use-ss}

Après avoir obtenu votre instruction logicielle, vous devez la transmettre en tant que paramètre dans le constructeur Access Enabler. Nous vous recommandons d’héberger l’instruction logicielle sur un emplacement distant. Ainsi, vous pouvez facilement révoquer et modifier l’instruction logicielle sans publier de nouvelle version de votre application.

## Création et utilisation d’un lien profond pour votre application {#create}

Sur Android, utilisez comme valeur de lien profond l’inverse du nom de domaine sélectionné lors de la création de l’instruction logicielle.

La valeur du lien profond créé doit être unique sur l’appareil Android. Lorsque plusieurs applications utilisent la même valeur de lien profond, les flux d’authentification et de déconnexion interférent.

## Utilisation de l’instruction logicielle et du lien profond {#use-both}

Dans le fichier de ressources de votre application `strings.xml`, ajoutez le code suivant :

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
