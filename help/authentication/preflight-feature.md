---
title: Fonctionnalité de contrôle en amont, activation, dépannage ou détermination du problème
description: Fonctionnalité de contrôle en amont, activation, dépannage ou détermination du problème
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# Fonctionnalité de contrôle en amont : comment activer, résoudre ou déterminer le problème {#preflight-feature}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

La façon dont l’authentification Adobe Pass calcule preAuthorizeResources a été modifiée. L’API PreAuthorization comporte une nouvelle implémentation. Cette implémentation remplace l’ancienne solution qui comprend la création de plusieurs appels d’autorisation uniquement.
L’interface externe de l’API PreAuthorization est inchangée. Aucune mise à jour n’est requise dans l’application du programmeur.

Les ressources Preflight peuvent être calculées de trois façons :

* **Méthode de branchement à MVPD** : cela implique l’Adobe d’effectuer plusieurs appels d’autorisation au MVPD (le client devra toutefois effectuer un appel de contrôle en amont).
* **Ligne de canal** : le MVPD expose la ligne de canal vers le haut pour l’utilisateur connecté dans la réponse d’authentification SAML et l’Adobe renvoie les ressources autorisées en fonction de cela. La réponse SAML authN dans le traceur SAML doit exposer cette liste.
* **Autorisation multicanal** : l’authentification client et Adobe effectue tous deux un appel unique au MVPD pour un ensemble de ressources.

Quel que soit le MVPD, l’application cliente effectuera un appel unique au point de terminaison Preflight (API checkPreauthorizedResources), en transmettant un ensemble d’identifiants de ressource. Selon l’une des méthodes ci-dessus prises en charge par MVPD, Adobe renvoie alors les ID de ressource préautorisés.

Si le contrôle en amont est basé sur la méthode fork &amp; join , le serveur principal d’authentification Adobe Pass vérifie une valeur définie pour les &quot;appels de préautorisation max.&quot; dans sa configuration. Il est configuré par Adobe.

La valeur par défaut de la configuration &quot;appels de préautorisation max.&quot; est &quot;5&quot;, ce qui signifie qu’au maximum 5 ressources peuvent être envoyées en prévol pour les MVPD de branchement et de jointure. Le transfert de plus de 5 ressources entraînera une exception et une liste nulle sera renvoyée. Il s’agit du comportement attendu. Nous pouvons le configurer sur n’importe quelle valeur si le MVPD ne prend pas en charge la ligne de canal ou l’autorisation multi-canal, mais seulement après les avoir consultés comme appels d’autorisation de branchement et de jointure multiples augmentera leur temps de chargement.

Par conséquent, voici les éléments à rechercher lors de l’activation/de la résolution des problèmes de contrôle en amont pour un MVPD :

* Méthode prise en charge par le MVPD (branchement et jointure, alignement de canal ou multicanal).
* Si seul le branchement et la jointure sont pris en charge, le programmeur doit se voir demander combien de resourceID il enverra dans l’appel de contrôle en amont.
* Le MVPD doit être consulté et doit connaître l’impact d’un nombre &#39;n&#39; d’appels d’autorisation de branchement et de jointure. Ensuite, la valeur doit être configurée dans config si elle est supérieure à 5.

**Limitation**

Veuillez noter que nous ne récupérons pas de resourceID depuis l’appel de contrôle en amont pour certains MVPD comme AT&amp;T &amp; TWC si l’un des resourceID est un faux ID ou un ID non reconnu dans la liste des resourceID qu’ils envoient dans l’appel de contrôle en amont même si nous avons également des ressources valides et autorisées dans cette liste.
