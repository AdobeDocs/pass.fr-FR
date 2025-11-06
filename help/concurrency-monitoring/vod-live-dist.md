---
title: Comment faire la distinction entre VOD et le contenu dynamique dans la surveillance simultanée
description: Comment faire la distinction entre VOD et le contenu dynamique dans la surveillance simultanée
exl-id: 51ba686a-7c1f-4403-9e8e-cd247bf9e345
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Comment : faire la distinction entre VOD et le contenu dynamique dans la surveillance simultanée {#dist-vod-live}

**Q :** Le service de surveillance simultanée peut-il faire la distinction entre le type de contenu lu (contenu en direct et vidéo à la demande) ?



La surveillance simultanée **A:** ne peut pas faire directement la distinction entre le contenu en direct et la vidéo à la demande (VOD). Le lecteur vidéo doit connaître le type de contenu qu’il lit et envoyer ces informations pendant l’appel d’initialisation de la session [session](/help/concurrency-monitoring/cm-api-overview.md#session-initial) (obligatoire pour la surveillance d’accès simultané). Le workflow standard ressemble à ceci :

1. Les clients de la surveillance de simultanéité définissent un ensemble de métadonnées sur lesquelles ils souhaitent que des règles soient implémentées (par exemple, content-type=live|vod, device-type=mobile|console|desktop).
1. L’équipe de surveillance de concurrence met en œuvre la politique souhaitée. Exemple :
   1. si content-type=live, max streams=3, latest gagne
   1. si content-type=vod, max streams=1, latest gagne

1. Lorsque l’utilisateur final lit le contenu, le lecteur vidéo doit envoyer des valeurs pour les champs de métadonnées qui ont été établis dans le cadre d’une politique.

1. Le service de surveillance d’accès simultané, en fonction de la politique définie et des valeurs reçues, émettra une décision (play/stop).

1. Le lecteur vidéo doit respecter cette décision pour que le système fonctionne.



## Informations connexes {#related-info-vod-live-dist}

* [Attributs de métadonnées standard de surveillance de simultanéité](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [Présentation de l’API de surveillance de simultanéité](/help/concurrency-monitoring/cm-api-overview.md)
