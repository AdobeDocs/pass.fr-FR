---
title: Fonction de contrôle en amont, comment activer, résoudre les problèmes ou déterminer le problème
description: Fonction de contrôle en amont, comment activer, résoudre les problèmes ou déterminer le problème
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 0%

---

# Fonctionnalité de contrôle en amont (héritée) : Activation, dépannage ou détermination du problème {#preflight-feature}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

La façon dont l’authentification Adobe Pass calcule les ressources preAuthorizeResources a changé. L’API PreAuthorization a une nouvelle mise en œuvre. Cette implémentation remplace l’ancienne solution qui consistait à effectuer uniquement des appels d’autorisation multiples.
L’interface externe de l’API PreAuthorization reste inchangée ; aucune mise à jour n’est requise dans l’application du programmeur.

Les ressources de contrôle en amont sont calculées de trois manières différentes :

* **Méthode de branchement et de jointure à MVPD** : cela implique que l&#39;Adobe effectue plusieurs appels d&#39;autorisation vers MVPD (le client devra toutefois effectuer un seul appel de contrôle en amont).
* **Alignement des canaux** : le MVPD expose l’alignement des canaux pour l’utilisateur connecté dans la réponse d’authentification SAML et l’Adobe renvoie les ressources autorisées en fonction de cela. La réponse authN SAML dans le traceur SAML doit exposer cette liste.
* **Autorisation multicanal** : l’authentification du client et de l’Adobe effectuent un seul appel à MVPD pour un ensemble de ressources.

Quel que soit le MVPD, l’application cliente effectue un seul appel au point d’entrée de contrôle en amont (checkPreauthorizedResources API), en transmettant un ensemble d’ID de ressource. Adobe utilisera l’une des méthodes ci-dessus prises en charge par MVPD pour renvoyer les ResourceID préautorisés.

Si le contrôle en amont repose sur la méthode branchement et jointure, le serveur principal d’authentification Adobe Pass vérifie une valeur définie pour les « appels de préautorisation max. » dans sa configuration. Elle est configurée par Adobe.

La valeur par défaut pour la configuration « appels de préautorisation max. » est « 5 », ce qui signifie qu’au maximum, seules 5 ressources peuvent être envoyées dans le contrôle en amont pour les MVPD de branchement et de jointure. La transmission de plus de 5 ressources entraîne une exception et une liste nulle est renvoyée. Il s’agit du comportement attendu. Nous pouvons le configurer sur n’importe quelle valeur si le MVPD ne prend pas en charge la configuration des canaux ou l’autorisation multicanal, mais uniquement après les avoir consultés, car les appels d’autorisation de branchement et de jointure multiples augmentent leurs temps de chargement.

Par conséquent, voici les éléments à prendre en compte lors de l’activation/du dépannage du contrôle en amont pour un MVPD :

* Méthode prise en charge par le MVPD (branchement et jointure, alignement de canaux ou multicanal).
* Si seul le branchement et la jointure sont pris en charge, il faut demander au programmeur combien d’ID de ressource il enverra dans l’appel de contrôle en amont.
* Le MVPD doit être consulté et doit connaître l’impact d’un nombre « n » d’appels d’autorisation de branchement et de jointure. Par la suite, la valeur doit être configurée dans config si elle est supérieure à 5.

**Limitation**

Veuillez noter que nous ne récupérons aucun resourceID de l’appel de contrôle en amont pour certains MVPD tels qu’AT&amp;T et TWC si l’un des resourceID est un faux ID ou un ID non reconnu dans la liste des resourceID qu’ils envoient dans l’appel de contrôle en amont, même si nous avons également des ressources valides et autorisées dans cette liste.
