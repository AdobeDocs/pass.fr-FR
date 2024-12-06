---
title: Présentation du SDK iOS/tvOS
description: Présentation du SDK iOS/tvOS
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '3731'
ht-degree: 0%

---

# Présentation du SDK iOS/tvOS {#iostvos-sdk-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


</br>


## Introduction {#intro}

iOS AccessEnabler est une bibliothèque Objective C iOS/tvOS qui permet aux applications mobiles d’utiliser l’authentification Adobe Pass pour les services de droits de TV Everywhere. L’implémentation se compose de l’interface *AccessEnabler* qui définit l’API de droits, ainsi que des protocoles *EntitlementDelegate* et *[EntitlementStatus](#ios%20entitlement%20status)* qui décrivent les rappels déclenchés par la bibliothèque. L’interface avec le protocole est référencée sous un nom commun : la bibliothèque AccessEnabler.

## Configuration requise pour iOS et tvOS {#reqs}

Pour connaître les exigences techniques actuelles relatives à la plateforme iOS et tvOS et à l’authentification Adobe Pass, voir [Configuration requise pour la plateforme/l’appareil/l’outil](#ios), et consulter les notes de mise à jour incluses avec le téléchargement du SDK. Dans le reste de cette page, vous trouverez des sections qui mentionnent les modifications qui s’appliquent à des versions de SDK spécifiques et supérieures. Par exemple, voici une remarque légitime concernant le SDK 1.7.5 :

## Présentation des processus client natifs {#flows}

Les workflows clients natifs sont généralement identiques ou très similaires à ceux des clients Adobe Pass Authentication basés sur un navigateur. Il existe toutefois quelques exceptions, comme décrit ci-dessous.

- [Processus de post-initialisation](#post-init)
- [Processus d’authentification initiale générique](#generic)
- [Processus de déconnexion](#logout)


### Processus de post-initialisation {#post-init}

Tous les workflows de droits pris en charge par AccessEnabler supposent que vous avez déjà appelé [`setRequestor()`](#setReq) pour établir votre identité. Vous effectuez cet appel pour ne fournir votre ID de demandeur qu’une seule fois, généralement pendant la phase d’initialisation/de configuration de votre application.


Avec un client natif iOS, après votre premier appel à [`setRequestor()`](#setReq), vous avez le choix entre procéder :

- Vous pouvez commencer à lancer des appels de droits immédiatement et leur permettre d’être mis en file d’attente silencieuse, si nécessaire.

- Vous pouvez recevoir une confirmation de la réussite/l’échec de [`setRequestor()`](#setReq) en mettant en oeuvre le rappel [`setRequestorComplete()`](#setReqComplete).

- Vous pouvez effectuer les deux opérations ci-dessus.

Votre choix consiste à demander à votre application d’attendre la notification du succès de [`setRequestor()`](#setReq) ou de vous fier au mécanisme de file d’attente d’appels d’AccessEnabler. Comme toutes les demandes d’autorisation et d’authentification suivantes ont besoin de l’identifiant du demandeur et des informations de configuration associées, la méthode [`setRequestor()`](#setReq) bloque efficacement tous les appels d’API d’authentification et d’autorisation jusqu’à ce que l’initialisation soit terminée.



### Processus d’authentification initiale générique {#generic}

Le but de ce workflow est de se connecter à un utilisateur avec son MVPD. Une fois la connexion établie, le serveur principal émet un jeton d’authentification à l’utilisateur. Bien que l’authentification soit généralement effectuée dans le cadre du processus d’autorisation, la section suivante décrit le fonctionnement de l’authentification en isolation, sans inclure d’étapes d’autorisation.

Notez que, bien que ce processus diffère pour les clients natifs du processus d’authentification type basé sur un navigateur, les étapes 1 à 5 sont les mêmes pour les clients natifs et les clients basés sur un navigateur.

1. Votre application lance le workflow d’authentification avec un appel à la méthode `getAuthentication() `API d’AccessEnabler, qui recherche un jeton d’authentification mis en cache valide.
1. Si l’utilisateur est actuellement authentifié, AccessEnabler appelle votre fonction de rappel [`setAuthenticationStatus()`](#setAuthNStatus), en transmettant un état d’authentification indiquant la réussite et en mettant fin au flux.
1. Si l’utilisateur n’est pas actuellement authentifié, AccessEnabler poursuit le flux d’authentification en déterminant si la dernière tentative d’authentification de l’utilisateur a réussi avec un MVPD donné. Si un ID MVPD est mis en cache ET que l’indicateur `canAuthenticate` est défini sur true OU qu’un MVPD a été sélectionné à l’aide de [`setSelectedProvider()`](#setSelProv), l’utilisateur n’est pas invité avec la boîte de dialogue de sélection MVPD. Le flux d’authentification continue à utiliser la valeur mise en cache du MVPD (c’est-à-dire le même MVPD qui a été utilisé lors de la dernière authentification réussie). Un appel réseau est effectué au serveur principal et l’utilisateur est redirigé vers la page de connexion MVPD (étape 6 ci-dessous).
1. Si aucun ID MVPD n’est mis en cache ET qu’aucun MVPD n’a été sélectionné à l’aide de [`setSelectedProvider()`](#setSelProv) OU que l’indicateur `canAuthenticate` est défini sur false, le rappel [`displayProviderDialog()`](#dispProvDialog) est appelé. Ce rappel demande à votre application de créer l’interface utilisateur qui présente à l’utilisateur une liste de MVPD parmi lesquelles choisir. Un tableau d’objets MVPD est fourni, contenant les informations nécessaires à la création du sélecteur MVPD. Chaque objet MVPD décrit une entité MVPD et contient des informations telles que l’identifiant du MVPD (ex. XFINITY, AT\&amp;T, etc.) et l’URL où se trouve le logo MVPD.
1. Une fois qu’un MVPD particulier est sélectionné, votre application doit informer AccessEnabler du choix de l’utilisateur. Une fois que l’utilisateur sélectionne le MVPD souhaité, vous informez AccessEnabler de la sélection de l’utilisateur via un appel à la méthode [`setSelectedProvider()`](#setSelProv).
1. IOS AccessEnabler appelle votre rappel `navigateToUrl:` ou `navigateToUrl:useSVC:` pour rediriger l’utilisateur vers la page de connexion MVPD. En déclenchant l’une ou l’autre, AccessEnabler demande à votre application de créer un contrôleur `UIWebView/WKWebView or SFSafariViewController` et de charger l’URL fournie dans le paramètre `url` du rappel. Il s’agit de l’URL du point de terminaison d’authentification sur le serveur principal. Pour tvOS AccessEnabler, le rappel [status()](#status_callback_implementation) est appelé avec un paramètre `statusDictionary` et l’interrogation de la deuxième authentification d’écran est immédiatement lancée. `statusDictionary` contient le `registration code` qui doit être utilisé pour la deuxième authentification d’écran.
1. Dans le cas d’iOS AccessEnabler, l’utilisateur accède à la page de connexion du MVPD pour saisir ses informations d’identification via le support de votre contrôleur `UIWebView/WKWebView or SFSafariViewController `d’application. Notez que plusieurs opérations de redirection se produisent pendant ce transfert et votre application doit surveiller les URL chargées par le contrôleur lors des opérations de redirection multiples.
1. Dans le cas d’iOS AccessEnabler, lorsque le contrôleur `UIWebView/WKWebView or SFSafariViewController` charge une URL personnalisée spécifique, votre application doit fermer le contrôleur et appeler la méthode d’API `handleExternalURL:url ` d’AccessEnabler. Notez que cette URL personnalisée spécifique n’est en fait pas valide et qu’elle n’est pas destinée à être réellement chargée par le contrôleur. Elle ne doit être interprétée que par votre application comme un signal que le flux d’authentification est terminé et qu’il est sûr de fermer le contrôleur `UIWebView/WKWebView or SFSafariViewController`. Si votre application est requise pour utiliser un contrôleur `SFSafariViewController `, l’URL personnalisée spécifique est définie par le `application's custom scheme` (par exemple : `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), sinon cette URL personnalisée spécifique est définie par la constante `ADOBEPASS_REDIRECT_URL` (c’est-à-dire `adobepass://ios.app`).
1. Une fois que votre application ferme le contrôleur `UIWebView/WKWebView or SFSafariViewController` et appelle la méthode `handleExternalURL:url `API d’AccessEnabler, AccessEnabler récupère le jeton d’authentification du serveur principal et informe votre application que le flux d’authentification est terminé. AccessEnabler appelle le rappel [`setAuthenticationStatus()`](#setAuthNStatus) avec un code d’état de 1, indiquant la réussite. En cas d’erreur lors de l’exécution de ces étapes, le rappel [`setAuthenticationStatus()`](#setAuthNStatus) est déclenché avec un code d’état 0, indiquant l’échec de l’authentification, ainsi qu’un code d’erreur correspondant.


>[!WARNING]
>
> Au cours des étapes au cours desquelles AccessEnabler abandonne le contrôle sur votre application (c’est-à-dire lorsque la boîte de dialogue de sélection du fournisseur s’affiche, ou lorsque UIWebView/WKWebView ou SFSafariViewController s’affiche), l’utilisateur a la possibilité d’annuler le flux d’authentification. Dans ce cas, votre application est chargée d’informer AccessEnabler de cet événement et d’appeler la méthode d’API [`setSelectedProvider()`](#setSelProv), en transmettant null en tant que paramètre. Cela permet à AccessEnabler de nettoyer son état interne et de réinitialiser le flux d’authentification. Sous tvOS, vous pouvez utiliser la même méthode pour annuler l’interrogation d’authentification.


### Processus de déconnexion {#logout}

Pour les clients natifs, la déconnexion est gérée de la même manière que le processus d’authentification décrit ci-dessus.

1. Votre application lance le workflow de déconnexion avec un appel à la méthode `logout() `API d’AccessEnabler. La déconnexion est le résultat d’une série d’opérations de redirection HTTP en raison du fait que l’utilisateur doit être déconnecté des serveurs d’authentification Adobe Pass et des serveurs du MVPD. Comme ce flux ne peut pas être terminé avec une requête HTTP simple émise par la bibliothèque AccessEnabler, un contrôleur `UIWebView/WKWebView or SFSafariViewController` doit être instancié pour pouvoir suivre les opérations de redirection HTTP.

1. Un modèle similaire au flux d’authentification est utilisé. IOS AccessEnabler déclenche le rappel `navigateToUrl:` ou `navigateToUrl:useSVC:` pour créer un contrôleur `UIWebView/WKWebView or SFSafariViewController` et charger l’URL fournie dans le paramètre `url` du rappel. Il s’agit de l’URL du point de terminaison de la déconnexion sur le serveur principal. Pour tvOS AccessEnabler, ni le rappel `navigateToUrl:` ni le rappel `navigateToUrl:useSVC:` ne sont appelés.

1. Au fur et à mesure qu&#39;elle passe par plusieurs redirections, votre application doit surveiller l&#39;activité du contrôleur `UIWebView/WKWebView or SFSafariViewController `et détecter le moment où elle charge une URL personnalisée spécifique. Notez que cette URL personnalisée spécifique n’est en fait pas valide et qu’elle n’est pas destinée à être réellement chargée par le contrôleur. Elle ne doit être interprétée que par votre application comme un signal indiquant que le flux de déconnexion est terminé et qu’il est sûr de fermer le contrôleur. Lorsque le contrôleur charge cette URL personnalisée spécifique, votre application doit fermer le contrôleur et appeler la méthode d’API `handleExternalURL:url ` d’AccessEnabler. Si votre application est requise pour utiliser un contrôleur `SFSafariViewController `, l’URL personnalisée spécifique est définie par le `application's custom scheme` (par exemple, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), sinon cette URL personnalisée spécifique est définie par la constante ` ADOBEPASS_REDIRECT_URL  `c’est-à-dire `adobepass://ios.app`).

1. En fin de compte, AccessEnabler appelle le rappel [`setAuthenticationStatus()`](#setAuthNStatus) avec un code d’état de 0, ce qui indique la réussite du flux de déconnexion.

Le flux de déconnexion diffère du flux d’authentification dans la mesure où l’utilisateur n’est pas tenu d’interagir de quelque manière que ce soit avec le contrôleur `UIWebView/WKWebView or SFSafariViewController`. Par conséquent, Adobe recommande de rendre le contrôle invisible (c’est-à-dire masqué) pendant le processus de déconnexion.

## Jetons {#tokens}

- [Définitions et utilisation](#definitions)
- [Instructions de mise en cache](#caching)
- [Persistance](#persistence)
- [Format](#format)
- [Liaison de périphérique](#device_binding)


### Définitions et utilisation {#definitions}

La solution de droit d’authentification Adobe Pass tourne autour de la génération de données (jetons) spécifiques générées par l’authentification Adobe Pass une fois les workflows d’authentification et d’autorisation terminés avec succès. Ces jetons sont stockés localement sur le périphérique iOS du client.



La durée de vie des jetons est limitée. Lors de l’expiration, les jetons doivent être réémis par le réinitialisation des workflows d’authentification et/ou d’autorisation.



Il existe trois types de jetons émis lors des workflows de droits :

- **Jeton d’authentification :** Le résultat final du workflow d’authentification de l’utilisateur sera un GUID d’authentification que AccessEnabler peut utiliser pour effectuer des requêtes d’autorisation pour le compte de l’utilisateur. Ce GUID d’authentification aura une valeur TTL (durée de vie) associée qui peut être différente de la session d’authentification de l’utilisateur. Un jeton d’authentification est généré en liant le GUID d’authentification au périphérique qui lance les demandes d’authentification.
- **Jeton d’autorisation :** accorde l’accès à une ressource protégée spécifique identifiée par un identifiant de ressource unique. Il se compose d’un octroi d’autorisation émis par la personne qui l’a autorisé, ainsi que de l’ID de ressource d’origine. Ces informations sont liées au périphérique qui lance la requête.
- **Jeton multimédia de courte durée :** AccessEnabler accorde l’accès à l’application d’hébergement pour une ressource donnée en renvoyant un jeton multimédia de courte durée. Ce jeton est généré en fonction du jeton d’autorisation précédemment acquis pour cette ressource spécifique. En outre, ce jeton n’est pas lié à l’appareil et la durée de vie associée est beaucoup plus courte (par défaut : 5 minutes).

Une fois l’authentification et l’autorisation réussies, l’authentification Adobe Pass émet des jetons d’authentification, d’autorisation et de média de courte durée. Ces jetons doivent être mis en cache sur l’appareil de l’utilisateur et utilisés pendant la durée de vie associée.



### Instructions de mise en cache {#caching}

- Jeton d’authentification
- Jeton d’autorisation
- Jeton de média de courte durée


#### Jeton d’authentification

- **AccessEnabler 1.7:** Ce SDK introduit une nouvelle méthode de stockage de jetons, qui permet plusieurs compartiments Programmer-MVPD et, par conséquent, plusieurs jetons d’authentification. Désormais, la même disposition de stockage est utilisée à la fois pour le scénario &quot;Authentification par demandeur&quot; et pour le flux d’authentification normal. La seule différence entre les deux est la manière dont l’authentification est effectuée : &quot;Authentification par demandeur&quot; contient une nouvelle amélioration (Authentification passive) qui permet à AccessEnabler d’effectuer l’authentification back-channel, en fonction de l’existence d’un jeton d’authentification dans le stockage (pour un autre programmeur). L’utilisateur ne doit s’authentifier qu’une seule fois et cette session sera utilisée pour obtenir des jetons d’authentification dans d’autres applications. Ce flux back-channel a lieu pendant l’appel [`setRequestor()`](#setReq) et est principalement transparent pour le programmeur. **Il existe toutefois une exigence importante ici : le programmeur DOIT appeler setRequestor() à partir du thread d&#39;interface utilisateur principal.**
- **AccessEnabler 1.6 et versions antérieures :** La manière dont les jetons d’authentification sont mis en cache sur l’appareil dépend de l’indicateur &quot;**Authentication per Requestor&quot;** associé au MVPD actuel :

<!-- end list -->

1. Si la fonction &quot;Authentification par demandeur&quot; est désactivée, un jeton d’authentification unique est stocké localement dans le tableau de bord global ; ce jeton sera partagé entre toutes les applications intégrées au MVPD actuel.
1. Si la fonction &quot;Authentification par demandeur&quot; est activée, un jeton est explicitement associé au programmeur qui a exécuté le flux d’authentification (le jeton ne sera pas stocké dans le tableau de bord global, mais dans un fichier privé visible uniquement par l’application de ce programmeur). Plus précisément, l’authentification unique (SSO) entre différentes applications sera désactivée ; l’utilisateur devra exécuter explicitement le flux d’authentification lors du passage à une nouvelle application (en indiquant que le programmeur de la deuxième application est intégré au MVPD actuel et qu’il n’existe aucun jeton d’authentification pour ce programmeur dans le cache local).



#### Jeton d’autorisation

A tout moment, seul un jeton d’autorisation PAR RESSOURCE est mis en cache par AccessEnabler. Plusieurs jetons d’autorisation peuvent être mis en cache, mais ils sont associés à différentes ressources. Chaque fois qu’un nouveau jeton d’autorisation est émis et qu’un ancien existe déjà pour *la même ressource*, le nouveau jeton remplace la valeur mise en cache existante.



#### Jeton de média

Le jeton multimédia de courte durée ne doit PAS être mis en cache du tout. Le jeton multimédia doit être récupéré à partir du serveur chaque fois qu’une API d’autorisation est appelée, car il est limité à une utilisation unique.



### Persistance {#persistence}

Les jetons doivent être persistants sur plusieurs exécutions consécutives de la même application. Cela signifie qu’une fois que les jetons d’authentification et d’autorisation ont été acquis et que l’utilisateur ferme l’application, les mêmes jetons sont disponibles pour l’application lorsque l’utilisateur rouvre l’application. En outre, il est souhaitable que ces jetons soient persistants dans plusieurs applications. En d’autres termes, lorsqu’un utilisateur utilise une application pour se connecter à un fournisseur d’identité spécifique (obtention réussie des jetons d’authentification et d’autorisation), les mêmes jetons peuvent être utilisés par le biais d’une autre application et l’utilisateur n’est plus invité à saisir les informations d’identification lors de sa connexion via le même fournisseur d’identité. Ce type de workflow d’authentification/d’autorisation transparent fait de la solution d’authentification Adobe Pass une solution TV-partout
implémentation.



## iOS

La bibliothèque iOS AccessEnabler traite des problèmes de partage de données entre applications en stockant les données de jeton dans une structure de données &quot;de type Presse-papiers&quot; appelée la *table de collage*. Cette ressource partagée au niveau du système fournit les ingrédients clés qui permettent la mise en oeuvre du cas d’utilisation de jetons persistants souhaité :

- **Prise en charge de l’espace de stockage structuré** - La carte de collage n’est pas simplement une structure de mémoire simple et linéaire de type mémoire tampon. Il fournit un mécanisme de stockage de type dictionnaire qui permet l’indexation des données en fonction des valeurs de clé spécifiées par l’utilisateur.
- **Prise en charge de la persistance des données à l’aide du système de fichiers sous-jacent** - Le contenu de la structure de la carte de collage peut être marqué comme persistant, auquel cas les données sont enregistrées dans la mémoire interne de l’appareil.



Une fois qu’un jeton particulier est placé dans le cache de jetons, sa validité est vérifiée à différents moments par la bibliothèque AccessEnabler.  Un jeton valide est défini comme suit :

- Le délai d’activation du jeton n’a pas expiré.
- L’émetteur du jeton est inclus dans la liste des fournisseurs d’identité autorisés.



## tvOS

Comme le tableau de bord n’est pas disponible sur tvOS, la bibliothèque tvOS AccessEnabler utilise NsUserDefaults comme option de stockage. Cela résout le problème de la persistance des jetons d’authentification et d’autorisation, mais les informations stockées ne peuvent pas être partagées entre différentes applications.



**Modifications du tableau de bord iOS 10 -** Lors de la mise à niveau à partir d’une version précédente d’iOS, le tableau de bord est effacé. Cela signifie que toutes les applications doivent être réauthentifiées.



**Modifications du tableau de bord iOS 7 -** En raison de changements dans le fonctionnement des tableaux de bord sur iOS 7, il y aura un SSO croisé limité entre les applications s’exécutant sur iOS 7. Les applications ayant le même `<Bundle Seed ID>` (également appelé `<Team ID>`) partageront des jetons, ce qui signifie que les applications A1 et A2 du même programmeur X partageront des jetons, tandis que l’application A1 (Programmeur X) et l’application A3 (Programmeur Y) ne partageront pas de jetons.

- Bundle Seed ID/Team ID est le même entre 2 applications si elles sont générées par le même profil de mise en service. Pour plus d’informations, cliquez sur ce lien :
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- Cette limitation &quot;Cross SSO&quot; sera présente dans iOS 7, quel que soit le SDK d’authentification Adobe Pass utilisé.

Veuillez lire cette note technique pour plus d’informations sur la configuration de la connexion unique sur iOS 7 et versions ultérieures (la note technique s’applique à Access Enabler v1.8 et versions ultérieures) : <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### Stockage des jetons (AccessEnabler 1.7)

À partir de la version 1.7 d’AccessEnabler, le stockage des jetons peut prendre en charge plusieurs combinaisons Programmer-MVPD, en s’appuyant sur une structure de mappage imbriquée à plusieurs niveaux pouvant contenir plusieurs jetons d’authentification. Ce nouveau stockage n’affecte en aucune manière l’API publique AccessEnabler et ne nécessite aucune modification du côté du programmeur. Voici un exemple :
illustre cette nouvelle fonctionnalité :

1. Ouvrez App1 (développé par Programmer1).
1. Authentifiez-vous avec MVPD1 (qui est intégré à Programmer1).
1. Suspendre/Fermer l’application actuelle et ouvrir App2 (développé par Programmer2).
1. Supposons que Programmer2 ne soit pas intégré à MVPD2 ; par conséquent, l’utilisateur ne sera PAS authentifié dans App2.
1. Authentifiez-vous avec MVPD2 (intégré à Programmer2) dans App2.
1. Revenez à App1 ; l’utilisateur sera toujours authentifié avec Programmer1.

Dans les anciennes versions d’AccessEnabler, l’étape 6 affichait l’utilisateur comme non authentifié, car le stockage des jetons ne prenait auparavant en charge qu’un seul jeton d’authentification.



La déconnexion d’une session Programmer/MVPD effacera l’ensemble du stockage sous-jacent, y compris tous les autres jetons d’authentification Programmer/MVPD sur l’appareil. D’un autre côté, l’annulation du flux d’authentification (en appelant [`setSelectedProvider(null)`](#setSelProv)) N’effacera PAS le stockage sous-jacent, mais cela n’affectera que la tentative d’authentification actuelle du programmeur/MVPD (en effaçant le MVPD pour le programmeur actuel).



### Importateur de jetons (AccessEnabler 1.7)

Une autre fonctionnalité de stockage incluse dans AccessEnabler 1.7 permet d’importer des jetons d’authentification à partir d’anciennes zones de stockage. Cet &quot;Importateur de jetons&quot; permet d’assurer la compatibilité entre les versions consécutives d’AccessEnabler, en conservant l’état SSO même lorsque la version de stockage est mise à niveau. L’importateur est exécuté pendant le flux [`setRequestor()`](#setReq) et s’exécute dans les deux scénarios suivants (en supposant qu’aucun jeton d’authentification valide pour le programmeur actuel ne soit présent dans le stockage actuel) :

- La première installation d’une application 1.7 développée par un programmeur spécifique
- Chemin de mise à niveau vers un futur AccessEnabler qui utilise un nouveau stockage

L’opération d’importation est transparente pour le programmeur et ne nécessite aucun changement de code dans l’application cliente.



### Correcteur de jeton (AccessEnabler 1.7.5)

Depuis AccessEnabler 1.7.5, ce service s’exécute sur [`setRequestor()`](#setReq)`. `Il a été développé à la suite du basculement d’iOS 7 de l’adresse Wi-Fi MAC vers l’IDFA pour le suivi. L’analyseur assure que le stockage actuel contient uniquement des jetons d’authentification valides (valides en termes d’identifiant de l’appareil, anciennement calculé à l’aide de l’adresse MAC, avant iOS7). Le programme Token Sanitizer supprime tous les jetons non valides.



Le service Token Sanitizer est particulièrement utile lorsqu’une application AccessEnabler 1.7.5 est utilisée sur iOS 6, puis que l’utilisateur effectue la mise à jour vers iOS 7. Dans ce cas, tous les jetons d’authentification obtenus sur iOS 6 seront désormais non valides (en raison du changement de l’algorithme d’identifiant de périphérique de l’utilisation de l’adresse MAC à l’IDFA). L’analyseur efface tous les jetons non valides et l’utilisateur n’est alors pas authentifié.



Si l’outil de nettoyage des jetons ne supprimait pas les jetons non valides, AccessEnabler ne parvenait pas à obtenir les autorisations en raison de la présence de jetons d’authentification non valides. Cela s’avérerait difficile à déboguer pour l’utilisateur final, car les messages d’erreur d’autorisation peuvent être énigmatiques et ne sont pas trop utiles pour déterminer la cause du problème.



### Format {#format}

- [Jeton AuthN](#authn_token)
- [Jeton AuthZ](#authz_token)
- [Jeton de média court](#short_token)
- [Liaison de périphérique](#device_binding)


Notez que le format des jetons AuthN et AuthZ est inclus ici uniquement pour des informations de fond. La structure de ces jetons peut être modifiée par l’authentification Adobe Pass à tout moment. Les programmeurs n’ont pas besoin de connaître la structure exacte des jetons AuthN et AuthZ pour mettre en oeuvre leurs applications, car les jetons AuthN et AuthZ ne sont pas exposés sur l’appareil local. Le jeton de média court *est* exposé à l’application du programmeur.



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


#### Jeton de média court {#short_token}

La liste ci-dessous présente le format du jeton multimédia court. Ce jeton est exposé à l’application du programmeur. Il est transmis à l’application du programmeur à la fin d’un processus de droit réussi :

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


### Liaison de périphérique {#device_binding}

Dans les listes XML ci-dessus, notez la balise intitulée `simpleTokenFingerprint`. L’objectif de cette balise est de contenir des informations d’individualisation des identifiants d’appareil natifs. La bibliothèque AccessEnabler peut obtenir ces informations d’individualisation et les mettre à la disposition des services d’authentification Adobe Pass lors des appels de droits. Le service utilisera ces informations et les incorporera dans les jetons réels, liant ainsi efficacement les jetons à un appareil spécifique. L’objectif final est de rendre les jetons non transférables sur plusieurs appareils.



Puisqu’il s’agit évidemment d’une fonctionnalité liée à la sécurité, ces informations sont intrinsèquement &quot;sensibles&quot; du point de vue de la sécurité. Par conséquent, ces informations doivent être protégées contre l’altération et l’écoutes. Le problème d’écoute est résolu en envoyant les demandes d’authentification/d’autorisation via le protocole HTTPS. La protection de l’altération est gérée par la signature numérique des informations d’identification de l’appareil. La bibliothèque AccessEnabler calcule un ID d’appareil à partir des informations fournies par l’appareil, puis envoie l’ID d’appareil &quot;en clair&quot; aux serveurs d’authentification Adobe Pass en tant que paramètre de requête. Les serveurs d’authentification Adobe Pass signent numériquement l’identifiant de l’appareil avec la clé privée de l’Adobe et l’ajoutent au jeton d’authentification renvoyé à AccessEnabler. Par conséquent, l’identifiant de l’appareil est lié au jeton d’authentification. Pendant le flux d’autorisation, AccessEnabler envoie à nouveau l’identifiant de l’appareil en clair, avec le jeton d’authentification. L’échec du processus de validation entraînera automatiquement l’échec des workflows d’authentification/d’autorisation. Les serveurs d’authentification Adobe Pass appliquent la clé privée à l’identifiant de l’appareil et la comparent à la valeur du jeton d’authentification. S’ils ne correspondent pas, ce flux de droits échoue.



**Remarque sur la liaison de périphériques dans AccessEnabler 1.7.5 :** À partir de la version 1.7.5 d’AccessEnabler, le mode de calcul de l’identifiant d’appareil change afin de fournir des informations d’individualisation à un appareil iOS. Cette modification reflète un changement dans iOS 7 : depuis iOS 7, Apple ne fournit plus l’adresse MAC WiFi comme option de suivi, au profit de son identifiant pour les annonceurs (IDFA). Étant donné que les informations d’individualisation d’une application s’exécutant sur iOS 7 sont basées sur l’IDFA et que ces informations sont incorporées dans les jetons de flux de droits, cela signifie qu’il existe un certain nombre d’effets possibles sur l’expérience utilisateur résultant de cette modification. Les différents effets possibles sont basés sur la version d’iOS mise à niveau de l’utilisateur associée à la version d’AccessEnabler à partir de laquelle le programmeur effectue une mise à niveau. Pour plus d’informations sur cette modification, voir les Notes de mise à jour incluses dans le SDK AccessEnabler 1.7.5.

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
