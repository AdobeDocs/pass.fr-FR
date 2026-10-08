---
title: Présentation des environnements Adobe
description: Présentation des environnements Adobe
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%
---
# Présentation des environnements Adobe {#understanding-the-adobe-environments}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

La documentation officielle décrivant les environnements Adobe est disponible [Configuration de votre environnement et tests dans Pre-qual](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md) :

Les environnements Adobe, résumés en quelques mots :

Adobe comporte deux environnements : **pré-qualification** et **version**.

* En ce qui concerne l’environnement de préqualification, nous préparons le nouveau build à publier.

* Le build de la version actuelle se trouve dans l’environnement de version.

Chaque environnement comporte deux profils : **évaluation** et **production**.

* Le profil d’évaluation se connecte au serveur d’évaluation des MVPD
* Le profil de production se connecte au profil de production des MVPD.

La raison d’être de ces deux profils est que, sur le profil d’évaluation, nous préparons de nouveaux partenaires à passer en ligne. Ils souhaitent tester le système avec le build à venir (préqualification) ou avec le build de publication (plus stable).

Si un partenaire souhaite tester la nouvelle version, quelques étapes supplémentaires doivent être effectuées. Voir [ Configuration de votre environnement et test dans Pre-qual](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md).

En suivant les étapes ci-dessus, vous êtes assuré que la prochaine version sera testée dans l’environnement de préqualification.
