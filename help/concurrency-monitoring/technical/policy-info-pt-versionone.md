---
title: Point d'information sur la politique
description: Point d'information sur la politique
exl-id: 964bb28d-cfef-4a37-b6c4-10cc59be0b47
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# Point d&#39;information sur la politique {#pip}

>[!NOTE]
>
>Cette page est obsolète, car elle s’applique à la version précédente de l’API qui n’est plus recommandée pour les nouvelles intégrations

Le diagramme suivant illustre le flux au cas où le client opterait pour le **Point d’information de politique** auquel cas CM est simplement utilisé pour interroger l’activité et toute la logique d’accès est incorporée dans l’application cliente) :

![](../assets/pip-workflow.png)



Le diagramme ci-dessous illustre le fonctionnement du comptage de flux pour un utilisateur qui regarde du contenu depuis 2 appareils.

![](../assets/pip-sequence.png)

En résumé, le flux de messages habituel est le suivant :

1. Dans un premier temps, pour un utilisateur qui n’a jamais utilisé le service, une page web est chargée/une application est ouverte, où une application instrumentée par le service de surveillance de simultanéité effectue un appel d’initialisation de session.
1. Le service de surveillance d’accès simultané renvoie la nouvelle ressource de flux pour les pulsations, ainsi que l’activité actuelle de l’utilisateur.
1. Pendant la lecture vidéo, l’application instrumentée effectue des appels de pulsation vers le service de surveillance simultanée, ce qui indique que l’utilisateur ou l’utilisatrice consomme actuellement une vidéo.
1. À tout autre moment, d’autres applications instrumentées peuvent effectuer des appels de requête de statut auprès du service de surveillance de simultanéité, ce qui renvoie l’activité actuelle de l’utilisateur ou de l’utilisatrice.
1. Du côté de la lecture vidéo, l’application instrumentée peut effectuer un appel Heartbeat avec « event=stop », ce qui signifie que la vidéo s’est arrêtée et que le flux actuel ne doit plus être comptabilisé comme un flux actif.
