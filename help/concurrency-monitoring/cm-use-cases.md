---
title: Cas d’utilisation
description: Cas d’utilisation dans la surveillance de la simultanéité.
exl-id: 6cc30bb6-e985-4d9a-9f99-a7f04ae8deb7
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---

# Cas d’utilisation {#use-cases}

Le principal cas d’utilisation du service de comptage de flux consiste à compter le nombre de flux vidéo simultanés regardés par un utilisateur et à fournir une décision concernant son utilisation simultanée pour le même identifiant de compte.

Pour surveiller l’utilisation par les abonnés, un service centralisé est nécessaire pour regrouper l’activité des utilisateurs et utilisatrices, que ce soit sur le site web ou l’application du programmeur, sur le portail de contenu MVPD ou sur une propriété syndiquée.

Les principaux cas d’utilisation pris en charge par ce service centralisé doivent être les suivants :

1. Dès qu’un abonné commence à regarder une vidéo, l’application peut **initialiser une session de diffusion en continu** et commencer à **créer des rapports d’activité** des données.
1. Dans le même service central, une autre instance recevra les ***décisions CM***. Si l’application comporte une ou plusieurs politiques enregistrées dans le service CM, le service répondra avec une décision d’accès basée sur l’activité actuelle.


## Création d’une session {#create-session}

Cet appel d’API permet au client de créer une session de CM lorsque l’utilisateur appuie sur le bouton « lire » pour regarder du contenu. La réponse du serveur contiendra la nouvelle URL du flux (contenant l’identifiant du flux) pour la maintenir active et l’heure à laquelle le flux expirera. L’application cliente doit signaler l’activité par pulsations. L’appel d’initialisation de session doit inclure des métadonnées sous la forme de paires clé/valeur envoyées en tant que données de formulaire (ou paramètres de chaîne de requête). En outre, la réponse inclut également un indicateur pour indiquer si la lecture est « conforme à la politique ». Si ce n’est pas le cas, la lecture n’est pas autorisée.

## Activité de reporting {#reporting-activity}

Une fois la session créée, l’application doit envoyer régulièrement des pulsations pour que ce flux reste actif. En outre, il est recommandé que l’application cliente arrête le flux une fois que l’utilisateur ou l’utilisatrice arrête la lecture, de sorte que le flux ne soit pas comptabilisé comme actif avant la temporisation.

La réponse de l’appel de pulsation peut permettre à l’application cliente de continuer la lecture vidéo (lorsqu’elle est conforme à la politique) ou lui demander d’arrêter la lecture vidéo. Dans le cas où le flux vidéo n’est pas conforme, l’application cliente doit l’arrêter. La réponse fournit des informations afin que l’application cliente affiche un message d’erreur et/ou des actions disponibles pour que l’utilisateur puisse continuer la lecture.
