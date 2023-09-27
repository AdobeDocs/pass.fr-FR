---
title: Enregistrement d’applications Android
description: Enregistrement d’applications Android
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Enregistrement d’applications Android {#android-application-registration}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

À compter de la version 3.0 du SDK Android AccessEnabler, nous sommes en train de modifier le mécanisme d’authentification avec les serveurs d’Adobe. Au lieu d’utiliser une clé publique et un système secret pour signer le requestorID, nous introduisons le concept d’une chaîne d’instruction logicielle qui peut être utilisée pour obtenir un jeton d’accès utilisé ultérieurement pour tous les appels effectués par le SDK vers nos serveurs. Outre une instruction logicielle, vous devez également créer un lien profond pour votre application.

Pour plus d’informations, voir [Enregistrement du client dynamique](/help/authentication/dynamic-client-registration.md)

## Qu’est-ce qu’une déclaration logicielle ? {#what}

Une instruction logicielle est un jeton JWT qui contient des informations sur votre application. Chaque application doit avoir une instruction logicielle unique utilisée par nos serveurs pour identifier l’application dans le système d’Adobe. L’instruction logicielle doit être transmise lorsque vous initialisez le SDK AccessEnabler et elle sera utilisée pour enregistrer l’application avec Adobe. Lors de l’enregistrement, le SDK reçoit un identifiant client et un secret client qui seront utilisés pour obtenir un jeton d’accès. Tout appel du SDK à nos serveurs nécessite un jeton d’accès valide. Le SDK est chargé d’enregistrer l’application, d’obtenir et d’actualiser le jeton d’accès.

>[!NOTE]
>
>Les instructions de logiciel sont spécifiques à l’application et une instruction logicielle individuelle ne peut pas être utilisée pour plusieurs applications. Notez que les instructions logicielles au niveau du programmeur ont la même contrainte. Elles ne peuvent être utilisées que pour une seule application, qu’il s’agisse d’un seul canal ou d’un multicanal.

## Comment obtenir un relevé logiciel ? {#how-to-get-ss}

### Si vous avez accès au tableau de bord TVE d’Adobe :

* Ouvrez votre navigateur et accédez à [Tableau de bord Adobe Pass TVE](https://console.auth.adobe.com).
* Accédez à `Channels` et sélectionnez votre canal.
* Accédez à `Registered Applications` Onglet.
* Cliquez sur `Add new application`.
* Attribuez un nom et une version à votre application et sélectionnez les plateformes sur lesquelles elle sera disponible. Android dans notre cas.
* Indiquez un nom de domaine en effectuant une sélection dans une liste de domaines déjà configurés pour votre programmeur.
* Envoyez vos modifications au serveur, puis revenez à l’onglet Applications enregistrées de votre canal.
* Une liste devrait s’afficher avec toutes les applications enregistrées. Sélectionnez la variable **Télécharger** sur l’application que vous venez de créer. Vous devrez peut-être attendre quelques minutes avant que votre déclaration logicielle ne soit prête à être téléchargée.
* Un fichier texte sera téléchargé. Utilisez son contenu comme déclaration logicielle.

Pour plus d’informations, voir [Gestion dynamique de l&#39;enregistrement des clients](/help/authentication/dynamic-client-registration-management.md)

### Si vous n’avez pas accès au tableau de bord TVE d’Adobe :

Envoyer un ticket à `tve-support@adobe.com`. Veuillez inclure toutes les informations nécessaires, telles que le canal, le nom de l’application, la version et les plateformes. Une personne de notre équipe d’assistance créera une déclaration logicielle pour vous.

## Comment utiliser l’instruction logicielle ? {#how-to-use-ss}

Après avoir obtenu votre instruction logicielle, vous devez la transmettre en tant que paramètre dans le constructeur Access Enabler. Nous vous recommandons d’héberger l’instruction logicielle sur un emplacement distant. Ainsi, vous pouvez facilement révoquer et modifier l’instruction logicielle sans publier une nouvelle version de votre application.

## Création et utilisation d’un lien profond pour votre application {#create}

Sur Android, utilisez comme valeur de lien profond l’inverse du nom de domaine sélectionné lors de la création de l’instruction logicielle.

La valeur du lien profond créé doit être unique sur l’appareil Android. Lorsque plusieurs applications utilisent la même valeur de lien profond, les flux d’authentification et de déconnexion interférent.

## Utilisation de l’instruction logicielle et du lien profond {#use-both}

Dans le fichier de ressources de votre application `strings.xml` ajoutez le code suivant :

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
