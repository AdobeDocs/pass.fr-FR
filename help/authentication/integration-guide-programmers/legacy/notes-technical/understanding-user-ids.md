---
title: Comprendre les ID utilisateur
description: Comprendre les ID utilisateur
exl-id: 813a8501-db72-4850-a387-c8db6120db80
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 1%

---

# (Hérité) Comprendre les ID d’utilisateur {#understanding-user-ids}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Conceptuellement, chaque utilisateur qui initie un flux de droits est associé à un ID utilisateur unique. Cependant, au cours d’un flux de droits, cet identifiant utilisateur peut être présenté de différentes manières, en fonction de l’API auprès de laquelle vous l’obtenez.

Le `sessionGUID` du jeton de média court est le formulaire sécurisé de l’ID utilisateur, disponible via l’appel `sendTrackingData()`. Dans toutes les intégrations actuelles, il s’agit d’un GUID persistant pour l’utilisateur dans le temps et sur les appareils. La source du GUID commence par l’ID utilisateur de la réponse d’authentification provenant du MVPD. Cependant, certains MVPD pourraient changer d’avis à l’avenir et commencer à envoyer un GUID transitoire. Si un programmeur souhaite s’assurer que l’identifiant utilisateur source MVPD dans la réponse AuthN est persistant, il doit en tenir compte dans ses contrats avec les MVPD.

Voici les différentes manières dont l’ID utilisateur est représenté dans les API d’authentification d’Adobe Pass :

| Propriété | Objectif | Haché | Signé numériquement | Description |
| --- | --- | --- | --- | --- |
| sendTrackingData() propriété GUID | Suivi/analyses | Oui | Non | - Identifiant utilisateur MVPD, haché par Adobe. L’ID d’utilisateur n’est pas traçable de la source au MVPD. </br> </br> - Cette forme de la carte d’identité n’est pas signée numériquement et n’est donc pas sécurisée pour la prévention des fraudes. Toutefois, cela suffit pour les analyses.  </br> </br> - Cette forme d’identifiant utilisateur est fournie côté client sur tous les événements générés par l’authentification Adobe Pass dans le flux AuthN/AuthZ. |
| Propriété sessionGUID du jeton de média court | Tracking des fraudes d’utilisation simultanée | Oui | Oui | - Identique à l’ID utilisateur via sendTrackingData(), mais signé numériquement pour protéger son intégrité, cet ID est suffisant pour être utilisé pour le suivi des fraudes. </br> </br> - Il est destiné à être traité côté serveur après utilisation de notre bibliothèque de validation et peut être analysé pour détecter des modèles de fraude avant de diffuser le flux vidéo au client.  Il appartient au programmeur d’effectuer l’une de ces tâches. |
| Propriété getMetadata() userID | Liaison de comptes, enquête sur les fraudes avec MVPD | Non | Non | - Cette propriété permet à Adobe d’exposer l’ID utilisateur MVPD source réel au programmeur. </br> </br> - Dans la configuration d’Adobe, il peut être défini comme chiffré ou non (selon la préférence MVPD). S&#39;il est chiffré, il sera chiffré avec la clé publique du certificat du programmeur fourni à l&#39;Adobe, de sorte qu&#39;il ne soit pas exposé en clair au client. </br> </br> - Cela donne au programmeur l’ID d’utilisateur réel du MVPD. Il peut donc être utilisé pour la liaison de compte ou l’enquête de fraude directement avec le MVPD. |


**En conclusion**

* En règle générale, le MVPD fournit un ID unique persistant <u>et le transmet à l’Adobe lors d’une authentification réussie</u>. Il est généralement cohérent sur tous les réseaux. L’exception est Comcast MVPD, qui fournit un identifiant utilisateur différent pour chaque canal.

* L’ID d’utilisateur MVPD ne contient pas de PII et il ne s’agit PAS d’un numéro de compte. Il n’est pas nécessaire de l’exposer sous une forme chiffrée, car nous avons validé avec tous les MVPD qu’aucune PII n’est envoyée.

La manière dont vous utilisez l’ID utilisateur dépend du cas d’utilisation :

* Si vous en avez besoin pour le suivi/l’analyse, l’endroit le plus pratique est de l’obtenir auprès de `sendTrackingData()`.
* Si vous en avez besoin côté serveur pour les données de diffusion de flux, de fraude ou d’exploitation, vous pouvez l’obtenir à partir du programme de validation des jetons multimédia.
* Si vous en avez besoin pour la liaison de comptes et une fraude plus grave, vérifiez la disponibilité auprès de votre contact d&#39;Adobe.
