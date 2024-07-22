---
title: Cas d’utilisation des programmeurs
description: Cas d’utilisation des programmeurs
exl-id: 51ca7e4f-b0d8-4e35-8398-2efb4879de2a
source-git-commit: 189d0e5c83d1bc171c6ff3057f22dbf4055ea6b1
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 2%

---

# Cas d’utilisation des programmeurs {#programmer-use-cases}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

Ce document résume les cas d’utilisation de l’intégration du programmeur pris en charge par l’authentification Adobe Pass. Vous pouvez consulter cette page avant de commencer un projet d’intégration pour connaître les fonctionnalités actuellement prises en charge.

## Cas d’utilisation {#use-cases}


### Intégration de base : authentification et autorisation fédérées pour un réseau à canal unique {#basic-integration}

**Priorité** - Élevée

**Ventilation** - Application TVE unique de marque Programmeur avec 1 réseau de canal hébergé dans l’expérience

Cela permet aux programmeurs d’offrir du contenu haut de gamme, dans leur propre application TVE* de marque, avec une vérification fédérée des droits au MVPD. L’ID du demandeur doit correspondre à la marque de l’application qui diffuse le contenu à la visionneuse. Dans ce scénario, il existe une relation de 1 à 1 entre l’ID du demandeur d’authentification Adobe Pass et l’ID de ressource qui est vérifié pour les droits.

>[!NOTE]
>
>L’application TVE est utilisée dans ce document pour faire référence collectivement aux différents types d’applications (applications web, applications mobiles, etc.) pris en charge par l’authentification Adobe Pass. La colonne Plateformes ci-dessous peut contenir des détails sur les plateformes prises en charge pour des cas d’utilisation spécifiques.

#### Cas d’utilisation spécifiques (communs à la plupart des intégrations) {#sp-use-cases-basic-int}

| Priorité | Cas d’utilisation | Description | Plateformes | Notes MVPD |
|:--------:|:-----------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------:|:-----------------------------------------:|
| Elevée | Découverte MVPD à partir de l’application de programmation TVE | L’utilisateur démarre sur l’application TVE de marque du programmeur et est invité à sélectionner son fournisseur MVPD. | Web (SWF/JS)                    Mobile (iOS/Android)                   API sans client (pour le 2e écran) |                                           |
| Elevée | Authentification fédérée à partir de l’application TVE du programmeur | L’utilisateur démarre sur l’application TVE de marque du programmeur et, après avoir sélectionné son fournisseur MVPD, il passe à la page de connexion du MVPD pour saisir ses informations d’identification. | Web (SWF/JS)                    Mobile (iOS/Android) |                                           |
| Elevée | Autorisation à partir de l’application TVE du programmeur | Une fois l’utilisateur authentifié, l’application TVE du programmeur est en mesure d’effectuer des demandes d’autorisation de canal arrière au MVPD pour vérifier les droits de l’utilisateur. En règle générale, il s’agit simplement de vérifier si le réseau de canaux se trouve dans le package d’abonnement MVPD des utilisateurs.                                  Dans ce cas, l’identifiant du demandeur et l’identifiant de ressource correspondent à 1:1. | Toutes les plateformes |                                           |
| Moyenne | Déconnexion à partir de l’application TVE du programmeur | Permet à l’utilisateur de se déconnecter et d’effacer les jetons AuthN/AuthZ de l’authentification Adobe Pass. Dans de nombreux cas, cela déconnecte également l’utilisateur du MVPD. Mais les MVPDs varient selon qu’ils sont pris en charge ou non. Il efface toujours la session d’authentification Adobe Pass et les jetons. | Toutes les plateformes, à l’exception de XBox Native | Plusieurs MVPD ne prennent pas cela en charge. |
| Elevée | Connexion unique sur plusieurs sites et applications | Permet à l’utilisateur de partager la session de connexion sur plusieurs sites et applications sans avoir à se reconnecter. | Toutes les plateformes, à l’exception de l’API sans client | Nécessite au moins SDK 1.7 pour certains MVPD. |

### Une seule application TVE hébergeant plusieurs réseaux de canaux {#single-app-multi-channel}

**Priorité** - Élevée

Permet au programmeur d’agréger plusieurs réseaux de contenu de canal sur la même destination de marque pour ses visionneuses.

#### Cas d’utilisation spécifiques {#sp-use-cases-singl-tve-app}

| Priorité | Cas d’utilisation | Description | Plateformes | Notes MVPD |
|--------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Elevée | Autorisation de canal distinct | L’utilisateur peut regarder le contenu de plusieurs réseaux de canaux au sein de la même application TVE. Le programmeur peut effectuer des appels d’autorisation spécifiques à chaque réseau de canaux pour confirmer les droits des utilisateurs. | Toutes les plateformes | Tous les distributeurs multicanaux de programmes audiovisuels prennent désormais en charge cette fonctionnalité sous une forme ou une autre. |
| Faible | Requête d’autorisation de contrôle en amont | Cela permet au programmeur de vérifier les canaux dont dispose l’utilisateur dans son package au cours d’un seul appel API. Cela est effectué avant les appels AuthZ réels pour filtrer le contenu hors de l’interface utilisateur auquel l’utilisateur n’a pas accès. |               | La plupart des MVPD n’exposent pas encore ces données en tant qu’attributs utilisateur. Par conséquent, Adobe effectue effectivement des appels AuthZ pour les obtenir. En outre, la plupart des MVPD sont limités à 5 à la fois, car ils ne prennent pas en charge plusieurs canaux dans un seul appel.                             Il est très important de vérifier combien de canaux le programmeur doit contrôler en amont. Quel que soit le nombre, nous devrons vérifier que tout va bien avec les distributeurs multicanaux de programmes audiovisuels. La plupart des distributeurs multicanaux de programmes audiovisuels ne prennent actuellement pas en charge plus de 5 canaux (3e trimestre 2013). |

### Autorisation au niveau des ressources {#asset-level-authz}

**Priority** - Low

**Ventilation** - Transmettre Un Identifiant De Ressource Lors D’Une Demande D’Autorisation

**Plateformes** - Toutes les plateformes

#### Cas d’utilisation spécifiques {#sp-use-cases-asset-lvl-authz}

Permet au MVPD d’obtenir des analyses du niveau de ressource pour chaque appel AuthZ. Cela présente l’inconvénient de nier le cache de l’authentification Adobe Pass AuthZ.

| Priorité | Cas d’utilisation | Description | Plateformes | Notes MVPD |
|--------|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|-------------|--------------------------------------|
| Faible | Transfert d’un identifiant de ressource sur une demande d’autorisation | Permet au MVPD d’obtenir des analyses du niveau de ressource pour chaque appel AuthZ.  Comporte l’inconvénient de la négation du cache de l’authentification Adobe Pass AuthZ. | Toutes les plateformes | Actuellement, un seul MVPD prend en charge cette fonctionnalité. |




### Contrôles parentaux {#parental-controls}

**Priority** - Low

Permet l’application des restrictions de compte utilisateur MVPD sur l’application TVE du programmeur.

| Priorité | Cas d’utilisation | Description | Plateformes | Notes MVPD |
|--------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|-----------------------------------|
| Faible | Filtrage du contenu en fonction des attributs de l’utilisateur | Permet au programmeur de vérifier la note maximale autorisée pour un utilisateur avant de rendre la liste du contenu disponible à l’utilisateur. | Web (Flash/JS)                    Mobile (iOS/Android) | Fonctionne uniquement avec un MVPD actuellement. |
| Faible | Transmission du classement du contenu dans la requête AuthZ | Permet au programmeur de transmettre l’évaluation spécifique du contenu que l’utilisateur souhaite regarder dans le cadre de la requête AuthZ au MVPD.                             Lié à #3, car les évaluations se rapportent généralement au niveau de la ressource. | Toutes les plateformes | Fonctionne uniquement avec un MVPD actuellement. |

#### Personnalisation de l’intégration MVPD par marque de programmeur {#mvpd-int-cust-prog-brand}

**Priorité** - Medium

Active l’expérience personnalisée pendant AuthN ou pour les messages d’erreur AuthZ.

| Priorité | Cas d’utilisation | Description | Plateformes | Notes MVPD |
|--------|------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|-----------------------------------------|
| Moyenne | Transmettre l’identifiant du fournisseur de services dans la requête AuthN. | Activez une valorisation de marque spécifique sur la page de connexion MVPD spécifique au fournisseur de services. Activez également la sélection automatique de la valeur par défaut pour correspondre à l’audience, par exemple Espagnol pour Univision. | Toutes les plateformes | Variable par MVPD. Certains ne le supportent pas. |
| Moyenne | Messages d’erreur personnalisés sur la réponse AuthZ | Active les messages d’erreur du programmeur ou de la marque provenant du MVPD qui peuvent inclure un message spécifique pour la mise à niveau avec un lien qui met à niveau le package. | Web, Android, iOS | Variable par MVPD. Certains ne le supportent pas. |


### Cas d’utilisation de périphériques connectés {#connected-devices}

| Priorité | Cas d’utilisation | Description | Plateformes | Notes MVPD |
|--------|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Moyenne | SSO XBox LiveID sur plusieurs applications et consoles | Permet à l’utilisateur de partager une session AuthN entre les applications et entre différentes consoles de jeu, liées à son compte LiveID. | XBox Native SDK | La plupart des MVPD n’aiment pas cela, car le modèle type consiste à lier le jeton à l’appareil, et non à l’utilisateur.                             Nous ne recommandons plus cette approche, si possible. |
| Elevée | Appareil connecté avec des jetons liés à l’appID sur l’appareil | Permet au programmeur de lier le droit MVPD dans le jeton à l’appID sur l’appareil sur lequel il a été émis. | API sans client | L’appareil connecté est ainsi plus étroitement aligné sur la mise en oeuvre standard Pass pour les jetons.                             L’amélioration doit encore être un identifiant à l’échelle de l’appareil. |

### Longueur TTL AuthN spécifique à l’appareil {#authn-ttl-length}

Activez les droits TVE pour les événements spéciaux qui peuvent ne pas être des ressources se trouvant dans la base de données de droits MVPD comme les canaux normaux.

| Priorité | Cas d’utilisation | Description | Plateformes | Notes MVPD |
|--------|------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------|
| Elevée | Définir différentes valeurs TTL par plateforme | Permet au programmeur d’établir une longueur TTL différente pour les appareils web, mobiles et connectés. Actuellement, l’authentification Adobe Pass prend en charge la possibilité d’avoir 3 valeurs TTL distinctes :                                Web (Flash)                    Mobile/HTML5                    Sans client - Appareils connectés |           | Certains MVPD définissent le TTL de manière dynamique. Adobe peut, si nécessaire, remplacer ces paramètres dynamiques à l’aide des paramètres de configuration. |

### Applications spéciales basées sur des événements {#special-event}

**Priority** - Low

Activez les droits TVE pour les événements spéciaux qui peuvent ne pas être des ressources se trouvant dans la base de données de droits MVPD comme les canaux normaux.

| Priorité | Cas d’utilisation | Description | Plateformes | Notes MVPD |
|--------|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|------------------------------------------|
| Faible | Plusieurs canaux comme proxy pour un événement | Cela a été fait pour les Jeux Olympiques, où l&#39;abonné avait besoin de deux canaux différents dans son kit pour y avoir accès. Dans ce cas, l’authentification Adobe Pass a créé un nouvel ID de ressource et tous les MVPD ont effectué le mappage vers les canaux spécifiques de leur côté.  Cela a fonctionné bien avec suffisamment d&#39;préavis. C’était important car la plupart des MVPD ne prennent pas en charge plusieurs appels de ressources. | Toutes les plateformes | Pris en charge par tous les distributeurs multicanaux de programmes audiovisuels avec préavis approprié. |
| Faible | Application d’événement spécial, à l’aide des ressources de canal existantes | Cela a été fait pour March Madness. Le fournisseur de contenu a créé une application avec un nouvel ID de demandeur. Tous les MVPD devaient ajouter la prise en charge du nouvel ID de demandeur dans leur système. Les resourceID étaient des canaux normaux.  Certains MVPD devaient également mapper les canaux comme valides sous le nouveau demandeur, donc plus de temps était nécessaire pour ces cas. | Toutes les plateformes | Pris en charge par tous les distributeurs multicanaux de programmes audiovisuels avec préavis approprié. |
| Faible | requestorID existant, resourceID | Cela a été fait pour le tournoi de golf de Principal. C&#39;était juste un petit événement pendant quelques jours, et les Principal avaient leur propre application mobile qui était bien adaptée pour afficher le contenu. Le programmeur prévoyait de payer pour le trafic d&#39;authentification Adobe Pass et d&#39;utiliser simplement leur requestorID standard et resourceID. La seule astuce était de demander au programmeur de partager un certificat mobile pour la signature requestorID avec les Principal, et de l’ajouter à leur configuration comme certificat de sauvegarde pour ce week-end. | Toutes les plateformes | Aucun impact pour les MVPD |

### Intégration du serveur de contenu {#content-server-integration}

**Priority**- Medium

Activation de la validation du jeton multimédia avant de publier le flux vidéo sur le lecteur client.

| Priorité | Cas d’utilisation | Description | Plateformes | Notes MVPD |
|---------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|----------|
| Elevée | Programmeur Federated Player - Avec autorisation au niveau de la page | Les API d’authentification Adobe Pass sont effectuées dans JavaScript dans la page et le jeton est transmis au lecteur. Le jeton peut être transmis au service de validation de plusieurs façons :                                 Obtention du paramètre sur l’URL du service de validation                    Paramètre d’URL transmis dans la chaîne de requête de l’URL du flux                    API d’interface externe                    FlashVars |           |            |
| Moyenne | Programmeur Federated Player - Avec autorisation de lecteur interne | Les API d’authentification Adobe Pass sont effectuées en ActionScript dans le SWF du lecteur. Le jeton est donc disponible pour le lecteur à partir du rappel. |           |            |
| Elevée | Lecteur synchronisé : hébergé sur MVPD Portal avec autorisation au niveau de la page à l’aide d’un iFrame pour encapsuler le lecteur | Similaire au lecteur avec l’autorisation de niveau page, mais avec l’iFrame de wrapper de page du lecteur dans le portail MVPD. L’authentification doit se produire séparément dans le portail MVPD. |           |                        |


<!--
>[!RELATEDINFORMATION]
>
>* MVPD Integration Features
>* Entitlement Flow
>* Platform / Device Requirements
-->
