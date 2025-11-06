---
title: Flux d’API sans client en l’absence d’identifiant d’appareil
description: Flux d’API sans client en l’absence d’identifiant d’appareil
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Flux d’API sans client (hérité) en l’absence d’identifiant d’appareil {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>


## Problème

Toutes les applications Smart Device ne seront pas en mesure de fournir un identifiant d&#39;appareil unique.  Étant donné que deviceId est un paramètre obligatoire, le service renvoie une erreur 400 s’il n’est pas transmis.


## Solution Temporaire / Solution De Contournement

Pour les clients sans ID d’appareil :

1. Appelez le service de code d’enregistrement la première fois avec `deviceId=dummy`
1. Dans la réponse, extrayez l’UUID. L’UUID est disponible dans l’élément « id » de la réponse du code d’enregistrement (formats de réponse XML et JSON).
1. Appelez le service d’enregistrement une seconde fois. Cette fois, passez `deviceId=<uuid obtained in step #2>`
1. Affichez le code d’enregistrement obtenu à l’étape 3 dans l’interface utilisateur de la console


Une fois ces étapes effectuées, l’authentification Adobe Pass utilise l’UUID comme ID d’appareil. Stockez cet identifiant d’appareil (UUID) dans le stockage local de l’appareil. Si l’utilisateur génère un nouveau code d’enregistrement, vous devez exécuter à nouveau les étapes 1 à 4, puis remplacer l’ID d’appareil (UUID) précédemment stocké par le nouveau.



## Solution Permanente

Adobe changera cela dans une prochaine version, en `deviceId` une payload facultative lors de la création du code d’enregistrement et en utilisant UUID comme clé de jeton au lieu de `deviceId`, lorsque `deviceId` n’est pas présent.

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
