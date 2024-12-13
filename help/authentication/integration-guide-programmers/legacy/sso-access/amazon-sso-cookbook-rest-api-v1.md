---
title: Manuel de l’authentification unique Amazon (API REST V1)
description: Manuel de l’authentification unique Amazon (API REST V1)
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Manuel de l’authentification unique Amazon (hérité) (API REST V1) {#amazon-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

L’API REST d’authentification Adobe Pass V1 prend en charge l’authentification unique (SSO) de Platform pour les utilisateurs finaux des applications clientes s’exécutant sur FireOS.

Ce document agit comme une extension de l’aperçu [API REST V1](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md) existant qui fournit une vue d’ensemble.

## authentification unique Amazon à l’aide des flux d’identité de la plateforme {#cookbook}

### Conditions préalables {#prerequisites}

Avant de poursuivre l’authentification unique Amazon à l’aide des flux d’identité de plateforme, assurez-vous que les conditions préalables suivantes sont remplies.

#### Intégrer Amazon SSO SDK {#integrate-amazon-sso-sdk}

L’application de diffusion en continu doit intégrer la bibliothèque [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) pour l’authentification unique (SSO) dans sa version.

* Téléchargez et copiez la dernière bibliothèque Amazon SSO SDK dans un dossier `/SSOEnabler` parallèle au répertoire de l’application.

* Mettez à jour les fichiers manifeste et Gradle pour utiliser la bibliothèque SDK SSO d’Amazon.

  **Manifest:**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  Sous les référentiels :

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  Sous dependencies :

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Utiliser Amazon SSO SDK {#use-amazon-sso-sdk}

L’application de diffusion en continu doit utiliser le SDK SSO d’Amazon pour obtenir la payload du jeton SSO (identité de la plateforme).

Le SDK SSO d’Amazon fournit des API synchrones et asynchrones pour obtenir la payload du jeton SSO (identité de la plateforme).

L’application de diffusion en continu peut choisir l’une des deux options en fonction de son architecture.

##### API asynchrones

* Récupérez l’instance de `SSOEnabler` et définissez le `SSOEnablerCallback` :

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  Cela peut être effectué lors de l’initialisation de l’application de diffusion en continu.

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  Le lot de réponse de succès du jeton SSO contiendra :
   * Un jeton SSO en tant que `string` avec la clé « SSOToken ».

  <br/>

  Le lot de réponse d’échec du jeton SSO contient :
   * Un code d’erreur en tant que `int` avec la clé « ErrorCode ».
   * Une description d’erreur en tant que `string` avec la clé « ErrorDescription ».

  <br/>

* Obtenez le jeton SSO :

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  Cette API fournit la réponse via le rappel défini lors de l’initialisation.

##### API synchrones

* Récupérez l’instance `SSOEnabler` :

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* Obtenez le jeton SSO :

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  Cette API bloque le thread appelant et répond avec le lot de résultats. Puisqu’il s’agit d’un appel synchrone, veillez à ne pas l’utiliser dans votre thread principal.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  Cette API définit la valeur du délai d’expiration de l’appel synchrone. La valeur de délai d’expiration par défaut est de 1 minute.

#### Secours pour l’authentification unique Amazon {#fallback-amazon-sso}

L’application de diffusion en continu doit gérer les scénarios de secours du flux SSO d’Amazon au flux d’authentification standard.

Assurez-vous que l’application de diffusion en continu gère :

* L’absence de l’application Amazon complémentaire qui doit s’exécuter sur l’appareil Amazon.
   * L’application de diffusion en continu peut rencontrer une `ClassNotFoundException` au moment de l’exécution sur la `com.amazon.ottssotokenlib.SSOEnabler` de classe suivante.

* L’absence de payload du jeton SSO (identité de plateforme) qui doit être renvoyée par les API ci-dessus.
   * L’application de streaming peut contacter Amazon et les représentants de l’Adobe pour en savoir plus.

### Workflow {#workflow}

La payload du jeton SSO Amazon (identité de la plateforme) doit être présente sur toutes les requêtes HTTP effectuées sur les points d’entrée de l’authentification Adobe Pass :

```
/adobe-services/*
/reggie/*
/api/*
```

>[!IMPORTANT]
> 
> L’application de diffusion en continu peut ignorer l’envoi de la payload du jeton SSO Amazon (identité de plateforme) lors de l’appel `/authenticate`, comme elle l’a été lors de l’appel `/regcode`.

L’authentification Adobe Pass prend en charge les méthodes suivantes pour recevoir la payload du jeton SSO (identité de plateforme) qui est un identifiant à l’échelle de l’appareil ou de la plateforme :

* Comme en-tête nommé : `Adobe-Subject-Token`
* Comme paramètre de requête nommé : `ast`
* Comme paramètre de publication nommé : `ast`

>[!IMPORTANT]
>
> Si elle est envoyée en tant que paramètre de requête, l’URL entière peut devenir très longue et être rejetée.
>
> S’il est envoyé en tant que paramètre de requête/post, il doit être inclus lors de la génération de la signature de la requête.

#### Exemples

**Envoi en tant qu’en-tête**

```HTTPS
GET /api/v1/config/{requestorId} HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**Envoi en tant que paramètre de requête**

```HTTPS
GET /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com
```

**Envoi en tant que paramètre post**

```HTTPS
POST /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com 
Content-Type: multipart/form-data;
```

>[!IMPORTANT]
>
> Si la valeur de l&#39;en-tête `Adobe-Subject-Token` ou du paramètre `ast` est manquante ou non valide, l&#39;authentification Adobe Pass traite les requêtes sans prendre en compte l&#39;authentification SSO.
