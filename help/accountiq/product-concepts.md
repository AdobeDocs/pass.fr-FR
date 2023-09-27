---
title: Glossaire du compte IQ
description: Glossaire de terminologies de produit.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1383'
ht-degree: 0%

---

# Concepts de produit et glossaire {#glossary}

Consultez les terminologies de produit suivantes et leurs définitions.

## [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

Un panneau de tableau de bord avec des graphiques qui divisent les segments actuels partageant les scores en catégories de plage de partage très faible, faible, modéré, élevé et très élevé.

## [!UICONTROL Action] {#action-def}

Un événement direct ou indirect associé à un événement [Opération](#operation-def) qui affecte les caractéristiques (par exemple, le score de partage ou le nombre d’appareils utilisés) d’un segment d’opération (ou d’une cohorte) associé.

## [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

Un panneau de tableau de bord avec des graphiques qui divisent les segments actuels partageant les scores en catégories de plage de partage très faible, faible, modéré, élevé et très élevé, ainsi qu’en pourcentage de chaque catégorie de la quantité totale de diffusion pour le segment.

## [!UICONTROL AuthN] {#authn-def}

Authentification ou nombre de tentatives d’authentification. Une tentative d’authentification est le processus par lequel un utilisateur sans état d’authentification actuellement valide est redirigé vers le MVPD de son choix, où il s’identifie au MVPD, généralement avec un nom d’utilisateur et un mot de passe.

## [!UICONTROL AuthN OK] {#authn-ok-def}

Nombre d’authentifications réussies. Une authentification réussie se produit lorsqu’une identification d’utilisateur est confirmée par un MVPD et que l’utilisateur est redirigé vers l’application ou le site de programmation.

## [!UICONTROL AuthZ] {#authz-def}

Autorisation ou nombre de demandes d’autorisation. Une demande d’autorisation est le processus par lequel un programmeur demande l’autorisation à un MVPD par Adobe pour commencer la diffusion en continu du contenu demandé par un utilisateur. Le MVPD accorde généralement la demande en fonction des droits de contenu associés à l’abonnement MVPD de l’utilisateur (par exemple, si le canal associé au contenu figure dans l’abonnement de l’utilisateur). Certaines réponses de demande d’autorisation sont mises en cache par Adobe, ce qui permet à l’Adobe de répondre immédiatement sans transmettre la demande au MVPD.

## [!UICONTROL AuthZ OK] {#authz-ok-def}

Nombre d’autorisations réussies.

## [!UICONTROL Channel] {#channel-def}

Le canal, également appelé Propriété, est une source de contenu vidéo liée aux thèmes. représente traditionnellement un flux vidéo continu distinct à adressage numérique à partir d’un MVPD. Le canal est directement mappé à un canal de contenu accessible disponible pour les abonnés via leur décodeur de définition (STB).

## [!UICONTROL Cluster] {#cluster-def}

Une grappe est un ensemble d’emplacements et d’appareils. Les grappes sont créées en recherchant des emplacements communs entre les appareils. Les appareils qui ont été vus à un emplacement commun sont considérés comme appartenant au même cluster. Deux périphériques peuvent se trouver dans la même grappe, même s’ils n’ont pas d’emplacements communs, mais peuvent être connectés via les emplacements d’autres périphériques.

### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Une grappe qui ne contient aucun appareil statique.

### [!UICONTROL Static cluster] {#static-cluster-def}

Une grappe comportant au moins un appareil statique.

## [!UICONTROL Concurrency] {#consurrency-def}

Le simultané est défini par deux diffusions (ou plus) lues simultanément ou très proches dans le temps, de sorte que l’intervalle entre elles ne peut pas être justifié en voyageant à une vitesse normale.
L’utilisation simultanée est calculée à l’aide de la vitesse maximale (miles/heure) entre 2 grappes différentes. Un utilisateur est considéré comme ayant une utilisation simultanée s&#39;il a une vitesse supérieure à 124 m/h sur une distance inférieure à 124 miles ou s&#39;il a une vitesse supérieure à 400 m/h sur une distance supérieure à 124 miles. La distance est calculée entre les emplacements de différentes grappes. L’utilisation simultanée est autorisée dans la même grappe.

## [!UICONTROL Device] {#device-def}

Produit matériel vidéo numérique capable de lire du contenu TV partout et pris en charge par Adobe Pass. Par exemple, les smartphones, les ordinateurs portables et de bureau, les consoles de jeux et les téléviseurs intelligents.

## [!UICONTROL Geographical Span] {#geographical-span-def}

Distance entre les points les plus éloignés d’un ensemble d’emplacements.

## [!UICONTROL Granularity] {#granularity-def}

En référence à une période, la taille de la période, telle qu’une semaine ou un mois.

## [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Valeur calculée pour chacun des indicateurs de risque (comptes, utilisation, total) pour tous les programmeurs et les distributeurs multicanaux de programmes pendant la période sélectionnée.

## [!UICONTROL IP] {#ip-def}

Adresse du protocole Internet attribuée à un appareil par un fournisseur d’accès Internet. Par exemple, le fournisseur de services câblés et le fournisseur de services mobiles.

## [!UICONTROL Isolation Mode] {#isolation-mode-def}

Type d’analyse de partage dans laquelle l’évaluation d’un compte se limite aux événements qui se sont produits directement sur les programmeurs du segment sélectionné.  Normalement, tous les événements de compte sont évalués, ce qui fournit une estimation beaucoup plus précise du partage.  Certaines données MVPD sont structurées de manière à ne permettre qu’une analyse du mode Isolation.

## [!UICONTROL Location] {#location-def}

Un point unique sur terre. On parle également de géolocalisation pour une demande de lecture spécifique, détectée à l’aide des données Pass, avec une précision de 1000mx1000m (un km carré).

<!-- ### Home location {#home-location-def}

the precision is 0.01 ~ 2000mx2000m (4 square km) - at this moment the home location is detected using an ML algo, based on data provided by two mvpds. The probability is ~ 80%. We are not using the zip code for the majority of the users. Currently, this information is not used in assessing the sharing probability. -->

## [!UICONTROL Media Company] {#media-company-def}

Media Company est propriétaire d’un groupe de réseaux de médias.

## [!UICONTROL Metric] {#metric}

Mesure est un attribut du compte abonné (par exemple, son MVPD, les programmeurs et les canaux du contenu qu’ils diffusent, le nombre d’appareils qu’ils utilisent).

## [!UICONTROL Mobile device] {#mobile-device-def}

Un appareil à grande mobilité. Par exemple, téléphone mobile et tablette.

## [!UICONTROL MVPD] {#mvpd-def}

MVPD, également connu sous le nom de Distributeur, est un agrégateur, un revendeur et un distributeur de contenu vidéo de Media Company.

## Opération {#operation-def}

L’opération est un enregistrement créé pour suivre l’effet d’une [action](#action-def) sur un segment associé. Par exemple, une action peut être une limitation du nombre de diffusions simultanées autorisées pour les comptes identifiés par le segment.

## [!UICONTROL Overall sharing score] {#overall-sharing-score}

Une valeur qui aide les utilisateurs à comprendre l’ampleur du partage de mot de passe sur les propriétés du programmeur ou par les abonnés MVPD et leur donne un sentiment d’urgence d’agir dessus.

<!--**Aggregated Risk Index**
Also known as Risk Index and Sharing Risk Index, it is a value that helps users understand the magnitude of password sharing on Programmer properties or by MVPD subscribers and provides them a sense of urgency to act upon it.-->

<!--**Risk Index - Overall**
A value computed as an average of "Risk Index - Accounts" and the "Risk Index - Usage". Overall Sharing Risk Index-->

## [!UICONTROL Play Request] {#play-requests-def}

Demande effectuée par une application cliente ou un site à l’Adobe pour demander un jeton multimédia afin d’enregistrer et de sécuriser le démarrage d’un flux.

## [!UICONTROL Programmer] {#programmer-def}

Programmeur, également connu sous le nom de Network, est une société qui est filiale d’une plus grande entreprise (corporation) qui possède et gère un ou plusieurs canaux.

## [!UICONTROL requestorID] {#requestorid-def}

ID utilisé par une société de médias pour s’identifier elle-même ou une filiale à un MVPD.  Selon l’implémentation du programmeur, cela peut correspondre à une société, un programmeur ou un canal Media.  L’ID le plus courant utilisé par une société de médias pour s’identifier ou une filiale à un MVPD.  Selon l’implémentation du programmeur, cela peut correspondre à une société, un programmeur ou un canal Media.  Traditionnellement, il est mappé sur un canal.  Avec la création de pseudo-canaux tels que MML (March Madness Live) et des mesures techniques visant à résoudre les limitations de données pilotées par MVPD, requestorID commence à devenir plus associé à Media Company.

## [!UICONTROL resourceID] {#resource-id-def}

Contenu demandé par l’utilisateur final.  Traditionnellement, le canal est identifié comme étant associé au contenu demandé par l’utilisateur.  Les améliorations du système permettent à cet identifiant de représenter des programmes spécifiques (par exemple, avec des évaluations spécifiques), l’identifiant continue à identifier le canal associé.

## [!UICONTROL Risk Index - Usage] {#risk-index-usage}

Également appelé Utilisation des comptes partagés, il s’agit d’une valeur calculée en fonction du nombre de demandes de lecture effectuées par chaque compte, pondéré par la probabilité de partage de chaque compte. Il est également connu sous le nom d’indice de risque de l’utilisation par les comptes partagés.

## [!UICONTROL Segment] {#segmet-def}

Le segment est un ensemble de comptes qui respectent les conditions définies par l’utilisateur par les mesures sélectionnées (par exemple, &quot;utilisateurs des MVPD A, B, C, D ou E qui ont regardé les canaux X, Y ou Z&quot;).

## [!UICONTROL Sharing level] {#sharing-level-def}

Également appelé Index de risque - Index de risque des comptes ou comptes partagés, il s’agit d’une valeur calculée sur la base d’une moyenne de la probabilité de partage calculée pour chaque compte dans l’ensemble des distributeurs multicanaux sélectionnés qui a diffusé en continu à partir d’un des canaux de programmation sélectionnés pendant la période sélectionnée.

## [!UICONTROL Static device] {#static-device-def}

Un appareil à très faible mobilité. Par exemple, console de jeu, décodeur et téléviseur.

## [!UICONTROL Time frame] {#time-frame-def}

Connue également sous le nom de Période ou Emplacement temporel, c’est la fenêtre de temps qui contient l’activité de requête de lecture représentée dans l’interface utilisateur et les tableaux du début à la fin.

## [!UICONTROL Top MVPDs in segment] {#top-mvpds-def}

Les principaux MVPD (au plus 10) dans le segment sélectionné comme mesure par niveau de partage, utilisation des comptes partagés ou note de partage globale.

## [!UICONTROL Trend] {#trend-def}

Différence en pourcentage dans la mesure associée (par exemple, le pourcentage du nombre total de requêtes de lecture) entre la période en cours et la période précédente.

## [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

Le nombre de comptes MVPD uniques pour une période donnée qui ont interagi avec le programmeur TV Everywhere applications ou sites impliquant Adobe Pass pendant une période donnée.  Cette interaction inclut toute activité sur l’application de programmation ou le site qui entraîne un appel à un service Adobe Pass. Par exemple, la vérification de l’état authN ou authZ, l’authentification et l’autorisation. Le nombre total d’abonnés uniques inclut également le nombre de périphériques uniques si l’utilisation par un programmeur de TempPass (aperçu gratuit) d’Adobe fait partie du segment.

## [!UICONTROL Usage] {#usage-defs}

### [!UICONTROL Avid user] {#avid-user-def}

Plus de 37 demandes de lecture par mois.

### [!UICONTROL Infrequent user] {#infrequent-users-def}

Moins de 9 demandes de lecture par mois.

### [!UICONTROL Regular user] {#regular-user-def}

De 9 à 37 demandes de lecture par mois.

## [!UICONTROL Usage Pattern] {#usage-patern-def}

L’un des ensembles fini de libellés de catégorie appliqués à un compte qui caractérise le mieux les utilisateurs du compte en termes de groupes sociaux ou de comportements (par exemple, une petite famille, un voyageur ou un voyageur, de partage sur les réseaux sociaux, etc.).

## [!UICONTROL Zip Code] {#zip-code-def}

Code postal américain associé à des emplacements aux États-Unis.
