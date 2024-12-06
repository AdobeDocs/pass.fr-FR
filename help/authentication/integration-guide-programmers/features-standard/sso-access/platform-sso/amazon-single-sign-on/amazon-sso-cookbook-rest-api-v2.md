---
title: Guide pas à pas Amazon SSO (API REST V2)
description: Guide pas à pas Amazon SSO (API REST V2)
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---

# Guide pas à pas Amazon SSO (API REST V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

L’API REST d’authentification Adobe Pass V2 prend en charge l’authentification unique (SSO) de Platform pour les utilisateurs finaux des applications clientes s’exécutant sur FireOS.

Ce document agit comme une extension de l’ [’API REST V2 Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) qui fournit une vue de haut niveau et le document qui décrit la mise en oeuvre de l’ [authentification unique à l’aide des flux d’identités de la plateforme](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Authentification unique Amazon à l’aide des flux d’identités de plateforme {#cookbook}

### Conditions préalables {#prerequisites}

Avant de poursuivre l’authentification unique Amazon à l’aide des flux d’identités de plateforme, assurez-vous que les conditions préalables suivantes sont remplies.

#### Intégration du SDK SSO Amazon {#integrate-amazon-sso-sdk}

L’application de diffusion en continu doit intégrer la bibliothèque [SDK Amazon SSO](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) pour l’authentification unique (SSO) dans sa version.

* Téléchargez et copiez la dernière bibliothèque SDK Amazon SSO dans un dossier `/SSOEnabler` parallèlement au répertoire de l’application.

* Mettez à jour les fichiers manifest et Gradle pour utiliser la bibliothèque SDK Amazon SSO.

  **Manifeste :**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  Sous Référentiels :

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  Sous dependencies :

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Utilisation du SDK SSO Amazon {#use-amazon-sso-sdk}

L’application de diffusion en continu doit utiliser le SDK Amazon SSO pour obtenir la charge utile du jeton d’authentification unique (identité de plateforme).

Le SDK SSO Amazon fournit des API synchrones et asynchrones pour obtenir la charge utile du jeton SSO (identité de plateforme).

L’application en continu peut choisir l’une des deux options en fonction de son architecture.

##### API asynchrones

* Obtenez l’instance `SSOEnabler` et définissez le `SSOEnablerCallback` :

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

  Le lot de réponse de réussite du jeton d’authentification unique contient :
   * Jeton SSO sous la forme `string` avec la clé &quot;SSOToken&quot;.

  <br/>

  Le lot de réponse d’échec du jeton SSO contient :
   * Un code d’erreur sous la forme `int` avec la clé &quot;ErrorCode&quot;.
   * Une description d’erreur sous la forme `string` avec la clé &quot;ErrorDescription&quot;.

  <br/>

* Obtenez le jeton SSO :

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  Cette API fournira la réponse via le jeu de rappel lors de l’initialisation.

##### API synchrones

* Obtenez l’instance `SSOEnabler` :

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* Obtenez le jeton SSO :

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  Cette API bloquera le thread appelant et répondra avec le lot de résultats. Comme il s’agit d’un appel synchrone, veillez à ne pas l’utiliser dans votre thread principal.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  Cette API définit la valeur de délai d’expiration de l’appel synchrone. La valeur par défaut du délai d’expiration est de 1 minute.

#### Secours pour Amazon SSO {#fallback-amazon-sso}

L’application de diffusion en continu doit gérer les scénarios de secours du flux SSO Amazon au flux d’authentification standard.

Assurez-vous que l’application de diffusion en continu gère les éléments suivants :

* L’absence de l’application compagnon Amazon qui doit être en cours d’exécution sur le périphérique Amazon.
   * L’application de diffusion en continu peut rencontrer un `ClassNotFoundException` au moment de l’exécution sur la classe suivante `com.amazon.ottssotokenlib.SSOEnabler`.

* L’absence de la charge utile du jeton SSO (identité de plateforme) qui doit être renvoyée par les API ci-dessus.
   * L’application de diffusion en continu peut contacter les représentants d’Amazon et de l’Adobe pour enquêter.

### Workflow {#workflow}

La charge utile du jeton SSO Amazon (identité de plateforme) doit être présente sur toutes les requêtes HTTP effectuées sur les points d’entrée de l’API REST V2 d’authentification Adobe Pass :

```
/api/v2/*
```

L’API REST d’authentification Adobe Pass V2 prend en charge les méthodes suivantes pour recevoir la charge utile du jeton d’authentification unique (identité de plateforme), qui est un identifiant au périmètre de l’appareil ou de la plateforme :

* En-tête nommé : `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> Pour plus d&#39;informations sur l&#39;en-tête `Adobe-Subject-Token`, consultez la documentation [Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

#### Exemples

**Envoi en tant qu&#39;en-tête**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> Si la valeur d’en-tête `Adobe-Subject-Token` est manquante ou non valide, l’authentification Adobe Pass traitera les demandes sans prendre en compte l’authentification unique.
