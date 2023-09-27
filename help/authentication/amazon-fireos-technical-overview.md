---
title: Présentation technique d’Amazon FireOS
description: Présentation technique d’Amazon FireOS
exl-id: 939683ee-0dd9-42ab-9fde-8686d2dc0cd0
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2154'
ht-degree: 0%

---

# Présentation technique d’Amazon FireOS {#amazon-fireos-technical-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Introduction {#intro}

Amazon FireOS AccessEnabler est représenté par deux composants : une bibliothèque stub AccessEnabler utilisée par l’application et une bibliothèque Java Android au niveau du système qui permet aux applications mobiles d’utiliser l’authentification Adobe Pass pour les services de droits de TV Everywhere. Une mise en oeuvre Android pour Amazon FireOS se compose de l’interface AccessEnabler qui définit l’API de droits, ainsi que d’un protocole EntitlementDelegate qui décrit les rappels déclenchés par la bibliothèque. La bibliothèque Android AccessEnabler au niveau du système permet d’accéder aux services Amazon pour activer l’authentification unique au niveau de la plateforme.

## Présentation des processus client natifs {#native_client_workflows}

Les workflows clients natifs sont généralement identiques à ceux des clients Adobe Pass Authentication basés sur un navigateur, ou très similaires. Il existe toutefois quelques exceptions, comme décrit ci-dessous.


### Processus de post-initialisation {#post-init}

Tous les workflows de droits pris en charge par AccessEnabler supposent que vous avez précédemment appelé [`setRequestor()`](#setRequestor) pour établir votre identité. Vous effectuez cet appel pour ne fournir votre ID de demandeur qu’une seule fois, généralement pendant la phase d’initialisation/de configuration de votre application.

Avec les clients natifs (par exemple, AmazonFireOS), après votre premier appel à [`setRequestor()`](#setRequestor), vous avez la possibilité de procéder comme suit :

- Vous pouvez commencer à lancer des appels de droits immédiatement et leur permettre d’être mis en file d’attente silencieuse, si nécessaire.
- Vous pouvez également recevoir une confirmation de la réussite ou de l’échec de [`setRequestor()`](#setRequestor) en mettant en oeuvre le rappel setRequestorComplete() .
- Ou les deux.

C’est à vous de décider d’attendre ou non la notification du succès de [`setRequestor()`](#setRequestor) ou de dépendre du mécanisme de file d’attente d’appels d’AccessEnabler. Comme toutes les demandes d’autorisation et d’authentification suivantes ont besoin de l’identifiant du demandeur et des informations de configuration associées, la variable [`setRequestor()`](#setRequestor) bloque efficacement tous les appels de l’API d’authentification et d’autorisation jusqu’à ce que l’initialisation soit terminée.

### Processus d’authentification initiale générique {#generic}

Le but de ce workflow est de se connecter à un utilisateur avec son MVPD.  Une fois la connexion établie, le serveur principal émet un jeton d’authentification à l’utilisateur. Bien que l’authentification soit généralement effectuée dans le cadre du processus d’autorisation, la section suivante décrit le fonctionnement de l’authentification en isolation, sans inclure d’étapes d’autorisation.

Notez que bien que le processus client natif suivant diffère du processus d’authentification type basé sur un navigateur, les étapes 1 à 5 sont les mêmes pour les clients natifs et les clients basés sur un navigateur :

1. Votre page ou lecteur lance le workflow d’authentification avec un appel à [getAuthentication()](#getAuthN), qui recherche un jeton d’authentification mis en cache valide. Cette méthode est facultative. `redirectURL` si vous ne fournissez pas de valeur pour `redirectURL`, après une authentification réussie, l’utilisateur est renvoyé à l’URL à partir de laquelle l’authentification a été initialisée.
1. AccessEnabler détermine l’état d’authentification actuel. Si l’utilisateur est actuellement authentifié, AccessEnabler appelle votre `setAuthenticationStatus()` fonction de rappel, transmission d’un état d’authentification indiquant la réussite (étape 7 ci-dessous).
1. Si l’utilisateur n’est pas authentifié, AccessEnabler poursuit le flux d’authentification en déterminant si la dernière tentative d’authentification de l’utilisateur a réussi avec un MVPD donné. Si un ID MVPD est mis en cache ET que la variable `canAuthenticate` L’indicateur est défini sur true OU un MVPD a été sélectionné à l’aide de [`setSelectedProvider()`](#setSelectedProvider), l’utilisateur n’est pas invité à saisir la boîte de dialogue de sélection MVPD. Le flux d’authentification continue à utiliser la valeur mise en cache du MVPD (c’est-à-dire le même MVPD qui a été utilisé lors de la dernière authentification réussie). Un appel réseau est effectué au serveur principal et l’utilisateur est redirigé vers la page de connexion MVPD (étape 6 ci-dessous).
1. Si aucun ID MVPD n’est mis en cache ET qu’aucun MVPD n’a été sélectionné à l’aide de [`setSelectedProvider()`](#setSelectedProvider) OU le `canAuthenticate` est défini sur false, l’indicateur [`displayProviderDialog()`](#displayProviderDialog) callback est appelé. Ce rappel demande à votre page ou lecteur de créer l’interface utilisateur qui présente à l’utilisateur une liste de distributeurs multicanaux de programmes audiovisuels parmi lesquels choisir. Un tableau d’objets MVPD est fourni, contenant les informations nécessaires à la création du sélecteur MVPD. Chaque objet MVPD décrit une entité MVPD et contient des informations telles que l’identifiant du MVPD (ex. XFINITY, AT\&amp;T, etc.). et l’URL où se trouve le logo MVPD.
1. Une fois qu’un MVPD particulier est sélectionné, votre page ou lecteur doit informer AccessEnabler du choix de l’utilisateur. Pour les clients non Flashs, une fois que l’utilisateur a sélectionné le MVPD souhaité, vous informez AccessEnabler de la sélection de l’utilisateur via un appel au [`setSelectedProvider()`](#setSelectedProvider) . Les clients Flash distribuent plutôt un partage `MVPDEvent` de type &quot;`mvpdSelection`&quot;, transmission du fournisseur sélectionné.
1. Pour les applications Amazon, la variable [`navigateToUrl()`](#navigagteToUrl) callback sera ignoré. La bibliothèque Access Enabler facilite l’accès à un contrôle WebView commun pour authentifier les utilisateurs.
1. Via le `WebView`, l’utilisateur arrive sur la page de connexion du MVPD et saisit ses informations d’identification. Notez que plusieurs opérations de redirection se produisent au cours de ce transfert.
1. Une fois que WebView finalise l’authentification, il ferme et informe AccessEnabler que l’utilisateur s’est connecté correctement, AccessEnabler récupère le jeton d’authentification réel à partir du serveur principal. AccessEnabler appelle la fonction [`setAuthenticationStatus()`](#setAuthNStatus) rappel avec un code d’état de 1, indiquant la réussite. En cas d’erreur lors de l’exécution de ces étapes, la variable [`setAuthenticationStatus()`](#setAuthNStatus) Le rappel est déclenché avec un code d’état de 0, ainsi qu’un code d’erreur correspondant, indiquant que l’utilisateur n’est pas authentifié.

### Processus de déconnexion {#logout}

Pour les clients natifs, les déconnexions sont gérées de la même manière que le processus d’authentification décrit ci-dessus. Selon ce modèle, AccessEnabler crée une `WebView` et chargera le contrôle avec l’URL du point de terminaison de la déconnexion sur le serveur principal. Une fois le processus de déconnexion terminé, les jetons sont effacés.

Notez que le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir avec la variable `WebView` de quelque manière que ce soit. Une fois la déconnexion terminée, AccessEnabler appelle la fonction `setAuthenticationStatus()` rappel avec un code d’état de 0, indiquant que l’utilisateur n’est pas authentifié.

## Jetons {#tokens}

### Définitions et utilisation {#definitions}

La solution de droit d’authentification Adobe Pass tourne autour de la génération de données (jetons) spécifiques générées par l’authentification Adobe Pass une fois les workflows d’authentification et d’autorisation terminés avec succès. Ces jetons sont stockés localement sur l’appareil Amazon FireOS du client.

La durée de vie des jetons est limitée. Lors de l’expiration, les jetons doivent être réémis en réinitialisant les workflows d’authentification et/ou d’autorisation.

Il existe trois types de jetons émis lors des workflows de droits :

- **Jeton d’authentification** - Le résultat final du workflow d’authentification de l’utilisateur est un GUID d’authentification que AccessEnabler peut utiliser pour effectuer des requêtes d’autorisation pour le compte de l’utilisateur. Ce GUID d’authentification aura une valeur TTL (durée de vie) associée qui peut être différente de la session d’authentification de l’utilisateur. L’authentification Adobe Pass génère un jeton d’authentification en liant le GUID d’authentification au périphérique qui lance les demandes d’authentification.
- **Jeton d’autorisation** - Accorde l’accès à une ressource protégée spécifique identifiée par une `resourceID`. Il s’agit d’une autorisation émise par la personne qui l’a autorisée, ainsi que de l’original `resourceID`. Ces informations sont liées au périphérique qui lance la requête.
- **Jeton multimédia de courte durée** - AccessEnabler accorde l&#39;accès à l&#39;application d&#39;hébergement pour une ressource donnée en renvoyant un jeton multimédia de courte durée. Ce jeton est généré en fonction du jeton d’autorisation précédemment acquis pour cette ressource spécifique. En outre, ce jeton n’est pas lié à l’appareil et la durée de vie associée est beaucoup plus courte (par défaut : 5 minutes).

Une fois l’authentification et l’autorisation réussies, l’authentification Adobe Pass émet des jetons d’authentification, d’autorisation et de média de courte durée. Ces jetons doivent être mis en cache sur l’appareil de l’utilisateur et utilisés pendant la durée de vie associée.

### Instructions de mise en cache {#caching}


#### Jeton d’authentification

- **AccessEnabler 1.10.1 pour FireOS** est basé sur AccessEnabler pour Android 1.9.1 - Ce SDK introduit une nouvelle méthode de stockage de jetons, en activant plusieurs compartiments Programmer-MVPD et donc plusieurs jetons d’authentification.

#### Jeton d’autorisation

A tout moment, seul un jeton d’autorisation par ressource est mis en cache par AccessEnabler. Plusieurs jetons d’autorisation peuvent être mis en cache, mais ils sont associés à différentes ressources. Chaque fois qu’un nouveau jeton d’autorisation est émis et qu’un ancien existe déjà pour la même ressource, le nouveau jeton remplace la valeur mise en cache existante.

#### Jeton de média

Le jeton multimédia de courte durée ne doit PAS être mis en cache du tout. Le jeton multimédia doit être récupéré à partir du serveur chaque fois qu’une API d’autorisation est appelée, car il est limité à une utilisation unique.

### Persistance {#persistence}

Les jetons doivent être persistants sur plusieurs exécutions consécutives de la même application. Cela signifie qu’une fois que les jetons d’authentification et d’autorisation ont été acquis et que l’utilisateur ferme l’application, les mêmes jetons sont disponibles pour l’application lorsque l’utilisateur rouvre l’application. En outre, il est souhaitable que ces jetons soient persistants dans plusieurs applications. En d’autres termes, lorsqu’un utilisateur utilise une application pour se connecter à un fournisseur d’identité spécifique (obtention réussie des jetons d’authentification et d’autorisation), les mêmes jetons peuvent être utilisés par le biais d’une autre application et l’utilisateur n’est plus invité à saisir les informations d’identification lors de sa connexion via le même fournisseur d’identité.

Ce type de workflow d’authentification/d’autorisation transparent fait de la solution d’authentification Adobe Pass une véritable implémentation de TV-partout. D’un point de vue purement ingénieur, la bibliothèque Android AccessEnabler traite des problèmes de partage de données entre applications en stockant les données de jeton dans un fichier de base de données situé sur un stockage externe. Cette ressource partagée au niveau du système fournit les ingrédients clés qui permettent la mise en oeuvre du cas d’utilisation de jetons persistants souhaité :

- Prise en charge du stockage structuré : le stockage du jeton d’authentification Adobe Pass n’est pas simplement une structure de mémoire linéaire comme un tampon. Il fournit un mécanisme de stockage de type dictionnaire qui permet l’indexation des données en fonction des valeurs de clé spécifiées par l’utilisateur.
- Prise en charge de la persistance des données à l’aide du système de fichiers sous-jacent : le contenu du fichier de base de données est conservé par défaut et les données sont enregistrées dans la mémoire externe de l’appareil.

Une fois qu’un jeton particulier est placé dans le cache de jetons, sa validité est vérifiée à différents moments par la bibliothèque AccessEnabler.  Un jeton valide est défini comme suit :

- Le délai d’activation du jeton n’a pas expiré
- L’émetteur du jeton est inclus dans la liste des fournisseurs d’identité autorisés

Le stockage des jetons peut prendre en charge plusieurs combinaisons Programmer-MVPD, en s’appuyant sur une structure de mappage imbriquée à plusieurs niveaux qui peut contenir plusieurs jetons d’authentification. Ce nouveau stockage n’affecte en aucune manière l’API publique AccessEnabler et ne nécessite aucune modification du côté du programmeur. Voici un exemple qui illustre cette nouvelle fonctionnalité :

1. Ouvrez App1 (développé par Programmer1).
1. Authentifiez-vous avec MVPD1 (qui est intégré à Programmer1).
1. Suspendre/Fermer l’application actuelle et ouvrir App2 (développé par Programmer2).
1. Supposons que Programmer2 ne soit pas intégré à MVPD2 ; par conséquent, l’utilisateur ne sera PAS authentifié dans App2.
1. Authentifiez-vous avec MVPD2 (intégré à Programmer2) dans App2.
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


#### Jeton de média court {#short_media_token}

La liste ci-dessous présente le format du jeton multimédia court.  Ce jeton est exposé à l’application du programmeur.  Il est transmis à l’application du programmeur à la fin d’un processus de droit réussi :

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


#### Liaison de périphérique {#device_binding}

Dans les listes XML ci-dessus, notez la balise intitulée `simpleTokenFingerprint`. L’objectif de cette balise est de contenir des informations d’individualisation des identifiants d’appareil natifs. La bibliothèque AccessEnabler peut obtenir ces informations d’individualisation et les mettre à la disposition des services d’authentification Adobe Pass lors des appels de droits. Le service utilisera ces informations et les incorporera dans les jetons réels, liant ainsi efficacement les jetons à un appareil spécifique. L’objectif final est de rendre les jetons non transférables sur plusieurs appareils.

Dans les listes XML ci-dessus, notez la balise intitulée simpleTokenFingerprint. L’objectif de cette balise est de contenir des informations d’individualisation des identifiants d’appareil natifs. La bibliothèque AccessEnabler peut obtenir ces informations d’individualisation et les mettre à la disposition des services d’authentification Adobe Pass lors des appels de droits. Le service utilisera ces informations et les incorporera dans les jetons réels, liant ainsi efficacement les jetons à un appareil spécifique. L’objectif final est de rendre les jetons non transférables sur plusieurs appareils.

Puisqu’il s’agit évidemment d’une fonctionnalité liée à la sécurité, ces informations sont intrinsèquement &quot;sensibles&quot; du point de vue de la sécurité. Par conséquent, ces informations doivent être protégées contre l’altération et l’écoutes. Le problème d’écoute est résolu en envoyant les demandes d’authentification/d’autorisation via le protocole HTTPS. La protection de l’altération est gérée par la signature numérique des informations d’identification de l’appareil. La bibliothèque AccessEnabler calcule un ID d’appareil à partir des informations fournies par l’appareil, puis envoie l’ID d’appareil &quot;en clair&quot; aux serveurs d’authentification Adobe Pass en tant que paramètre de requête.  Les serveurs d’authentification Adobe Pass signent numériquement l’identifiant de l’appareil avec la clé privée de l’Adobe et l’ajoutent au jeton d’authentification renvoyé à AccessEnabler. Par conséquent, l’identifiant de l’appareil est lié au jeton d’authentification.  Pendant le flux d’autorisation, AccessEnabler envoie à nouveau l’identifiant de l’appareil en clair, avec le jeton d’authentification.  L’échec du processus de validation entraînera automatiquement l’échec des workflows d’authentification/d’autorisation.  Les serveurs d’authentification Adobe Pass appliquent la clé privée à l’identifiant de l’appareil et la comparent à la valeur du jeton d’authentification.  S’ils ne correspondent pas, ce flux de droits échoue.
