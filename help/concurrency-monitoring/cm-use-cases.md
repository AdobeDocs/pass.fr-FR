---
title: Cas d’utilisation
description: Cas d’utilisation de la surveillance simultanée.
exl-id: 6cc30bb6-e985-4d9a-9f99-a7f04ae8deb7
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---

# Cas d’utilisation {#use-cases}

Le principal cas d’utilisation du service de comptage des diffusions consiste à comptabiliser le nombre de diffusions vidéo simultanées regardées par un utilisateur et à prendre une décision concernant son utilisation simultanée pour le même ID de compte.

Pour surveiller l’utilisation par les abonnés, un service centralisé peut agréger l’activité des utilisateurs, que ce soit sur le site web ou l’application du programmeur, sur le portail de contenu du MVPD ou sur une propriété syndiquée.

Les principaux cas d’utilisation pris en charge par ce service centralisé doivent être les suivants :

1. Dès qu’un abonné commence à regarder une vidéo, l’application peut **initialiser une session de diffusion en continu** et lancer des données **d’activité de création de rapports**.
1. Dans le même service central, une autre instance recevra ***décisions de CM*** : si l’application a une ou plusieurs stratégies enregistrées dans le service CM, le service répondra avec la décision d’accès en fonction de l’activité actuelle.


## Création d’une session {#create-session}

Cet appel API permet au client de créer une session CM lorsque l’utilisateur appuie sur le bouton &quot;play&quot; pour regarder du contenu. La réponse du serveur contiendra la nouvelle URL du flux (contenant l’ID de flux) pour le maintenir en vie et le délai d’expiration du flux. Il est prévu que l’application cliente signale l’activité au moyen de pulsations. L’appel d’initialisation de session doit inclure des métadonnées sous la forme de paires clé/valeur envoyées en tant que données de formulaire (ou paramètres de chaîne de requête). En outre, la réponse comprend également un indicateur pour indiquer si la lecture est &quot;conforme à la stratégie&quot;. Si ce n’est pas le cas, la lecture n’est pas autorisée.

## Activité de création de rapports {#reporting-activity}

Une fois une session créée, l’application doit envoyer des pulsations régulièrement pour que ce flux reste actif. En outre, il est recommandé que l’application cliente arrête la diffusion une fois que l’utilisateur a arrêté la lecture, de sorte que la diffusion ne soit pas considérée comme active avant l’expiration du délai.

La réponse de l’appel de pulsation peut permettre à l’application cliente de poursuivre la lecture vidéo (lorsqu’elle est conforme à la stratégie) ou lui demander d’arrêter la lecture vidéo. Si le flux vidéo n’est pas compatible, l’application cliente doit l’arrêter. La réponse fournit des informations afin que l’application cliente affiche un message d’erreur et/ou les actions disponibles pour que l’utilisateur puisse continuer la lecture.
