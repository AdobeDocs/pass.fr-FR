---
title: Métadonnées personnalisées
description: Métadonnées personnalisées
exl-id: 0cfd1158-8c6c-47c2-b838-5490ff4bf0ce
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# Métadonnées personnalisées {#cm}

>[!NOTE]
>
> Cette page est obsolète car elle s’applique à la version précédente de l’API qui n’est plus recommandée pour les nouvelles intégrations.

Le service permet aux clients d’utiliser des champs standard et personnalisés dans les requêtes et la prise de décision. Les champs standard sont toujours disponibles pour n’importe quel flux (car ils sont obligatoires pour la création de flux ou générés par le serveur) :

* application
* mvpd
* accountId
* streamId
* streamStart
* initiateur


Les champs personnalisés sont toutes les paires clé/valeur transmises lors de l’initialisation du flux, sous la forme de paramètres de chaîne de formulaire ou de requête. Les champs standard et personnalisés peuvent ensuite être utilisés dans les scénarios suivants (pour plus d’informations, consultez la documentation réelle des ressources API impliquées ci-dessous) :

* Demande de champs supplémentaires (via le paramètre de chaîne de requête des champs) lors de la récupération de la liste de diffusion pour un compte (la ressource /streams)
* Ventilation de l’activité de compte en spécifiant les dimensions de groupe par (la ressource /activity)
* Définition de stratégies côté serveur basées sur des valeurs de champ ou des cardinalités (les exemples utilisent pseudo-SQL uniquement pour plus de clarté) :
* Configurez une stratégie de sorte qu’elle s’applique uniquement à des valeurs de champ spécifiques (par exemple, une stratégie iOS dédiée : WHERE osType IS &#39;iOS&#39;).
* Limitez le nombre de valeurs distinctes pour un champ donné (par exemple, pas plus de X appareils distincts : AVING DISTINCT COUNT(deviceId) >= 2)
* Limitez le nombre de diffusions actives par valeur de champ (par exemple, pas plus de X diffusions actives pour un seul type d’appareil : GROUP BY deviceType HAVING COUNT(streamId) >= 3)


En fonction de ces clés/valeurs envoyées, différentes règles peuvent être établies. Il peut s’agir de l’une des étapes suivantes :

1. Le client décide d’envoyer le groupe de paramètres, qui aura comme valeurs &quot;SPORTS&quot; et &quot;KIDS&quot;.
1. Ensuite, l’application doit procéder comme suit :
   * Pour les canaux sportifs, lors de l’initialisation du flux, l’application enverrait ***type=SPORTS*** comme paramètre de requête.
   * Pour les canaux avec du contenu lié aux enfants, lors de l’initialisation du flux, l’application enverrait ***type=KIDS*** comme paramètre de requête.
1. Une stratégie de ce type peut ensuite être définie :
   * `GROUP by type HAVING COUNT(streamID) < 4) IF type=KIDS`
   * `GROUP by type HAVING COUNT(streamID) < 2) IF type=SPORTS`
1. Cela signifie essentiellement que lorsqu’un utilisateur regarde du sport, il ne peut pas le faire sur plus d’un appareil. Toutefois, lorsque l’utilisateur regarde le contenu des enfants, l’affichage est autorisé sur les appareils 3 max.
