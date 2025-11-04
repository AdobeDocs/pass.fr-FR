---
title: À propos de l'authentification Adobe Pass
description: À propos de l'authentification Adobe Pass
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: 7ca9d8996756086a6b963c0b6d5b0bb64608ecbc
workflow-type: tm+mt
source-wordcount: '1828'
ht-degree: 0%

---

# À Propos De L’Authentification Adobe® Pass {#about-adobe-pass-authentication}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## À propos de TV Everywhere {#about-tv-everywhere}

Les téléspectateurs d&#39;aujourd&#39;hui s&#39;attendent à un accès continu au contenu de la télévision payante, en tout temps et n&#39;importe où. Avec l&#39;essor des appareils connectés à Internet, les auditoires consomment du contenu sur une gamme croissante de plateformes, notamment :

* Ordinateurs portables
* Comprimés
* Smartphones
* Sites web
* Applications Fédérées
* Consoles de jeux
* Décodeurs
* Smart TV

TV Everywhere est une initiative du secteur qui garantit que les abonnés à la télévision payante peuvent accéder au contenu pour lequel ils paient déjà sur plusieurs appareils, à l’intérieur et à l’extérieur de leur domicile.

Alors que la télévision traditionnelle (linéaire) reste puissante, les segments à plus forte croissance de la consommation vidéo sont le contenu décalé dans le temps, la diffusion en continu en ligne et les écrans alternatifs. Ce changement a bouleversé le marché de la distribution vidéo, faisant de TV Everywhere une solution cruciale qui aligne les intérêts des **programmeurs, fournisseurs de télévision payante et abonnés.**

### Objectifs de TV Everywhere {#goals-tv-everywhere}

**Objectif technique**

* Permettre aux clients de la télévision payante d’accéder facilement au contenu auquel ils sont abonnés sur tous les appareils et plateformes.

**Objectifs commerciaux**

* Préserver et renforcer les relations client existantes tout en offrant de nouvelles opportunités.
* Permettre aux programmeurs et aux propriétaires de contenu d’atteindre un public plus large et de maximiser la valeur du contenu premium.
* Étendez vos marques grâce à un engagement en ligne direct avec les visiteurs.

### Les défis de la télévision partout {#challenges-tv-everywhere}

Avec les opportunités de TV Everywhere viennent des défis importants, les droits sociaux étant les plus critiques. Pour qu’une visionneuse puisse accéder au contenu de l’abonnement, un système doit vérifier ses droits.

Les questions clés sont les suivantes :

* L’utilisateur a-t-il un abonnement actif auprès d’un fournisseur de télévision payante ?
* Leur abonnement inclut-il le contenu demandé ?

La détermination des droits est particulièrement difficile pour les programmeurs et les propriétaires de contenu, car les opérateurs de télévision payante contrôlent les données des clients et les privilèges d&#39;accès.

Au-delà des droits, plusieurs défis techniques et d’intégration se posent, notamment :

* Développement d’une stratégie multi-appareils qui garantit un accès transparent.
* Gérer des relations complexes entre les programmeurs et les fournisseurs de télévision payante.
* Prévention des accès frauduleux et respect des conditions d’utilisation.
* Offre une expérience d’authentification cohérente et conviviale sur les sites web et les applications.
* Maintien d&#39;un délai de mise sur le marché rapide pour s&#39;aligner sur les accords d&#39;affiliation.
* Contrôle des coûts associés à plusieurs intégrations.

Ces défis rendent les intégrations directes entre les programmeurs et les multiples systèmes d&#39;authentification de la télévision payante très gourmandes en ressources, nécessitant à la fois du temps et une expertise technique.

Pour surmonter ces obstacles, **Adobe® Pass Authentication** simplifie et rationalise la vérification des droits, permettant un accès transparent et sécurisé au contenu TV Everywhere.

## Présentation de l’authentification Adobe Pass {#introduction-adobe-pass-authentication}

L’authentification Adobe Pass assure l’intermédiation sécurisée des transactions de droits entre les programmeurs et les fournisseurs de télévision payante, en veillant à ce que les bons clients puissent accéder au bon contenu facilement.

![](/help/authentication/assets/programmers-connect-authn.png)

*Certains des programmeurs et des fournisseurs de télévision payante qui se connectent via l’authentification Adobe Pass*

**Qui bénéficie de l’authentification Adobe Pass**

* **Programmeurs**

  Qui souhaitent s&#39;intégrer facilement aux fournisseurs de services de télévision payante, également appelés MVPD (Multichannel Video Programming Distributors), pour atteindre le plus large public et optimiser les revenus.

* **Fournisseurs de services de télévision payante (MVPD)**

  Qui cherchent à entrer en contact avec plusieurs propriétaires de contenu (programmeurs) par le biais d’une seule intégration, améliorant ainsi la satisfaction des clients en facilitant l’accès au contenu d’abonnement en ligne.

* **Clients de la télévision payante**

  Qui veulent regarder le contenu pour lequel ils paient déjà à tout moment, n’importe où, sur n’importe quel appareil.

**Programmeurs**

Les programmeurs qui cherchent à maximiser la portée de l&#39;audience et le chiffre d&#39;affaires peuvent :

* Intégrez facilement les principaux fournisseurs de télévision payante sans gérer plusieurs connexions directes.
* Élargissez leur audience en assurant l’authentification auprès de tous les principaux fournisseurs et plateformes.
* Protégez le contenu Premium avec une authentification sécurisée, ce qui limite l’accès aux utilisateurs et appareils autorisés.
* Activez l’authentification unique (SSO) pour améliorer l’expérience utilisateur sur les applications et les sites Web.

**Fournisseurs de services de télévision payante (MVPD)**

Les fournisseurs de télévision payante peuvent améliorer l&#39;expérience client et rationaliser leurs opérations en :

* Connexion à plusieurs propriétaires de contenu par le biais d’une seule intégration.
* Offrant une expérience de visionnage transparente de marque sur tous les appareils et plateformes.
* Assurer une authentification sécurisée pour empêcher l’accès non autorisé et gérer les flux simultanés par foyer.

**Clients de la télévision payante**

Les abonnés bénéficient de TV Everywhere, ce qui leur permet de regarder le contenu pour lequel ils paient déjà à tout moment, n&#39;importe où et sur n&#39;importe quel appareil.

### Intégration à l’authentification Adobe Pass {#integrating-adobe-pass-authentication}

Que vous soyez un fournisseur de télévision payante ou un programmeur, l&#39;intégration à l&#39;authentification Adobe Pass nécessite une participation active. Vous trouverez ci-dessous un aperçu général du processus pour les deux rôles.

#### Processus d&#39;intégration du programmeur {#programmer-integration-process}

Des conseils supplémentaires sont disponibles une fois l’intégration officiellement lancée, mais le processus implique généralement les étapes suivantes :

**Conditions préalables**

* Un système de gestion de contenu (CMS).
* Mécanisme de diffusion de contenu pouvant inclure un réseau de diffusion de contenu (CDN) tiers.
* Plateforme vidéo en ligne existante avec un lecteur multimédia incorporé dans un site web ou une application autonome.

**Tâches d&#39;intégration**

* Intégrez le DCR de l’API REST [Adobe Pass Authentication](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).
* Intégrez l’authentification Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md).
* Intégrez l’authentification Adobe Pass [vérificateur de jeton de média](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).
* Développez une interface utilisateur pour les workflows d’authentification, d’autorisation et de déconnexion.

Pour plus d’informations sur le processus d’intégration du programmeur, reportez-vous aux documents [Guide de démarrage rapide du programmeur](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md) .

#### Processus d&#39;intégration du fournisseur de télévision payante {#pay-tv-provider-integration-process}

Des conseils supplémentaires sont disponibles une fois l’intégration officiellement lancée, mais le processus implique généralement les étapes suivantes :

**Conditions préalables**

* Signez le contrat de non-divulgation de l’authentification Adobe Pass (NDA).
* Fournissez à l’équipe d’ingénieurs Adobe Pass Authentication les spécifications du système d’authentification et d’autorisation. Un fournisseur d’identité SAML (IdP) pour l’authentification et un système d’autorisation SOAP sont recommandés pour l’intégration la plus simple.

**Tâches d&#39;intégration**

* Établissez la connectivité avec les serveurs d’authentification Adobe Pass.
* Terminez la mise à jour de l’évaluation et assurez l’assurance qualité.
* Terminez la version de production et assurez l’assurance qualité.

L’intégration standard est gratuite pour les fournisseurs de télévision payante, y compris la documentation et la prise en charge de base des e-mails. Toutefois, les fournisseurs qui ont besoin d&#39;une aide importante ou d&#39;un calendrier accéléré peuvent devoir payer des frais de soutien ou choisir de travailler avec un tiers partenaire expérimenté, comme Synacor.

L’authentification Adobe Pass peut prendre en charge efficacement la logique commerciale spécifique au fournisseur de télévision payante de deux manières principales :

* Pour une logique commerciale autonome appliquée par le fournisseur de télévision payante à la réception d’une demande d’autorisation, Adobe fournit les données nécessaires (par exemple, identifiant unique de l’appareil, adresse IP) pour prendre en charge l’application.
* Pour la logique commerciale qui nécessite l’intervention de l’utilisateur ou une gestion spécifique par Adobe, les propriétés personnalisées peuvent être conservées pour chaque fournisseur de télévision payante. Ces configurations peuvent inclure des workflows prédéfinis déclenchés à des moments spécifiques du processus d’authentification.

Pour plus d&#39;informations sur le processus d&#39;intégration des fournisseurs de télévision payante, consultez les documents Guide de démarrage rapide de [MVPD](/help/authentication/kickstart/mvpd-kickstart-guide.md) et Guide d&#39;intégration de [MVPD](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md).

### Flux de droits {#entitlement-flow}

L’authentification Adobe Pass agit comme un proxy et facilite le flux de droits entre les programmeurs et les MVPD en offrant des interfaces sécurisées et cohérentes aux deux parties.

Pour les programmeurs, l’authentification Adobe Pass fournit des API dans le cadre d’un niveau **Standard** ou **Premium** :

* API d’authentification Adobe Pass standard :
   * [DCR DE L’API REST](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* API d’authentification Premium Adobe Pass :
   * [Réinitialiser l’API Temp Pass](/help/premium-workflow/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [Fonction TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md)
   * [API de dégradation](/help/premium-workflow/degraded-access/degradation-feature.md#degradation-api-access)
      * [Fonctionnalité de dégradation](/help/premium-workflow/degraded-access/degradation-feature.md)
   * [API de surveillance du service de droit](/help/premium-workflow/esm/entitlement-service-monitoring-api.md)

Pour plus d’informations sur le flux de droits, consultez la documentation du [ Guide d’intégration du programmeur ](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md#entitlement-flow).

#### Comprendre les droits {#understanding-entitlements}

La solution Authentification Adobe Pass s’articule autour de la création de droits, c’est-à-dire d’éléments de données spécifiques générés lors de la réussite des workflows d’authentification et d’autorisation. Ces droits donnent accès à du contenu protégé, mais leur durée de vie est limitée. Une fois qu’un droit expire, il doit être renouvelé en réinitialisant les processus d’authentification ou d’autorisation.

Pour plus d’informations sur les droits, reportez-vous aux documents suivants :

* **Profils**

  Une fois l’authentification réussie, l’authentification Adobe Pass crée un profil authentifié (« de longue durée ») associé à l’application, à l’appareil et à l’identifiant du fournisseur de services demandeur (identifiant du demandeur).

* **[Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Une fois l’authentification réussie (et dans certains cas après autorisation également), l’authentification Adobe Pass reçoit des métadonnées de l’utilisateur de la part de MVPD qui peuvent l’exposer à l’application demandeuse.

* **[Décisions](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Une fois l’autorisation réussie, l’authentification Adobe Pass crée une décision d’autorisation (« de longue durée ») associée à l’application, à l’appareil, à l’identifiant du fournisseur de services (identifiant du demandeur) et à une ressource protégée spécifique (identifiant de ressource).

* **[Jetons multimédia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Une fois l’autorisation réussie, l’authentification Adobe Pass crée un jeton multimédia (« de courte durée ») associé à une demande de lecture réussie et prend en charge les bonnes pratiques du secteur pour atténuer la fraude (par exemple, l’extraction de flux).

Les valeurs de durée de vie (« TTL ») des profils et des décisions sont définies en fonction d&#39;accords entre les programmeurs et les fournisseurs de télévision payante, qui conviennent d&#39;une valeur qui sert au mieux toutes les personnes impliquées.

#### Création de l’interface utilisateur {#building-user-interface}

Les programmeurs sont chargés de concevoir et de mettre en œuvre l’interface utilisateur pour le workflow des droits dans leur site web ou application, tandis que certains éléments, tels que le processus de connexion, sont gérés par le fournisseur de télévision payante.

Au minimum, les programmeurs doivent :

* **Implémenter une interface de sélection de fournisseur**
   * Autorisez les nouveaux utilisateurs à identifier leur fournisseur de télévision payante et à se connecter pour la première fois.
   * Certains fournisseurs de télévision payante redirigent les utilisateurs vers une page de connexion externe, tandis que d’autres nécessitent une connexion dans un iframe. Les programmeurs doivent implémenter une fonction de rappel pour générer l’iframe si nécessaire.

* **Gérer une liste des fournisseurs de télévision payante pris en charge**
   * Assurez-vous que les utilisateurs et utilisatrices peuvent uniquement accéder au contenu par le biais de fournisseurs approuvés.

* **Indiquer le statut d’authentification**
   * Indique le moment où un utilisateur est authentifié dans l’application ou sur le site Web.

* **Identification des ressources protégées**
   * Indiquez clairement quel contenu nécessite une autorisation avant l’affichage.
   * Mettez à jour l’interface utilisateur pour refléter une autorisation réussie une fois l’accès accordé.

## FAQ {#faqs}

**Qu&#39;est-ce que la télévision partout ?**

TV Everywhere est une initiative du secteur qui permet aux clients de la télévision payante d’accéder au contenu premium auquel ils sont déjà abonnés sur divers appareils connectés à Internet. Cela inclut les ordinateurs personnels, les tablettes, les smartphones, les consoles de jeux, les décodeurs et les télévisions intelligentes. Le principal défi consiste à assurer un processus d’authentification transparent et convivial, permettant aux clients d’accéder à leur contenu d’abonnement sans multiples connexions ni obstacles techniques.

**Qu’est-ce que l’authentification Adobe Pass et comment prend-elle en charge TV Everywhere ?**

L’authentification Adobe Pass donne vie à TV Everywhere en vérifiant en toute sécurité le droit d’un utilisateur au contenu d’une manière simple et efficace. Il s’agit d’un service hébergé qui facilite une intégration back-end rapide basée sur des règles métier définies à la fois par les programmeurs et les fournisseurs de télévision payante. Il en résulte un délai de mise sur le marché plus rapide pour toutes les parties prenantes, un environnement plus sûr qui réduit au minimum la fraude et une meilleure expérience utilisateur, avec plus de contenu télévisuel accessible sur plusieurs plateformes.

**Comment l’authentification Adobe Pass est-elle diffusée ?**

L’authentification Adobe Pass est fournie en tant que solution SaaS (Software as a Service). Cette approche garantit une communication sécurisée entre les utilisateurs finaux, les programmeurs et les fournisseurs de télévision payante pour la validation des droits sur le contenu.

**En quoi l’authentification Adobe Pass est-elle différente des autres solutions TV Everywhere ?**

L’authentification Adobe Pass offre plusieurs avantages par rapport aux autres solutions :

* **Authentification unique (SSO) transparente** - Contrairement aux intégrations directes avec des fournisseurs individuels, l’authentification Adobe Pass permet une expérience de connexion persistante lorsque les utilisateurs passent d’un site web ou d’une application à l’autre.
* **Large Market Penetration** - Une fois qu’un programmeur s’intègre à l’authentification Adobe Pass, il a immédiatement accès aux opérateurs de télévision payante qui couvrent plus de 90 % des foyers américains.
* **Intégration à l’écosystème Adobe** - Fonctionne en toute transparence avec d’autres solutions Adobe pour la diffusion, la protection et la monétisation de contenu, y compris Adobe Analytics.

**Dans quelle mesure l’authentification Adobe Pass est-elle sécurisée ?**

La sécurité est une priorité absolue. L’authentification Adobe Pass garantit que seuls les utilisateurs et utilisatrices autorisés peuvent accéder au contenu Premium en liant l’accès à l’appareil de l’utilisateur ou de l’utilisatrice. Il fournit également des options pour limiter le nombre de flux, de sessions ou d’appareils simultanés par foyer.

**Quels appareils l’authentification Adobe Pass prend-elle en charge ?**

L’authentification Adobe Pass est conçue pour fonctionner sur un large éventail d’appareils, tels que les appareils web, les appareils mobiles ou les appareils connectés à la télévision.

**L’authentification Adobe Pass coûte-t-elle quelque chose aux utilisateurs finaux ?**

Non. L’authentification Adobe Pass est gratuite pour les utilisateurs finaux.
