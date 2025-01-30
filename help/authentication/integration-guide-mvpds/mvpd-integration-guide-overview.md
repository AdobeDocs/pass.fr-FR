---
title: Guide d’intégration de MVPD
description: Guide d’intégration de MVPD
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 2b9a8ce374f7a73cd815e9735d672e5c9ba285cc
workflow-type: tm+mt
source-wordcount: '1265'
ht-degree: 0%

---

# Guide d’intégration de MVPD {#mvpd-integration-guide}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Ce guide d’intégration est destiné aux distributeurs de programmation vidéo multicanaux (MVPD) qui prévoient de s’intégrer à l’authentification Adobe ® Pass.

TV Everywhere (TVE) est une initiative transformatrice dans l&#39;industrie de la télévision payante, qui permet aux abonnés d&#39;accéder au contenu pour lequel ils paient déjà sur plusieurs appareils, que ce soit à la maison ou en déplacement. Pour les fournisseurs de télévision payante, TVE offre des opportunités significatives parmi lesquelles le renforcement des relations client existantes et l&#39;ouverture de portes à de nouvelles. Toutefois, ces possibilités s&#39;accompagnent de défis.

Dans l’écosystème TVE, les **programmeurs** fournissent le contenu, tandis que les **MVPD** (Multichannel Video Programming Distributors) gèrent les données client nécessaires pour vérifier si les téléspectateurs sont des abonnés éligibles. Bien que la coordination de l&#39;authentification et de l&#39;autorisation avec un seul programmeur puisse être gérable, le faire avec des dizaines ou même des centaines de programmeurs introduit une complexité considérable.

C’est là que l’authentification **Adobe ® Pass** simplifie le processus. Les MVPD n’ont qu’à mettre en œuvre une seule intégration rationalisée avec Adobe Pass pour accéder à l’ensemble de l’écosystème TVE. La structure d’intégration fournie accélère la mise sur le marché, fournit un environnement sécurisé pour atténuer la fraude et améliore l’expérience client en fournissant plus de contenu TV sur plusieurs plateformes.

## Authentification Adobe Pass pour TV Everywhere {#adobe-pass-authentication-for-tv-everywhere}

L’authentification Adobe Pass fonctionne comme une solution SaaS (Software as a Service) conçue pour permettre une intégration rapide du serveur principal (serveur à serveur), en respectant les règles métier des distributeurs de programmation vidéo multicanaux (MVPD) et des programmeurs.

### Normes et protocoles {#standards-protocols}

L’authentification Adobe Pass est entièrement alignée sur les normes et protocoles émergents de TV Everywhere (TVE), ce qui permet une intégration et une conformité transparentes dans l’ensemble de l’écosystème.

* **Spécification CableLabs OLCA (Online Content Access)**\
  L’authentification Adobe Pass respecte la spécification CableLabs OLCA, qui définit les exigences techniques et l’architecture pour la diffusion de contenu vidéo à partir de sources en ligne aux clients de la télévision payante. Adobe a participé activement au projet de test d&#39;interopérabilité CableLabs en juin 2011, réussissant avec succès le processus de test pour la mise en œuvre d&#39;un fournisseur de services.

L’authentification Adobe Pass est conçue pour prendre en charge plusieurs protocoles (par exemple, SAML, OAuth 2.0, etc.), cette flexibilité permettant de futures extensions, y compris des protocoles personnalisés, en fonction de l’évolution des besoins.

La plupart des intégrations utilisent le protocole SAML (Security Assertion Markup Language), un standard principal pour l’authentification. L’authentification Adobe Pass agit comme un fournisseur de services proxy dans le framework SAML, conservant la réponse d’authentification SAML en tant que jeton sécurisé dans le domaine commun d’Adobe.

En résumé, l’authentification Adobe Pass est indépendante du protocole et conçue pour s’aligner étroitement sur les normes OLCA. Ces normes établissent un cadre commun pour les programmeurs, les MVPD et les fournisseurs de services, assurant une approche cohérente de la mise en œuvre des fonctionnalités TVE.

### Intégration et assistance {#integration-support}

L’authentification Adobe Pass collabore avec les équipes techniques de MVPD pour configurer des intégrations personnalisées en fonction de leurs besoins spécifiques, comme suit :

* **Intégrations standard**\
  Offert gratuitement, y compris la documentation et une assistance de base par e-mail.

* **Assistance améliorée**\
  Pour les personnalisations importantes ou les délais accélérés, des frais d’assistance peuvent s’appliquer.

L’authentification Adobe Pass prend en charge la gestion efficace de la logique commerciale MVPD, comme suit :

* **Logique Commerciale Autonome**\
  Pour la logique commerciale entièrement appliquée par MVPD lors des demandes d’autorisation, Adobe fournit les données nécessaires. Ces données peuvent inclure, sans s’y limiter, l’identifiant unique de l’appareil et l’adresse IP de l’appareil de l’utilisateur qui effectue la demande.

* **Prise en charge des propriétés personnalisées**\
  Pour la logique commerciale nécessitant l’intervention de l’utilisateur ou une gestion spécifique à l’Adobe, Adobe peut gérer des configurations personnalisées pour chaque MVPD. Ces politiques activent des workflows prédéfinis qui peuvent être déclenchés à des points spécifiques du workflow des droits. Pour plus de détails, contactez votre représentant de l’Adobe.

## Flux de droits {#entitlement-flow}

Le flux de droits est une série d’étapes qu’une application de programmation (TVE) doit suivre pour diffuser du contenu protégé. Le flux comprend plusieurs phases qui impliquent des interactions avec le MVPD :

* [Phase d’authentification](#authentication-phase)
* (Facultatif) Phase De Préautorisation
* [Phase d’autorisation](#authorization-phase)
* Phase de déconnexion

Lors de la visite initiale d’un utilisateur ou d’une utilisatrice dans une application de programmation (TVE), le flux des droits suit la séquence décrite. Cependant, lors des visites ultérieures, l’application peut contourner certaines étapes en fonction du statut de l’authentification et des politiques d’affichage applicables.

>[!NOTE]
>
> L’application Programmeur (TVE) est utilisée dans ce document pour faire référence collectivement aux types d’applications s’exécutant sur différentes plateformes (navigateurs, appareils mobiles, appareils connectés à la télévision, etc.) prises en charge par l’authentification Adobe Pass.

### Phase d’authentification {#authentication-phase}

Les étapes suivantes décrivent les étapes de haut niveau dans le cas d’une intégration SAML :

1. **Chargement de l’application du programmeur (site web)**\
   L’utilisateur accède à l’application du programmeur (site web), qui intègre l’authentification Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Demande de contenu protégé**\
   Lorsque l’utilisateur tente d’accéder au contenu protégé, l’application du programmeur affiche une liste de fichiers MVPD dans laquelle il peut sélectionner des fichiers.

1. **Initialisation de la demande d’authentification**\
   Lorsque MVPD est sélectionné, l’utilisateur est redirigé vers un serveur d’authentification Adobe Pass. Ici, une demande d’authentification SAML chiffrée pour le MVPD sélectionné est générée, en cas d’intégration SAML. Cette demande est envoyée au MVPD au nom du programmeur. Selon le système de MVPD, le navigateur de l’utilisateur est redirigé vers la page de connexion de MVPD ou un iFrame de connexion est intégré à l’application du programmeur.

1. **Connexion à MVPD**\
   Le MVPD accepte la demande et présente son interface de connexion, soit par redirection, soit par iFrame.

1. **Connexion utilisateur et validation**\
   L’utilisateur se connecte avec ses informations d’identification MVPD. Le MVPD valide le statut d’abonnement de l’utilisateur et établit sa propre session HTTP.

1. **Réponse de MVPD à l&#39;authentification Adobe Pass**\
   Une fois la validation terminée, le MVPD génère une réponse SAML (chiffrée) et la renvoie vers l’authentification Adobe Pass.

1. **Génération de profil**\
   L’authentification Adobe Pass vérifie la réponse SAML, génère un profil utilisateur qui est mis en cache et redirige l’utilisateur vers l’application du programmeur (site web).

### Phase d’autorisation {#authorization-phase}

**Étapes de haut niveau**

Les étapes suivantes décrivent les étapes de haut niveau :

1. **Gestion des identifiants de ressource**\
   Le contenu protégé est identifié par un [identifiant de ressource](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier), qui peut être une simple chaîne ou une structure plus complexe. Cet identifiant est prédéfini et accepté par le programmeur et le MVPD. L’application du programmeur envoie l’identifiant de la ressource à l’API REST [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) d’Adobe Pass Authentication.

1. **Vérification De L’Autorisation MVPD**\
   Le serveur d’authentification Adobe Pass communique avec le point d’entrée d’autorisation MVPD à l’aide de protocoles normalisés.

1. **Réponse de MVPD à l&#39;authentification Adobe Pass**\
   Une fois la validation terminée, le MVPD confirme que l’utilisateur a le droit (ou non) d’accéder au contenu et renvoie une réponse à Authentification Adobe Pass.

1. **Génération de jetons de décision et de média**\
   L’authentification Adobe Pass vérifie la réponse, génère une [décision](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) qui est mise en cache et renvoie la décision contenant un jeton de média à l’application du programmeur (site web).

1. **Vérification de l’accès au contenu**\
   L’application du programmeur utilise le [vérificateur de jeton multimédia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) pour confirmer que l’utilisateur ou l’utilisatrice approprié(e) accède au contenu correct. Une fois la validation effectuée, l’utilisateur se voit accorder l’accès à l’affichage du contenu protégé.

## Comprendre les droits {#understanding-entitlements}

La solution Authentification Adobe Pass s’articule autour de la création de droits, c’est-à-dire d’éléments de données spécifiques générés lors de la réussite des workflows d’authentification et d’autorisation. Ces droits donnent accès à du contenu protégé, mais leur durée de vie est limitée. Une fois qu’un droit expire, il doit être renouvelé en réinitialisant les processus d’authentification ou d’autorisation.

Pour plus d’informations sur les droits, reportez-vous aux documents suivants :

* **Profils**

  Une fois l’authentification réussie, l’authentification Adobe Pass crée un profil authentifié (« de longue durée ») associé à l’application, à l’appareil et à l’identifiant du fournisseur de services demandeur (identifiant du demandeur).

* **[Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Une fois l’authentification réussie (et dans certains cas après autorisation également), l’authentification Adobe Pass reçoit des métadonnées de l’utilisateur de la part de MVPD qui peuvent l’exposer à l’application demandeuse.

* **[Décisions](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Une fois l’autorisation réussie, l’authentification Adobe Pass crée une décision d’autorisation (« de longue durée ») associée à l’application, à l’appareil, à l’identifiant du fournisseur de services (identifiant du demandeur) et à une ressource protégée spécifique (identifiant de ressource).

* **[Jetons multimédia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Une fois l’autorisation réussie, l’authentification Adobe Pass crée un jeton multimédia (« de courte durée ») associé à une requête de lecture réussie.
