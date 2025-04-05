---
title: Manuel de l’authentification unique Amazon (API REST V2)
description: Manuel de l’authentification unique Amazon (API REST V2)
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Manuel de l’authentification unique Amazon (API REST V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

L’API REST d’authentification Adobe Pass V2 prend en charge l’authentification unique (SSO) de Platform pour les utilisateurs finaux des applications clientes s’exécutant sur FireOS.

Ce document agit comme une extension de l’aperçu [API REST V2)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) existant qui fournit une vue d’ensemble et le document qui décrit comment implémenter [authentification unique à l’aide des flux d’identité de plateforme](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## authentification unique Amazon à l’aide des flux d’identité de la plateforme {#cookbook}

L’authentification Adobe Pass collabore avec Amazon pour améliorer l’expérience de connexion des utilisateurs et faciliter l’authentification unique (SSO) dans les applications TV Everywhere pour les abonnés aux chaînes de télévision.

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
   * L’application de streaming peut contacter les représentants d’Amazon et d’Adobe pour en savoir plus.

### Workflow {#workflow}

La payload du jeton SSO Amazon (identité de la plateforme) doit être présente sur toutes les requêtes HTTP effectuées par rapport aux points d’entrée de l’API REST d’authentification Adobe Pass V2 :

```
/api/v2/*
```

L’API REST d’authentification Adobe Pass V2 prend en charge les méthodes suivantes pour recevoir la payload du jeton SSO (identité de plateforme) qui est un identifiant de la portée de l’appareil ou de la plateforme :

* Comme en-tête nommé : `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> Pour plus d’informations sur `Adobe-Subject-Token`’en-tête , consultez la documentation de [Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

#### Exemples

**Envoi en tant qu’en-tête**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> Si la valeur de l’en-tête `Adobe-Subject-Token` est manquante ou non valide, l’authentification Adobe Pass traite les requêtes sans prendre en compte l’authentification SSO.
