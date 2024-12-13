---
title: Déboguer le SDK AccessEnabler iOS/tvOS à l’aide des journaux d’application de la console
description: Déboguer le SDK AccessEnabler iOS/tvOS à l’aide des journaux d’application de la console
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---

# (Hérité) Déboguer l’AccessEnabler iOS/tvOS SDK à l’aide des journaux d’application de la console {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Vue d’ensemble

Le but de ce document est de capturer et de présenter l’évolution du mécanisme de journalisation SDK d’iOS/tvOS d’AccessEnabler, ainsi que quelques détails utiles pour déboguer le framework d’AccessEnabler à l’aide des journaux d’applications de la console.

## État du mécanisme de journalisation

Le mécanisme de journalisation d’AccessEnabler iOS/tvOS a pour but d’émettre des messages utiles pour résoudre les problèmes éventuels qu’une application utilisant le framework AccessEnabler peut rencontrer en raison de celui-ci.

### AccessEnabler iOS/tvOS 3.5.0 et versions ultérieures

À partir de la version 3.5.0 d’AccessEnabler iOS/tvOS, le mécanisme de journalisation apporte les améliorations suivantes sous forme de modifications :

* La structure AccessEnabler utilise l’implémentation recommandée [OSLog](https://developer.apple.com/documentation/os/oslog) d’Apple.

* La structure AccessEnabler offre la possibilité de filtrer les journaux d’applications de la console en fonction du sous-système : **com.adobe.pass.AccessEnabler**. Tous les messages émis par le SDK font partie de com.adobe.pass.AccessEnabler.

* La structure AccessEnabler offre la possibilité de filtrer les journaux des applications de la console selon N’importe lequel (préfixe) : **[AccessEnabler]**. Tous les messages émis par le SDK comportent le préfixe [AccessEnabler].

* La structure AccessEnabler offre la possibilité de filtrer les journaux d&#39;application de la console en fonction de la catégorie : **debug**, **error** conjointement avec l&#39;un des deux critères ci-dessus : Subsystem ou Any (prefix).

## Débogage à l’aide des journaux de l’application Console

En fonction des problèmes étudiés, vous souhaiterez peut-être inclure ou exclure les messages de journalisation émis par le framework AccessEnabler. Vous trouverez donc ci-dessous quelques détails utiles qui peuvent vous aider lors des enquêtes et de l’utilisation des journaux des applications de la console.


### AccessEnabler iOS/tvOS 3.5.0 et versions ultérieures

#### Inclusion {#including}

Tout d’abord, pour pouvoir consulter l’un des messages de journalisation émis par le framework AccessEnabler **vous** devez sélectionner « Inclure les messages d’informations » et « Inclure les messages de débogage » dans la section Action de l’application Console, comme illustré dans l’image ci-dessous.

![](../../../assets/include-info-debug-msg.png)


Pour pouvoir déboguer la fonctionnalité du SDK AccessEnabler iOS/tvOS et **voir** les journaux de framework AccessEnabler, vous pouvez :

* Recherchez dans l’application Console à l’aide de l’option **Subsystem** qui est égale à la valeur com.adobe.pass.AccessEnabler comme dans l’image ci-dessous.

![](../../../assets/subsys-console-app.png)

* Recherchez dans l’application de console à l’aide de l’option **Any** qui contient les
  Valeur [AccessEnabler] comme dans l’image ci-dessous.

![](../../../assets/any-optn-console-app.png)

Outre les deux critères ci-dessus, vous pouvez également utiliser l’option **Catégorie** conjointement avec **Sous-système** ou **N’importe lequel (préfixe)** pour rechercher explicitement des messages de niveau **débogage** ou **erreur** émis par le SDK AccessEnabler iOS/tvOS.

#### Exclusion

Pour mieux déboguer les fonctionnalités d&#39;autres composants et **exclure** les journaux de framework AccessEnabler, vous pouvez :

* Recherchez dans l’application Console à l’aide de l’option **Subsystem** qui n’est pas égale à la valeur com.adobe.pass.AccessEnabler .
* Recherchez dans l’application Console à l’aide de l’option **Any** qui ne contient pas la valeur [AccessEnabler].

## Signaler un problème

Lorsque vous signalez un problème à l’authentification Adobe Pass, tenez compte des suggestions suivantes :

* essayez de fournir les étapes de reproduction.
* essayez de fournir la ou les versions du système d’exploitation et le ou les modèles d’appareils sur lesquels le problème se produit.
* essayez de fournir la version du SDK AccessEnabler iOS/tvOS rencontrant le problème.
* essayez de capturer et de joindre tous les messages de journalisation SDK AccessEnabler iOS/tvOS à l’aide de l’une des deux options présentées dans la section [Inclusion](#including).
