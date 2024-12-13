---
title: Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API d’Adobe
description: Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API d’Adobe
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# (Hérité) Comment tester les flux d’authentification et d’autorisation à l’aide du site de test d’API d’Adobe {#How-to-test-auth-flows}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Afin de tester les flux AuthN et AuthZ, nous avons préparé un site de test **API** à votre disposition. Notre équipe d&#39;assistance se fera un plaisir de vous fournir les informations d&#39;identification. Vous pouvez nous contacter à **support@tve.zendesk.com**.


## Première partie {#part-I}

Pour effectuer un test par rapport à l’environnement RELEASE, passez directement à la partie II.  Pour effectuer des tests dans l’environnement de préqualification, voir [ Configuration de votre environnement et Tests dans la préqualification ](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md).

## Deuxième partie

Après avoir terminé la première partie, effectuez les étapes suivantes :


1. Ouvrez la page web : [test de l’API Staging](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Chargez l’activateur d’accès à partir de l’URL suivante :
   * [Javascript d’activation d’Access pour l’évaluation](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * SOIT
   * [Javascript d’activation d’Access pour la production](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * CLIQUEZ ENSUITE sur le bouton « **Charger l’activateur d’accès** ».
1. Définissez maintenant la valeur de l’ID du demandeur sur « **requestorID** » et cliquez sur le bouton « setRequestor ».
1. Ensuite, appuyez sur le bouton « getAuthentication » et attendez l’affichage du sélecteur.
1. Sélectionnez le « **MVPD** » dans le sélecteur.
1. Saisissez vos informations d&#39;identification sur la page de connexion « **MVPD** ».
1. Après avoir été redirigé vers , répétez les étapes 1 à 3
1. Après avoir rétabli l’étape 3 sur « setAuthenticationStatus », vous devriez voir la valeur « 1 ». Si l’authentification n’a pas fonctionné, la boîte de dialogue MVPD s’affiche.
1. Pour tester l’autorisation, dans le champ d’entrée de ressource, saisissez « **requestorID** » et cliquez sur le bouton « getAuthorization ».
1. Par conséquent, dans la zone de texte « setToken »-\>« resource id », la ressource s’affiche et dans la zone de texte « setToken »-\>« token », shortAuthorizationToken s’affiche, ce qui signifie que l’authentification a réussi.
1. Vous pouvez maintenant cliquer sur le bouton « se déconnecter » pour supprimer les jetons.
