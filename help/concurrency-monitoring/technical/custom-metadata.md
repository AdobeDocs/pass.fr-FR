---
title: Métadonnées personnalisées
description: Métadonnées personnalisées
exl-id: 0cfd1158-8c6c-47c2-b838-5490ff4bf0ce
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# Métadonnées personnalisées {#cm}

>[!NOTE]
>
> Cette page est obsolète, car elle s’applique à la version précédente de l’API qui n’est plus recommandée pour les nouvelles intégrations

Le service permet aux clients d’utiliser à la fois les champs standard et personnalisés dans les requêtes et la prise de décision. Les champs standard sont toujours disponibles pour n’importe quel flux (car ils sont obligatoires pour la création du flux ou générés par le serveur) :

* application
* mvpd
* accountId
* streamId
* streamStart
* initiateur


Les champs personnalisés sont toutes les paires clé/valeur transmises lors de l’initialisation du flux, sous la forme de paramètres de formulaire ou de chaîne de requête. Les champs standard et personnalisés peuvent ensuite être utilisés dans les scénarios suivants (pour plus d’informations, consultez la documentation réelle pour les ressources d’API impliquées ci-dessous) :

* Demande de champs supplémentaires (via le paramètre de chaîne de requête fields) lors de la récupération de la liste de flux pour un compte (la ressource /streams)
* Répartir l’activité du compte en spécifiant les dimensions à regrouper (la ressource /activity)
* Définir des politiques côté serveur en fonction des valeurs de champ ou des cardinalités (les exemples utilisent le langage pseudo-SQL uniquement pour plus de clarté) :
* Configurez une politique à appliquer uniquement à des valeurs de champ spécifiques (par exemple, une politique iOS dédiée : OÙ osType EST « iOS »).
* Limitez le nombre de valeurs distinctes pour un champ donné (par exemple, pas plus de X appareils distincts : HAVING DISTINCT COUNT(deviceId) >= 2)
* Limitez le nombre de flux actifs par valeur de champ (par exemple, pas plus de X flux actifs pour un seul type d’appareil : GROUP BY deviceType HAVING COUNT(streamId) >= 3)


En fonction de ces clés/valeurs envoyées, différentes règles peuvent être établies. Il pourrait s’agir de l’un des éléments suivants :

1. Le client décide d&#39;envoyer le groupe de paramètres qui aura pour valeurs « SPORTS » et « KIDS ».
1. L’application doit alors effectuer les opérations suivantes :
   * Pour les canaux sportifs, à l’initialisation du flux, l’application enverrait ***type=SPORTS*** comme paramètre de requête
   * Pour les canaux avec du contenu associé aux enfants, à l’initialisation du flux, l’application enverrait ***type=KIDS*** comme paramètre de requête
1. Ensuite, une politique comme celle-ci peut être définie :
   * `GROUP by type HAVING COUNT(streamID) < 4) IF type=KIDS`
   * `GROUP by type HAVING COUNT(streamID) < 2) IF type=SPORTS`
1. Cela signifie essentiellement que lorsqu’un utilisateur regarde un sport, il ne peut pas le faire sur plus d’un appareil, mais lorsque l’utilisateur regarde du contenu pour enfants, la visualisation est autorisée sur 3 appareils au maximum.
