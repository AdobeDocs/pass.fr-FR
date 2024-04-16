---
title: Glossaire du compte IQ
description: Glossaire de terminologies de produit.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# Concepts de produit et glossaire {#glossary}

## Terminologies courantes dans D2C et TV partout

Les terminologies de produit suivantes et leurs définitions sont communes à toutes les [versions du compte IQ](versions-aiq.md).

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

Un panneau de tableau de bord avec des graphiques qui divisent les scores de partage du segment actuel en catégories de plage de partage très faibles, faibles, modérés, élevés et très élevés.

### [!UICONTROL Action] {#action-def}

Un événement direct ou indirect associé à un événement [Opération](#operation-def) qui affecte les caractéristiques (par exemple, le score de partage ou le nombre d’appareils utilisés) d’un segment d’opération associé.

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

Un panneau de tableau de bord avec des graphiques qui divisent les scores de partage du segment actuel en catégories de plage de partage très faibles, faibles, modérés, élevés et très élevés, ainsi qu’en pourcentage de chaque catégorie de la quantité totale de diffusion en continu pour le segment.

### [!UICONTROL AuthN] {#authn-def}

Nombre de tentatives d’authentification. Une tentative d’authentification est le processus par lequel un utilisateur tente de se connecter avec le service D2C ou MVPD. Pour les utilisateurs de TV partout, l’utilisateur est redirigé vers le MVPD de son choix, où il s’identifie au MVPD - généralement avec un nom d’utilisateur et un mot de passe.

### [!UICONTROL AuthN OK] {#authn-ok-def}

Nombre d’authentifications réussies. Une authentification réussie se produit lorsque l’identification d’un utilisateur est confirmée par un service D2C ou MVPD. Pour les utilisateurs de TV partout, l’utilisateur est redirigé vers l’application ou le site du programmeur.

### [!UICONTROL Cluster] {#cluster-def}

Une grappe est un ensemble d’emplacements et d’appareils. Les grappes sont créées en recherchant des emplacements communs entre les appareils. Les appareils qui ont été vus à un emplacement commun sont considérés comme appartenant au même cluster. Deux périphériques peuvent se trouver dans la même grappe, même s’ils n’ont pas d’emplacements communs, mais peuvent être connectés via les emplacements d’autres périphériques.

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Une grappe qui ne contient aucun appareil statique.

#### [!UICONTROL Static cluster] {#static-cluster-def}

Une grappe comportant au moins un appareil statique.

### [!UICONTROL Concurrency] {#consurrency-def}

Le simultané est défini par deux diffusions (ou plus) lues simultanément ou très proches dans le temps, de sorte que l’intervalle entre elles ne peut pas être justifié en voyageant à une vitesse normale.
L’utilisation simultanée est calculée à l’aide de la vitesse maximale (miles/heure) entre 2 grappes différentes. Un utilisateur est considéré comme ayant une utilisation simultanée s&#39;il a une vitesse supérieure à 124 m/h sur une distance inférieure à 124 miles ou s&#39;il a une vitesse supérieure à 400 m/h sur une distance supérieure à 124 miles. La distance est calculée entre les emplacements de différentes grappes. L’utilisation simultanée est autorisée dans la même grappe.

### [!UICONTROL Device] {#device-def}

Produit matériel vidéo numérique capable de lire du contenu en continu. Par exemple, les smartphones, les ordinateurs portables et de bureau, les consoles de jeux et les téléviseurs intelligents.

### [!UICONTROL Evaluation period] {#evaluation-period-def}

La période d’évaluation correspond au temps entre le début de l’action associée à l’opération et la fin de l’action ou sa mesure.

### [!UICONTROL Geographical Span] {#geographical-span-def}

Distance entre les points les plus éloignés d’un ensemble d’emplacements.

### [!UICONTROL Granularity] {#granularity-def}

En référence à l’intervalle, taille de la période, telle qu’une semaine ou un mois.

### [!UICONTROL IP] {#ip-def}

Adresse du protocole Internet attribuée à un appareil par un fournisseur d’accès Internet. Par exemple, le fournisseur de services câblés et le fournisseur de services mobiles.

### [!UICONTROL Location] {#location-def}

Un point unique sur terre. On parle également de géolocalisation pour une demande de jeu spécifique avec une précision de 1 000 x 1 000 m (un km carré).

### [!UICONTROL Media Company] {#media-company-def}

Media Company est propriétaire d’un groupe de réseaux de médias.

### [!UICONTROL Metric] {#metric}

Mesure est un attribut du compte d’abonné (par exemple, son MVPD, les programmeurs et les canaux du contenu qu’ils diffusent, ainsi que le nombre d’appareils qu’ils utilisent).

### [!UICONTROL Mobile device] {#mobile-device-def}

Un appareil à grande mobilité. Par exemple, téléphone mobile et tablette.

### Opération {#operation-def}

L’opération est un enregistrement créé pour suivre l’effet d’une [action](#action-def) sur un segment associé. Par exemple, une action peut être une limitation du nombre de diffusions simultanées autorisées pour les comptes identifiés par le segment.

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

Une valeur qui aide les utilisateurs à comprendre l’ampleur du partage de mots de passe et leur donne un sentiment d’urgence d’agir dessus.

### [!UICONTROL Play Request] {#play-requests-def}

Équivalent au démarrage d’un flux. Cet événement marque le début d’une diffusion utilisateur en continu de contenu.

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

Également appelé Utilisation des comptes partagés, il s’agit d’une valeur calculée en fonction du nombre de demandes de lecture effectuées par chaque compte, pondéré par la probabilité de partage de chaque compte. Il est également connu sous le nom d’indice de risque de l’utilisation par les comptes partagés.

### [!UICONTROL Segment] {#segmet-def}

Le segment est un ensemble de comptes qui respectent les conditions définies par l’utilisateur et spécifiées par les mesures sélectionnées. Par exemple, &quot;les utilisateurs des régions A, B, C, D ou E qui ont plus de trois appareils&quot;.

### [!UICONTROL Sharing level] {#sharing-level-def}

Également appelé Index de risque - Index de risque des comptes ou comptes partagés, il s’agit d’une valeur calculée sur la base d’une moyenne de la probabilité de partage calculée pour chaque compte du segment actuel qui a diffusé au moins une fois au cours de l’intervalle sélectionné.

### [!UICONTROL Static device] {#static-device-def}

Un appareil à très faible mobilité. Par exemple, console de jeu, décodeur et téléviseur.

### [!UICONTROL Time interval] {#time-interval-def}

Connue également sous le nom de période, c’est la fenêtre de temps qui contient l’activité de requête de lecture représentée dans l’interface utilisateur et les tableaux du début à la fin.

### [!UICONTROL Trend] {#trend-def}

Différence en pourcentage dans la mesure associée (par exemple, le pourcentage du nombre total de requêtes de lecture) entre la période en cours et la période précédente.

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

Nombre de comptes uniques qui ont diffusé en continu au moins une fois au cours d’une période donnée.

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

Plus de 37 demandes de lecture par mois.

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

Moins de 9 demandes de lecture par mois.

#### [!UICONTROL Regular user] {#regular-user-def}

De 9 à 37 demandes de lecture par mois.

### [!UICONTROL Usage Pattern] {#usage-patern-def}

L’un des ensembles fini de libellés de catégorie appliqués à un compte qui caractérise le mieux les utilisateurs du compte en termes de groupes sociaux ou de comportements (par exemple, une petite famille, un voyageur ou un voyageur, de partage sur les réseaux sociaux, etc.).

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

Une catégorie de vidéo est un libellé spécifique qui qualifie la nature de la vidéo. Par exemple, la région de la source, les types de contenu, tels que VOD ou actif, les étiquettes d’événement ou spécifiques, telles que l’équipe.

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

Une catégorie vidéo est définie par la combinaison du programmeur, du canal et du MVPD associé au flux.

### [!UICONTROL Zip Code] {#zip-code-def}

Code postal américain associé à des emplacements aux États-Unis.
<!--calculated metrics-->


## Terminologies spécifiques de TV partout

### [!UICONTROL AuthZ] {#authz-def}

Autorisation ou nombre de demandes d’autorisation. Une demande d’autorisation est le processus par lequel un programmeur demande l’autorisation à un MVPD par Adobe pour commencer la diffusion en continu du contenu demandé par un utilisateur. Le MVPD accorde généralement la demande en fonction des droits de contenu associés à l’abonnement MVPD de l’utilisateur (par exemple, si le canal associé au contenu figure dans l’abonnement de l’utilisateur). Certaines réponses de demande d’autorisation sont mises en cache par Adobe, ce qui permet à l’Adobe de répondre immédiatement sans transmettre la demande au MVPD.

### [!UICONTROL AuthZ OK] {#authz-ok-def}

Nombre d’autorisations réussies.

### [!UICONTROL Channel] {#channel-def}

Le canal, également appelé Propriété, est une source de contenu vidéo liée aux thèmes. représente traditionnellement un flux vidéo continu distinct à adressage numérique à partir d’un MVPD. Le canal est directement mappé à un canal de contenu accessible disponible pour les abonnés via leur décodeur de définition (STB).

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Une valeur calculée pour chacun des indicateurs de risque (comptes, utilisation, total) pour tous les programmeurs et les distributeurs multicanaux de programmes pendant l’intervalle de temps sélectionné.

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

Type d’analyse de partage dans laquelle l’évaluation d’un compte se limite aux événements qui se sont produits directement sur les programmeurs du segment sélectionné.  Normalement, tous les événements de compte sont évalués, ce qui fournit une estimation beaucoup plus précise du partage.  Certaines données MVPD sont structurées de manière à ne permettre qu’une analyse du mode Isolation.

### [!UICONTROL MVPD] {#mvpd-def}

MVPD, également appelé Distributor, est un agrégateur, revendeur et distributeur de contenu vidéo de Media Company.

### [!UICONTROL Programmer] {#programmer-def}

Programmeur, également connu sous le nom de Network, est une société qui est filiale d’une plus grande entreprise (corporation) qui possède et gère un ou plusieurs canaux.

### [!UICONTROL requestorID] {#requestorid-def}

ID utilisé par une société de médias pour s’identifier elle-même ou une filiale à un MVPD.  Selon l’implémentation du programmeur, cela peut correspondre à une société, un programmeur ou un canal Media. Traditionnellement, il est mappé sur un canal.  Avec la création de pseudo-canaux tels que MML (March Madness Live) et des modifications techniques visant à résoudre les limitations de données pilotées par MVPD, requestorID commence à devenir plus associé à Media Company.

### [!UICONTROL resourceID] {#resource-id-def}

Contenu demandé par l’utilisateur final. Traditionnellement, le canal est identifié comme étant associé au contenu demandé par l’utilisateur.  Les améliorations du système permettent à cet identifiant de représenter des programmes spécifiques (par exemple, avec des évaluations spécifiques), l’identifiant continue à identifier le canal associé.


