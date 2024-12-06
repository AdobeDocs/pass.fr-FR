---
title: Débogage du SDK AccessEnabler iOS/tvOS à l’aide des journaux d’application de la console
description: Débogage du SDK AccessEnabler iOS/tvOS à l’aide des journaux d’application de la console
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# Débogage du SDK AccessEnabler iOS/tvOS à l’aide des journaux d’application de la console {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


## Vue d’ensemble

Ce document a pour but de capturer et de présenter l’évolution du mécanisme de journalisation du SDK iOS/tvOS d’AccessEnabler, ainsi que quelques détails utiles pour déboguer la structure AccessEnabler à l’aide des journaux d’application de la console.

## État du mécanisme de journalisation

Le mécanisme de journalisation d’AccessEnabler iOS/tvOS a pour objectif d’émettre des messages utiles pour résoudre les problèmes éventuels qu’une application utilisant la structure AccessEnabler pourrait rencontrer en raison de ce problème.

### AccessEnabler iOS/tvOS 3.5.0 et versions ultérieures

À partir de la version iOS/tvOS 3.5.0 d’AccessEnabler, le mécanisme de journalisation introduit les améliorations suivantes lors des modifications :

* La structure AccessEnabler utilise l’implémentation recommandée par Apple [OSLog](https://developer.apple.com/documentation/os/oslog).

* La structure AccessEnabler introduit la possibilité de filtrer les journaux d’application de la console en fonction du sous-système : **com.adobe.pass.AccessEnabler**. Tous les messages émis par le SDK font partie de com.adobe.pass.AccessEnabler.

* La structure AccessEnabler introduit la possibilité de filtrer les journaux d’application de la console en fonction de n’importe quel (préfixe) : **[AccessEnabler]**. Tous les messages émis par le SDK sont précédés du préfixe [AccessEnabler].

* La structure AccessEnabler introduit la possibilité de filtrer les journaux d’application de la console en fonction de la catégorie : **debug**, **error** en association avec l’un des deux critères ci-dessus : Subsystem ou Any (préfixe).

## Débogage à l’aide des journaux de l’application Console

En fonction des problèmes qui font l’objet d’une enquête, vous pouvez inclure ou exclure les messages de journalisation émis par l’infrastructure AccessEnabler. Vous trouverez donc ci-dessous des détails utiles qui peuvent vous aider lors des enquêtes et lors de l’utilisation des journaux d’application de la console.


### AccessEnabler iOS/tvOS 3.5.0 et versions ultérieures

#### Inclusion {#including}

Tout d’abord, pour pouvoir voir les messages de journalisation émis par la structure AccessEnabler, vous **devez** sélectionner les options &quot;Inclure les messages d’information&quot; et &quot;Inclure les messages de débogage&quot; dans la section Action de l’application Console, comme présenté dans l’image ci-dessous.

![](../assets/include-info-debug-msg.png)


Pour pouvoir déboguer les fonctionnalités du SDK AccessEnabler iOS/tvOS et **voir** les journaux de structure AccessEnabler que vous pouvez :

* Effectuez une recherche dans l’application de console à l’aide de l’option **Subsystem** qui équivaut à la valeur com.adobe.pass.AccessEnabler comme dans l’image ci-dessous.

![](../assets/subsys-console-app.png)

* Recherchez dans l’application de console à l’aide de l’option **Any** qui contient la variable
  Valeur [AccessEnabler] comme dans l’image ci-dessous.

![](../assets/any-optn-console-app.png)

Outre les deux critères ci-dessus, vous pouvez également utiliser l’option **Category** conjointement avec **Subsystem** ou **Any (prefix)** pour rechercher explicitement les messages de niveau **debug** ou **error** émis par le SDK AccessEnabler iOS/tvOS.

#### Exclusion

Pour pouvoir mieux déboguer les fonctionnalités des autres composants et **exclure** les journaux de structure AccessEnabler, vous pouvez :

* Recherchez dans l’application de console à l’aide de l’option **Subsystem** qui n’est pas égale à la valeur com.adobe.pass.AccessEnabler.
* Recherchez dans l’application de console à l’aide de l’option **Any** qui ne contient pas la valeur [AccessEnabler].

## Signalement d’un problème

Lorsque vous signalez un problème à l’authentification Adobe Pass, veuillez tenir compte des suggestions suivantes :

* veuillez essayer de fournir les étapes de reproduction.
* essayez de fournir la ou les versions du système d’exploitation et le ou les modèles de périphérique sur lesquels le problème se produit.
* essayez de fournir la version du SDK AccessEnabler iOS/tvOS qui rencontre le problème.
* essayez de capturer et de joindre tous les messages de journalisation du SDK AccessEnabler iOS/tvOS à l’aide de l’une des deux options présentées dans la section [Inclusion](#including) .
