---
title: Présentation des fichiers MVPD
description: Présentation des fichiers MVPD
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '2727'
ht-degree: 0%

---

# Présentation des fichiers MVPD {#mvpd-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

Cette présentation est destinée aux distributeurs de programmation vidéo multicanaux (MVPD). Pour obtenir de la documentation supplémentaire, notamment des guides de démarrage rapide et d’intégration, consultez la section Informations connexes à la fin de ce document.



TV Everywhere (TVE) est le mouvement industriel désormais bien connu qui permet aux abonnés de la télévision payante d&#39;accéder au contenu pour lequel ils paient déjà, sur plusieurs appareils, à la fois à l&#39;intérieur et à l&#39;extérieur de leurs foyers.  Pour les fournisseurs de télévision payante, TVE crée de nouvelles opportunités, à la fois pour préserver les relations existantes avec les clients et pour en permettre de nouvelles. Mais ces opportunités s’accompagnent aussi de défis. Dans le paysage TVE, les programmeurs fournissent le contenu, mais les MVPD conservent les informations du client pour vérifier que les spectateurs potentiels sont des abonnés valides.



Coordonner l’authentification et l’autorisation des visionneuses avec un programmeur peut être simple, mais la coordination avec des dizaines ou des centaines de programmeurs différents devient de plus en plus complexe. Cependant, avec Adobe ® Pass, les MVPD n&#39;ont qu&#39;à mettre en place une seule intégration simple pour avoir accès à l&#39;ensemble de l&#39;écosystème TVE, y compris les programmeurs tels que NBCUniversal Media, Turner Broadcasting (TBS, TNT, CNN), Fox Broadcast Networks, Hulu, etc.  L’authentification Adobe Pass fournit une structure d’intégration qui rend la détermination des droits des utilisateurs simple et sécurisée.



Conclusion : l’authentification Adobe Pass assure l’intermédiation sécurisée des transactions de droits entre les programmeurs et les MVPD, facilitant l’accès des utilisateurs au contenu des abonnements. En d’autres termes, l’authentification Adobe Pass permet aux bons clients d’accéder facilement et rapidement au contenu approprié.


Avec l’authentification Adobe Pass, les fichiers MVPD reçoivent :

Intégration facile avec les programmeurs.  Connectez instantanément plusieurs propriétaires de contenu grâce à une seule intégration.

Amélioration de l’engagement client.  Profitez d’une expérience de marque fluide lorsque vos clients visualisent du contenu sur plusieurs plateformes et appareils.

Authentification sécurisée.  Assurez-vous que seuls les utilisateurs et appareils autorisés ont accès au contenu Premium et (éventuellement) limitez le nombre d’appareils et de flux simultanés pouvant se connecter par compte domestique.

## FAQ {#faq}

Dans quelle mesure l’authentification Adobe Pass est-elle sécurisée ? La priorité numéro un de l’architecture d’authentification d’Adobe Pass est de s’assurer que seuls les observateurs autorisés sont authentifiés et qu’ils ont accès au contenu premium. L’authentification Adobe Pass lie étroitement l’accès aux appareils de visualisation et peut aider à limiter les flux, les sessions et/ou les appareils pour un foyer donné.


Un Flash Player est-il requis ? L’authentification Adobe Pass pour TV Everywhere est indépendante du lecteur et de la plateforme et s’intègre à toutes les applications de lecture, y compris Silverlight et HTML 5. En outre, l’authentification Adobe Pass fournit une prise en charge native des appareils tels que les téléphones et les tablettes exécutant iOS et Android.


Quels appareils l’authentification Adobe Pass prend-elle en charge ? L’authentification Adobe Pass est prise en charge par pratiquement n’importe quel appareil avec le kit web HTML 5 pour les expériences d’affichage dans le navigateur. En outre, Adobe Pass Authentication continue de déployer des kits de développement logiciel (SDK) natifs pour diverses plateformes spécifiques à chaque appareil, dont iOS, Android™, Xbox360 (obsolète) et Adobe Air® (obsolète). Plus récemment, l’authentification Adobe Pass a proposé une solution sans client pour les appareils qui ne peuvent pas effectuer le rendu des pages de navigateur (par exemple, les télévisions « intelligentes », les décodeurs et les consoles de jeux).  La possibilité de générer des pages de navigateur est une exigence pour l’authentification des utilisateurs avec des fichiers MVPD.


L’authentification Adobe Pass prend-elle en charge les normes émergentes pour TV Everywhere ? L’authentification Adobe Pass est conforme à la spécification CableLabs OLCA (Online Content Access), qui fournit des exigences techniques et une architecture pour la diffusion de vidéos à un client de télévision payante à partir de sources en ligne. En juin 2011, Adobe a participé au projet de test interopt conjoint de CableLabs et a réussi le processus de test pour la mise en œuvre d&#39;un fournisseur de services. L’authentification d’Adobe Pass est vérifiée (complète et testée) par rapport aux spécifications OLCA pour l’authentification. Le composant d&#39;autorisation est terminé, mais la vérification des tests attend actuellement la publication de l&#39;environnement de test CableLabs. L&#39;Adobe est également un membre actif de l&#39;OATC (Open Authentication Technical Consortium) et participe à plusieurs des projets de rédaction de spécifications des sous-comités dans le cadre de cet organisme.



Qu’est-ce que l’authentification ? L’authentification est le processus par lequel un MVPD confirme qu’un utilisateur donné est un client connu.



Qu’est-ce qu’une autorisation ? L’autorisation est le processus par lequel un MVPD confirme qu’un utilisateur authentifié dispose d’un abonnement valide à une ressource donnée.



## Architecture {#architecture}

Adobe Pass Authentication est un service hébergé qui permet une intégration back-end (serveur à serveur) rapide basée sur les règles métier requises par les MVPD et les programmeurs. Cela signifie un délai de mise sur le marché rapide pour toutes les parties, un environnement plus sûr pour prévenir la fraude et une expérience client supérieure, avec plus de contenu télévisuel disponible pour plus de personnes sur plus de plateformes.


L’authentification Adobe Pass est proposée via le modèle SaaS (Software as a Service) et permet une communication plus sécurisée entre les utilisateurs finaux, les MVPD et les programmeurs, afin de valider le droit au contenu. Les principaux composants du service sont les suivants :

Côté serveur : serveur d’authentification Adobe Pass hébergé. Il s’agit d’un serveur d’applications qui engage une communication back-canal (serveur à serveur) avec les systèmes d’authentification des fichiers MVPD.
Côté client :
L’activateur d’accès côté client : l’activateur d’accès est un petit fichier qui est chargé dans la page web ou l’application de lecteur d’un programmeur. Il fournit des API de droits à l’application d’affichage de contenu du programmeur et communique avec le serveur d’authentification Adobe Pass.
Services web sans client (pour les appareils non compatibles avec le web) : services web RESTful qui fournissent des API de droits pour les appareils tels que les Smart TV, les consoles de jeux et les décodeurs.

>[!NOTE]
>
>En tant que MVPD, vos services web doivent être en mesure de reconnaître les demandes d’authentification et d’autorisation provenant de l’authentification Adobe Pass et de répondre avec les données requises dans le format prévu.
>

L’authentification Adobe Pass vous permet de fournir aux clients la gestion fédérée des identités, également appelée authentification et autorisation d’authentification unique (SSO). Avec l’authentification Adobe Pass, les abonnés n’ont pas besoin de se reconnecter après leur première authentification, tant que cette authentification est autorisée par le MVPD à persister. (Généralement, 30 jours.) Pour ce faire, l’authentification Adobe Pass fournit un domaine commun pour les jetons d’authentification de nos clients. Ces informations d’état d’authentification sont disponibles pour tous les sites participants intégrés à un MVPD donné.


Actuellement, la plupart des intégrations de l’authentification Adobe Pass avec les MVPD utilisent le protocole SAML, l’une des principales normes d’authentification. L’authentification Adobe Pass agit comme un fournisseur de services proxy dans l’architecture SAML et conserve la réponse d’authentification SAML en tant que jeton sécurisé dans le domaine commun d’Adobe. L’authentification Adobe Pass est compatible avec SAML 2.0. Cependant, bien que l’authentification Adobe Pass soit généralement utilisée avec les solutions SSO SAML à ce stade, l’architecture d’authentification Adobe Pass n’est liée à aucun protocole spécifique. Par conséquent, la prise en charge de nouveaux protocoles, tels qu’un basé sur OAuth 2.0 ou des protocoles personnalisés, peut être ajoutée au fil du temps.


Adobe travaille avec une équipe technique de MVPD pour configurer l’authentification Adobe Pass afin de répondre aux besoins des intégrations existantes. L’intégration est gratuite pour les MVPD, en supposant une intégration « standard » et des exigences minimales de prise en charge (documentation et prise en charge de base des e-mails). Si un MVPD nécessite une assistance importante ou un calendrier d&#39;intervention plus long, des frais d&#39;assistance peuvent être facturés, ou le fournisseur peut vouloir travailler avec un tiers familier avec notre solution, tel que Synacor.


L’authentification Adobe Pass prend également en charge la gestion efficace de la logique commerciale MVPD, comme suit :

Pour la logique métier autonome pouvant être appliquée par le MVPD lorsqu’une demande d’autorisation est reçue, Adobe fournit les données nécessaires pour prendre en charge l’application de la logique métier lorsque le MVPD reçoit une demande d’autorisation. Ces données peuvent inclure, sans s’y limiter, l’ID d’appareil unique de l’utilisateur qui effectue la demande et l’adresse IP de l’appareil.

Pour la logique commerciale qui nécessite l’intervention de l’utilisateur et/ou une gestion spécifique par la solution d’Adobe, Adobe peut conserver certaines propriétés personnalisées pour chaque MVPD. Ces configurations/politiques spécifiques à MVPD incluent l’activation de workflows prédéfinis qui peuvent être déclenchés à des points spécifiques du workflow de niveau supérieur. Pour plus d’informations sur la prise en charge des propriétés personnalisées, contactez votre représentant d’Adobe.

Le diagramme suivant illustre la relation entre MVPD et le programmeur avec ces composants d’authentification Adobe Pass :

![](../assets/high-level-architecture-nflows.png)

*Image : architecture et flux de haut niveau*

## Composants d’authentification Adobe Pass {#components}

Vous trouverez ci-dessous un aperçu de certains des principaux composants de l’écosystème d’authentification d’Adobe Pass. Il s’agit notamment des éléments suivants :

* [Les services Web Access Enabler / Clientless](#ae)
* [Serveur principal hébergé par l’Adobe](#backend)
* [Jetons](#tokens)

### Access Enabler / Services Web Sans Client {#ae}

Access Enabler facilite toutes les interactions d’authentification et d’autorisation avec l’utilisateur et s’exécute localement sur son système. C’est Access Enabler qui gère les workflows de droits réels avec le MVPD, tandis que le programmeur conserve la responsabilité de la page web ou de l’application de lecteur de niveau supérieur.

Les services web sans client sont fournis par l’authentification Adobe Pass pour les appareils qui ne peuvent pas générer de pages web.  Pour ces appareils, le processus d’octroi des droits est lancé et le contenu est affiché sur l’appareil intelligent, tandis que l’authentification à l’aide d’un MVPD se fait sur un appareil compatible avec le Web (ordinateur, téléphone intelligent et tablette).

Le activateur d’accès :

* Lance les workflows d’authentification et d’autorisation spécifiques à MVPD.
* Met en cache les réponses d’autorisation réussies par ressource/canal du programmeur afin de minimiser le trafic de requête inutile.
* configurables pour des workflows prédéfinis spécifiques à chaque MVPD, tels que l’enregistrement explicite des appareils.
* Il est disponible sous les formes suivantes :
   * Fichier de SWF que l’exécution du Flash Player peut exécuter
   * Un fichier JS exécuté directement par le navigateur
   * Activateur d’accès natif pour diverses plateformes, notamment iOS, Android et Xbox.

### Serveur principal hébergé par Adobe {#backend}

Serveur principal de l’authentification Adobe Pass, hébergé par l’Adobe :

* Fournit les workflows d’authentification et d’autorisation avec les fichiers MVPD qui nécessitent une communication serveur à serveur entre l’authentification Adobe Pass et l’opérateur.
* Conserve la configuration pour les sites et applications de programmation.
* Héberge les fichiers de composant Access Enabler téléchargeables.
* Génère des jetons d’authentification et d’autorisation.

### Jetons {#tokens}

La solution Droits d’authentification Adobe Pass est centrée sur la génération d’éléments de données spécifiques obtenus à la réussite des workflows d’authentification/autorisation. Ces données sont appelées jetons. Ils ont une durée de vie limitée et sont stockés en toute sécurité dans des emplacements dépendant de la plateforme. Lors de l’expiration, les jetons doivent être émis à nouveau par le biais du redémarrage des workflows d’authentification et/ou d’autorisation.

Il existe trois types de jetons qui sont émis pendant les workflows d’authentification/autorisation. Deux sont de « longue durée », assurant la continuité de l’expérience de visionnage de l’utilisateur. Le troisième, un jeton de courte durée, soutient les meilleures pratiques de l&#39;industrie pour atténuer la fraude par l&#39;extraction de flux. Les valeurs de durée de vie (« TTL ») des jetons sont définies en fonction des accords entre les MVPD et les programmeurs. Vous décidez de la valeur de durée de vie la mieux adaptée à votre entreprise et à vos clients.

**Jeton d’authentification de longue durée**. Le succès de l’authentification se produit une fois qu’un client utilise l’authentification Adobe Pass pour se connecter à son compte MVPD. L’authentification Adobe Pass génère ensuite un jeton d’authentification de longue durée (« authN ») lié à l’appareil demandeur et (selon le MVPD) un identifiant global unique (« GUID ») qui identifie l’utilisateur de manière anonyme.

**Jeton d’autorisation de longue durée**. Une fois l’autorisation réussie, l’authentification Adobe Pass crée un jeton d’autorisation de longue durée (« authZ »). Ce jeton n’est pas portable, car il est lié à l’appareil demandeur et à une ressource protégée spécifique (par exemple, un canal, une série ou un épisode). Access Enabler utilise le jeton authZ de longue durée pour créer les jetons de média de courte durée utilisés pour l’accès réel à l’affichage.

**Jeton de média de courte durée**. Une fois l’utilisateur autorisé, l’authentification Adobe Pass génère un jeton authZ qu’elle utilise pour générer un jeton de média de courte durée à usage unique, signé par Adobe et chiffré afin d’éviter toute falsification lors de l’exchange. Comme le jeton de courte durée est exposé au site d’incorporation par le biais de l’API Access Enabler ou des services web sans client, avant de fournir l’accès à la ressource protégée, le serveur de médias du programmeur doit utiliser un composant d’authentification Adobe Pass, le vérificateur de jeton de média, pour valider le jeton.

## Cycle de vie de l’intégration MVPD {#lifecycle}

L’illustration suivante présente le cycle de vie de l’intégration entre l’authentification Adobe Pass et un MVPD.

![](../assets/mvpd-int-lifecycle.png)

*Image : cycle de vie de l’intégration MVPD*

## Organigramme des droits {#chart}

Le diagramme de flux suivant présente le processus global de confirmation des droits à l’aide de l’authentification Adobe Pass :

![](../assets/authn-authz-entitlmnt-flow.png)

*Figure : processus de confirmation des droits à l’aide de l’authentification Adobe Pass*

## Étapes d’authentification {#authn-steps}

Les étapes suivantes présentent un exemple de flux d’authentification d’Adobe Pass.  Il s’agit de la partie du processus de droits dans laquelle un programmeur détermine si l’utilisateur est un client valide d’un MVPD.  Dans ce scénario, l’utilisateur est un abonné valide à un MVPD.  L’utilisateur tente d’afficher du contenu protégé à l’aide de l’application de Flash d’un programmeur :

1. L’utilisateur accède à la page web du programmeur, qui charge l’application de Flash du programmeur et les composants Adobe Pass Authentication Access Enabler sur l’ordinateur de l’utilisateur. L’application de Flash utilise Access Enabler pour définir l’identité du programmeur avec l’authentification Adobe Pass, et l’authentification Adobe Pass définit l’Access Enabler avec les données de configuration et d’état pour ce programmeur (le « demandeur »). Access Enabler doit recevoir ces données du serveur avant d’effectuer tout autre appel API.  Remarque technique : le programmeur a défini son identité avec la méthode `setRequestor()` d’Access Enabler.
1. Lorsque l&#39;utilisateur tente d&#39;afficher le contenu protégé du programmeur, l&#39;application du programmeur présente à l&#39;utilisateur une liste de fichiers MVPD à partir de laquelle l&#39;utilisateur sélectionne un fournisseur.
1. L’utilisateur est redirigé vers un serveur d’authentification Adobe Pass, où une requête SAML chiffrée pour le MVPD sélectionné par l’utilisateur est créée. Cette demande est envoyée au MVPD en tant que demande d’authentification de la part du programmeur. En fonction du système de MVPD, le navigateur de l’utilisateur est redirigé vers le site de MVPD pour la connexion ou un iFrame de connexion est créé dans l’application du programmeur.
1. Dans les deux cas (redirection ou iFrame), le MVPD accepte la demande et affiche sa page de connexion.
1. L’utilisateur se connecte avec le MVPD, le MVPD valide le statut de l’utilisateur en tant que client payant, puis le MVPD crée sa propre session HTTP.
1. Lorsque l’utilisateur est validé, le MVPD crée une réponse (SAML et chiffrée) que le MVPD renvoie à l’authentification Adobe Pass.
1. L’authentification Adobe Pass reçoit la réponse MVPD, vérifie qu’une session HTTP d’authentification Adobe Pass est ouverte, valide la réponse [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) de MVPD et la redirige vers le site du programmeur.
1. Le site du programmeur est rechargé, l&#39;activateur d&#39;accès est rechargé et le programmeur appelle à nouveau setRequestor().  Le deuxième appel à setRequestor() est nécessaire car la configuration actuelle a été modifiée. Un indicateur est désormais présent pour informer Access Enabler qu&#39;un jeton AuthN attend d&#39;être généré sur le serveur.
1. Access Enabler constate qu’une authentification est en attente et demande le jeton au serveur d’authentification Adobe Pass. Le jeton est récupéré à partir du serveur en appelant les fonctionnalités DRM du Flash Player.
1. Le jeton AuthN est stocké dans le cache LSO de Flash Player du programmeur ; l’authentification est maintenant terminée et la session est détruite sur le serveur d’authentification Adobe Pass.

## Étapes d’autorisation {#authz-steps}

Les étapes suivantes continuent à partir de la section précédente ([Étapes d’authentification](#authn-steps)) :

1. Lorsque l’utilisateur ou l’utilisatrice tente d’accéder au contenu protégé du programmeur, l’application du programmeur recherche d’abord un jeton AuthN sur l’ordinateur ou l’appareil local de l’utilisateur ou de l’utilisatrice.  Si ce jeton n’est pas présent, suivez les [étapes d’authentification](#authn-steps) ci-dessus.  Si le jeton AuthN est présent, le flux d’autorisation se poursuit lorsque l’application du programmeur lance un appel à Access Enabler avec une demande d’obtention des droits de visionnage de l’utilisateur pour un élément spécifique de contenu protégé.
1. L’élément spécifique de contenu protégé est représenté par un « identifiant de ressource ».  Il peut s’agir d’une simple chaîne ou d’une structure plus complexe, mais dans tous les cas, la nature de l’identifiant de la ressource est convenue à l’avance entre le programmeur et le MVPD.  L’application du programmeur transmet l’identifiant de la ressource à Access Enabler.  Access Enabler recherche un jeton AuthZ sur l’ordinateur ou l’appareil local de l’utilisateur ou de l’utilisatrice.  Si le jeton AuthZ n’est pas présent, l’activateur d’accès transmet la requête au serveur d’authentification Adobe Pass principal.
1. Le serveur d’authentification Adobe Pass communique avec le point d’entrée d’autorisation des MVPD à l’aide de protocoles normalisés.  Si la réponse de MVPD indique que l’utilisateur a le droit d’afficher le contenu protégé, le serveur d’authentification Adobe Pass crée un jeton AuthZ et le transmet à l’activateur d’accès, qui stocke le jeton AuthZ sur l’ordinateur de l’utilisateur.
1. Avec un jeton AuthZ stocké sur l’ordinateur ou l’appareil de l’utilisateur ou de l’utilisatrice, l’application du programmeur appelle l’activateur d’accès pour obtenir un jeton multimédia du serveur d’authentification Adobe Pass et fournit ce jeton à l’application du programmeur.
1. Enfin, l’application du programmeur utilise le composant Vérificateur de jeton multimédia pour confirmer que l’utilisateur approprié consulte le contenu approprié, et une fois le jeton multimédia en place, il est autorisé à consulter le contenu protégé.

<!--
>![RELATEDINFORMATION]
>
>*   Kickstart Guides, [MVPD kickstart](/help/authentication/mvpd-kickstart-guide.md) and [programmer kickstart](/help/authentication/programmer-kickstart-guide.md). These guides explain the initial steps to take to begin integrating with Adobe Pass Authentication.
>
>*   [MVPD Integration Guide](/help/authentication/mvpd-kickstart-guide.md). This is a lower level technical guide for MVPDs, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>
>*   [Overview For Programmers](/help/authentication/programmer-overview.md). The same high level of conceptual information as in this MVPD overview, but directed toward the content providers (Programmers).
-->
