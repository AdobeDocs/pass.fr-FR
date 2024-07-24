---
title: Configuration de votre environnement et test dans un environnement de préqualification
description: Configuration de votre environnement et test dans un environnement de préqualification
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: 3a6a5633c728398a3847ee3e341e82aba915f0d9
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Configuration de votre environnement et tests en pré-qualification{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre informatif seulement. L’utilisation de cette API nécessite une licence en cours de validité d’Adobe. Aucune utilisation non autorisée n’est autorisée.

L’objectif de cette note technique est d’aider nos partenaires à configurer leur environnement et à commencer à tester une nouvelle version déployée sur l’environnement de préqualification Adobe.

Puisqu’il existe deux types de build : ***la production*** et ***le staging***, dans ce document, nous nous concentrerons sur la configuration de la production avec la mention que toutes les étapes sont les mêmes pour le staging, seules les URL sont différentes.

Les étapes 1 et 2 consistent à configurer l’environnement de test sur l’une des machines d’essai, l’étape 3 est une vérification du flux de base et les étapes 4 et 5 présentent quelques directives de test.

>[!IMPORTANT]
>
> Il est très important d’exécuter les étapes 1 et 2 chaque fois que vous souhaitez changer d’environnement de test (passage du profil intermédiaire au profil de production ou l’inverse)


## ÉTAPE 1. Résolution d’un problème de transfert de domaine vers une adresse IP {#resolving-pass-domain-to-an-ip}

* Pour trouver une adresse IP de l&#39;équilibreur de charge utilisable pour l&#39;usurpation, exécutez la commande suivante :

* **Sous Windows**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

* **Sur Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

>[!NOTE]
>
>Domaines exclus de la réponse car ils ne sont pas pertinents et peuvent différer d’un utilisateur à l’autre.

>[!IMPORTANT]
>
> Ces adresses IP peuvent changer à l’avenir et elles peuvent ne pas être les mêmes pour les utilisateurs de différentes régions géographiques.


## ÉTAPE 2.  Usurpation de l’environnement de pré-qualification en production {#spoofing-the-prequalification-environment}

* Modifiez le *fichier c :\\windows\\System32\\drivers\\etc\\hosts* (sous Windows) ou */etc/hosts* (sous Macintosh/Linux/Android) et ajoutez ce qui suit :

* Profil de production d’usurpation
   * Référence 52.13.71.11 entitlement.auth.adobe.com sp.auth.adobe.com api.auth.adobe.com

**Spoofing sur Android :** Pour parodier sur Android, vous devez utiliser un émulateur Android.

* Une fois l’usurpation d’identité mise en place, vous pouvez simplement utiliser les URL standard pour les profils de production et d’évaluation : (c’est-à-dire, `http://sp.auth-staging.adobe.com` et `http://entitlement.auth-staging.adobe.com` et vous atteindrez l’*environnement de préqualification/ production* du* nouveau build.


## ÉTAPE 3.  Vérifiez que vous pointez vers l’environnement approprié. {#Verify-you-are-pointing-to-the-right-environment}

**C’est une étape facile :**

* Load [Entitlement prequal environment](https://entitlement-prequal.auth.adobe.com/environment.html) et [entitlement](https://entitlement.auth.adobe.com/environment.html). Ils devraient renvoyer la même réponse.


## ÉTAPE 4.  Effectuer un flux d’authentification/autorisation simple à l’aide du site Web du programmeur {#peform-a-simple-auth-flow}

* Cette étape nécessite l’adresse du site Web du programmeur et des informations d’identification MVPD valides (un utilisateur authentifié et autorisé).

## ÉTAPE 5.  Effectuer des tests de scénarios à l’aide des sites Web du programmeur {#perform-scenario-testing-using-programmer-website}

* Une fois la configuration de l’environnement terminée et vous être assuré que le flux d’authentification-autorisation de base fonctionne, vous pouvez passer au test de scénarios plus complexes.


## ÉTAPE 6.  Effectuer des tests à l’aide du site de test d’API {#perform-testing-using-api-testing-site}

* Si vous souhaitez approfondir le test de l’authentification Adobe Pass, nous vous recommandons d’utiliser le [site de test d’API](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Vous trouverez plus d’informations sur le site de test d’API à l’adresse [Comment tester les flux d’authentification et d’autorisation à l’aide du site de test d’API d’Adobe](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).
