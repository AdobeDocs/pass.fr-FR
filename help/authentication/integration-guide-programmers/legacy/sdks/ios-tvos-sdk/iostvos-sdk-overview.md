---
title: Présentation d’iOS/tvOS SDK
description: Présentation d’iOS/tvOS SDK
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3754'
ht-degree: 0%

---

# Présentation d’iOS/tvOS SDK (hérité) {#iostvos-sdk-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>


## Introduction {#intro}

iOS AccessEnabler est une bibliothèque Objective C iOS/tvOS qui permet aux applications mobiles d&#39;utiliser l&#39;authentification Adobe Pass pour les services de droits de TV Everywhere. L’implémentation se compose de l’interface *AccessEnabler* qui définit l’API de droits et des protocoles *EntitlementDelegate* et *[EntitlementStatus](#ios%20entitlement%20status)* qui décrivent les rappels déclenchés par la bibliothèque. L’interface et le protocole sont référencés sous un nom commun : la bibliothèque AccessEnabler.

## Configuration requise pour iOS et tvOS {#reqs}

Pour connaître les exigences techniques actuelles relatives à la plateforme iOS et tvOS et à l’authentification Adobe Pass, consultez la section [Configuration requise pour la plateforme/l’appareil/l’outil](#ios) et consultez les notes de mise à jour accompagnant le téléchargement de SDK. Tout au long du reste de cette page, vous trouverez des sections qui notent les modifications qui s’appliquent à des versions de SDK particulières et à des versions ultérieures. Par exemple, voici une note légitime concernant le SDK 1.7.5 :

## Présentation des workflows client natifs {#flows}

Les workflows clients natifs sont généralement identiques ou très similaires à ceux des clients d’authentification Adobe Pass basés sur un navigateur. Cependant, il existe quelques exceptions, comme décrit ci-dessous.

- [Workflow de post-initialisation](#post-init)
- [Workflow d’authentification initiale générique](#generic)
- [Workflow de déconnexion](#logout)


### Workflow de post-initialisation {#post-init}

Tous les workflows de droits pris en charge par AccessEnabler supposent que vous avez précédemment appelé [`setRequestor()`](#setReq) pour établir votre identité. Vous passez cet appel pour fournir votre ID de demandeur une seule fois, généralement au cours de la phase d’initialisation/configuration de votre application.


Avec un client natif iOS, après votre appel initial à [`setRequestor()`](#setReq), vous avez le choix de la manière de procéder :

- Vous pouvez commencer à effectuer immédiatement des appels de droits et les autoriser à être placés en file d&#39;attente silencieuse, si nécessaire.

- Vous pouvez recevoir une confirmation du succès/de l’échec de l’[`setRequestor()`](#setReq) en implémentant le rappel [`setRequestorComplete()`](#setReqComplete) .

- Vous pouvez effectuer les deux opérations ci-dessus.

Vous pouvez choisir d’attendre que votre application soit informée du succès de l’[`setRequestor()`](#setReq) ou de faire appel au mécanisme de file d’attente d’appels d’AccessEnabler. Comme toutes les demandes d’authentification et d’autorisation suivantes nécessitent l’ID du demandeur et les informations de configuration associées, la méthode [`setRequestor()`](#setReq) bloque efficacement tous les appels d’API d’authentification et d’autorisation jusqu’à ce que l’initialisation soit terminée.



### Workflow d’authentification initiale générique {#generic}

L’objectif de ce workflow est de connecter un utilisateur avec son MVPD. Une fois la connexion établie, le serveur principal émet un jeton d’authentification à l’utilisateur. Bien que l’authentification soit généralement effectuée dans le cadre du processus d’autorisation, ce qui suit décrit comment l’authentification peut fonctionner de manière isolée et n’inclut aucune étape d’autorisation.

Notez que bien que ce workflow diffère pour les clients natifs du workflow d’authentification standard basé sur un navigateur, les étapes 1 à 5 sont identiques pour les clients natifs et les clients basés sur un navigateur.

1. Votre application lance le workflow d’authentification avec un appel à la méthode `getAuthentication() `API d’AccessEnabler, qui vérifie un jeton d’authentification mis en cache valide.
1. Si l’utilisateur est actuellement authentifié, AccessEnabler appelle votre fonction de rappel [`setAuthenticationStatus()`](#setAuthNStatus), transmettant un statut d’authentification indiquant la réussite et mettant fin au flux.
1. Si l’utilisateur n’est pas authentifié actuellement, AccessEnabler poursuit le flux d’authentification en déterminant si la dernière tentative d’authentification de l’utilisateur a réussi avec un MVPD donné. Si un ID MVPD est mis en cache ET que l’indicateur `canAuthenticate` est true OU si un MVPD a été sélectionné à l’aide de [`setSelectedProvider()`](#setSelProv), l’utilisateur n’est pas invité à ouvrir la boîte de dialogue de sélection de MVPD. Le flux d’authentification continue à utiliser la valeur mise en cache du MVPD (c’est-à-dire le même MVPD que celui utilisé lors de la dernière authentification réussie). Un appel réseau est effectué au serveur principal et l’utilisateur est redirigé vers la page de connexion de MVPD (étape 6 ci-dessous).
1. Si aucun ID MVPD n’est mis en cache ET qu’aucun MVPD n’a été sélectionné à l’aide de [`setSelectedProvider()`](#setSelProv) OU que l’indicateur `canAuthenticate` est défini sur false, le rappel [`displayProviderDialog()`](#dispProvDialog) est appelé. Ce rappel demande à votre application de créer l’interface utilisateur qui présente à l’utilisateur une liste de fichiers MVPD parmi lesquels choisir. Un tableau d’objets MVPD contenant les informations nécessaires à la création du sélecteur MVPD est fourni. Chaque objet MVPD décrit une entité MVPD et contient des informations telles que l’identifiant du MVPD (par exemple XFINITY, AT\&amp;T, etc.) et l’URL contenant le logo MVPD.
1. Une fois qu’un MVPD particulier est sélectionné, votre application doit informer AccessEnabler du choix de l’utilisateur. Une fois que l’utilisateur sélectionne le MVPD souhaité, vous informez l’AccessEnabler de la sélection de l’utilisateur par le biais d’un appel à la méthode [`setSelectedProvider()`](#setSelProv).
1. IOS AccessEnabler appelle votre rappel `navigateToUrl:` ou `navigateToUrl:useSVC:` pour rediriger l’utilisateur vers la page de connexion de MVPD. En déclenchant l’un ou l’autre, AccessEnabler envoie une requête à votre application pour créer un contrôleur de `UIWebView/WKWebView or SFSafariViewController` et charger l’URL fournie dans le paramètre `url` du rappel . Il s’agit de l’URL du point d’entrée d’authentification du serveur principal. Pour tvOS AccessEnabler, le rappel [status()](#status_callback_implementation) est appelé avec un paramètre `statusDictionary` et l’interrogation pour la deuxième authentification d’écran est immédiatement lancée. Le `statusDictionary` contient le `registration code` qui doit être utilisé pour la deuxième authentification d’écran.
1. Dans le cas d’iOS AccessEnabler, l’utilisateur accède à la page de connexion de MVPD pour saisir ses informations d’identification par le biais du contrôleur de votre application `UIWebView/WKWebView or SFSafariViewController `. Notez que plusieurs opérations de redirection se produisent au cours de ce transfert et que votre application doit surveiller les URL chargées par le contrôleur lors de plusieurs opérations de redirection.
1. Dans le cas d’iOS AccessEnabler, lorsque le contrôleur de `UIWebView/WKWebView or SFSafariViewController` charge une URL personnalisée spécifique, votre application doit fermer le contrôleur et appeler la méthode `handleExternalURL:url `API d’AccessEnabler. Notez que cette URL personnalisée spécifique n&#39;est pas valide et qu&#39;elle n&#39;est pas destinée à être chargée par le contrôleur. Elle doit être interprétée par votre application uniquement comme un signal indiquant que le flux d’authentification est terminé et qu’il est sûr de fermer le contrôleur `UIWebView/WKWebView or SFSafariViewController`. Si votre application doit utiliser un `SFSafariViewController `contrôleur, l’URL personnalisée spécifique est définie par le `application's custom scheme` (par exemple : `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Dans le cas contraire, cette URL personnalisée spécifique est définie par la constante `ADOBEPASS_REDIRECT_URL` (par exemple : `adobepass://ios.app`).
1. Une fois que votre application ferme le contrôleur `UIWebView/WKWebView or SFSafariViewController` et appelle la méthode `handleExternalURL:url `API d’AccessEnabler, celle-ci récupère le jeton d’authentification du serveur principal et informe votre application que le flux d’authentification est terminé. AccessEnabler appelle le rappel [`setAuthenticationStatus()`](#setAuthNStatus) avec un code d&#39;état de 1, indiquant la réussite. En cas d’erreur lors de l’exécution de ces étapes, le rappel [`setAuthenticationStatus()`](#setAuthNStatus) est déclenché avec un code d’état de 0 indiquant l’échec de l’authentification, ainsi qu’un code d’erreur correspondant.


>[!WARNING]
>
> Lors des étapes au cours desquelles AccessEnabler cède le contrôle à votre application (c’est-à-dire lorsque la boîte de dialogue de sélection du fournisseur est affichée ou lorsque l’UIWebView/WKWebView ou SFSafariViewController est affiché), l’utilisateur a la possibilité d’annuler le flux d’authentification. Dans ce cas, il incombe à votre application d’informer AccessEnabler de cet événement et d’appeler la méthode d’API [`setSelectedProvider()`](#setSelProv), en transmettant la valeur null comme paramètre. Cela permet à AccessEnabler de nettoyer son état interne et de réinitialiser le flux d’authentification. Sous tvOS, vous pouvez utiliser la même méthode pour annuler l’interrogation d’authentification.


### Workflow de déconnexion {#logout}

Pour les clients natifs, la déconnexion est gérée de la même manière que le processus d’authentification décrit ci-dessus.

1. Votre application lance le workflow de déconnexion avec un appel à la méthode `logout() `API d’AccessEnabler. La déconnexion est le résultat d’une série d’opérations de redirection HTTP en raison du fait que l’utilisateur doit être déconnecté des serveurs d’authentification Adobe Pass et des serveurs de MVPD. Comme ce flux ne peut pas être exécuté avec une simple requête HTTP émise par la bibliothèque AccessEnabler, un contrôleur `UIWebView/WKWebView or SFSafariViewController` doit être instancié pour pouvoir suivre les opérations de redirection HTTP.

1. Un motif similaire au flux d’authentification est utilisé. IOS AccessEnabler déclenche le rappel `navigateToUrl:` ou le `navigateToUrl:useSVC:` pour créer un contrôleur de `UIWebView/WKWebView or SFSafariViewController` et charger l’URL fournie dans le paramètre `url` du rappel. Il s’agit de l’URL du point d’entrée de déconnexion sur le serveur principal. Pour tvOS AccessEnabler, ni le rappel `navigateToUrl:` ni le rappel `navigateToUrl:useSVC:` n’est appelé.

1. Comme il passe par plusieurs redirections, votre application doit surveiller l&#39;activité du contrôleur `UIWebView/WKWebView or SFSafariViewController ` et détecter le moment où il charge une URL personnalisée spécifique. Notez que cette URL personnalisée spécifique n&#39;est pas valide et qu&#39;elle n&#39;est pas destinée à être chargée par le contrôleur. Il doit être interprété par votre application uniquement comme un signal indiquant que le flux de déconnexion est terminé et qu&#39;il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer le contrôleur et appeler la méthode `handleExternalURL:url `API AccessEnabler. Si votre application doit utiliser un `SFSafariViewController ` contrôleur, l’URL personnalisée spécifique est définie par le `application's custom scheme` (par exemple, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Dans le cas contraire, cette URL personnalisée spécifique est définie par la ` ADOBEPASS_REDIRECT_URL  ` constante (par exemple, `adobepass://ios.app`).

1. À la fin, AccessEnabler appelle le rappel [`setAuthenticationStatus()`](#setAuthNStatus) avec un code d&#39;état de 0, indiquant le succès du flux de déconnexion.

Le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir de quelque manière que ce soit avec le contrôleur de `UIWebView/WKWebView or SFSafariViewController`. Par conséquent, Adobe vous recommande de rendre le contrôle invisible (c’est-à-dire masqué) pendant le processus de déconnexion.

## Jetons {#tokens}

- [Définitions et utilisation](#definitions)
- [Instructions de mise en cache](#caching)
- [Persistance](#persistence)
- [Format](#format)
- [Liaison de l’appareil](#device_binding)


### Définitions et utilisation {#definitions}

La solution Droits d’authentification Adobe Pass tourne autour de la génération de données spécifiques (jetons) que l’authentification Adobe Pass génère une fois les workflows d’authentification et d’autorisation terminés. Ces jetons sont stockés localement sur l’appareil iOS du client.



Les jetons ont une durée de vie limitée. À leur expiration, les jetons doivent être émis à nouveau par le biais du redémarrage des workflows d’authentification et/ou d’autorisation.



Il existe trois types de jetons émis pendant les workflows de droits :

- **Jeton d’authentification :** le résultat final du workflow d’authentification de l’utilisateur sera une GUID d’authentification que l’AccessEnabler peut utiliser pour effectuer des requêtes d’autorisation au nom de l’utilisateur. Ce GUID d’authentification est associé à une valeur de durée de vie (TTL) qui peut être différente de la session d’authentification de l’utilisateur elle-même. Un jeton d’authentification sera généré en liant le GUID d’authentification à l’appareil à l’origine des demandes d’authentification.
- **Jeton d’autorisation :** accorde l’accès à une ressource protégée spécifique identifiée par un resourceID unique. Il se compose d’une autorisation délivrée par l’ordonnateur et de l’identifiant de ressource d’origine. Ces informations sont liées à l’appareil à l’origine de la demande.
- **Jeton multimédia de courte durée :** AccessEnabler accorde l’accès à l’application d’hébergement pour une ressource donnée en renvoyant un jeton multimédia de courte durée. Ce jeton est généré en fonction du jeton d’autorisation précédemment acquis pour cette ressource spécifique. En outre, ce jeton n’est pas lié à l’appareil et la durée de vie associée est considérablement plus courte (par défaut : 5 minutes).

Une fois l’authentification et l’autorisation réussies, l’authentification Adobe Pass émet des jetons d’authentification, d’autorisation et de média de courte durée. Ces jetons doivent être mis en cache sur l’appareil de l’utilisateur et utilisés pendant la durée de vie qui leur est associée.



### Instructions de mise en cache {#caching}

- Jeton d’authentification
- Jeton d’autorisation
- Jeton de média de courte durée


#### Jeton d’authentification

- **AccessEnabler 1.7:** ce SDK introduit une nouvelle méthode de stockage de jetons, permettant plusieurs compartiments Programmer-MVPD et, par conséquent, plusieurs jetons d’authentification. Désormais, la même disposition de stockage est utilisée pour le scénario « Authentification par demandeur » et pour le flux d’authentification normal. La seule différence entre les deux réside dans la manière dont l&#39;authentification est effectuée : « Authentification par demandeur » contient une nouvelle amélioration (Authentification passive) qui permet à AccessEnabler d&#39;effectuer une authentification back-channel, basée sur l&#39;existence d&#39;un jeton d&#39;authentification dans le stockage (pour un autre programmeur). L’utilisateur ne doit s’authentifier qu’une seule fois, et cette session sera utilisée pour obtenir des jetons d’authentification dans d’autres applications. Ce flux back-channel a lieu pendant l’appel [`setRequestor()`](#setReq) et est principalement transparent pour le programmeur. **Il existe toutefois une exigence importante ici : le programmeur DOIT appeler setRequestor() à partir du thread principal de l’interface utilisateur.**
- **AccessEnabler 1.6 et versions antérieures :** la manière dont les jetons d’authentification sont mis en cache sur l’appareil dépend de l’indicateur « **Authentification par demandeur »** associé au MVPD actuel :

<!-- end list -->

1. Si la fonction « Authentification par demandeur » est désactivée, un seul jeton d’authentification est stocké localement dans la table de montage globale ; ce jeton est partagé entre toutes les applications intégrées au MVPD actuel.
1. Si la fonction « Authentification par demandeur » est activée, un jeton est explicitement associé au programmeur qui a exécuté le flux d’authentification (le jeton n’est pas stocké dans la table de montage globale, mais dans un fichier privé visible uniquement par l’application de ce programmeur). Plus précisément, l’authentification unique (SSO) entre différentes applications sera désactivée ; l’utilisateur devra exécuter explicitement le flux d’authentification lors du passage à une nouvelle application (à condition que le programmeur de la deuxième application soit intégré au MVPD actuel et qu’il n’existe aucun jeton d’authentification pour ce programmeur dans le cache local).



#### Jeton d’autorisation

À un moment donné, un seul jeton d’autorisation par RESSOURCE est mis en cache par AccessEnabler. Plusieurs jetons d’autorisation peuvent être mis en cache, mais ils sont associés à différentes ressources. Chaque fois qu’un nouveau jeton d’autorisation est émis et qu’un ancien jeton existe déjà pour *la même ressource*, le nouveau jeton remplace la valeur mise en cache existante.



#### Jeton multimédia

Le jeton de média de courte durée ne doit PAS être mis en cache du tout. Le jeton multimédia doit être récupéré à partir du serveur chaque fois qu’une API d’autorisation est appelée, car son utilisation est limitée à une utilisation unique.



### Persistance {#persistence}

Les jetons doivent être persistants sur plusieurs exécutions consécutives de la même application. Cela signifie qu’une fois les jetons d’authentification et d’autorisation acquis et que l’utilisateur ferme l’application, les mêmes jetons sont disponibles pour l’application lorsque l’utilisateur ouvre à nouveau l’application. En outre, il est souhaitable que ces jetons soient persistants dans plusieurs applications. En d’autres termes, une fois qu’un utilisateur ou une utilisatrice utilise une application pour se connecter avec un fournisseur d’identité spécifique (obtention réussie des jetons d’authentification et d’autorisation), les mêmes jetons peuvent être utilisés par le biais d’une autre application. En outre, l’utilisateur ou l’utilisatrice n’est plus invité à fournir des informations d’identification lors de la connexion via le même fournisseur d’identité. Ce type de workflow d’authentification/autorisation transparent est ce qui fait de la solution d’authentification Adobe Pass une véritable solution TV-Everywhere
mise en œuvre.



## iOS

La bibliothèque iOS AccessEnabler résout les problèmes de partage de données entre applications en stockant les données de jeton dans une structure de données « de type presse-papiers » appelée tableau de *collage*. Cette ressource partagée au niveau du système fournit les ingrédients clés qui permettent la mise en œuvre du cas d’utilisation de jetons persistants souhaité :

- **Prise en charge du stockage structuré** - La carte de collage n’est pas simplement une structure de mémoire tampon linéaire simple. Il fournit un mécanisme de stockage de type dictionnaire qui permet l’indexation des données en fonction de valeurs de clé spécifiées par l’utilisateur.
- **Prise en charge de la persistance des données à l’aide du système de fichiers sous-jacent** - Le contenu de la structure du panorama de collage peut être marqué comme persistant, auquel cas les données sont enregistrées sur la mémoire interne de l’appareil.



Une fois qu’un jeton particulier est placé dans le cache de jetons, sa validité est vérifiée à différents moments par la bibliothèque AccessEnabler.  Un jeton valide est défini comme suit :

- La TTL du jeton n’a pas expiré.
- L’émetteur du jeton est inclus dans la liste des fournisseurs d’identité autorisés.



## tvOS

Étant donné que sur tvOS, la table de montage n’est pas disponible, la bibliothèque tvOS AccessEnabler utilise NsUserDefaults comme option de stockage. Cela résout le problème de la persistance des jetons d’authentification et d’autorisation, mais les informations stockées ne peuvent pas être partagées entre différentes applications.



**Modifications de la table de montage iOS 10 -** Lors de la mise à niveau à partir d’une version précédente d’iOS, la table de montage sera effacée. Cela signifie que toutes les applications doivent être réauthentifiées.



**Modifications de la table de montage d’iOS 7 -** En raison de modifications dans le fonctionnement des tables de montage sur iOS 7, l’authentification unique (Cross-SSO) entre les applications s’exécutant sur iOS 7 sera limitée. Les applications ayant le même `<Bundle Seed ID>` (également appelées `<Team ID>`) partagent des jetons, ce qui signifie que les applications A1 et A2 du même programmeur X partagent des jetons, tandis que l’application A1 (programmeur X) et l’application A3 (programmeur Y) ne partagent pas de jetons.

- L’ID de contrôle/ID d’équipe de bundle est le même entre 2 applications si elles sont générées par le même profil d’approvisionnement. Pour plus d’informations, cliquez sur ce lien :
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- Cette limitation « Cross-SSO » sera présente dans iOS 7, quelle que soit la SDK d’authentification Adobe Pass utilisée.

Veuillez lire cette note technique pour plus d’informations sur la configuration de la connexion unique sur iOS 7 et versions ultérieures (la note technique s’applique à Access Enabler version 1.8 et versions ultérieures) : <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### Stockage des jetons (AccessEnabler 1.7)

À partir d’AccessEnabler 1.7, le stockage des jetons peut prendre en charge plusieurs combinaisons Programmeur-MVPD, en s’appuyant sur une structure de mappage imbriqué à plusieurs niveaux qui peut contenir plusieurs jetons d’authentification. Ce nouveau stockage n’affecte en rien l’API publique AccessEnabler et ne nécessite aucune modification du côté du programmeur. Voici un exemple qui :
illustre cette nouvelle fonctionnalité :

1. Open App1 (développé par Programmer1).
1. Authentifiez-vous avec MVPD1 (intégré à Programmer1).
1. Suspendre / Fermer l&#39;application courante, et ouvrir App2 (développé par Programmer2).
1. Supposons que Programmer2 ne soit pas intégré à MVPD2 ; par conséquent, l’utilisateur ne sera PAS authentifié dans App2.
1. Authentifiez-vous avec MVPD2 (qui est intégré à Programmer2) dans App2.
1. Revenez à App1 ; l’utilisateur sera toujours authentifié avec Programmer1.

Dans les anciennes versions d’AccessEnabler, l’étape 6 rendait l’utilisateur comme non authentifié, car le stockage des jetons ne prenait auparavant en charge qu’un seul jeton d’authentification.



La déconnexion d’une session Programmer/MVPD effacera l’ensemble du stockage sous-jacent, y compris tous les autres jetons d’authentification Programmer/MVPD de l’appareil. D’un autre côté, l’annulation du flux d’authentification (en invoquant [`setSelectedProvider(null)`](#setSelProv)) n’effacera PAS le stockage sous-jacent, mais elle n’affectera que la tentative d’authentification actuelle du programmeur/MVPD (en effaçant le MVPD pour le programmeur actuel).



### Importateur de jetons (AccessEnabler 1.7)

Une autre fonctionnalité liée au stockage incluse dans AccessEnabler 1.7 permet d’importer des jetons d’authentification à partir de zones de stockage plus anciennes. Cet « importateur de jetons » permet d’obtenir la compatibilité entre les versions consécutives d’AccessEnabler, en maintenant l’état SSO même lorsque la version de stockage est mise à niveau. L’importateur est exécuté pendant le flux de [`setRequestor()`](#setReq) et s’exécute dans les deux scénarios suivants (en supposant qu’aucun jeton d’authentification valide pour le programmeur actuel ne soit présent dans le stockage actuel) :

- La première installation d&#39;une application 1.7 développée par un programmeur spécifique
- Mettre à niveau le chemin vers un futur AccessEnabler qui utilise un nouveau stockage

L’opération d’importation est transparente pour le programmeur et ne nécessite aucune modification du code dans l’application cliente.



### Assainisseur de jetons (AccessEnabler 1.7.5)

À partir de la version 1.7.5 d’AccessEnabler, ce service s’exécute sur [`setRequestor()`](#setReq)`. `. Il a été développé à la suite du changement d’iOS 7, qui fait passer l’adresse WiFi MAC à l’IDFA pour le suivi. L’assainisseur garantit que le stockage actuel contient uniquement des jetons d’authentification valides (valides en termes d’ID d’appareil, précédemment calculé à l’aide de l’adresse MAC, avant iOS7). L’assainisseur de jetons supprime tous les jetons non valides.



Le service Assainisseur de jetons est particulièrement utile lorsqu’une application AccessEnabler 1.7.5 est utilisée sur iOS 6, puis que l’utilisateur ou l’utilisatrice effectue une mise à jour vers iOS 7. Dans ce cas, tous les jetons d’authentification obtenus sur iOS 6 ne sont plus valides (en raison du changement d’algorithme de l’ID d’appareil, qui passe de l’utilisation de l’adresse MAC à l’IDFA). L’assainisseur efface tous les jetons non valides et l’utilisateur n’est alors pas authentifié.



Si l’assainisseur de jetons ne supprime pas les jetons non valides, AccessEnabler ne parviendra pas à obtenir les autorisations en raison de la présence de jetons d’authentification non valides. Cela s’avérerait difficile à déboguer pour l’utilisateur final, car les messages d’erreur d’autorisation peuvent être énigmatiques et ne pas trop aider à déterminer ce qui a provoqué le problème.



### Format {#format}

- [Jeton AuthN](#authn_token)
- [Jeton AuthZ](#authz_token)
- [Jeton multimédia court](#short_token)
- [Liaison de l’appareil](#device_binding)


Notez que le format des jetons AuthN et AuthZ est inclus ici à titre d’information uniquement. La structure de ces jetons peut être modifiée à tout moment par l’authentification Adobe Pass. Les programmeurs n’ont pas besoin de connaître la structure exacte des jetons AuthN et AuthZ pour implémenter leurs applications, car les jetons AuthN et AuthZ ne sont pas exposés sur l’appareil local. Le jeton de média court *is* est exposé à l’application du programmeur.



#### Jeton d’authentification {#authn_token}

La liste ci-dessous présente le format du jeton d’authentification :

```
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

```
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


#### Jeton multimédia court {#short_token}

La liste ci-dessous présente le format du jeton de média court. Ce jeton est exposé à l’application du programmeur. Il est transmis à l’application du programmeur à la fin d’un processus d’octroi de droits réussi :

```
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


### Liaison de l’appareil {#device_binding}

Dans les listes XML ci-dessus, notez la balise intitulée `simpleTokenFingerprint`. Cette balise est destinée à contenir des informations d’individualisation de l’identifiant de l’appareil natif. La bibliothèque AccessEnabler peut obtenir ces informations d&#39;individualisation et les mettre à la disposition des services d&#39;authentification Adobe Pass lors des appels de droits. Le service utilisera ces informations et les incorporera dans les jetons réels, liant ainsi efficacement les jetons à un appareil spécifique. L’objectif final est de rendre les jetons non transférables sur tous les appareils.



Comme il s’agit évidemment d’une fonctionnalité liée à la sécurité, ces informations sont par nature « sensibles » du point de vue de la sécurité. Par conséquent, ces renseignements doivent être protégés contre l&#39;altération et l&#39;écoute électronique. Le problème d’écoute est résolu en envoyant les demandes d’authentification/autorisation via le protocole HTTPS. La protection contre l&#39;altération est gérée en signant numériquement les informations d&#39;identification du dispositif. La bibliothèque AccessEnabler calcule un identifiant d’appareil à partir des informations fournies par l’appareil, puis envoie l’identifiant d’appareil « en clair » aux serveurs d’authentification Adobe Pass en tant que paramètre de requête. Les serveurs d’authentification d’Adobe Pass signent numériquement l’ID d’appareil avec la clé privée Adobe et l’ajoutent au jeton d’authentification renvoyé à AccessEnabler. Ainsi, l’ID de l’appareil est lié au jeton d’authentification. Pendant le flux d’autorisation, AccessEnabler envoie à nouveau l’ID d’appareil en clair, ainsi que le jeton d’authentification. L’échec du processus de validation entraîne automatiquement l’échec des workflows d’authentification/autorisation. Les serveurs d’authentification d’Adobe Pass appliquent la clé privée à l’ID d’appareil et la comparent à la valeur du jeton d’authentification. S’ils ne correspondent pas, ce flux de droits échoue.



**Remarque sur la liaison d’appareil dans AccessEnabler 1.7.5:** À partir d’AccessEnabler 1.7.5, la façon dont l’ID d’appareil est calculé pour fournir des informations d’individualisation pour un appareil iOS a changé. Cette modification reflète un changement dans iOS 7 : à partir d’iOS 7, Apple ne fournit plus l’adresse MAC WiFi comme option de tracking, au profit de son identifiant pour les annonceurs (IDFA). Étant donné que les informations d’individualisation d’une application s’exécutant sur iOS 7 reposent sur l’IDFA et que ces informations sont intégrées aux jetons de flux de droits, cela signifie que cette modification peut avoir plusieurs effets différents sur l’expérience utilisateur. Les différents effets possibles dépendent de la version d’iOS à partir de laquelle l’utilisateur effectue la mise à niveau, associée à la version d’AccessEnabler à partir de laquelle le programmeur effectue la mise à niveau. Pour plus d’informations sur cette modification, consultez les notes de mise à jour incluses dans AccessEnabler SDK 1.7.5.

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
