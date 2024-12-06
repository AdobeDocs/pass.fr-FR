---
title: Présentation du SDK Android
description: Présentation du SDK Android
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2731'
ht-degree: 0%

---

# Présentation du SDK Android {#android-sdk-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

Android AccessEnabler est une bibliothèque Java Android qui permet aux applications mobiles d’utiliser l’authentification Adobe Pass pour les services de droits de TV Everywhere. Une implémentation Android se compose de l’interface AccessEnabler qui définit l’API de droits, ainsi que d’un protocole EntitlementDelegate qui décrit les rappels déclenchés par la bibliothèque. L’interface avec le protocole est référencée sous un nom commun : la bibliothèque Android AccessEnabler.

## Configuration requise pour Android {#reqs}

Pour connaître les exigences techniques actuelles relatives à la plateforme Android et à l’authentification Adobe Pass, voir [Configuration requise pour la plateforme/l’appareil/l’outil](#android) ou consulter les notes de mise à jour incluses avec le téléchargement du SDK Android.

## Présentation des processus client natifs {#native_client_workflows}

Les workflows clients natifs sont généralement identiques à ceux des clients Adobe Pass Authentication basés sur un navigateur, ou très similaires. Il existe toutefois quelques exceptions, comme décrit ci-dessous.

- [Processus de post-initialisation](#post-init)
- [Processus d’authentification initiale générique](#generic)
- [Processus de déconnexion](#logout)

### Processus de post-initialisation {#post-init}

Tous les workflows de droits pris en charge par AccessEnabler supposent que vous avez déjà appelé [`setRequestor()`](#setRequestor) pour établir votre identité. Vous effectuez cet appel pour ne fournir votre ID de demandeur qu’une seule fois, généralement pendant la phase d’initialisation/de configuration de votre application.


Avec les clients natifs (Android, par exemple), après votre premier appel à [`setRequestor()`](#setRequestor), vous avez le choix entre procéder :

- Vous pouvez commencer à lancer des appels de droits immédiatement et leur permettre d’être mis en file d’attente silencieuse, si nécessaire.

- Vous pouvez également recevoir une confirmation de la réussite/l’échec de [`setRequestor()`](#setRequestor) en mettant en oeuvre le rappel setRequestorComplete() .

- Ou les deux.

A vous de décider d&#39;attendre la notification du succès de [`setRequestor()`](#setRequestor) ou de vous fier au mécanisme de file d&#39;attente d&#39;appels d&#39;AccessEnabler. Comme toutes les demandes d’autorisation et d’authentification suivantes ont besoin de l’identifiant du demandeur et des informations de configuration associées, la méthode [`setRequestor()`](#setRequestor) bloque efficacement tous les appels d’API d’authentification et d’autorisation jusqu’à ce que l’initialisation soit terminée.

### Processus d’authentification initiale générique {#generic}

Le but de ce workflow est de se connecter à un utilisateur avec son MVPD.  Une fois la connexion établie, le serveur principal émet un jeton d’authentification à l’utilisateur. Bien que l’authentification soit généralement effectuée dans le cadre du processus d’autorisation, la section suivante décrit le fonctionnement de l’authentification en isolation, sans inclure d’étapes d’autorisation.

Notez que bien que le processus client natif suivant diffère du processus d’authentification type basé sur un navigateur, les étapes 1 à 5 sont les mêmes pour les clients natifs et les clients basés sur un navigateur :

1. Votre page ou lecteur lance le workflow d’authentification avec un appel à [getAuthentication()](#getAuthN), qui recherche un jeton d’authentification en mémoire cache valide. Cette méthode comporte un paramètre `redirectURL` facultatif. Si vous ne fournissez pas de valeur pour `redirectURL`, après une authentification réussie, l’utilisateur est renvoyé à l’URL à partir de laquelle l’authentification a été initialisée.
1. AccessEnabler détermine l’état d’authentification actuel. Si l’utilisateur est actuellement authentifié, AccessEnabler appelle votre fonction de rappel `setAuthenticationStatus()`, en transmettant un état d’authentification indiquant la réussite (étape 7 ci-dessous).
1. Si l’utilisateur n’est pas authentifié, AccessEnabler poursuit le flux d’authentification en déterminant si la dernière tentative d’authentification de l’utilisateur a réussi avec un MVPD donné. Si un ID MVPD est mis en cache ET que l’indicateur `canAuthenticate` est défini sur true OU qu’un MVPD a été sélectionné à l’aide de [`setSelectedProvider()`](#setSelectedProvider), l’utilisateur n’est pas invité avec la boîte de dialogue de sélection MVPD. Le flux d’authentification continue à utiliser la valeur mise en cache du MVPD (c’est-à-dire le même MVPD qui a été utilisé lors de la dernière authentification réussie). Un appel réseau est effectué au serveur principal et l’utilisateur est redirigé vers la page de connexion MVPD (étape 6 ci-dessous).
1. Si aucun ID MVPD n’est mis en cache ET qu’aucun MVPD n’a été sélectionné à l’aide de [`setSelectedProvider()`](#setSelectedProvider) OU que l’indicateur `canAuthenticate` est défini sur false, le rappel [`displayProviderDialog()`](#displayProviderDialog) est appelé. Ce rappel demande à votre page ou lecteur de créer l’interface utilisateur qui présente à l’utilisateur une liste de distributeurs multicanaux de programmes audiovisuels parmi lesquels choisir. Un tableau d’objets MVPD est fourni, contenant les informations nécessaires à la création du sélecteur MVPD. Chaque objet MVPD décrit une entité MVPD et contient des informations telles que l’identifiant du MVPD (ex. XFINITY, AT\&amp;T, etc.) et l’URL où se trouve le logo MVPD.
1. Une fois qu’un MVPD particulier est sélectionné, votre page ou lecteur doit informer AccessEnabler du choix de l’utilisateur. Pour les clients non Flashs, une fois que l’utilisateur a sélectionné le MVPD souhaité, vous informez AccessEnabler de la sélection de l’utilisateur via un appel à la méthode [`setSelectedProvider()`](#setSelectedProvider). Les clients Flash distribuent plutôt un `MVPDEvent` partagé de type &quot;`mvpdSelection`&quot;, en transmettant le fournisseur sélectionné.
1. Pour les applications Android, si com.android.chrome est disponible, l’URL d’authentification est chargée dans les onglets personnalisés de Chrome.
1. Par le biais des onglets personnalisés Chrome, l’utilisateur arrive sur la page de connexion du MVPD et saisit ses informations d’identification. Notez que plusieurs opérations de redirection se produisent au cours de ce transfert.
1. Lorsque les onglets personnalisés Chrome détectent qu’une URL correspond au schéma (adobepass://) et au lien profond à partir de la ressource &quot;redirect\_uri&quot; (c’est-à-dire adobepass://com.adobepass ), AccessEnabler récupère le jeton d’authentification réel à partir des serveurs principaux. Notez que les URL de redirection finales ne sont pas valides et qu’elles ne sont pas destinées à être chargées par les onglets personnalisés Chrome. Ils ne doivent être interprétés que par le SDK comme un signal indiquant que le flux d’authentification est terminé.
1. AccessEnabler informe votre application que le flux d&#39;authentification est terminé. AccessEnabler appelle le rappel [`setAuthenticationStatus()`](#setAuthNStatus) avec un code d’état de 1, indiquant la réussite. En cas d’erreur lors de l’exécution de ces étapes, le rappel [`setAuthenticationStatus()`](#setAuthNStatus) est déclenché avec un code d’état de 0, ainsi qu’un code d’erreur correspondant, indiquant l’échec de l’authentification.

### Processus de déconnexion {#logout}

Pour les clients natifs, les déconnexions sont gérées de la même manière que le processus d’authentification décrit ci-dessus. Suivant ce modèle, AccessEnabler ouvre Chrome Custom Tabs et charge l’URL du point de terminaison de la déconnexion sur le serveur principal.



**Remarque :** La déconnexion d’une session Programmer/MVPD efface
le stockage sous-jacent pour ce MVPD spécifique, y compris l’ensemble du
autres jetons d’authentification du programmeur obtenus par SSO sur
cet appareil. Les jetons obtenus pour d’autres MVPD ou non par SSO ne seront pas
être supprimées.


## Jetons {#tokens}

- [Définitions et utilisation](#definitions)
- [Instructions de mise en cache](#caching)
- [Persistance](#persistence)
- [Format](#format)
- [Liaison de périphérique](#device_binding)



### Définitions et utilisation {#definitions}

La solution de droit d’authentification Adobe Pass tourne autour de la génération de données (jetons) spécifiques générées par l’authentification Adobe Pass une fois les workflows d’authentification et d’autorisation terminés avec succès. Ces jetons sont stockés localement sur le périphérique Android du client.

La durée de vie des jetons est limitée. Lors de l’expiration, les jetons doivent être réémis en réinitialisant les workflows d’authentification et/ou d’autorisation.

Il existe trois types de jetons émis lors des workflows de droits :

- **Jeton d’authentification** - Le résultat final du workflow d’authentification de l’utilisateur sera un GUID d’authentification que AccessEnabler peut utiliser pour effectuer des requêtes d’autorisation pour le compte de l’utilisateur. Ce GUID d’authentification aura une valeur TTL (durée de vie) associée qui peut être différente de la session d’authentification de l’utilisateur. L’authentification Adobe Pass génère un jeton d’authentification en liant le GUID d’authentification au périphérique qui lance les demandes d’authentification.
- **Jeton d’autorisation** - Accorde l’accès à une ressource protégée spécifique identifiée par un `resourceID` unique. Il se compose d’une autorisation émise par la personne qui l’a autorisée, ainsi que du `resourceID` original. Ces informations sont liées au périphérique qui lance la requête.
- **Jeton multimédia de courte durée** : AccessEnabler accorde l’accès à l’application d’hébergement pour une ressource donnée en renvoyant un jeton multimédia de courte durée. Ce jeton est généré en fonction du jeton d’autorisation précédemment acquis pour cette ressource spécifique. En outre, ce jeton n’est pas lié à l’appareil et la durée de vie associée est beaucoup plus courte (par défaut : 5 minutes).

Une fois l’authentification et l’autorisation réussies, l’authentification Adobe Pass émet des jetons d’authentification, d’autorisation et de média de courte durée. Ces jetons doivent être mis en cache sur l’appareil de l’utilisateur et utilisés pendant la durée de vie associée.



### Instructions de mise en cache {#caching}

- Jeton d’authentification
- Jeton d’autorisation
- Jeton de média


#### Jeton d’authentification

- **AccessEnabler 1.6 et versions antérieures** - La manière dont les jetons d’authentification sont mis en cache sur l’appareil dépend de l’indicateur &quot;**Authentication per Requestor&quot;** associé au MVPD actuel :


1. Si la fonction &quot;Authentification par demandeur&quot; est *désactivée*, un jeton d’authentification unique est stocké localement dans le tableau de bord global. Ce jeton sera partagé entre toutes les applications intégrées au MVPD actuel.
1. Si la fonction &quot;Authentification par demandeur&quot; est *activée*, un jeton sera explicitement associé au programmeur qui a exécuté le flux d’authentification (le jeton ne sera pas stocké dans le tableau de bord global, mais dans un fichier privé visible uniquement par l’application de ce programmeur). Plus précisément, l’authentification unique (SSO) entre différentes applications sera désactivée ; l’utilisateur devra exécuter explicitement le flux d’authentification lors du passage à une nouvelle application (à condition que le programmeur de la deuxième application soit intégré au MVPD actuel et qu’il n’existe aucun jeton d’authentification pour ce programmeur dans le cache local).

   **Remarque :** AEM 1.6 Note technique GSON Google : [Comment résoudre les dépendances Gson](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7** - Ce SDK introduit une nouvelle méthode de stockage de jetons, qui permet plusieurs compartiments Programmer-MVPD et, par conséquent, plusieurs jetons d’authentification. A partir d’AEM 1.7, la même disposition de stockage est utilisée à la fois pour le scénario &quot;Authentification par demandeur&quot; et pour le flux d’authentification normal. La seule différence entre les deux réside dans la manière dont l’authentification est effectuée : &quot;Authentification par demandeur&quot; contient une nouvelle amélioration (authentification passive) qui permet à AccessEnabler d’effectuer l’authentification back-channel, en fonction de l’existence d’un jeton d’authentification dans le stockage (pour un autre programmeur). L’utilisateur ne doit authentifier qu’une seule fois. Cette session sera utilisée pour obtenir des jetons d’authentification dans les applications suivantes. Ce flux back-channel a lieu pendant l’appel [`setRequestor()`](#setRequestor) et est principalement transparent pour le programmeur. Il existe toutefois une exigence importante ici : le programmeur DOIT appeler [`setRequestor()`](#setRequestor) à partir du thread d’interface utilisateur principal et depuis une activité.


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



À partir de la version 1.7 d’AccessEnabler, le stockage des jetons peut prendre en charge plusieurs combinaisons Programmer-MVPD, en s’appuyant sur une structure de mappage imbriquée à plusieurs niveaux pouvant contenir plusieurs jetons d’authentification. Ce nouveau stockage n’affecte en aucune manière l’API publique AccessEnabler et ne nécessite aucune modification du côté du programmeur. Voici un exemple qui illustre cette nouvelle fonctionnalité :

1. Ouvrez App1 (développé par Programmer1).
1. Authentifiez-vous avec MVPD1 (qui est intégré à Programmer1).
1. Suspendre/Fermer l’application actuelle et ouvrir App2 (développé par Programmer2).
1. Supposons que Programmer2 ne soit pas intégré à MVPD2 ; par conséquent, l’utilisateur ne sera PAS authentifié dans App2.
1. Authentifiez-vous avec MVPD2 (intégré à Programmer2) dans App2.
1. Revenez à App1 ; l’utilisateur sera toujours authentifié avec Programmer1.

Dans les anciennes versions d’AccessEnabler, l’étape 6 affichait l’utilisateur comme non authentifié, car le stockage des jetons ne prenait auparavant en charge qu’un seul jeton d’authentification.


**REMARQUE :** La déconnexion d’une session Programmer/MVPD effacera le stockage sous-jacent, y compris tous les autres jetons d’authentification Programmer sur l’appareil avec SSO. Les jetons obtenus pour d’autres MVPD ou non par SSO ne seront pas supprimés. L’annulation du flux d’authentification (appel de [`setSelectedProvider(null)`](#setSelectedProvider)) N’effacera PAS le stockage sous-jacent, mais affectera uniquement la tentative d’authentification actuelle du programmeur/MVPD (en effaçant le MVPD pour le programmeur actuel).


Une autre fonctionnalité de stockage incluse dans AccessEnabler 1.7 permet d’importer des jetons d’authentification à partir d’anciennes zones de stockage. Cet &quot;Importateur de jetons&quot; permet d’assurer la compatibilité entre les versions consécutives d’AccessEnabler, en conservant l’état SSO même lorsque la version de stockage est mise à niveau.

L’importateur est exécuté pendant le flux [`setRequestor()`](#setRequestor) et s’exécute dans les deux scénarios suivants (en supposant qu’aucun jeton d’authentification valide pour le programmeur actuel ne soit présent dans le stockage actuel) :

- La première installation d’une application 1.7 développée par un programmeur spécifique
- Chemin de mise à niveau vers un AccessEnabler futur qui utilise un nouveau stockage

L’opération d’importation est transparente pour le programmeur et ne nécessite aucun changement de code dans l’application cliente.



### Format {#format}

- [Jeton d’authentification](#authn_token)
- [Jeton d’autorisation](#authz_token)
- [Jeton de média court](#short_media_token)
- [Liaison de périphérique](#device_binding)

#### Jeton d’authentification {#authn_token}

La liste ci-dessous présente le format du jeton d’authentification :

```XML
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

```XML
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

```XML
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


<!--
## Related Information {#related}

- [Android Integration Cookbook](#)
- [Android API](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass Authentication native SDK (Tech Note)](#)
- [AE 1.6: How to resolve Gson dependencies (Tech Note)](#)
-->
