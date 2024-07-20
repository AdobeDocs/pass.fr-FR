---
title: Présentation pour les programmeurs
description: Présentation pour les programmeurs
exl-id: 64a12e49-0ecb-4b81-977d-60c10925bb59
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '4274'
ht-degree: 0%

---

# Présentation Des Programmeurs {#programmers-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#introduction}

Cet aperçu est destiné au fournisseur de contenu (Programmeur) qui prévoit d’intégrer Adobe® Pass dans son site web ou son application. Pour obtenir de la documentation supplémentaire, notamment des guides de démarrage et d’intégration, voir Informations connexes ci-dessous.

Aujourd&#39;hui, vos téléspectateurs peuvent accéder à Internet à tout moment, n&#39;importe où, et demander l&#39;accès au contenu protégé directement à vous, le programmeur. Il peut souhaiter regarder un événement ponctuel ou rechercher des droits d’affichage pour une série télévisée entière que vous diffusez.

Mais avant d’autoriser l’accès à votre contenu protégé, vous devez déterminer si un client est autorisé à afficher ce contenu. Ont-ils un abonnement avec un distributeur de programmes vidéo multicanaux (MVPD) ? Si tel est le cas, cet abonnement inclut-il votre programmation ?

Déterminer les droits d’un observateur n’est pas toujours simple pour un programmeur. Les MVPD détiennent les données d’identification et les privilèges d’accès pour leurs clients.  Ajoutez le fait que les visionneuses qui tentent d’accéder à votre contenu protégé s’abonnent à divers MVPD, chacun disposant de différents systèmes, et il est facile de voir que déterminer le droit de la visionneuse à un contenu protégé peut rapidement devenir compliqué et techniquement difficile :

![](assets/user-ent-by-progr.png)

*Figure : Droit De L’Utilisateur Déterminé Directement Par Le Programmeur*

L’authentification Adobe Pass pour TV partout permet d’arbitrer en toute sécurité ces transactions de droits entre les programmeurs et les MVPD. L’authentification Adobe Pass permet aux programmeurs de fournir facilement, rapidement et en toute sécurité du contenu protégé à des clients valides :

![](assets/user-ent-mediatedby-authn.png)

*Figure : Droits de l’utilisateur médiatisés par l’authentification Adobe Pass*

L’authentification Adobe Pass agit comme votre proxy dans exchange avec les MVPD participants, de sorte que vous pouvez présenter vos visionneuses avec une interface intersite cohérente. L’authentification Adobe Pass vous permet également de fournir aux visionneuses l’authentification et l’autorisation de connexion unique (SSO). L’authentification et l’autorisation sont suivies pour tous les services participants, de sorte qu’un abonné n’ait pas à se reconnecter après sa première authentification sur son propre système.

* **Authentification** - Processus de confirmation avec un MVPD qu’un utilisateur donné est un client connu.
* **Authorization** - Processus de confirmation avec un MVPD qu’un utilisateur authentifié possède un abonnement valide à une ressource spécifiée.

### Fonctionnement de l’authentification Adobe Pass {#HowItWorks}

L’application de visionnage de contenu du programmeur interagit avec l’authentification Adobe Pass à l’aide du composant client Access Enabler ou des services web RESTful de l’API sans client (pour les appareils non compatibles web tels que les téléviseurs intelligents, les consoles de jeux, les décodeurs, etc.). L’activateur d’accès s’exécute sur le système de l’utilisateur, où il facilite tous les workflows de droits.  Le composant Accéder à l’activateur est téléchargé depuis son site d’hébergement à l’Adobe lorsque les clients accèdent à votre site et demandent du contenu protégé.  Les serveurs d’authentification Adobe Pass hébergent les services web RESTful utilisés dans la solution sans client.

L’authentification Adobe Pass gère les processus de droits réels tout en fournissant des primitives que vous utilisez pour :

* Définissez votre identité. (Le programmeur est le &quot;demandeur&quot; dans le flux de droits d’authentification Adobe Pass.)
* Authentifiez un utilisateur avec un MVPD particulier.  (Le MVPD est le &quot;fournisseur d’identité&quot; ou &quot;IdP&quot; dans le flux de droits d’authentification Adobe Pass.)
* Autoriser un utilisateur avec le MVPD pour une ressource particulière.
* Déconnectez-vous de l’utilisateur.

Le programmeur est responsable de sa page web ou de son application de lecteur de niveau supérieur qui effectue les opérations suivantes :

* Mise en oeuvre de l’interface utilisateur
* Interagit avec les services Web Access Enabler ou API sans client

L’objectif de l’authentification Adobe Pass est de créer une méthode simple et modulaire pour les programmeurs et les MVPD afin de gérer la vérification des droits.

## Présentation des jetons {#understanding-tokens}

La solution de droits d’authentification Adobe Pass se concentre sur la génération de données spécifiques créées à la fin des workflows d’authentification et d’autorisation. Ces données sont appelées jetons. Les jetons ont une durée de vie limitée. Lorsqu’ils expirent, les jetons doivent être réémis par le réinitialisation des workflows d’authentification et d’autorisation.

Pour plus d’informations sur les jetons, reportez-vous aux sections suivantes :

* [Types de jetons](#token-types)
* [Stockage des jetons](#token-storage)
* [Sécurité des jetons](#token-security)
* [Partage de jetons](#token-sharing)

### Types de jetons {#token-types}

Trois types de jetons sont émis lors des workflows d’authentification et d’autorisation. Les jetons AuthN et AuthZ sont de &quot;longue durée&quot;, ce qui assure la continuité de l’expérience de visionnage de l’utilisateur. Le jeton multimédia est un jeton de courte durée qui fournit une assistance pour les bonnes pratiques du secteur afin de prévenir la fraude par le biais de l’extraction de flux. Les programmeurs spécifient les valeurs de durée de vie (TTL) pour chaque type de jeton en fonction des accords conclus avec les MVPD. Les programmeurs décident d’une valeur TTL qui convient le mieux à votre entreprise et à vos clients.

* **Jeton AuthN** (&quot;De longue durée&quot;) : Une fois l’authentification réussie, l’authentification Adobe Pass crée un jeton AuthN associé à l’appareil demandeur et à un identifiant unique global (GUID).
   * L’authentification Adobe Pass envoie le jeton AuthN à l’activateur d’accès, qui le met en cache de manière sécurisée sur le système du client.  Bien que le jeton AuthN soit présent et non expiré, il est disponible pour toutes les applications qui utilisent l’authentification Adobe Pass. L’activateur d’accès utilise le jeton AuthN pour le flux d’autorisation.
   * À tout moment, un seul jeton AuthN est mis en cache. Chaque fois qu’un nouveau jeton AuthN est émis et qu’un ancien existe déjà, l’authentification Adobe Pass remplace le jeton mis en cache.
* **Jeton AuthZ** (&quot;De longue durée&quot;) : Une fois l’autorisation effectuée, l’authentification Adobe Pass crée un jeton AuthZ associé à l’appareil demandeur et à une ressource protégée spécifique.  La ressource protégée est identifiée par un identifiant de ressource unique.
   * L’authentification Adobe Pass envoie le jeton AuthZ à l’activateur d’accès, qui le met en cache de manière sécurisée sur le système local. L’activateur d’accès utilise ensuite le jeton AuthZ pour créer le jeton de média de courte durée utilisé pour l’accès réel à l’affichage.
   * A tout moment, un seul jeton AuthZ par ressource est mis en cache. L’authentification Adobe Pass peut mettre en cache plusieurs jetons AuthZ, à condition qu’ils soient associés à différentes ressources. Chaque fois qu’un nouveau jeton AuthZ est émis et qu’un ancien existe déjà pour la même ressource, l’authentification Adobe Pass remplace le jeton mis en cache.
* **Jeton multimédia** (&quot;De courte durée&quot;) : l’activateur d’accès utilise le jeton AuthZ pour générer un jeton multimédia de courte durée (par défaut : 7 minutes). Il s’agit du moment où une requête de lecture réussie est considérée comme ayant eu lieu.
   * Avant de permettre l’accès à la ressource protégée, votre serveur multimédia doit utiliser un composant d’authentification Adobe Pass, le Vérificateur de jeton multimédia, pour valider le jeton multimédia.
   * Comme le jeton multimédia n’est pas lié à l’appareil, sa durée de vie est nettement plus courte (par défaut : 7 minutes) que celle des jetons AuthN et AuthZ de longue durée.
   * Le jeton multimédia de courte durée est limité à une utilisation unique et n’est jamais mis en cache. Elle est récupérée à partir du serveur d’authentification Adobe Pass chaque fois qu’une API d’autorisation est appelée.

### Stockage des jetons {#token-storage}

Access Enabler stocke les jetons de longue durée (AuthN et AuthZ) dans des emplacements spécifiques à son environnement :

* **Flash 10.1** (ou supérieur) : les jetons de longue durée sont stockés en tant qu’objets partagés locaux.
* **HTML5** : les jetons de longue durée sont conservés en toute sécurité dans la boutique locale du navigateur HTML5.
* **iOS** : les jetons de longue durée sont stockés sur un tableau de bord persistant, où ils sont accessibles par d’autres applications clientes d’authentification Adobe Pass.
* **Android** : les jetons de longue durée sont stockés dans un fichier de base de données partagé, où ils sont accessibles par d’autres applications clientes d’authentification Adobe Pass.
* **Appareils API sans client** : les jetons sont stockés sur les serveurs d’authentification Adobe Pass.

### Sécurité des jetons {#token-security}

Le serveur d’authentification Adobe Pass signe numériquement tous les jetons de longue durée à l’aide de l’identifiant d’appareil (dérivé des caractéristiques matérielles de l’appareil). La signature numérique diffère de la manière dont elle est générée, protégée et validée selon l’environnement :

* **Flash 10.1** (ou supérieur) - L’identifiant de l’appareil repose sur les informations d’identification de l’appareil, un certificat unique émis par le serveur d’individualisation de l’Adobe. Cette sécurité équivaut à la technologie DRM de FAXS. Cette validation côté serveur compare l’identifiant unique de l’appareil dans le jeton avec les informations d’identification de l’appareil (qui sont communiquées en toute sécurité du Flash Player à l’authentification Adobe Pass). Les informations d’identification de l’appareil identifient également la version du client FAXS et la version de Flash Player (ou AIR) à laquelle il a été émis. La liaison de l’appareil est plus forte qu’avec HTML5. Par conséquent, la durée de vie des jetons est généralement plus longue avec Flash.
* **HTML5** - L’appareil est personnalisé côté client. Il utilise les caractéristiques disponibles par le biais de JavaScript pour produire un identifiant pseudo-périphérique qui inclut les versions du navigateur et du système d’exploitation, une adresse IP et un GUID de cookie du navigateur (identifiant unique global). Cet identifiant d’appareil de jeton est comparé à l’identifiant pseudo-appareil actuel de l’appareil. L’adresse IP pouvant changer lors d’une utilisation normale, même au cours d’une même session, l’authentification Adobe Pass stocke les jetons HTML5 à deux emplacements : localStorage et sessionStorage. Si l’IP change et que le jeton sessionStorage est toujours valide, la session est conservée. Avec HTML5, la liaison de l’appareil n’est pas aussi forte. Par conséquent, la durée de vie des jetons est généralement plus courte que celle des Flashs.
* **Clients natifs** (iOS et Android) - Les jetons de longue durée contiennent des informations d’individualisation des identifiants d’appareil natifs et sont donc liés à l’appareil demandeur. Les demandes d’authentification et d’autorisation sont envoyées via HTTPS et les informations d’identification de l’appareil sont numériquement signées par la bibliothèque Access Enabler avant de les envoyer aux serveurs principaux. Côté serveur, les informations d’identifiant de l’appareil sont validées par rapport à sa signature numérique associée.
* **Clients API sans client** - La solution API sans client possède un ensemble de protocoles de sécurité qui impliquent la signature numérique de tous les appels API. Les jetons générés pendant les flux de droits sont stockés de manière sécurisée sur les serveurs d’authentification Adobe Pass.

L’authentification Adobe Pass valide chaque jeton de longue durée afin de s’assurer que l’appareil accédant au contenu est identique à celui qui a émis le jeton. Pour tous les jetons, une validation côté client garantit que la signature numérique est intacte et que l’intégrité du jeton est préservée. Lorsque la validation de l’identifiant d’appareil échoue, la session d’authentification est invalidée et l’utilisateur est invité à se reconnecter, ce qui réinitialise les jetons.

### Partage de jetons {#token-sharing}

Les applications sur différentes plateformes ne partagent pas de jetons. Il existe plusieurs raisons à cela, notamment :

* Comme décrit dans [Stockage de jeton](#token-storage), la méthode de stockage des jetons varie selon les plateformes (par exemple, Objets partagés locaux pour le Flash, Stockage WebStorage pour JavaScript).
* Le degré de sécurité des jetons varie d’une plate-forme à l’autre. Par exemple, les jetons de Flash sont fortement liés à un appareil à l’aide de FAXS. Dans un environnement JavaScript pur, les jetons ne bénéficient pas du même niveau de prise en charge DRM qu’en Flash.  Le partage de jetons JS avec des applications de Flash augmenterait la possibilité que des jetons moins sécurisés exploitent un environnement plus sécurisé.

## Cycle de vie de l’intégration de programmeur {#prog-integ-lifecycle}

![](assets/progr-flow-int-lifecycle.png)

*Figure : Intégration de l’authentification au site web et à l’application du programmeur*


## Flux logiques {#logical-flows}

### Organigramme des droits {#chart}

L’organigramme suivant présente le processus global de confirmation des droits (à l’aide du composant client Adobe Pass Authentication Access Enabler) :

![](assets/ent-flowchart.png)

*Figure : Processus de confirmation des droits*

### Étapes d’authentification {#authn-steps}

Les étapes suivantes présentent un exemple du flux d’authentification de l’authentification Adobe Pass.  Il s’agit de la partie du processus de droit dans laquelle un programmeur détermine si l’utilisateur est un client valide d’un MVPD.  Dans ce scénario, l’utilisateur est un abonné valide à un MVPD.  L’utilisateur tente d’afficher du contenu protégé à l’aide d’une application de Flash du programmeur :

1. L’utilisateur accède à la page web du programmeur, qui charge l’application de Flash du programmeur et les composants Adobe Pass Authentication Access Enabler sur la machine de l’utilisateur. L’application de Flash utilise Access Enabler pour définir l’identification du programmeur avec l’authentification Adobe Pass, et l’authentification Adobe Pass prime l’activation d’accès avec la configuration et les données d’état pour ce programmeur (le &quot;demandeur&quot;). L’activateur d’accès doit recevoir ces données du serveur avant d’effectuer tout autre appel API. Remarque technique : le programmeur a défini son identité avec la méthode `setRequestor()` d’Access Enabler ; pour plus d’informations, consultez le [Guide d’intégration du programmeur](/help/authentication/programmer-integration-guide-overview.md).
1. Lorsque l’utilisateur tente d’afficher le contenu protégé du programmeur, l’application du programmeur présente à l’utilisateur une liste de MVPD, à partir de laquelle l’utilisateur sélectionne un fournisseur.
1. L’utilisateur est redirigé vers un serveur d’authentification Adobe Pass, où est créée une requête [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) chiffrée pour le MVPD sélectionné par l’utilisateur. Cette requête est envoyée en tant que requête d’authentification au nom du programmeur au MVPD. Selon le système du MVPD, le navigateur de l’utilisateur est alors redirigé vers le site du MVPD pour se connecter ou un iFrame de connexion est créé dans l’application du programmeur.
1. Dans les deux cas (redirection ou iFrame), le MVPD accepte la demande et affiche sa page de connexion.
1. L’utilisateur se connecte avec le MVPD, le MVPD valide le statut de l’utilisateur en tant que client payant, puis le MVPD crée sa propre session HTTP.
1. Lorsque l’utilisateur est validé, le MVPD crée une réponse (SAML et cryptée), que le MVPD renvoie à l’authentification Adobe Pass.
1. L’authentification Adobe Pass reçoit la réponse MVPD, voit qu’une session HTTP d’authentification Adobe Pass est ouverte, valide la réponse SAML du MVPD et redirige vers le site du programmeur.
1. Le site du programmeur est rechargé, Access Enabler est rechargé et le programmeur appelle à nouveau setRequestor() .  Le deuxième appel à setRequestor() est nécessaire car la configuration actuelle a changé. Un indicateur s’affiche maintenant pour informer l’activateur d’accès qu’un jeton AuthN attend d’être généré sur le serveur.
1. Access Enabler constate qu’une authentification est en attente et demande le jeton au serveur d’authentification Adobe Pass. Le jeton est récupéré à partir du serveur en appelant les fonctionnalités DRM du Flash Player.
1. Le jeton AuthN est stocké dans le cache LSO du Flash Player du programmeur ; l’authentification est maintenant terminée et la session est détruite sur le serveur d’authentification Adobe Pass.

### Étapes d’autorisation {#authz-steps}

Les étapes suivantes continuent à partir des [étapes d’authentification](#authn-steps) :

1. Lorsque l’utilisateur tente d’accéder au contenu protégé du programmeur, l’application du programmeur commence par rechercher un jeton AuthN sur l’ordinateur ou l’appareil local de l’utilisateur.  Si ce jeton n’est pas présent, les [étapes d’authentification](#authn-steps) ci-dessus sont suivies.  Si le jeton AuthN est présent, le flux d’autorisation se poursuit avec l’application du programmeur initiant un appel à l’activateur d’accès avec une requête pour obtenir les droits d’affichage de l’utilisateur pour un élément spécifique de contenu protégé.
1. L’élément spécifique du contenu protégé est représenté par un &quot;identifiant de ressource&quot;.  Il peut s’agir d’une chaîne simple ou d’une structure plus complexe, mais dans tous les cas, la nature de l’identifiant de la ressource est convenue à l’avance entre le programmeur et le MVPD.  L’application du programmeur transmet l’identifiant de ressource à l’activateur d’accès.  Access Enabler recherche un jeton AuthZ sur l’ordinateur ou l’appareil local de l’utilisateur.  Si le jeton AuthZ n’est pas présent, Access Enabler transmet la demande au serveur d’authentification Adobe Pass principal.
1. Le serveur d’authentification Adobe Pass communique avec le point de terminaison d’autorisation des MVPD à l’aide de protocoles normalisés.  Si la réponse du MVPD indique que l’utilisateur est autorisé à afficher le contenu protégé, le serveur d’authentification Adobe Pass crée un jeton AuthZ et le transmet à l’Activateur d’accès, qui stocke le jeton AuthZ sur l’ordinateur de l’utilisateur.
1. Avec un jeton AuthZ stocké sur l’ordinateur ou l’appareil de l’utilisateur, l’application du programmeur appelle l’activateur d’accès pour obtenir un jeton multimédia à partir du serveur d’authentification Adobe Pass et fournit ce jeton à l’application du programmeur.
1. Enfin, l’application du programmeur utilise le composant de vérification de jeton multimédia pour confirmer que l’utilisateur approprié visualise le contenu approprié et qu’avec le jeton multimédia en place, l’utilisateur est autorisé à afficher le contenu protégé.


## Enregistrement et initialisation {#reg-and-init}

La première étape de l’utilisation de l’authentification Adobe Pass consiste à s’enregistrer auprès d’un Adobe ou d’un partenaire autorisé par l’authentification Adobe Pass.

Lorsque vous vous inscrivez, vous fournissez une liste des domaines à partir desquels vous allez communiquer avec l’authentification Adobe Pass. Par exemple, les domaines Turner Broadcasting System incluent tbs.com et tnt.tv en tant que domaines enregistrés pour l’authentification Adobe Pass. Chacun de ces sites de contenu a accès à l’authentification Adobe Pass et se voit attribuer son propre ID de demandeur (par exemple, &quot;SOC&quot; et &quot;TNT&quot;). Si vous décidez d’ajouter d’autres sites, vous devez informer l’Adobe des noms de domaine supplémentaires afin de les placer dans la liste des domaines autorisés et de leur attribuer des identifiants de demandeur supplémentaires.

>[!WARNING]
>
>Pendant que vous testez votre intégration, assurez-vous que votre serveur de test se trouve sur un domaine enregistré que vous avez l’intention d’utiliser en production. Les requêtes, même les requêtes de test, provenant de domaines non placés sur liste blanche sont ignorées. Par exemple, si vous avez demandé domain.com à être utilisé en production, assurez-vous de déployer votre intégration de test sous domain.com, test.domain.com et staging.domain.com.
>
>Les requêtes qui contiennent le nom d’utilisateur et / ou le mot de passe dans l’URL sont ignorées même si les domaines sont placés sur liste blanche. Exemple : `//username@registered-domain/`

L’identifiant du demandeur identifie de manière unique le client du programmeur dans toutes les communications avec le composant client Access Enabler de l’authentification Adobe Pass. Toutes les données statiques d’authentification Adobe Pass associées au programmeur sont associées à cet identifiant.

>[!TIP]
>
>Outre votre ID de demandeur, lorsque vous vous enregistrez, vous recevez également des URL fonctionnelles pour le composant client Access Enabler et le vérificateur de jeton multimédia.

## Étapes d’intégration {#integration-steps}

>[!TIP]
>
>Si vous utilisez le Open Source Media Framework d’Adobe (&quot;OSMF&quot;) pour le développement de votre lecteur multimédia, le moyen le plus rapide d’utiliser l’authentification Adobe Pass est d’intégrer le module OSMF *(obsolète)* dans le code de votre lecteur.
>
><!--For details, see [Adobe Pass Authentication Plugin For OSMF](https://tve.helpdocsonline.com/9-2-2) in the Programmer Integration Guide.-->

1. [Configuration du demandeur](#requestor)
1. [Gestion de l’authentification et de l’autorisation](#authn-authz)
1. [Prise en charge de la connexion unique](#ssl)

### 1. Configuration du demandeur {#requestor}

#### 1a. Enregistrement avec Adobe

La première étape consiste à vous enregistrer auprès d’un Adobe ou d’un partenaire autorisé pour l’authentification Adobe Pass.  Lorsque vous vous enregistrez, vous recevez un ou plusieurs identifiants uniques globaux (GUID). Chaque GUID émis est associé à un domaine à partir duquel l’accès est autorisé à l’authentification Adobe Pass. Vous transmettez un GUID (appelé ID du demandeur) pour le domaine demandeur afin d’enregistrer votre identité pour chaque session au cours de laquelle vous interagissez avec Access Enabler. Pour plus d’informations, voir [Enregistrement et initialisation](#reg-and-init) dans le guide d’intégration de programmeur.

#### 1b. Intégration de l’activateur d’accès initial

L’étape suivante consiste à intégrer Access Enabler à l’application ou à la page web existante de votre lecteur multimédia :

* Vous pouvez incorporer la version par Flash, `AccessEnabler.swf`, dans un lecteur vidéo basé sur un Flash, ou l’incorporer directement dans l’HTML de votre page web. Vous pouvez communiquer avec le SWF Access Enabler dans ActionScript ou dans JavaScript. L’API de base est ActionScript, mais si vous préférez travailler avec JavaScript, une bibliothèque wrapper complète peut être incluse sur vos pages.
* Pour les environnements hors Flash, vous pouvez :
   * Utilisez la version HTML5/JavaScript, AccessEnabler.js, et communiquez avec elle via l’API JavaScript.
   * Utilisation de l’extension native AIR pour l’authentification Adobe Pass pour combiner du code natif avec des classes d’ActionScript intégrées
   * Utilisez l’une des versions clientes natives de la bibliothèque Access Enabler (iOS ou Android).

### 2. Gestion de l’authentification et de l’autorisation {#authn-authz}

#### 2a. Communication avec Access Enabler

La communication entre Access Enabler et votre application web de page ou de lecteur est asynchrone. Votre application appelle les méthodes Access Enabler et Access Enabler répond par des rappels que vous enregistrez auprès de la bibliothèque Access Enabler.

* Lorsque votre application effectue une demande d’autorisation, l’Activateur d’accès lance automatiquement une demande d’authentification, si un jeton AuthN valide n’est pas déjà sur le système local. Lorsque l’authentification réussit, le jeton AuthN de l’utilisateur est stocké localement, de sorte qu’il n’est pas nécessaire de se reconnecter. S’ils ont réussi à s’authentifier via l’authentification Adobe Pass dans un autre contexte (par exemple, par le biais du site web MVPD ou avec un autre programmeur), Access Enabler a accès au jeton AuthN local et aucune authentification supplémentaire n’est nécessaire.
* Lorsqu’un utilisateur tente d’accéder à votre contenu protégé, vous envoyez une demande d’autorisation à Access Enabler. Après avoir vérifié (ou initié) l’authentification, l’Access Enabler contacte le MVPD (via le serveur d’authentification Adobe Pass) pour déterminer si le client est autorisé à afficher le contenu protégé. Votre application ne doit envoyer la demande qu’à Access Enabler, puis gérer la réponse (réussite ou échec de l’autorisation). Si l’autorisation réussit, un jeton AuthZ est stocké sur le système client.  Enfin, votre application reçoit un jeton multimédia de courte durée à utiliser dans votre propre procédure d’autorisation.

>[!NOTE]
>
>* L’authentification se produit en tant qu’exchange SAML, entre l’authentification Adobe Pass en tant que fournisseur de service (SP) et le MVPD en tant que fournisseur d’identité (IdP).
>
>* L’autorisation utilise un exchange de service Web back-channel (serveur à serveur) entre l’authentification Adobe Pass (le SP) et un MVPD (l’IdP).


#### 2b. Fournir Une Interface Utilisateur De Droit {#entitlement-ui}

Vous fournissez votre propre interface utilisateur pour permettre à l’utilisateur d’accéder à votre contenu. Certains éléments, tels que le processus de connexion réel, sont fournis par le MVPD et certains éléments sont éventuellement disponibles dans le cadre de l’authentification Adobe Pass. Au minimum, vous procédez comme suit :

* **Mettez en oeuvre une interface de sélection MVPD qui permet à un nouvel utilisateur d’identifier son MVPD et de se connecter pour la première fois**. Pour le développement, Access Enabler fournit une interface utilisateur de base qui donne au client le choix des MVPD et lance le processus de connexion. Pour la production, vous devez mettre en oeuvre votre propre boîte de dialogue Sélecteur MVPD . Certains MVPD redirigent vers leur propre site pour se connecter et d’autres requièrent l’affichage de leurs pages de connexion dans un iFrame. Vous devez mettre en oeuvre un rappel qui crée cet iFrame pour gérer les cas où le MVPD de l’utilisateur affiche sa page de connexion dans un iFrame.
* **Identifiez le contenu protégé**. Le contenu protégé nécessite une autorisation d’accès. Votre interface doit indiquer le contenu protégé et le contenu autorisé.  L’état d’autorisation est souvent indiqué avec les icônes &quot;déverrouillé&quot; et &quot;verrouillé&quot;.
* **Afficher qu’un utilisateur est authentifié**. Vous devez indiquer l’état d’authentification d’un utilisateur comme faisant partie de tous les moyens que vous utilisez pour identifier le contenu protégé. Vous pouvez interroger Access Enabler pour déterminer si le client a déjà été authentifié.

#### 2c. Intégration du vérificateur de jeton multimédia {#int-media-token-ver}

Vous devez intégrer le composant Vérificateur de jeton multimédia d’authentification Adobe Pass à votre serveur multimédia.  Ainsi, votre vérificateur de jeton existant peut reconnaître les jetons multimédias de courte durée fournis par l’authentification Adobe Pass avec une autorisation réussie. Le vérificateur de jeton multimédia valide les jetons multimédia en tant que dernière étape de sécurité avant que vous ne fournissiez à l’utilisateur un accès au contenu protégé. Vous recevez l’emplacement à partir duquel télécharger le vérificateur de jeton multimédia lorsque vous vous inscrivez auprès d’Adobe.

### 3. Prise en charge de la connexion unique {#ssl}

Dans la plupart des cas, votre lecteur multimédia est chargé de gérer les déconnexions des utilisateurs via une API Access Enabler simple. Lorsque vous appelez logout(), Access Enabler effectue les opérations suivantes :

* Déconnexion de l’utilisateur actuel
* Efface toutes les informations d’authentification et d’autorisation pour l’utilisateur déconnecté
* Supprime tous les jetons AuthN et AuthZ du système local de l’utilisateur.

Si l’utilisateur laisse son ordinateur inactif suffisamment longtemps pour que ses jetons expirent, il peut tout de même revenir à la session et lancer la déconnexion. L’authentification Adobe Pass garantit que tous les jetons sont supprimés et avertit le MVPD de supprimer également leur session.

Lorsque la déconnexion est lancée à partir d’un site qui n’est pas intégré à l’authentification Adobe Pass, le MVPD peut appeler le service de déconnexion unique de l’authentification Adobe Pass par le biais d’une redirection du navigateur.

## Présentation des ID utilisateur {#user-ids}

D’un point de vue conceptuel, chaque utilisateur qui initie un flux de droits est associé à un identifiant utilisateur unique.  Cependant, au cours d’un flux de droits, cet identifiant utilisateur peut être présenté de différentes manières, selon l’API à partir de laquelle vous obtenez l’identifiant.

Le sessionGUID dans le jeton de média court est la forme sécurisée de l’ID utilisateur, disponible via l’appel sendTrackingData() .   Dans toutes les intégrations actuelles, il s’agit d’un GUID persistant pour l’utilisateur à travers le temps et les appareils, mais la source du GUID commence par l’UserID dans la réponse SAML du MVPD.   Cependant, certains distributeurs pourraient changer d&#39;avis à l&#39;avenir et commencer à envoyer un GUID transitoire.  Si un programmeur souhaite s’assurer que l’ID utilisateur source MVPD dans la réponse AuthN est persistant, il doit le prendre en compte dans ses accords avec les MVPD.

Voici les différentes manières dont l’ID utilisateur est représenté dans les API d’authentification Adobe Pass :

* `sendTrackingData()` Propriété du GUID : il s’agit de la version hachée Adobe de l’identifiant utilisateur MVPD.  Il est haché de sorte que cet ID utilisateur ne puisse pas être redirigé vers la source à partir du MVPD.   Cet identifiant est unique et généralement persistant, mais il ne peut pas être partagé avec le MVPD pour comparer le comportement d’utilisation spécifique à ce que les MVPD ont de leur côté.   Il n’est pas signé numériquement, il n’est donc pas insondable pour la prévention de la fraude, mais il est suffisant pour l’analyse.  Ce formulaire d’identifiant utilisateur est fourni côté client pour tous les événements générés par l’authentification Adobe Pass dans le flux AuthN/AuthZ.
* Propriété `sessionGUID` du jeton de média court : identique à l’ID utilisateur via `sendTrackingData()`. Cependant, celui-ci est signé numériquement pour protéger son intégrité.  Cela rend cette valeur suffisante pour le suivi des fraudes d’utilisation simultanée. Il est destiné à être traité côté serveur après l’utilisation de notre bibliothèque de validateurs et peut être analysé à la recherche de modèles de fraude avant de publier le flux vidéo sur le client.  Faire l&#39;une de ces tâches dépend du programmeur.
* `getMetadata() userID `property - Cette propriété permet à Adobe d’exposer l’identifiant utilisateur MVPD source au programmeur. Il sera crypté avec la clé publique du certificat que nous avons du Programmeur, de sorte qu&#39;il ne soit pas exposé au client clairement. Cela donne au programmeur le UserID réel du MVPD, il peut donc être utilisé pour la liaison de comptes ou l’enquête sur la fraude directement avec le MVPD.

**En conclusion**

* L’identifiant utilisateur MVPD est un identifiant unique persistant généralement, bien que non garanti, qui est **généré à partir des MVPD et transmis à l’Adobe lors d’une authentification réussie**. Il est généralement cohérent sur tous les réseaux, à quelques exceptions près.
* L’identifiant utilisateur MVPD ne contient pas de PII et il ne s’agit PAS d’un numéro de compte. Il n’est pas nécessaire d’être exposé sous une forme chiffrée, car nous avons validé avec tous les MVPD qu’aucune PII n’est envoyée.


L’utilisation de l’ID utilisateur dépend du cas d’utilisation :

* Si vous en avez besoin pour le suivi/l’analyse, l’emplacement le plus pratique est de l’obtenir à partir de `sendTrackingData()`.
* Si vous en avez besoin du côté serveur pour la publication de flux, les fraudes ou les données opérationnelles, vous pouvez l’obtenir à partir de Media Token Validator.
* Si vous en avez besoin pour la liaison de comptes et une fraude plus approfondie, contactez votre Adobe pour connaître la disponibilité.

<!--
>[!RELATEDINFORMATION]
>
>* **Kickstart Guides** These guides explain the initial steps to take once you have decided to begin integrating with Adobe Pass Authentication.
>* **Programmer Integration Guide** This is a lower level technical guide for Programmers, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>* [Overview For MVPDs](/help/authentication/mvpd-overview.md) This provides a similar level of conceptual information as in this Programmer overview, but is directed toward MVPDs.
-->
