---
title: Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API d’Adobe
description: Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API d’Adobe
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API d’Adobe {#How-to-test-auth-flows}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Pour tester les flux AuthN et AuthZ, nous avons préparé un **site de test d’API** à votre disposition. Notre équipe d’assistance sera ravie de vous fournir des informations d’identification. Vous pouvez nous contacter à l&#39;adresse **support@tve.zendesk.com**.


## Partie I {#part-I}

Pour les tests par rapport à l’environnement VERSION, accédez directement à la Partie II.  Pour effectuer un test dans l’environnement de préqualification, voir [Configuration de votre environnement et test dans l’environnement de préqualification](/help/authentication/notes-technical/setting-up-your-environment-and-testing-in-prequal.md).

## Partie II

Après avoir terminé la partie I, effectuez les étapes suivantes :


1. Ouvrez la page web : [Test de l’API intermédiaire](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Chargez l’activateur d’accès à partir de l’URL suivante :
   * [Accédez à JavaScript d’activation pour l’évaluation](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * OU
   * [Accédez à JavaScript d’activation pour la production](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * PUIS cliquez sur le bouton &quot;**Load Access Enabler**&quot;.
1. Définissez maintenant la valeur de l’ID du demandeur sur &quot;**requestorID**&quot; et cliquez sur le bouton &quot;setRequestor&quot;.
1. Ensuite, appuyez sur le bouton &quot;getAuthentication&quot; et attendez que le sélecteur d’affichage s’affiche.
1. Sélectionnez &quot;**MVPD**&quot; dans le sélecteur.
1. Entrez vos informations d’identification sur la page de connexion &quot;**MVPD**&quot;.
1. Après avoir été redirigé vers le passé, rétablir les étapes 1 à 3
1. Après avoir rétabli l’étape 3 sur &quot;setAuthenticationStatus&quot;, la valeur &quot;1&quot; devrait s’afficher. Si l’authentification n’a pas fonctionné, la boîte de dialogue MVPD s’affiche.
1. Pour tester l’autorisation, dans le champ de saisie de ressource, saisissez &quot;**requestorID**&quot; et cliquez sur le bouton &quot;getAuthorization&quot;.
1. Par conséquent, dans la zone de texte &quot;setToken&quot;-\>&quot;resource id&quot; , la ressource s’affiche et dans la zone de texte &quot;setToken&quot;-\>&quot;token&quot;, shortAuthorizationToken s’affiche, ce qui signifie que l’authZ a réussi.
1. Vous pouvez maintenant cliquer sur le bouton &quot;déconnexion&quot; pour supprimer les jetons.
