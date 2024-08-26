---
title: Amazon FireOS SSO à l’aide du guide pas à pas API client
description: Amazon FireOS SSO à l’aide du guide pas à pas API client
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '761'
ht-degree: 0%

---

# Amazon FireOS SSO à l’aide du guide pas à pas API client {#amazon-fireos-sso-using-clientless-api-cookbook}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Introduction {#Introduction}

Ce document fournit des instructions pour mettre en oeuvre la version SSO d’Amazon du flux d’authentification Adobe Pass à l’aide de l’API sans client. La première partie de ce document se concentre sur la spécificité de la version Amazon de l’architecture, pour les nombreux partenaires déjà familiarisés et expérimentés avec sa mise en oeuvre.

La deuxième partie du document décrit les principales étapes de mise en oeuvre de l’API sans client d’authentification Adobe Pass.

Pour une présentation technique générale du fonctionnement de la solution sans client, consultez la [présentation de l’API REST](/help/authentication/rest-api-overview.md). Adobe est le contact préféré pour la prise en charge de l’architecture globale et des premières implémentations.

## SSO sans client Amazon {#AMZ-Clientless-SSO}

### Architecture de haut niveau {#High-Level-Arch}

L’implémentation d’authentification unique sans client Amazon est simple et presque identique aux API sans client standard d’authentification Adobe Primetime.

Vous devrez utiliser le SDK Amazon pour récupérer une payload personnalisée et l’utiliser lors de l’appel d’API sans client Adobe.

Si la charge utile est reconnue et correspond à une session authentifiée, les API sans client renvoient immédiatement le jeton de votre session.

### Comment créer l’application pour utiliser le SDK Amazon {#Build-entries}

* Téléchargez et copiez le dernier [SDK Amazon Stub](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) dans un dossier /SSOEnabler parallèlement au répertoire de l’application.
* Mettez à jour les fichiers manifest/gradle pour utiliser la bibliothèque :

  **Ajoutez la ligne suivante à votre fichier de manifeste :**

  ```Java
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false"/\>
  ```

  **Entrées de fichier Gradle :**

  Sous Référentiels :

  ```java
  flatDir {
       dirs '../SSOEnabler'
  }
  ```

  Sous dependencies, ajoutez :

  ```Java
  provided fileTree(include: \['ottSSOTokenStub.jar'\], dir: '../SSOEnabler')
  ```


* Gestion de l’absence de l’application compagnon Amazon :

  Bien que cela ne soit pas probable, si le compagnon n’est pas présent sur le périphérique Amazon que votre application est en cours d’exécution, vous devez rencontrer une ClassNotFoundException au moment de l’exécution sur la classe suivante : `com.amazon.ottssotokenlib.SSOEnabler`.

  Si cela se produit, il vous suffit d’ignorer l’étape de payload et de revenir au flux PrimeTime normal. SSO ne sera pas activé, mais le flux d’authentification standard se produira normalement.

</br>

### Comment obtenir la payload SSO Amazon à l’aide du SDK Amazon {#UseAmazonSSO}

Lors de l’initialisation de votre application, obtenez une instance de SSOEnabler. En fonction de l’architecture de votre application, vous devez choisir entre une mise en oeuvre synchrone ou asynchrone.

Si, pour une raison quelconque, les appels d’API ne renvoient pas de payload, utilisez le flux non SSO normal et contactez vos partenaires Amazon et Adobe pour enquêter.

**API asynchrone**

* Obtenez l’instance SSO Enabler :

  ```Java
  ssoEnabler = SSOEnabler.getInstance(context);
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```


* Définition du rappel

  ```java
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

   * Le lot de réponse de succès contiendra :
      * Jeton SSO sous forme de chaîne avec la clé &quot;SSOToken&quot;
   * Le lot de réponse à l’échec contient :
      * code d’erreur en tant qu’entier avec la clé &quot;ErrorCode&quot;
      * description de l’erreur en tant que chaîne avec la clé &quot;ErrorDescription&quot;


* Obtention du jeton SSO

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

* Cette API fournira la réponse via le jeu de rappel lors de l’initialisation.

  **Ex**. appel à l’aide de l’instance singleton créée pendant init :

  ```JAVA
  ssoEnabler.getSSOTokenAsync().
  ```


**API synchrones**

* Obtention de l’instance SSO Enabler et définition du rappel

  ```JAVA
  ssoEnabler = SSOEnabler.getInstance(context);</span>
  ```

* Obtention du jeton SSO

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

   * Cette API bloquera le thread appelant et répondra avec le lot de résultats. Comme il s’agit d’un appel synchrone, veillez à ne pas l’utiliser dans votre thread principal.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

   * Valeur en millisecondes. si cette valeur est définie, remplacez la valeur du délai d’expiration par défaut de 1 minute pour l’API de synchronisation.


### Mise à jour de l’API sans client Adobe Pass pour utiliser l’enregistrement client dynamique {#clientlessdcr}

S’il s’agit de votre première implémentation, consultez la **présentation technique sans client** et contactez l’Adobe si vous avez besoin d’assistance.

L’API sans client Adobe exige que les applications utilisent l’enregistrement client dynamique pour effectuer des appels vers les serveurs Adobe.

* Pour utiliser l’enregistrement du client dynamique dans votre application, suivez les instructions de la [gestion de l’enregistrement du client dynamique](./dcr-api/dynamic-client-registration-overview.md#dynamic-client-registration-management) pour créer une application enregistrée et télécharger une instruction logicielle.

* Pour mettre en oeuvre l’API d’enregistrement du client dynamique afin d’effectuer des demandes d’authentification et d’autorisation sur les serveurs Adobe Pass, suivez les instructions de la section [ Flux d’enregistrement du client dynamique ](./dcr-api/flows/dynamic-client-registration-flow.md).

### Mise à jour de l’API sans client d’Adobe Pass pour utiliser la fonction SSO d’Amazon {#clientlesssso}

La charge utile Amazon SSO obtenue à partir du SDK Amazon doit être présente sur les demandes effectuées aux points de terminaison d’authentification Adobe Pass :

```
      /adobe-services/*
      /reggie/*
      /api/*
```


Tous les points de terminaison d’authentification Adobe Pass prennent en charge les méthodes suivantes pour recevoir l’identifiant de portée de périphérique ou l’identifiant de portée de plateforme (présent dans la payload SSO Amazon) :

* En-tête : &quot;Adobe-Objet-Jeton&quot;
* En tant que paramètre de requête : &quot;ast&quot;
* En tant que paramètre de publication : &quot;ast&quot;


>[!NOTE]
>
>Si l’identifiant de l’appareil ou l’identifiant de la plateforme est envoyé en tant que paramètre de requête/publication, il doit être inclus lors de la génération de la signature de la requête.

>[!NOTE]
>
>En utilisant le paramètre de requête &quot;ast&quot;, l’URL entière peut devenir très longue et rejetée. Lors de l’appel /authenticate, ce paramètre peut être ignoré tel qu’il a été fourni à l’appel /regcode .

**Exemples :**

**Envoi en tant qu&#39;en-tête personnalisé**

```HTTPS
GET /adobe-services/config/requestor HTTP/1.1 Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**Envoi en tant que paramètre de requête**

```HTTPS
GET /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com
```


**Envoi en tant que paramètre de publication**


```HTTPS
POST /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com Content-Type: multipart/form-data;
boundary=---- WebKitFormBoundary7MA4YWxkTrZu0gW
```

>[!NOTE]
>
>Si le SSO Amazon n’est pas présent ou non valide, l’authentification Adobe Pass ignore l’attribut et les appels sont exécutés comme si SSO n’était pas présent.
