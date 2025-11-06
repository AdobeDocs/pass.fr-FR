---
title: Configuration de votre environnement et tests dans Pre-Qual
description: Configuration de votre environnement et tests dans Pre-Qual
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# Configuration de votre environnement et tests dans Pre-Qual{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Cette note technique a pour but d’aider nos partenaires à configurer leur environnement et à commencer à tester une nouvelle version déployée dans l’environnement de préqualification Adobe.

Comme il existe deux versions de build : ***production*** et ***évaluation***, dans ce document nous nous concentrerons sur la configuration de production avec la mention que toutes les étapes sont identiques pour l’évaluation, seules les URL sont différentes.

Les étapes 1 et 2 consistent à configurer l’environnement de test sur l’une des machines de test, l’étape 3 consiste à vérifier le flux de base et les étapes 4 et 5 présentent quelques directives de test.

>[!IMPORTANT]
>
> Il est très important d’exécuter les étapes 1 et 2 chaque fois que vous souhaitez modifier votre environnement de test (en passant d’un profil d’évaluation à un profil de production ou inversement)


## ÉTAPE 1. Résolution du domaine Pass sur une adresse IP {#resolving-pass-domain-to-an-ip}

* Pour trouver une adresse IP de répartition de charge pouvant être utilisée pour l’usurpation de nom, exécutez la commande suivante :

* **Sous Windows**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

```cmd
C:\>nslookup entitlement-prequal.auth.adobe.com 
...
Addresses:  52.26.79.43
            54.190.212.171
```

```Choose any IP from **addresses** section (e.g. `54.190.212.171)```


* **Sous Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

```sh
    $ dig entitlement-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.26.79.43
    ............ 60 IN A      54.190.212.171
```

```Choose any IP from **A records (**e.g `54.190.212.171)```

>[!NOTE]
>
>Domaines exclus de la réponse, car ils ne sont pas pertinents et peuvent différer d’un utilisateur à l’autre.

>[!IMPORTANT]
>
> Il se peut que ces adresses IP changent à l’avenir et qu’elles ne soient pas les mêmes pour les utilisateurs de différentes régions géographiques.


## ÉTAPE 2.  Spirituation de l’environnement de préqualification en production {#spoofing-the-prequalification-environment}

* Modifiez le fichier *c:\\windows\\System32\\drivers\\etc\\hosts* (sous Windows) ou */etc/hosts* (sous Macintosh/Linux/Android) et ajoutez les éléments suivants :

* Profil de production d&#39;usurpation
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**Usurpation sur Android :** pour usurper Android, vous devez utiliser un émulateur Android.

* Une fois l’usurpation en place, vous pouvez simplement utiliser les URL standard pour les profils de production et d’évaluation : (c’est-à-dire, `http://sp.auth-staging.adobe.com` et `http://entitlement.auth-staging.adobe.com`, et vous accéderez à l’*environnement de préqualification/production* de la nouvelle build*.


## ÉTAPE 3.  Vérifiez que vous pointez vers le bon environnement. {#Verify-you-are-pointing-to-the-right-environment}

**Il s’agit d’une étape facile**

* chargement [environnement préégal de droits](https://entitlement-prequal.auth.adobe.com/environment.html) et [droits](https://entitlement.auth.adobe.com/environment.html). Ils doivent renvoyer la même réponse.


## ÉTAPE 4.  Exécuter un flux d’authentification/autorisation simple à l’aide du site web du programmeur {#peform-a-simple-auth-flow}

* Cette étape nécessite l’adresse du site web du programmeur et des informations d’identification MVPD valides (un utilisateur authentifié et autorisé).

## ÉTAPE 5.  Effectuer des tests de scénario à l’aide des sites web du programmeur. {#perform-scenario-testing-using-programmer-website}

* Après avoir terminé la configuration de l’environnement et vérifié que le flux d’authentification-autorisation de base fonctionne, vous pouvez procéder au test de scénarios plus complexes.


## ÉTAPE 6.  Effectuer des tests à l’aide du site de test d’API {#perform-testing-using-api-testing-site}

* Si vous souhaitez approfondir le test de l’authentification Adobe Pass, nous vous recommandons d’utiliser le site de test [API](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Pour plus d’informations sur le site de test des API, consultez [Comment tester les flux d’authentification et d’autorisation à l’aide du site de test des API Adobe](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).
