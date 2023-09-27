---
title: Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API d’Adobe
description: Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API d’Adobe
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API d’Adobe {#How-to-test-auth-flows}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Pour tester les flux AuthN et AuthZ, nous avons préparé une **Site de test d’API** qui est à votre disposition. Notre équipe d’assistance sera ravie de vous fournir des informations d’identification. Vous pouvez nous contacter via **support@tve.zendesk.com**.


## Partie I {#part-I}

Pour les tests par rapport à l’environnement VERSION, accédez directement à la Partie II.  Pour effectuer un test dans l’environnement de préqualification, voir [Configuration de votre environnement et test dans un environnement de préqualification](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

## Partie II

Après avoir terminé la partie I, effectuez les étapes suivantes :


1. Ouvrez la page web : [Test de l’API d’évaluation](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Chargez l’activateur d’accès à partir de l’URL suivante :
   * [Accès à JavaScript d’activation pour l’évaluation](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * OU
   * [Accès à JavaScript d’activation pour la production](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * CLIQUEZ ENSUITE sur le **Load Access Enabler**&quot;.
1. Maintenant, définissez la valeur de l’ID du demandeur sur &quot;**requestorID**&quot; et cliquez sur le bouton &quot;setRequestor&quot;.
1. Ensuite, appuyez sur le bouton &quot;getAuthentication&quot; et attendez que le sélecteur d’affichage s’affiche.
1. Sélectionnez le **MVPD**&quot; dans le sélecteur.
1. Saisissez vos informations d’identification sur le **MVPD**&quot;, page de connexion.
1. Après avoir été redirigé vers le passé, rétablir les étapes 1 à 3
1. Après avoir rétabli l’étape 3 sur &quot;setAuthenticationStatus&quot;, la valeur &quot;1&quot; devrait s’afficher. Si l’authentification n’a pas fonctionné, la boîte de dialogue MVPD s’affiche.
1. Pour tester l&#39;autorisation, dans le champ de saisie de la ressource, saisissez &quot;**requestorID**&quot; et cliquez sur le bouton &quot;getAuthorization&quot;.
1. Par conséquent, dans la zone de texte &quot;setToken&quot;-\>&quot;resource id&quot; , la ressource s’affiche et dans la zone de texte &quot;setToken&quot;-\>&quot;token&quot;, shortAuthorizationToken s’affiche, ce qui signifie que l’authZ a réussi.
1. Vous pouvez maintenant cliquer sur le bouton &quot;déconnexion&quot; pour supprimer les jetons.
