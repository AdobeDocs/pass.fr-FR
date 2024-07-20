---
title: Enregistrement de l’application Amazon FireOS
description: Enregistrement de l’application Amazon FireOS
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 0%

---

# Enregistrement de l’application Amazon FireOS {#amazon-fireos-application-registration}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Introduction {#intro}

À compter de la version 3.0 du SDK FireOS AccessEnabler, nous sommes en train de modifier le mécanisme d’authentification avec les serveurs d’Adobe. Au lieu d’utiliser une clé publique et un système secret pour signer le requestorID, nous introduisons le concept d’une chaîne d’instruction logicielle qui peut être utilisée pour obtenir un jeton d’accès utilisé ultérieurement pour tous les appels effectués par le SDK vers nos serveurs. Outre une instruction logicielle, vous devez également créer un lien profond pour votre application.

Pour plus d’informations, voir [Enregistrement du client dynamique](/help/authentication/dynamic-client-registration.md)

## Qu’est-ce qu’une déclaration logicielle ? {#what}

Une instruction logicielle est un jeton JWT qui contient des informations sur votre application. Chaque application doit avoir un relevé logiciel unique utilisé par nos serveurs pour identifier l’application dans le système d’Adobe. L’instruction logicielle doit être transmise lorsque vous initialisez le SDK AccessEnabler et elle sera utilisée pour enregistrer l’application avec Adobe. Lors de l’enregistrement, le SDK reçoit un identifiant client et un secret client qui seront utilisés pour obtenir un jeton d’accès. Tout appel du SDK à nos serveurs nécessite un jeton d’accès valide. Le SDK est chargé d’enregistrer l’application, d’obtenir et d’actualiser le jeton d’accès.

**Remarque :** Les instructions de logiciel sont spécifiques à l’application et une instruction logicielle individuelle ne peut pas être utilisée pour plusieurs applications. Veuillez noter que cela s&#39;applique également aux applications qui offrent l&#39;accès à plusieurs canaux.

## Comment obtenir un relevé logiciel ? {#how-to}

### Si vous avez accès au tableau de bord TVE d’Adobe :

1. Ouvrez votre navigateur et accédez à `https://console.auth.adobe.com`.

1. Accédez à la section **[!UICONTROL Channels]** , puis sélectionnez votre canal.

1. Accédez à l’onglet **[!UICONTROL Registered Applications]** .

1. Cliquez sur **[!UICONTROL Add new application]**.

1. Attribuez un nom et une version à votre application, puis sélectionnez les plateformes sur lesquelles elle sera disponible (Android, par exemple).

1. Fournissez un **[!UICONTROL Domain Name]** en choisissant parmi une liste de domaines déjà configurés pour votre programmeur.

1. Poussez vos modifications sur le serveur, puis revenez à l’onglet **[!UICONTROL Registered Applications]** de votre canal.

   Vous devriez voir une liste contenant toutes les applications enregistrées.

1. Cliquez sur **[!UICONTROL Download]** dans l’application que vous venez de créer.

   Vous devrez peut-être attendre quelques minutes avant que votre déclaration logicielle ne soit prête au téléchargement.

   Téléchargements d’un fichier texte. Utilisez son contenu comme déclaration logicielle.

Pour plus d’informations, voir [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)

### Si vous n’avez pas accès au tableau de bord TVE d’Adobe :

Envoyez un ticket à [tve-support@adobe.com](mailto:tve-support@adobe.com). Incluez toutes les informations nécessaires, y compris le canal, le nom de l’application, la version et les plateformes, et une personne de notre équipe d’assistance créera une déclaration logicielle pour vous.

## Utilisation de l’instruction logicielle {#use}

Après avoir obtenu votre instruction logicielle, vous devez la transmettre en tant que paramètre dans le constructeur Access Enabler. Adobe recommande d’héberger l’instruction logicielle sur un emplacement distant. Ainsi, vous pouvez facilement révoquer et modifier l’instruction logicielle sans publier de nouvelle version de votre application.

## Utilisation de l’instruction logicielle {#use-both}

Dans le fichier de ressources de votre application `strings.xml`, ajoutez le code suivant :

```XML
<string name="software_statement">softwarestatement value</string>
```
