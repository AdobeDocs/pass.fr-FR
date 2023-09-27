---
title: Présentation des MVPD
description: Présentation des MVPD
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2736'
ht-degree: 0%

---

# Présentation des MVPD {#mvpd-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

Cette présentation est destinée aux distributeurs de programmation vidéo multicanaux (MVPD). Pour obtenir de la documentation supplémentaire, notamment des guides de démarrage et d’intégration, reportez-vous à la section Informations connexes à la fin de ce document.



TV Everywhere (TVE) est le mouvement de l&#39;industrie désormais bien connu qui permet aux abonnés de la télévision payante d&#39;accéder au contenu pour lequel ils payent déjà, sur plusieurs appareils, à l&#39;intérieur comme à l&#39;extérieur de leur domicile.  Pour les fournisseurs de télévision payante, TVE crée de nouvelles opportunités, à la fois pour préserver les relations client existantes et pour en activer de nouvelles. Mais ces opportunités s&#39;accompagnent de défis. Dans le paysage TVE, les programmeurs fournissent le contenu, mais les MVPD contiennent les informations du client pour vérifier que les visionneuses potentielles sont des abonnés valides.



Il peut être simple de coordonner l’authentification et l’autorisation des visionneuses avec un seul programmeur, mais la coordination avec des dizaines ou des centaines de programmeurs différents devient de plus en plus complexe. Cependant, avec Adobe® Pass, les MVPD n&#39;ont besoin de mettre en oeuvre qu&#39;une seule intégration simple pour accéder à l&#39;ensemble de l&#39;écosystème de TVE, y compris les programmeurs comme NBCUniversversal Media, Turner Broadcasting (Thirlton, TNT, CNN), Fox Broadcast Networks, Hulu, etc.  L’authentification Adobe Pass fournit une structure d’intégration qui rend simple et sécurisé la détermination des droits des utilisateurs.



Ligne de bas : l’authentification Adobe Pass permet de arbitrer en toute sécurité les transactions de droits entre les programmeurs et les MVPD, ce qui facilite l’accès de la visionneuse au contenu de l’abonnement. En d’autres termes, l’authentification Adobe Pass permet aux bons clients d’accéder facilement et rapidement au contenu approprié.


Avec l’authentification Adobe Pass, les MVPD reçoivent :

Intégration simple avec les programmeurs.  Diffusez la connectivité instantanée de plusieurs propriétaires de contenu avec une seule intégration.

Amélioration de l’engagement des clients.  Concevez une expérience harmonieuse et personnalisée lorsque vos clients visualisent du contenu sur plusieurs plateformes et appareils.

Authentification sécurisée.  Assurez-vous que seuls les utilisateurs et les appareils autorisés ont accès à du contenu haut de gamme et (éventuellement) limitez le nombre de périphériques et de flux simultanés pouvant se connecter par compte personnel.

## FAQ {#faq}

Quelle est la sécurité de l’authentification Adobe Pass ? La priorité numéro un de l’architecture d’authentification Adobe Pass est de s’assurer que seules les visionneuses autorisées sont authentifiées et qu’elles ont accès au contenu Premium. L’authentification Adobe Pass lie étroitement l’accès aux appareils d’affichage et peut aider à limiter les diffusions, les sessions et/ou les appareils pour un foyer donné.


Le Flash Player est-il requis ? L’authentification Adobe Pass pour TV Everywhere est indépendante du lecteur et de la plateforme, en s’intégrant à n’importe quelle application de lecture, y compris Silverlight et HTML5. En outre, l’authentification Adobe Pass fournit une prise en charge native pour les appareils tels que les téléphones et les tablettes exécutant iOS et Android.


Quels appareils l’authentification Adobe Pass prend-elle en charge ? L’authentification Adobe Pass est prise en charge par pratiquement tous les appareils dotés du kit web HTML5 pour l’affichage dans le navigateur. En outre, Adobe Pass Authentication continue de déployer des kits de développement logiciel (SDK) natifs pour diverses plateformes spécifiques aux appareils, notamment iOS, Android™, Xbox360 (obsolète) et Adobe Air® (obsolète). Plus récemment, Adobe Pass Authentication a sorti une solution sans client pour les appareils qui ne peuvent pas effectuer le rendu des pages de navigateur (par exemple, les téléviseurs &quot;intelligents&quot;, les décodeurs et les consoles de jeu).  La possibilité de rendre des pages de navigateur est une exigence pour l’authentification des utilisateurs avec des MVPD.


L’authentification Adobe Pass prend-elle en charge les nouvelles normes pour TV partout ? L’authentification Adobe Pass est conforme à la spécification OLCA (Online Content Access) de CableLabs, qui fournit des exigences techniques et une architecture pour la diffusion de vidéo à un client de télévision payante à partir de sources en ligne. Adobe a participé au projet conjoint de test d&#39;interaction de CableLabs en juin 2011 et a passé le processus de test pour une mise en oeuvre par un fournisseur de services. L’authentification Adobe Pass est vérifiée (complète et testée) par rapport aux spécifications OLCA pour l’authentification. Le composant d’autorisation est terminé, mais la vérification des tests attend actuellement la sortie de l’environnement de test de CableLabs. Adobe est également un membre actif de l’OATC (Open Authentication Technical Consortium) et participe à plusieurs projets de rédaction de spécifications de ces sous-comités dans le cadre de cet organisme.



Qu’est-ce que l’authentification ? L’authentification est le processus par lequel un MVPD confirme qu’un utilisateur donné est un client connu.



Qu’est-ce que l’autorisation ? L’autorisation est le processus par lequel un MVPD confirme qu’un utilisateur authentifié possède un abonnement valide à une ressource donnée.



## Architecture {#architecture}

L’authentification Adobe Pass est un service hébergé qui permet une intégration rapide (serveur à serveur) en fonction des règles métier requises par les MVPD et les programmeurs. Cela signifie un temps de mise sur le marché rapide pour toutes les parties, un environnement plus sécurisé pour prévenir la fraude et une expérience client supérieure, avec plus de contenu TV disponible pour plus de personnes sur plus de plateformes.


L’authentification Adobe Pass est proposée via le modèle Software as a Service (SaaS) et permet une communication plus sécurisée entre les utilisateurs finaux, les MVPD et les programmeurs, afin de valider le droit au contenu. Les composants principaux du service sont les suivants :

Côté serveur : serveur d’authentification Adobe Pass hébergé. Il s’agit d’un serveur d’applications qui interagit dans la communication back channel (serveur à serveur) avec les systèmes d’authentification des MVPD.
Côté client : l’activateur d’accès côté client : l’activateur d’accès est un petit fichier chargé dans la page web ou l’application de lecteur d’un programmeur. Il fournit des API de droits à l’application d’affichage de contenu du programmeur et communique avec le serveur d’authentification Adobe Pass.
Services web sans client (pour les appareils non compatibles avec le web) : services web RESTful qui fournissent des API de droits pour les appareils tels que les téléviseurs intelligents, les consoles de jeux et les décodeurs.

>[!NOTE]
>
>En tant que MVPD, vos services web doivent être en mesure de reconnaître les demandes d’authentification et d’autorisation provenant de l’authentification Adobe Pass et de répondre avec les données requises dans le format attendu.
>

L’authentification Adobe Pass vous permet de fournir aux clients une gestion fédérée des identités, également appelée authentification et autorisation de connexion unique (SSO). Avec l’authentification Adobe Pass, les abonnés n’ont pas besoin de se reconnecter après leur première authentification, tant que cette authentification est autorisée par le MVPD à persister. (En règle générale, 30 jours.) Pour ce faire, l’authentification Adobe Pass fournit un domaine commun pour les jetons d’authentification pour nos clients. Ces informations d’état d’authentification sont disponibles pour tous les sites participants qui sont intégrés à un MVPD donné.


Actuellement, la plupart des intégrations d’authentification Adobe Pass avec les MVPD utilisent le protocole SAML, l’une des principales normes d’authentification. L’authentification Adobe Pass agit comme un fournisseur de service proxy dans l’architecture SAML et conserve la réponse de l’authentification SAML comme un jeton sécurisé dans le domaine commun de l’Adobe. L’authentification Adobe Pass est compatible avec SAML 2.0. Cependant, bien que l’authentification Adobe Pass soit généralement utilisée avec les solutions SAML SSO à ce stade, l’architecture de l’authentification Adobe Pass n’est liée à aucun protocole spécifique. Par conséquent, la prise en charge de nouveaux protocoles, tels que ceux basés sur OAuth 2.0 ou les protocoles personnalisés, peut être ajoutée au fil du temps.


Adobe travaille avec l’équipe technique d’un MVPD pour configurer l’authentification Adobe Pass afin de répondre aux besoins de toutes les intégrations existantes. L’intégration est gratuite pour les MVPD, en supposant une intégration &quot;standard&quot; et des exigences de prise en charge minimales (documentation et prise en charge de base des emails). Si un MVPD nécessite une assistance importante ou un calendrier réaffecté, des frais d’assistance peuvent être facturés, ou le fournisseur peut vouloir travailler avec un tiers familiarisé avec notre solution telle que Synacor.


L’authentification Adobe Pass prend également en charge la gestion efficace de la logique commerciale MVPD, comme suit :

Pour la logique commerciale autonome et pouvant être appliquée par le MVPD lorsqu’une demande d’autorisation est reçue, Adobe fournit les données nécessaires à la prise en charge de l’application de la logique commerciale lorsque le MVPD reçoit une demande d’autorisation. Ces données peuvent inclure, sans s’y limiter, l’identifiant unique de l’appareil pour l’utilisateur qui effectue la demande et l’adresse IP de l’appareil.

Pour la logique commerciale qui nécessite une intervention de l’utilisateur et/ou une gestion spécifique par la solution Adobe, Adobe peut conserver certaines propriétés personnalisées pour chaque MVPD. Ces configurations/stratégies spécifiques à MVPD incluent l’activation de workflows prédéfinis qui peuvent être démarrés à des points spécifiques du workflow de niveau supérieur. Pour plus d’informations sur la prise en charge des propriétés personnalisées, contactez votre représentant d’Adobe.

Le diagramme suivant illustre la relation entre le MVPD et le programmeur avec ces composants d’authentification Adobe Pass :

![](assets/high-level-architecture-nflows.png)

*Figure : Architecture de haut niveau et flux*

## Composants d’authentification Adobe Pass {#components}

Vous trouverez ci-dessous un aperçu de certains des principaux composants de l’écosystème d’authentification Adobe Pass. Il s’agit notamment :

* [Accès aux services Web sans client](#ae)
* [Serveur principal hébergé par Adobe](#backend)
* [Jetons](#tokens)

### Accès à Activateur/Services Web sans client {#ae}

L’activation d’accès facilite toutes les interactions d’authentification et d’autorisation avec l’utilisateur et s’exécute localement sur son système. C’est l’outil d’activation d’accès qui gère les processus de droits réels avec le MVPD, tandis que le programmeur conserve la responsabilité de la page web ou de l’application du lecteur de niveau supérieur.

Les services web sans client sont fournis par l’authentification Adobe Pass pour les appareils qui ne peuvent pas effectuer le rendu des pages web.  Pour ces appareils, le processus de droits est lancé et le contenu est affiché sur l’appareil intelligent, tandis que l’authentification avec un MVPD a lieu sur un appareil compatible avec le web (ordinateur, smartphone et tablette).

Access Enabler :

* Lance des workflows d’authentification et d’autorisation spécifiques au MVPD.
* Met en cache les réponses d’autorisation réussies par ressource/canal de programmeur afin de minimiser le trafic de requêtes inutile.
* Peut être configuré pour des workflows prédéfinis spécifiques à chaque MVPD, tels que l’enregistrement explicite de périphériques.
* Il est disponible sous les formes suivantes :
   * Fichier SWF que l’exécution du Flash Player peut exécuter
   * Un fichier JS exécuté directement par le navigateur
   * Un gestionnaire d’accès natif pour diverses plateformes, notamment iOS, Android et Xbox.

### Serveur principal hébergé par Adobe {#backend}

Serveur principal d’authentification Adobe Pass, hébergé par Adobe :

* Spécifie les workflows d’authentification et d’autorisation avec les MVPD qui nécessitent une communication serveur à serveur entre l’authentification Adobe Pass et l’opérateur.
* Maintient la configuration des sites et applications de programmation.
* Héberge les fichiers de composant Access Enabler téléchargeables.
* Génère des jetons d’authentification et d’autorisation.

### Jetons {#tokens}

La solution de droits d’authentification Adobe Pass se concentre sur la génération de données spécifiques obtenues à l’issue réussie des workflows d’authentification/d’autorisation. Ces données sont appelées jetons. Leur durée de vie est limitée et ils sont stockés en toute sécurité dans des emplacements dépendants de la plateforme. Lors de l’expiration, les jetons doivent être réémis en réinitialisant les workflows d’authentification et/ou d’autorisation.

Il existe trois types de jetons émis lors des workflows d’authentification/d’autorisation. Deux sont de &quot;longue durée&quot;, ce qui assure la continuité de l’expérience de visionnage de l’utilisateur. Le troisième, un jeton de courte durée, fournit une prise en charge des bonnes pratiques du secteur pour atténuer la fraude par le biais de l’extraction de flux. Les valeurs de durée de vie (&quot;TTL&quot;) des jetons sont définies sur la base d’accords entre les MVPD et les programmeurs. Vous décidez d’une valeur TTL qui convient le mieux à votre entreprise et à vos clients.

**Jeton d’authentification de longue durée**. L’authentification réussit une fois qu’un client utilise l’authentification Adobe Pass pour se connecter à son compte MVPD. L’authentification Adobe Pass produit ensuite un jeton d’authentification de longue durée (&quot;authN&quot;) lié à l’appareil demandeur et (selon le MVPD) un identifiant unique global (&quot;GUID&quot;) qui identifie anonymement l’utilisateur.

**Jeton d’autorisation de longue durée**. Une fois l’autorisation effectuée, l’authentification Adobe Pass crée un jeton d’autorisation de longue durée (&quot;authZ&quot;). Ce jeton n’est pas portable, car il est lié à l’appareil qui le demande et à une ressource protégée spécifique (un canal, une série ou un épisode, par exemple). L’activateur d’accès utilise le jeton authZ de longue durée pour créer les jetons multimédias de courte durée utilisés pour un accès réel à l’affichage.

**Jeton multimédia de courte durée**. Une fois l’utilisateur autorisé, l’authentification Adobe Pass génère un jeton authZ et utilise ce jeton pour générer un jeton multimédia de courte durée à usage unique, signé par Adobe et chiffré afin d’éviter toute manipulation pendant l’échange. Le jeton de courte durée étant exposé au site d’intégration par le biais de l’API Access Enabler ou des services web sans client, avant de permettre l’accès à la ressource protégée, le serveur multimédia du programmeur doit utiliser un composant d’authentification Adobe Pass, le Vérificateur de jeton multimédia, pour valider le jeton.

## Cycle de vie de l’intégration MVPD {#lifecycle}

L’illustration suivante présente le cycle de vie de l’intégration entre l’authentification Adobe Pass et un MVPD.

![](assets/mvpd-int-lifecycle.png)

*Figure : Cycle de vie de l’intégration MVPD*

## Organigramme des droits {#chart}

L’organigramme suivant présente le processus global de confirmation des droits à l’aide de l’authentification Adobe Pass :

![](assets/authn-authz-entitlmnt-flow.png)

*Figure : Processus de confirmation des droits à l’aide de l’authentification Adobe Pass*

## Étapes d’authentification {#authn-steps}

Les étapes suivantes présentent un exemple du flux d’authentification de l’authentification Adobe Pass.  Il s’agit de la partie du processus de droit dans laquelle un programmeur détermine si l’utilisateur est un client valide d’un MVPD.  Dans ce scénario, l’utilisateur est un abonné valide à un MVPD.  L’utilisateur tente d’afficher du contenu protégé à l’aide d’une application de Flash du programmeur :

1. L’utilisateur accède à la page web du programmeur, qui charge l’application de Flash du programmeur et les composants Adobe Pass Authentication Access Enabler sur la machine de l’utilisateur. L’application de Flash utilise Access Enabler pour définir l’identification du programmeur avec l’authentification Adobe Pass, et l’authentification Adobe Pass prime l’activation d’accès avec la configuration et les données d’état pour ce programmeur (le &quot;demandeur&quot;). L’activateur d’accès doit recevoir ces données du serveur avant d’effectuer tout autre appel API.  Remarque technique : le programmeur a défini son identité avec le `setRequestor()` ; pour plus d’informations, voir [Guide d’intégration de programmeur](/help/authentication/programmer-integration-guide-overview.md).
1. Lorsque l’utilisateur tente d’afficher le contenu protégé du programmeur, l’application du programmeur présente à l’utilisateur une liste de MVPD, à partir de laquelle l’utilisateur sélectionne un fournisseur.
1. L’utilisateur est redirigé vers un serveur d’authentification Adobe Pass, où est créée une requête SAML chiffrée pour le MVPD sélectionné par l’utilisateur. Cette requête est envoyée en tant que requête d’authentification au nom du programmeur au MVPD. Selon le système du MVPD, le navigateur de l’utilisateur est alors redirigé vers le site du MVPD pour se connecter ou un iFrame de connexion est créé dans l’application du programmeur.
1. Dans les deux cas (redirection ou iFrame), le MVPD accepte la demande et affiche sa page de connexion.
1. L’utilisateur se connecte avec le MVPD, le MVPD valide le statut de l’utilisateur en tant que client payant, puis le MVPD crée sa propre session HTTP.
1. Lorsque l’utilisateur est validé, le MVPD crée une réponse (SAML et cryptée), que le MVPD renvoie à l’authentification Adobe Pass.
1. L’authentification Adobe Pass reçoit la réponse MVPD, voit qu’une session HTTP d’authentification Adobe Pass est ouverte, valide la variable [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) réponse du MVPD et redirige vers le site du programmeur.
1. Le site du programmeur est rechargé, Access Enabler est rechargé et le programmeur appelle à nouveau setRequestor() .  Le deuxième appel à setRequestor() est nécessaire car la configuration actuelle a changé. Un indicateur s’affiche maintenant pour informer l’activateur d’accès qu’un jeton AuthN attend d’être généré sur le serveur.
1. Access Enabler constate qu’une authentification est en attente et demande le jeton au serveur d’authentification Adobe Pass. Le jeton est récupéré à partir du serveur en appelant les fonctionnalités DRM du Flash Player.
1. Le jeton AuthN est stocké dans le cache LSO du Flash Player du programmeur ; l’authentification est maintenant terminée et la session est détruite sur le serveur d’authentification Adobe Pass.

## Étapes d’autorisation {#authz-steps}

Les étapes suivantes continuent à partir de la section précédente ([Étapes d’authentification](#authn-steps)) :

1. Lorsque l’utilisateur tente d’accéder au contenu protégé du programmeur, l’application du programmeur commence par rechercher un jeton AuthN sur l’ordinateur ou l’appareil local de l’utilisateur.  Si ce jeton n’est pas présent, la variable [Étapes d’authentification](#authn-steps) ci-dessus sont suivis.  Si le jeton AuthN est présent, le flux d’autorisation se poursuit avec l’application du programmeur initiant un appel à l’activateur d’accès avec une requête pour obtenir les droits d’affichage de l’utilisateur pour un élément spécifique de contenu protégé.
1. L’élément spécifique du contenu protégé est représenté par un &quot;identifiant de ressource&quot;.  Il peut s’agir d’une chaîne simple ou d’une structure plus complexe, mais dans tous les cas, la nature de l’identifiant de la ressource est convenue à l’avance entre le programmeur et le MVPD.  L’application du programmeur transmet l’identifiant de ressource à l’activateur d’accès.  Access Enabler recherche un jeton AuthZ sur l’ordinateur ou l’appareil local de l’utilisateur.  Si le jeton AuthZ n’est pas présent, Access Enabler transmet la demande au serveur d’authentification Adobe Pass principal.
1. Le serveur d’authentification Adobe Pass communique avec le point de terminaison d’autorisation des MVPD à l’aide de protocoles normalisés.  Si la réponse du MVPD indique que l’utilisateur est autorisé à afficher le contenu protégé, le serveur d’authentification Adobe Pass crée un jeton AuthZ et le transmet à l’Activateur d’accès, qui stocke le jeton AuthZ sur l’ordinateur de l’utilisateur.
1. Avec un jeton AuthZ stocké sur l’ordinateur ou l’appareil de l’utilisateur, l’application du programmeur appelle l’activateur d’accès pour obtenir un jeton multimédia à partir du serveur d’authentification Adobe Pass et fournit ce jeton à l’application du programmeur.
1. Enfin, l’application du programmeur utilise le composant de vérification de jeton multimédia pour confirmer que l’utilisateur approprié visualise le contenu approprié et qu’avec le jeton multimédia en place, l’utilisateur est autorisé à afficher le contenu protégé.

<!--
>![RELATEDINFORMATION]
>
>*   Kickstart Guides, [MVPD kickstart](/help/authentication/mvpd-kickstart-guide.md) and [programmer kickstart](/help/authentication/programmer-kickstart-guide.md). These guides explain the initial steps to take to begin integrating with Adobe Pass Authentication.
>
>*   [MVPD Integration Guide](/help/authentication/mvpd-kickstart-guide.md). This is a lower level technical guide for MVPDs, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>
>*   [Overview For Programmers](/help/authentication/programmer-overview.md). The same high level of conceptual information as in this MVPD overview, but directed toward the content providers (Programmers).
-->
