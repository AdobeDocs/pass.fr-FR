---
title: Présentation des ID utilisateur
description: Présentation des ID utilisateur
exl-id: 813a8501-db72-4850-a387-c8db6120db80
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---

# Présentation des ID utilisateur {#understanding-user-ids}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

D’un point de vue conceptuel, chaque utilisateur qui initie un flux de droits est associé à un identifiant utilisateur unique. Cependant, au cours d’un flux de droits, cet identifiant utilisateur peut être présenté de différentes manières, selon l’API à partir de laquelle vous obtenez l’identifiant.

`sessionGUID` dans le jeton de média court est la forme sécurisée de l’identifiant utilisateur, disponible via l’appel `sendTrackingData()`. Dans toutes les intégrations actuelles, il s’agit d’un GUID persistant pour l’utilisateur à travers le temps et les appareils. La source du GUID commence par l’ID utilisateur de la réponse d’authentification provenant du MVPD. Cependant, certains distributeurs pourraient changer d&#39;avis à l&#39;avenir et commencer à envoyer un GUID transitoire. Si un programmeur souhaite s’assurer que l’ID utilisateur source MVPD dans la réponse AuthN est persistant, il doit le prendre en compte dans ses accords avec les MVPD.

Voici les différentes manières dont l’ID utilisateur est représenté dans les API d’authentification Adobe Pass :

| Propriété | Objectif | Hashed | Signé numériquement | Description |
| --- | --- | --- | --- | --- |
| sendTrackingData(), propriété GUID | Suivi/analyse | Oui | Non | - Identifiant utilisateur MVPD, haché par Adobe. L&#39;ID utilisateur ne peut pas être redirigé vers la source vers le MVPD. </br> </br> - Ce formulaire d’identifiant n’est pas signé numériquement, il n’est donc pas sécurisé pour la prévention de la fraude. Toutefois, cela est suffisant pour les analyses.  </br> </br> - Ce formulaire d’identifiant utilisateur est fourni côté client pour tous les événements générés par l’authentification Adobe Pass dans le flux AuthN/AuthZ. |
| Propriété sessionGUID du jeton multimédia court | Suivi des fraudes lors d’une utilisation simultanée | Oui | Oui | - Identique à l’ID utilisateur via sendTrackingData(). Cependant, celui-ci est signé numériquement pour protéger son intégrité et est suffisant pour permettre le suivi des fraudes. </br> </br> - Il est destiné à être traité côté serveur après l’utilisation de notre bibliothèque de validateurs et peut être analysé à la recherche de modèles de fraude avant de publier le flux vidéo sur le client.  Faire l&#39;une de ces tâches dépend du programmeur. |
| getMetadata(), propriété userID | Liaison de compte, enquête sur les fraudes avec MVPD | Non | Non | - Cette propriété permet à l’Adobe d’exposer l’identifiant utilisateur MVPD source au programmeur. </br> </br> - Dans la configuration de l’Adobe, il peut être défini comme chiffré ou non (selon la préférence MVPD). S&#39;il est crypté, il sera crypté avec la clé publique du certificat du programmeur fourni à Adobe, de sorte qu&#39;il ne soit pas exposé clairement au client. </br> </br> - Cela donne au programmeur l’identifiant utilisateur réel du MVPD. Il peut donc être utilisé pour la liaison de comptes ou l’enquête sur la fraude directement avec le MVPD. |


**En conclusion**

* En général, le MVPD fournit un identifiant unique persistant <u> et le transmet à l’Adobe lors d’une authentification réussie </u>. Il est généralement cohérent sur tous les réseaux. L’exception est Comcast MVPD, qui fournit un identifiant utilisateur différent pour chaque canal.

* L’identifiant utilisateur MVPD ne contient pas de PII et il ne s’agit PAS d’un numéro de compte. Il n’est pas nécessaire d’être exposé sous une forme chiffrée, car nous avons validé avec tous les MVPD qu’aucune PII n’est envoyée.

L’utilisation de l’identifiant utilisateur dépend du cas d’utilisation :

* Si vous en avez besoin pour le suivi/l’analyse, l’emplacement le plus pratique est de l’obtenir à partir de `sendTrackingData()`.
* Si vous en avez besoin du côté serveur pour la publication, la fraude ou les données opérationnelles du flux, vous pouvez l’obtenir à partir du programme de validation du jeton multimédia.
* Si vous en avez besoin pour la liaison de comptes et une fraude plus approfondie, contactez votre Adobe pour connaître la disponibilité.
