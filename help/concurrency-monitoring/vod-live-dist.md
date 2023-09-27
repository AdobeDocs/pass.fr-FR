---
title: Comment faire la distinction entre contenu VOD et contenu réel dans la surveillance de la simultanéité
description: Comment faire la distinction entre contenu VOD et contenu réel dans la surveillance de la simultanéité
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---


# Comment : distinguer le contenu VOD du contenu en direct dans la surveillance simultanée {#dist-vod-live}

**Q :** Le service de surveillance de la simultanéité peut-il faire la distinction entre le type de contenu en cours de lecture (contenu en direct et vidéo à la demande) ?



**A :** La surveillance simultanée ne peut pas directement faire la distinction entre le contenu en direct et la vidéo à la demande (VOD). Le lecteur vidéo doit connaître le type de contenu en cours de lecture et envoyer ces informations lors de la [appel d’initialisation de session](/help/concurrency-monitoring/cm-api-overview.md#session-initial) (requis pour la surveillance de la simultanéité). Le workflow normal ressemble à ceci :

1. Les clients de surveillance de la simultanéité définissent un ensemble de métadonnées sur lesquelles ils souhaitent que des règles soient implémentées (par exemple, content-type=live|vod, device-type=mobile|console|bureau).
1. L’équipe de surveillance de la simultanéité met en oeuvre la stratégie souhaitée. Exemple :
   1. si content-type=live, max streams=3, la dernière gagne
   1. si content-type=vod, max streams=1, la dernière gagne

1. Lorsque l’utilisateur final lit le contenu, le lecteur vidéo doit envoyer des valeurs pour les champs de métadonnées qui ont été définis comme faisant partie d’une stratégie.

1. Le service de surveillance de la simultanéité, basé sur la stratégie définie et les valeurs reçues, émet une décision (lecture/arrêt).

1. Pour que le système fonctionne, la décision doit être respectée par le lecteur vidéo.



## Informations connexes {#related-info-vod-live-dist}

* [Surveillance simultanée des attributs de métadonnées standard](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [Présentation de l’API de surveillance de la simultanéité](/help/concurrency-monitoring/cm-api-overview.md)
