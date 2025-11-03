---
title: Compréhension des mesures côté serveur
description: Compréhension des mesures côté serveur
exl-id: 516884e9-6b0b-451a-b84a-6514f571aa44
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '2232'
ht-degree: 0%

---

# Compréhension des mesures côté serveur {#understanding-server-side-metrics}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.


## Introduction {#intro}

Ce document décrit les mesures côté serveur de l’authentification Adobe Pass générées par le service Entitlement Service Monitoring (ESM). Il ne décrit pas les mêmes événements que ceux vus du point de vue client (ce que les programmeurs verraient s’ils devaient mettre en œuvre un service de mesure tel qu’Adobe Analytics sur leur page/application).

## Résumé des événements {#events_summary}

Du point de vue de l’authentification Adobe Pass côté serveur, les événements suivants sont générés :

* **Événements générés dans le flux d’authentification**(connexion réelle avec le MVPD)

   * Notification de tentative d’authentification - Celle-ci est générée lorsque l’utilisateur est envoyé sur le site de connexion MVPD.
   * Notification d’authentification en attente - Si l’utilisateur parvient à se connecter avec son MVPD, elle est générée lorsque l’utilisateur est        redirigé vers l’authentification Adobe Pass.
   * Notification d’authentification accordée : cette notification est générée lorsque l’utilisateur est de retour sur le site du programmeur et qu’il a récupéré le jeton d’authentification auprès de l’authentification Adobe Pass.
* **Flux d’autorisation** (simplement vérifier l’autorisation avec une
MVPD)\
  *Prérequis :* un jeton AuthN valide
   * Notification de la tentative d’authentification
   * Notification de l’authentification accordée
* **Demande de lecture réussie**\
  *Prérequis :* jetons AuthN et AuthZ valides
   * Notification d’un contrôle avec l’authentification Adobe Pass
   * Une demande de lecture nécessite à la fois une authentification accordée et une autorisation accordée


Le nombre d’utilisateurs uniques est traité en détail dans la section [Utilisateurs uniques](#unique-users) ci-dessous. À titre d’aperçu, puisque les réponses d’authentification et d’autorisation accordées sont généralement mises en cache, les formules suivantes s’appliquent :

* Nombre de tentatives AuthN \> Nombre de tentatives AuthN accordées
* Nombre de tentatives AuthZ \> Nombre d’authentifications accordées
* Nombre de tentatives AuthZ \> Nombre d’authentifications accordées (généralement)
* Nombre de requêtes de lecture réussies \> Nombre d’authentifications accordées


### Exemple {#example}

L’exemple suivant illustre les mesures côté serveur pour un mois pour .
une marque :

| Mesure | MVPD 1 | MVPD 2 | ... | MVPD n | Total |
| -------------------------- | ------ | ------ | - | ------ | ---------------------------------------------- |
| Authentifications Réussies | 1125 | 2892 |   | 2203 | SUM(MVP1+...MVPD n) |
| Autorisations réussies | 2527 | 5603 |   | 5904 | SUM(MVP1+...MVPD n) |
| Demandes de lecture réussies | 4201 | 10518 |   | 10737 | SUM(MVP1+...MVPD n) |
| Utilisateurs uniques | 1375 | 2400 |   | 2890 | SOMME de tous les utilisateurs pour tous les MVPD dédupliqués\* |
| Tentatives d’authentification | 2147 | 3887 |   | 3108 | SUM(MVP1+...MVPD n) |
| Tentatives d’autorisations | 2889 | 6139 |   | 6039 | SUM(MVP1+...MVPD n) |

</br>

Dans ce cas, la déduplication ne doit avoir aucun effet, car différents utilisateurs de MVPD ne doivent pas recevoir le même ID utilisateur. Lorsque vous effectuez une somme pour deux marques différentes mais le même MVPD, l’effet de déduplication devrait être beaucoup plus important.

## Déclencheurs d’événement {#event_triggers}

### Nouvel utilisateur - Flux complet {#new-user-full-flow}

Le graphique suivant décrit les événements et les étapes pour un utilisateur sans jeton d’authentification (un nouvel utilisateur ou un utilisateur dont le jeton d’authentification a expiré) :



![](/help/authentication/assets/ae-flow-with-events.png)



Le flux implique des allers-retours vers les MVPD pour l’authentification (#5 à \#7) et l’autorisation (\#11).



Une fois le flux terminé, les jetons d’authentification et d’autorisation sont mis en cache sur l’appareil de l’utilisateur. Les valeurs de durée de vie (TTL) des jetons d’authentification sont comprises entre 6 heures et 90 jours. Une expiration de jeton AuthN force automatiquement une expiration de jeton AuthZ. La valeur de durée de vie du jeton d’autorisation est généralement de 24 heures.

| Événements côté serveur déclenchés | <ul><li>Tentative d&#39;authentification, Authentification en attente, Authentification accordée</li><li>Tentative d’autorisation, autorisation accordée</li><li>Demande de lecture réussie</li></ul> |
|---|---|


### Utilisateur récurrent - Jetons AuthZ et AuthN mis en cache

Pour les utilisateurs disposant de jetons AuthZ et AuthN valides mis en cache, les éléments suivants sont requis :
étapes suivantes :


![](/help/authentication/assets/ae-flow-tokens-cached-web.png)



Cette opération se déclenche automatiquement lors de l’appel de `getAuthorization()` et implique uniquement des vérifications avec l’authentification Adobe Pass. Le MVPD n’est pas impliqué dans ce flux.


| Événements côté serveur déclenchés | * Demande de lecture réussie |
|---|---|


### Utilisateur récurrent - Jetons AuthN mis en cache, jeton AuthZ expiré

Pour les utilisateurs qui disposent toujours de jetons AuthN valides, les étapes suivantes se produisent :

![](/help/authentication/assets/ae-flow-authn-token-cached.png)


Ce flux implique un aller-retour au MVPD.


| Événements côté serveur déclenchés | <ul><li>Tentative d’autorisation, Autorisation OK</li><li>Demande de lecture réussie</li> |
|---|---|

## Événements d’authentification {#authn_events}

### Tentative d&#39;authentification {#authentication-attempt}

Comme illustré dans le diagramme ci-dessus, les événements d’authentification ne sont déclenchés que lorsque l’utilisateur effectue un aller-retour vers le MVPD ; les événements d’authentification n’incluent pas les authentifications de jeton mises en cache.

L’événement de tentative d’authentification est déclenché après que l’utilisateur a cliqué sur un MVPD particulier à partir du sélecteur.

* Le premier événement à proximité du côté MVPD est le chargement de la page
* L’authentification Adobe Pass ne compte pas les tentatives répétées de connexion de l’utilisateur à la page MVPD (mot de passe incorrect, réessayez)
* plusieurs tentatives sont comptabilisées comme une seule tentative
* Certains MVPD effectuent également l’autorisation à l’étape d’authentification et l’utilisateur n’est pas redirigé en cas d’échec de l’autorisation.

### Authentification en attente {#authentication-pending}

Cet événement se produit lorsque le processus de redirection vers l’authentification Adobe Pass a démarré.

### Authentification accordée {#authentication-granted}

L’utilisateur est un abonné connu de MVPD, généralement disposant d’un abonnement à la télévision payante, mais parfois uniquement d’un accès Internet. Une authentification réussie peut se produire soit parce que l’utilisateur a saisi explicitement des informations d’identification valides avec son MVPD, soit parce qu’il a précédemment saisi des informations d’identification valides et que « se souvenir de moi » a été coché (et que la session précédente n’a pas expiré).

Le MVPD envoie donc une réponse positive à la demande d’authentification lors de l’authentification Adobe Pass Adobe Pass, puis crée un jeton *AuthN*.

* L’authentification est généralement mise en cache pendant une longue période (un mois ou plus). Pour cette raison, les événements d’authentification ne seront plus présents jusqu’à l’expiration du jeton et le redémarrage du flux.
* Venir d’un autre site/application via l’authentification unique ne déclenchera pas d’événements d’authentification.



### Authentification Comcast {#comcast-authentication}

Comcast a un flux AuthN différent du reste des MVPD.

Les fonctionnalités suivantes décrivent les différences :

* **Comportement des cookies de session** : cette situation entraîne la suppression complète des jetons d’authentification une fois que l’utilisateur a fermé le navigateur. Cette fonctionnalité est uniquement présente sur le web. L’objectif principal est de s’assurer que votre session Comcast n’est pas conservée sur des ordinateurs non sécurisés/partagés. L’impact est qu’il y aura plus de tentatives d’authentification/flux accordés que pour le reste des MVPD.

* **AuthN par ID de demandeur** : la conversion ne permet pas la mise en cache de l’état AuthN d’un ID de demandeur à un autre. Pour cette raison, chaque site /application doit accéder à Comcast pour obtenir un jeton d’authentification. Outre les considérations relatives à l’expérience utilisateur, l’impact, comme ci-dessus, est que davantage de tentatives d’authentification/événements accordés seront générés.

* **Authentification passive** : pour améliorer l’expérience utilisateur, mais
Tout en conservant la fonctionnalité AuthN par ID de demandeur, un flux d’authentification passif se produit dans un iFrame masqué. L’utilisateur ne verra rien, mais les événements seront toujours déclenchés comme auparavant.

Si l’utilisateur clique sur « Se souvenir de moi » sur la page de connexion de Comcast, les visites ultérieures sur cette page (dans un délai de 2 semaines) ne seront qu’une rapide redirection. Sinon, les utilisateurs devront s’authentifier sur la page.

### Échec de l’authentification {#unsuccessful-authentication}

Une authentification infructueuse n’est pas un événement en soi dans l’authentification Adobe Pass, mais peut être calculée comme la différence entre les tentatives et les succès.

Dans la version de mai 2013, l’authentification Adobe Pass ajoutera les codes d’erreur pour les authentifications infructueuses en raison d’erreurs système ou réseau, y compris les erreurs DRM (échec de la liaison du jeton) et les erreurs LSO (pas d’espace pour écrire le jeton, etc.).

### Taux de conversion de l’authentification {#authenitication-conversion-rate}

Une mesure intéressante que les programmeurs peuvent suivre est le taux de conversion d’authentification, calculé comme (requêtes AuthN / AuthN accordé) %.

Quelques notes sur les mesures :

* Puisqu’il s’agit d’une mesure basée sur un événement, elle ne reflète pas vraiment le taux de conversion d’un utilisateur unique (si un utilisateur tente huit fois et réussit la neuvième fois). Le taux de conversion ci-dessus sera donc très mauvais.
* Il n’existe pas (encore) de moyen dans l’authentification Adobe Pass (côté serveur) de calculer une conversion d’authentification basée sur unique.
* Si des reprises d’authentification automatiques sont présentes sur le site/l’application, la mesure ci-dessus sera également biaisée.

## Événements d’autorisation {#authorization_events}

### Tentative d’autorisation {#authorization_attempt}

En plus d’obtenir un jeton d’authentification, les utilisateurs doivent également obtenir un jeton d’autorisation avant de lire le contenu. Cela se produit généralement après l’authentification ou si le jeton d’autorisation expire. Comme cette vérification est effectuée côté serveur (des serveurs d’authentification Adobe Pass aux serveurs MVPD), l’utilisateur n’est pas tenu de faire quoi que ce soit.

### Autorisation Accordée {#authorization-granted}

Une « autorisation accordée » signale que l&#39;abonnement de l&#39;utilisateur authentifié inclut la programmation demandée.

Notez que tous les MVPD ne prennent pas en charge une étape d’autorisation distincte ; pour certaines authentifications, cela équivaut à une autorisation. Le MVPD envoie à l’authentification Adobe Pass une réponse réussie à la requête AuthZ de canal retour et l’authentification Adobe Pass crée un jeton AuthZ.

* Le jeton AuthZ est mis en cache pendant une période, généralement 24 heures. Pendant cette période, aucun événement AuthZ ne sera déclenché.
* Certains MVPD fonctionnent avec des autorisations au niveau des ressources, d’autres avec des autorisations au niveau des canaux. Selon celui qui est utilisé, plus ou moins d’événements AuthZ sont déclenchés. Même pour l’autorisation au niveau du canal, la mise en cache est en place. Ainsi, si la même ressource est demandée dans moins de 24 heures, aucun événement ne sera déclenché.

### Autorisation refusée {#authorization-denied}

Si une autorisation est refusée, l&#39;utilisateur authentifié ne dispose pas d&#39;un abonnement confirmé à la programmation demandée. La cause la plus probable est que le canal ne fait pas partie du package d’abonnement de l’utilisateur ou de l’utilisatrice, mais cela peut également refléter le fait qu’un utilisateur ou une utilisatrice n’a accès qu’à Internet à partir du MVPD.

Pour certains MVPD, les utilisateurs sont authentifiés avec succès même s&#39;ils disposent uniquement d&#39;un abonnement Internet à partir du MVPD (pas d&#39;abonnement à la télévision payante). Dans ce cas, même si le canal pour lequel l’utilisateur demande une autorisation se trouve dans le package de base, l’autorisation sera refusée.

Certains MVPD proposent des messages d’erreur personnalisés pour les refus AuthZ qui peuvent inclure des offres de mise à niveau de leur package.


### Taux de conversion des autorisations {#authorization-conversion-rate}

Le taux de conversion d’authentification peut être calculé comme (requêtes AuthZ / AuthZ accordé) %.

### Demande De Lecture Réussie {#successful-play-request}

Un utilisateur authentifié et autorisé est autorisé à afficher le contenu protégé.

Lors d’une requête de lecture réussie, l’authentification Adobe Pass génère un jeton de média de courte durée affirmant que l’utilisateur est autorisé à visionner la vidéo demandée. Le programmeur utilise ce jeton multimédia pour une validation ultérieure de la visionneuse potentielle. Les jetons multimédia sont suivis en tant que requêtes de lecture réussies.

* L’authentification Adobe Pass ne suit *pas* si la lecture vidéo a réellement commencé après la génération du jeton de média. Par exemple, s’il existe une restriction géographique sur le contenu, la transaction compte toujours comme une demande de lecture réussie, même si le flux ne démarre jamais réellement.
* Étant donné que les jetons AuthN et AuthZ mettent en cache la réponse MVPD pendant un certain temps, l’événement de demande de lecture réussie est l’événement le plus fréquent dans les mesures.

## Utilisateurs uniques {#unique-users}

### Définition {#definition}

Une fois l’authentification réussie, l’authentification Adobe Pass effectue le suivi de l’existence d’un utilisateur unique, en fonction de la valeur de l’identifiant utilisateur MVPD renvoyée.  Cette valeur est basée sur les informations de connexion de l’utilisateur ou de l’utilisatrice, mais elle ne contient aucune information d’identification personnelle.

Cette valeur est également transmise au site/à l’application dans le rappel sendTrackingData.

Cette valeur peut être persistante sur tous les appareils (le MVPD produit la même valeur pour un utilisateur donné, quel que soit l’endroit où se produit la connexion) ou transitoire (pour chaque connexion, une nouvelle valeur est générée, que le MVPD mappe sur son serveur principal. En règle générale, les valeurs fournies par les MVPD pour l’authentification Adobe Pass sont persistantes entre les sessions et les appareils, mais, comme indiqué, la persistance n’est ni garantie ni validée.

Cette valeur est utilisée comme moyen de calculer les utilisateurs uniques. La valeur signalée (par ID de demandeur/intervalle/MVPD) est dédupliquée pour l’intervalle particulier. Ainsi, la somme des utilisateurs uniques par jour est généralement différente de la valeur mensuelle, la valeur mensuelle étant la plus faible.

Ce nombre inclut tous les événements de l’authentification Adobe Pass, moins les tentatives d’authentification (qui n’ont pas d’ID utilisateur), mais y compris les tentatives d’autorisation (et éventuellement les échecs).

### Exemples {#examples}

#### Jour 1 {#day1}

L’utilisateur XYZ se rend sur le site pour regarder une vidéo.

Événements déclenchés :

* Tentative d’authentification (aucun utilisateur unique pour le moment)
* Authentification accordée
   * à ce stade, nous identifions de manière unique l’utilisateur en fonction de ce que le MVPD renvoie. le nombre d’utilisateurs uniques quotidiens est donc augmenté de 1
   * le jeton AuthN est mis en cache pendant 30 jours.
* Tentative d’authentification / événement accordé
   * Jeton AuthZ mis en cache pendant 1 jour
* Événement de demande de lecture réussi

#### Jour 1 (Ultérieur) {#day1-later-on}

L’utilisateur XYZ regarde une autre vidéo.

Événements déclenchés :

* Événement de demande de lecture réussi (les autres sont mis en cache)
* Aucune augmentation des soldes uniques quotidiens ou mensuels

#### Jour 3 {#day3}

L’utilisateur XYZ regarde une autre vidéo.

Événements déclenchés :

* Tentative d’authentification / événement accordé
   * Depuis 1 jour de mise en cache à partir du jour 1 expiré
* Événement de demande de lecture réussi (les autres sont mis en cache)
* Utilisateurs uniques quotidiens augmentés de 1 ; les utilisateurs uniques mensuels sont toujours 1

#### Jour 31 {#day31}

L’utilisateur XYZ regarde une autre vidéo.

Comme au jour 1, car la mise en cache AuthN a expiré.

Si l’autorisation d’un même utilisateur échouait, le nombre mensuel d’utilisateurs uniques serait toujours augmenté de 1, car deux événements contiennent l’ID utilisateur : authentification accordée et tentative d’autorisation.

### Authentification unique (SSO) {#single-sign-on-sso}

Dans certains cas, le nombre d’utilisateurs uniques peut être supérieur au nombre d’authentifications réussies. C’est généralement le cas lorsque de nombreux utilisateurs et utilisatrices arrivent via l’authentification unique à partir d’autres sites/applications et qu’ils n’ont besoin d’obtenir une autorisation que sur le site/l’application en cours.

### Comparaison des utilisateurs uniques côté client et côté serveur {#comparing-client-side-and-server-side-unique-users}

Si la valeur de l’ID utilisateur de `sendTrackingData()` est utilisée côté client pour comptabiliser des utilisateurs uniques, les numéros côté client et côté serveur doivent correspondre.

Si les différences sont majeures, les raisons suivantes expliquent généralement
différence :

* Lecture vidéo unique par rapport à tous les événements uniques. Comme mentionné, l’authentification Adobe Pass comptabilise des utilisateurs uniques pour tous les événements, à l’exception des tentatives d’authentification. Cela signifie que si l’utilisateur s’authentifie uniquement (sur la page) mais ne visionne pas de vidéo, une augmentation du nombre d’utilisateurs et d’utilisatrices uniques est toujours déclenchée.

* Comptage des utilisateurs dont l’autorisation a échoué : l’authentification Adobe Pass comptabilise ces utilisateurs également dans le nombre signalé.

<!--
## Related Information {#related-information}

- [Entitlement Service Monitoring API](/help/premium-workflow/esm/entitlement-service-monitoring-api.md)

-->
