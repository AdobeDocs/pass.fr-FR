---
title: Mécanisme de vérification de l’intégrité du stockage iOS/tvOS
description: Mécanisme de vérification de l’intégrité d’iOS/tvOS
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 2%

---

# Mécanisme de vérification de l’intégrité iOS/tvOS (hérité) {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#Intro}

À partir de la version 3.8.3 du SDK AccessEnabler d’iOS/tvOS, l’option permettant d’effectuer des contrôles d’intégrité du stockage est disponible lors de l’initialisation d’AccessEnabler.

Pour utiliser ce mécanisme, l’api a été étendue avec une méthode d’initialisation supplémentaire pour la classe AccessEnabler.

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## Contrôles d’intégrité {#Checks}

Les contrôles d’intégrité du stockage sont utiles lorsque la corruption du stockage AccessEnabler est suspectée (par exemple lorsqu’une condition de concurrence se produit lors d’une opération de stockage en lecture/écriture).

Les vérifications suivantes peuvent être effectuées lors de l&#39;initialisation d&#39;AccessEnabler :
- Opérabilité du stockage : vérifie le succès des opérations de lecture et d’écriture
- Intégrité des valeurs stockées : vérifie que toutes les valeurs sont valides et au format attendu

>[!IMPORTANT]
> 
>Si l’une des vérifications échoue, toutes les valeurs du stockage sont effacées et l’utilisateur est déconnecté, ce qui peut entraîner une mauvaise expérience utilisateur. N’utilisez les contrôles d’intégrité du stockage que lorsque cela est jugé nécessaire.


## Comportement par défaut {#Default}

Les contrôles d&#39;intégrité du stockage sont désactivés par défaut lors de l&#39;initialisation d&#39;AccessEnabler à l&#39;aide de la méthode d&#39;initialisation par défaut :

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

Pour spécifier explicitement les contrôles d&#39;intégrité du stockage à effectuer lors de l&#39;initialisation d&#39;AccessEnabler, utilisez la méthode d&#39;initialisation suivante :

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

L’énumération IntegrityCheckType est exposée à l’application cliente et possède les valeurs suivantes :

| Valeur | Contrôles effectués | Stockage effacé | Description | Cas d’utilisation recommandé |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | Aucun | Jamais | Aucune vérification d&#39;intégrité n&#39;est effectuée à l&#39;initialisation du stockage | Lorsque les flux SDK fonctionnent comme prévu |
| INTEGRITY_CHECK_ALL | Opérabilité du stockage <br/> Validité des valeurs stockées | Échec lors de la vérification | Tous les contrôles d&#39;intégrité disponibles sont effectués à l&#39;initialisation du stockage | En cas de suspicion de corruption du stockage SDK. <br/> En cas d’échec de l’une des vérifications d’intégrité, l’utilisateur est déconnecté |
| INTEGRITY_CHECK_CLEAR | Aucun | Toujours | Le stockage est effacé à l&#39;initialisation du stockage | Lorsque les flux SDK ne peuvent pas être exécutés comme prévu |
