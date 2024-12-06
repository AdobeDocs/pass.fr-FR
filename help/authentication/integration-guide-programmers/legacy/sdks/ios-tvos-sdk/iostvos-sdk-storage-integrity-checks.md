---
title: Mécanisme de contrôle de l’intégrité du stockage iOS/tvOS
description: Mécanisme de vérification de l’intégrité iOS/tvOS
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 2%

---

# Mécanisme de vérification de l’intégrité iOS/tvOS {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#Intro}

À compter de la version 3.8.3 du SDK iOS/tvOS AccessEnabler, l’option permettant d’effectuer des vérifications d’intégrité du stockage est disponible lors de l’initialisation d’AccessEnabler.

Pour utiliser ce mécanisme, l’api a été étendue avec une méthode d’initialisation supplémentaire pour la classe AccessEnabler.

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## Vérifications de l’intégrité {#Checks}

Les contrôles d’intégrité du stockage sont utiles lorsque la corruption du stockage AccessEnabler est suspectée (par exemple lorsqu’une situation de concurrence se produit lors d’une opération de stockage lecture/écriture).

Les vérifications suivantes peuvent être effectuées lors de l’initialisation d’AccessEnabler :
- Opérabilité du stockage : vérifie la réussite des opérations de lecture et d’écriture
- Intégrité des valeurs stockées : vérifie que toutes les valeurs sont valides et au format attendu

>[!IMPORTANT]
> 
>Si l’une des vérifications échoue, toutes les valeurs du stockage sont effacées et l’utilisateur est déconnecté, ce qui peut entraîner une mauvaise expérience utilisateur. Utilisez les contrôles d’intégrité du stockage uniquement lorsque cela est nécessaire.


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

| Valeur | Vérifications effectuées | Stockage effacé | Description | Cas d’utilisation recommandé |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | Aucun | Jamais | Aucune vérification de l’intégrité n’est effectuée lors de l’initialisation du stockage | Lorsque les flux de SDK fonctionnent comme prévu |
| INTEGRITY_CHECK_ALL | Opérabilité de stockage <br/> Validité des valeurs stockées | Échec de la vérification | Toutes les vérifications d’intégrité disponibles sont effectuées lors de l’initialisation du stockage. | Lorsque la corruption du stockage du SDK est suspectée. <br/> En cas d’échec des contrôles d’intégrité, l’utilisateur est déconnecté. |
| INTEGRITY_CHECK_CLEAR | Aucun | Toujours | Le stockage est effacé lors de l’initialisation du stockage. | Lorsque les flux de SDK ne peuvent pas être terminés comme prévu |
