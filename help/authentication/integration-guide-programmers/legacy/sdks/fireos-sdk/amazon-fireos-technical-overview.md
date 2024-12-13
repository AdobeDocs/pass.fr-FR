---
title: Présentation technique d’Amazon FireOS
description: Présentation technique d’Amazon FireOS
exl-id: 939683ee-0dd9-42ab-9fde-8686d2dc0cd0
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '2167'
ht-degree: 0%

---

# Présentation technique d’Amazon FireOS (hérité) {#amazon-fireos-technical-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Introduction {#intro}

Amazon FireOS AccessEnabler est représenté par deux composants : une bibliothèque de stub AccessEnabler utilisée par l’application et une bibliothèque Java Android au niveau du système qui permet aux applications mobiles d’utiliser l’authentification Adobe Pass pour les services de droits de TV Everywhere. Une implémentation d’Android pour Amazon FireOS se compose de l’interface AccessEnabler qui définit l’API de droit et d’un protocole EntitlementDelegate qui décrit les rappels déclenchés par la bibliothèque. La bibliothèque Android AccessEnabler au niveau du système permet d’accéder aux services Amazon pour activer l’authentification unique au niveau de la plateforme.

## Présentation des workflows client natifs {#native_client_workflows}

Les workflows clients natifs sont généralement identiques ou très similaires à ceux des clients d’authentification Adobe Pass basés sur un navigateur. Cependant, il existe quelques exceptions, comme décrit ci-dessous.


### Workflow de post-initialisation {#post-init}

Tous les workflows de droits pris en charge par AccessEnabler supposent que vous avez précédemment appelé [`setRequestor()`](#setRequestor) pour établir votre identité. Vous passez cet appel pour fournir votre ID de demandeur une seule fois, généralement au cours de la phase d’initialisation/configuration de votre application.

Avec les clients natifs (par exemple, AmazonFireOS), après votre appel initial à [`setRequestor()`](#setRequestor), vous avez le choix de la manière de procéder :

- Vous pouvez commencer à effectuer immédiatement des appels de droits et les autoriser à être placés en file d&#39;attente silencieuse, si nécessaire.
- Vous pouvez également recevoir une confirmation du succès ou de l’échec de l’[`setRequestor()`](#setRequestor) en implémentant le rappel setRequestorComplete().
- Ou les deux.

C’est à vous de décider s’il faut attendre la notification du succès de l’[`setRequestor()`](#setRequestor) ou compter sur le mécanisme de file d’attente d’appels d’AccessEnabler. Comme toutes les demandes d’authentification et d’autorisation suivantes nécessitent l’ID du demandeur et les informations de configuration associées, la méthode [`setRequestor()`](#setRequestor) bloque efficacement tous les appels d’API d’authentification et d’autorisation jusqu’à ce que l’initialisation soit terminée.

### Workflow d’authentification initiale générique {#generic}

L’objectif de ce workflow est de connecter un utilisateur avec son MVPD.  Une fois la connexion établie, le serveur principal émet un jeton d’authentification à l’utilisateur. Bien que l’authentification soit généralement effectuée dans le cadre du processus d’autorisation, ce qui suit décrit comment l’authentification peut fonctionner de manière isolée et n’inclut aucune étape d’autorisation.

Notez que bien que le workflow du client natif suivant diffère du workflow d’authentification standard basé sur le navigateur, les étapes 1 à 5 sont identiques pour les clients natifs et les clients basés sur le navigateur :

1. Votre page ou lecteur lance le workflow d’authentification avec un appel à [getAuthentication()](#getAuthN) qui vérifie la validité du jeton d’authentification mis en cache. Cette méthode comporte un paramètre de `redirectURL` facultatif. Si vous ne fournissez pas de valeur pour `redirectURL`, après une authentification réussie, l’utilisateur est renvoyé à l’URL à partir de laquelle l’authentification a été initialisée.
1. AccessEnabler détermine le statut d&#39;authentification actuel. Si l’utilisateur est actuellement authentifié, AccessEnabler appelle votre fonction de rappel `setAuthenticationStatus()`, transmettant un statut d’authentification indiquant la réussite (étape 7 ci-dessous).
1. Si l’utilisateur n’est pas authentifié, AccessEnabler poursuit le flux d’authentification en déterminant si la dernière tentative d’authentification de l’utilisateur a réussi avec un MVPD donné. Si un ID MVPD est mis en cache ET que l’indicateur `canAuthenticate` est true OU si un MVPD a été sélectionné à l’aide de [`setSelectedProvider()`](#setSelectedProvider), l’utilisateur n’est pas invité à ouvrir la boîte de dialogue de sélection de MVPD. Le flux d’authentification continue à utiliser la valeur mise en cache du MVPD (c’est-à-dire le même MVPD que celui utilisé lors de la dernière authentification réussie). Un appel réseau est effectué au serveur principal et l’utilisateur est redirigé vers la page de connexion de MVPD (étape 6 ci-dessous).
1. Si aucun ID MVPD n’est mis en cache ET qu’aucun MVPD n’a été sélectionné à l’aide de [`setSelectedProvider()`](#setSelectedProvider) OU que l’indicateur `canAuthenticate` est défini sur false, le rappel [`displayProviderDialog()`](#displayProviderDialog) est appelé. Ce rappel demande à votre page ou à votre lecteur de créer l’interface utilisateur qui présente à l’utilisateur une liste de fichiers MVPD parmi lesquels choisir. Un tableau d’objets MVPD contenant les informations nécessaires à la création du sélecteur MVPD est fourni. Chaque objet MVPD décrit une entité MVPD et contient des informations telles que l’identifiant du MVPD (par exemple, XFINITY, AT\&amp;T, etc.) et l’URL où se trouve le logo MVPD.
1. Une fois qu’un MVPD particulier est sélectionné, votre page ou lecteur doit informer AccessEnabler du choix de l’utilisateur. Pour les clients hors Flash, une fois que l’utilisateur a sélectionné le MVPD souhaité, vous informez l’AccessEnabler de la sélection de l’utilisateur par un appel à la méthode [`setSelectedProvider()`](#setSelectedProvider). Les clients de Flash distribuent plutôt un `MVPDEvent` partagé de type « `mvpdSelection` », en transmettant le fournisseur sélectionné.
1. Pour les applications Amazon, le rappel [`navigateToUrl()`](#navigagteToUrl) sera ignoré. La bibliothèque Access Enabler facilite l&#39;accès à un contrôle WebView commun pour authentifier les utilisateurs.
1. Par le biais du `WebView` , l’utilisateur accède à la page de connexion de MVPD et saisit ses informations d’identification. Notez que plusieurs opérations de redirection se produisent lors de ce transfert.
1. Une fois que WebView aura finalisé l&#39;authentification, il se fermera et informera l&#39;AccessEnabler que l&#39;utilisateur s&#39;est connecté avec succès, l&#39;AccessEnabler récupère le jeton d&#39;authentification réel du serveur principal. AccessEnabler appelle le rappel [`setAuthenticationStatus()`](#setAuthNStatus) avec un code d&#39;état de 1, indiquant la réussite. En cas d’erreur lors de l’exécution de ces étapes, le rappel [`setAuthenticationStatus()`](#setAuthNStatus) est déclenché avec un code d’état de 0, ainsi qu’un code d’erreur correspondant, indiquant que l’utilisateur n’est pas authentifié.

### Workflow de déconnexion {#logout}

Pour les clients natifs, les déconnexions sont gérées de la même manière que le processus d’authentification décrit ci-dessus. Selon ce modèle, AccessEnabler crée un contrôle `WebView` et charge le contrôle avec l’URL du point d’entrée de déconnexion sur le serveur principal. Une fois le processus de déconnexion terminé, les jetons sont effacés.

Notez que le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir avec le `WebView` de quelque manière que ce soit. Une fois la déconnexion terminée, AccessEnabler appelle le rappel `setAuthenticationStatus()` avec un code d&#39;état de 0, indiquant que l&#39;utilisateur n&#39;est pas authentifié.

## Jetons {#tokens}

### Définitions et utilisation {#definitions}

La solution Droits d’authentification Adobe Pass tourne autour de la génération de données spécifiques (jetons) que l’authentification Adobe Pass génère une fois les workflows d’authentification et d’autorisation terminés. Ces jetons sont stockés localement sur l’appareil Amazon FireOS du client.

Les jetons ont une durée de vie limitée. À leur expiration, les jetons doivent être émis à nouveau par le biais du redémarrage des workflows d’authentification et/ou d’autorisation.

Il existe trois types de jetons émis pendant les workflows de droits :

- **Jeton d’authentification** - Le résultat final du workflow d’authentification de l’utilisateur sera une GUID d’authentification que AccessEnabler peut utiliser pour effectuer des requêtes d’autorisation au nom de l’utilisateur. Ce GUID d’authentification est associé à une valeur de durée de vie (TTL) qui peut être différente de la session d’authentification de l’utilisateur elle-même. L’authentification Adobe Pass génère un jeton d’authentification en liant le GUID d’authentification à l’appareil qui lance les demandes d’authentification.
- **Jeton d’autorisation** - Accorde l’accès à une ressource protégée spécifique identifiée par un `resourceID` unique. Il consiste en une autorisation délivrée par l&#39;ordonnateur et le `resourceID` original. Ces informations sont liées à l’appareil à l’origine de la demande.
- **Jeton multimédia de courte durée** - AccessEnabler accorde l’accès à l’application d’hébergement pour une ressource donnée en renvoyant un jeton multimédia de courte durée. Ce jeton est généré en fonction du jeton d’autorisation précédemment acquis pour cette ressource spécifique. En outre, ce jeton n’est pas lié à l’appareil et la durée de vie associée est considérablement plus courte (par défaut : 5 minutes).

Une fois l’authentification et l’autorisation réussies, l’authentification Adobe Pass émet des jetons d’authentification, d’autorisation et de média de courte durée. Ces jetons doivent être mis en cache sur l’appareil de l’utilisateur et utilisés pendant la durée de vie qui leur est associée.

### Instructions de mise en cache {#caching}


#### Jeton d’authentification

- **AccessEnabler 1.10.1 pour FireOS** est basé sur AccessEnabler pour Android 1.9.1 - Ce SDK introduit une nouvelle méthode de stockage de jetons, activant plusieurs compartiments Programmer-MVPD et, par conséquent, plusieurs jetons d’authentification.

#### Jeton d’autorisation

À un moment donné, un seul jeton d’autorisation par ressource est mis en cache par AccessEnabler. Plusieurs jetons d’autorisation peuvent être mis en cache, mais ils sont associés à différentes ressources. Chaque fois qu’un nouveau jeton d’autorisation est émis et qu’un ancien jeton existe déjà pour la même ressource, le nouveau jeton remplace la valeur mise en cache existante.

#### Jeton multimédia

Le jeton de média de courte durée ne doit PAS être mis en cache du tout. Le jeton multimédia doit être récupéré à partir du serveur chaque fois qu’une API d’autorisation est appelée, car son utilisation est limitée à une utilisation unique.

### Persistance {#persistence}

Les jetons doivent être persistants sur plusieurs exécutions consécutives de la même application. Cela signifie qu’une fois les jetons d’authentification et d’autorisation acquis et que l’utilisateur ferme l’application, les mêmes jetons sont disponibles pour l’application lorsque l’utilisateur ouvre à nouveau l’application. En outre, il est souhaitable que ces jetons soient persistants dans plusieurs applications. En d’autres termes, une fois qu’un utilisateur ou une utilisatrice utilise une application pour se connecter avec un fournisseur d’identité spécifique (obtention réussie des jetons d’authentification et d’autorisation), les mêmes jetons peuvent être utilisés par le biais d’une autre application. En outre, l’utilisateur ou l’utilisatrice n’est plus invité à fournir des informations d’identification lors de la connexion via le même fournisseur d’identité.

Ce type de workflow d’authentification/d’autorisation transparent est ce qui fait de la solution d’authentification Adobe Pass une véritable mise en œuvre de TV-Everywhere. D’un point de vue purement technique, la bibliothèque Android AccessEnabler résout les problèmes de partage de données entre applications en stockant les données de jeton dans un fichier de base de données situé sur un stockage externe. Ces ressources partagées au niveau du système fournissent les ingrédients clés qui permettent la mise en œuvre du cas d’utilisation de jetons persistants souhaité :

- Prise en charge du stockage structuré : le stockage des jetons d’authentification Adobe Pass n’est pas simplement une structure de mémoire tampon linéaire. Il fournit un mécanisme de stockage de type dictionnaire qui permet l’indexation des données en fonction de valeurs de clé spécifiées par l’utilisateur.
- Prise en charge de la persistance des données à l’aide du système de fichiers sous-jacent : le contenu du fichier de base de données est conservé par défaut et les données sont enregistrées sur la mémoire externe de l’appareil.

Une fois qu’un jeton particulier est placé dans le cache de jetons, sa validité est vérifiée à différents moments par la bibliothèque AccessEnabler.  Un jeton valide est défini comme suit :

- La TTL du jeton n’a pas expiré
- L’émetteur du jeton est inclus dans la liste des fournisseurs d’identité autorisés

Le stockage des jetons peut prendre en charge plusieurs combinaisons Programmeur-MVPD, en s’appuyant sur une structure de mappage imbriqué à plusieurs niveaux qui peut contenir plusieurs jetons d’authentification. Ce nouveau stockage n’affecte en rien l’API publique AccessEnabler et ne nécessite aucune modification du côté du programmeur. Voici un exemple illustrant cette nouvelle fonctionnalité :

1. Open App1 (développé par Programmer1).
1. Authentifiez-vous avec MVPD1 (intégré à Programmer1).
1. Suspendre / Fermer l&#39;application courante, et ouvrir App2 (développé par Programmer2).
1. Supposons que Programmer2 ne soit pas intégré à MVPD2 ; par conséquent, l’utilisateur ne sera PAS authentifié dans App2.
1. Authentifiez-vous avec MVPD2 (qui est intégré à Programmer2) dans App2.
1. Revenez à App1 ; l’utilisateur sera toujours authentifié avec Programmer1.

### Format {#format}

#### Jeton d’authentification {#authn_token}

La liste ci-dessous présente le format du jeton d’authentification :

```JSON
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthenticationToken>
        <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
        <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>   
    </simpleAuthenticationToken>
```


#### Jeton d’autorisation {#authz_token}

La liste ci-dessous présente le format du jeton d’autorisation :

```JSON
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthorizationToken>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
        <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>
    </simpleAuthorizationToken>
```


#### Jeton multimédia court {#short_media_token}

La liste ci-dessous présente le format du jeton de média court.  Ce jeton est exposé à l’application du programmeur.  Il est transmis à l’application du programmeur à la fin d’un processus d’octroi de droits réussi :

```JSON
    <signatureInfo>signature<signatureInfo>
    <shortAuthorizationToken>
      <sessionGUID>session_guid</sessionGUID>
      <requestorID>requestor_id</requestorID>
      <resourceID>resource_id</resourceID>
      <ttl>ttl_in_ms</ttl>
      <issueTime>issue_time</issueTime>
      <mvpdId>mvpd_id</mvpdId>
      <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
    </shortAuthorizationToken>
```


#### Liaison de l’appareil {#device_binding}

Dans les listes XML ci-dessus, notez la balise intitulée `simpleTokenFingerprint`. Cette balise est destinée à contenir des informations d’individualisation de l’identifiant de l’appareil natif. La bibliothèque AccessEnabler peut obtenir ces informations d&#39;individualisation et les mettre à la disposition des services d&#39;authentification Adobe Pass lors des appels de droits. Le service utilisera ces informations et les incorporera dans les jetons réels, liant ainsi efficacement les jetons à un appareil spécifique. L’objectif final est de rendre les jetons non transférables sur tous les appareils.

Dans les listes XML ci-dessus, notez la balise intitulée simpleTokenFingerprint. Cette balise est destinée à contenir des informations d’individualisation de l’identifiant de l’appareil natif. La bibliothèque AccessEnabler peut obtenir ces informations d&#39;individualisation et les mettre à la disposition des services d&#39;authentification Adobe Pass lors des appels de droits. Le service utilisera ces informations et les incorporera dans les jetons réels, liant ainsi efficacement les jetons à un appareil spécifique. L’objectif final est de rendre les jetons non transférables sur tous les appareils.

Comme il s’agit évidemment d’une fonctionnalité liée à la sécurité, ces informations sont par nature « sensibles » du point de vue de la sécurité. Par conséquent, ces renseignements doivent être protégés contre l&#39;altération et l&#39;écoute électronique. Le problème d’écoute est résolu en envoyant les demandes d’authentification/autorisation via le protocole HTTPS. La protection contre l&#39;altération est gérée en signant numériquement les informations d&#39;identification du dispositif. La bibliothèque AccessEnabler calcule un identifiant d’appareil à partir des informations fournies par l’appareil, puis envoie l’identifiant d’appareil « en clair » aux serveurs d’authentification Adobe Pass en tant que paramètre de requête.  Les serveurs d’authentification d’Adobe Pass signent numériquement l’ID d’appareil avec la clé privée d’Adobe et l’ajoutent au jeton d’authentification renvoyé à AccessEnabler. Ainsi, l’ID de l’appareil est lié au jeton d’authentification.  Pendant le flux d’autorisation, AccessEnabler envoie à nouveau l’ID d’appareil en clair, ainsi que le jeton d’authentification.  L’échec du processus de validation entraîne automatiquement l’échec des workflows d’authentification/autorisation.  Les serveurs d’authentification d’Adobe Pass appliquent la clé privée à l’ID d’appareil et la comparent à la valeur du jeton d’authentification.  S’ils ne correspondent pas, ce flux de droits échoue.
