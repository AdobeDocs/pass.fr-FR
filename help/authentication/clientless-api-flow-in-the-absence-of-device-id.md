---
title: Flux d’API sans client en l’absence d’identifiant d’appareil
description: Flux d’API sans client en l’absence d’identifiant d’appareil
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---

# Flux d’API sans client en l’absence d’identifiant d’appareil {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>


## Problème

Toutes les applications d’appareils intelligents ne seront pas en mesure de fournir un identifiant d’appareil unique.  Comme deviceId est un paramètre obligatoire, le service renvoie une erreur 400 s’il n’est pas transmis.


## Solution temporaire/solution de contournement

Pour les clients sans ID d’appareil :

1. Appelez le service de code d’enregistrement pour la première fois avec `deviceId=dummy`
1. A partir de la réponse, extrayez l’UUID. L’UUID est disponible dans l’élément &quot;id&quot; de la réponse du code d’enregistrement (formats de réponse XML et JSON).
1. Appelez le service d’enregistrement une seconde fois. Cette fois, passez `deviceId=<uuid obtained in step #2>`
1. Afficher le code d’enregistrement obtenu à l’étape 3 dans l’interface utilisateur de la console


Une fois ces étapes effectuées, l’authentification Adobe Pass utilise l’UUID comme identifiant de périphérique. Stockez cet identifiant d’appareil (UUID) dans le stockage local de l’appareil. Dans le cas où l’utilisateur génère un nouveau code d’enregistrement, vous devez exécuter à nouveau les étapes 1 à 4, puis remplacer l’UUID (ID de périphérique) précédemment stocké par le nouveau.



## Solution permanente

Adobe modifiera cette configuration dans une version ultérieure en effectuant `deviceId` une charge utile facultative lors de la création du code reg et de l’utilisation de l’UUID comme clé de jeton au lieu de `deviceId`, lorsque `deviceId` n’est pas présent.

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
