---
title: Fonctionnalités d’intégration MVPD
description: Fonctionnalités d’intégration MVPD
exl-id: fcd65940-9a86-49b2-9d52-9031fb763338
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1733'
ht-degree: 3%

---

# Fonctionnalités d’intégration MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#mvpd-int-features-overview}

L’authentification Adobe Pass prend en charge les nouvelles normes pour TV partout. Elle est conforme à la **spécification OLCA (Online Content Access)** de CableLabs, qui fournit des exigences techniques et une architecture pour la diffusion de vidéo à un client de télévision payante à partir de sources en ligne. Adobe a participé au projet conjoint de test d&#39;interaction de CableLabs en juin 2011 et a passé le processus de test pour une mise en oeuvre par un fournisseur de services.  Adobe est également un membre actif du **OATC (Open Authentication Technical Consortium)** et participe à plusieurs projets de rédaction de spécifications de sous-comités dans le cadre de cet organisme.

Après avoir décrit la conformité OLCA de l’authentification Adobe Pass et la participation de l’Adobe à l’OATC, il est également important de noter que l’authentification Adobe Pass est en fait &quot;indépendante du protocole&quot;.  Mais à ce stade de l&#39;ère TVE, l&#39;authentification Adobe Pass est définitivement orientée vers les normes OLCA.  Les normes définissent les méthodes actuellement convenues pour les différents lecteurs de TVE (programmeurs, distributeurs de programmes audiovisuels et fournisseurs de services) de mettre en oeuvre les fonctionnalités de TVE. La plupart de ces fonctionnalités sont répertoriées dans les tableaux ci-dessous, avec des liens vers des pages connexes qui fournissent des détails et des exemples sur la mise en oeuvre des fonctionnalités.

Les informations contenues dans les tables sont destinées à orienter le processus d’intégration de l’authentification Adobe Pass vers des fonctionnalités homogènes pour toutes les intégrations MVPD. La colonne Priorité classe les fonctionnalités dans A, B et C :

* R : Il s’agit de fonctionnalités &quot;must&quot; pour le déploiement initial d’une intégration.
* B - Il s’agit d’améliorations importantes à l’intégration initiale, à ajouter après le déploiement initial.
* C - Il s’agit d’autres améliorations de l’intégration qui peuvent être mises en oeuvre après les exigences &quot;B&quot;.


## 1. Principales fonctionnalités fonctionnelles {#core-func-features}


| Non. | Fonctionnalité | Description | Priorité | Notes |
|------|----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1,1 | [Connexion initiée par le programmeur](/help/authentication/authn-oauth2-protocol.md) | La visionneuse sélectionne le MVPD et lance le flux d’authentification (AuthN) à partir du site web ou de l’application de la marque Programmer. | A+ |                                                                                                                                                         |
| 1,2 | [Autorisation basée sur les canaux](/help/authentication/authz-usecase.md) | Une fois authentifié, l’autorisation (AuthZ) peut se produire en arrière-plan, en transmettant uniquement un identifiant de canal réseau et un identifiant d’utilisateur au MVPD. | A+ |                                                                                                                                                         |
| 1,3 | Persistance des ID utilisateur | Le MVPD fournit un UserID obscurci et persistant. | A |                                                                                                                                                         |
| 1,4 | [Prise en charge de l’authentification unique](/help/authentication/sso-support.md) | La visionneuse se connecte avec le MVPD sur le site d’une marque, puis peut accéder à une autre marque et ne pas être invitée à se reconnecter. La session AuthN est partagée entre les marques. La prise en charge de l’authentification unique est destinée aux sites web et aux applications mobiles/périphériques.  Pour les MVPD, il est nécessaire de renvoyer un ID utilisateur ou un autre jeton utilisateur pouvant être utilisé pour AuthZ entre les marques. | A |                                                                                                                                                         |
| 1,5 | Expérience utilisateur de connexion optimisée pour les appareils (UX) | Le MVPD prend en charge le redimensionnement de l’écran de connexion afin qu’il s’adapte aux dimensions de l’appareil utilisé par la vue. | A |                                                                                                                                                         |
| 1,6 | Logo du sélecteur MVPD par défaut | Le MVPD fournit une URL vers un logo par défaut de dimensions appropriées (112x33 pixels). | A | Le logo doit être hébergé par le MVPD et doit être mis en cache par le CDN. |
| 1,7 | [Application de plage du fournisseur de services](/help/authentication/serv-provider-scoping.md) | MVPD prend en charge la transmission de l’identifiant de marque (valeur du demandeur) dans la requête AuthN. | A- | Cela permet une expérience de connexion spécifique au fournisseur de services. |
| 1,8 | [Autorisation de plusieurs canaux](/help/authentication/mvpd-preflight-authz.md#preflight-multich-authz) | Le MVPD prend en charge un programmeur qui envoie plusieurs canaux dans une seule demande d’autorisation. | B+ |                                                                                                                                                         |
| 1,9 | Authentification basée sur iFrame ou &quot;pop-up JS&quot; | Le programmeur peut intégrer le flux de connexion dans un iFrame ou une expérience contextuelle au lieu d’une redirection HTTP. | B |                                                                                                                                                         |
| 1,10 | [Déconnexion initiée par le programmeur](/help/authentication/usecase-mvpd-logout.md) | La visionneuse dispose d’une session authentifiée à la fois sur le site ou l’application du programmeur et avec le MVPD. La visionneuse peut lancer une déconnexion fédérée du site du programmeur, et la déconnexion efface également la session sur le portail MVPD. | B | Garantit que les ordinateurs partagés sont plus protégés contre les abus du point de vue du programmeur. |
| 1,11 | [Message d’erreur d’autorisation personnalisée](/help/authentication/error-reporting.md) | Le MVPD transmet sa propre chaîne d’erreur personnalisée, qui est appropriée pour le site ou l’application fédérée du programmeur à afficher à l’utilisateur. | B | Active les scénarios de vente incitative |
| 1,12 | **Métadonnées utilisateur sur la réponse d’authentification** | La réponse MVPD AuthN peut inclure des métadonnées utilisateur qui servent d’indice pour la personnalisation de l’expérience de l’utilisateur au cours du flux de droits. Cette exigence active les indices de contrôle parental du MVPD au programmeur. | B- |                                                                                                                                                         |
| 1,13 | Authentification initiée par MVPD | La visionneuse termine une session AuthN réussie sur le portail MVPD, puis accède au site TVE du programmeur. L’utilisateur n’est pas invité à saisir le sélecteur MVPD et est automatiquement authentifié. | B- |                                                                                                                                                         |
| 1,14 | Portée de l’ID utilisateur | L’ID utilisateur MVPD doit se présenter sous deux formes : l’une est destinée aux programmeurs et l’autre à l’Adobe de portée pour la fraude.  Cela permet à l’Adobe de partager l’identifiant utilisateur MVPD de la portée du programmeur sans autre chiffrement/obscurcissement. | C |                                                                                                                                                         |
| 1,15 | Autorisation basée sur les ressources | Une fois AuthN terminé, AuthZ peut se produire en arrière-plan, en transmettant des données structurées qui peuvent inclure le réseau, l’affichage, la ressource, la notation de contrôle parental, etc., si nécessaire. Cela permet le contrôle parental sur chaque appel AuthZ du programmeur au MVPD. | C |                                                                                                                                                         |
| 1,16 | Déconnexion initiée par MVPD | La visionneuse dispose d’une session authentifiée à la fois sur le site ou l’application du programmeur et avec le MVPD. La visionneuse peut lancer une déconnexion fédérée du site du MVPD qui efface également la session sur tous les sites de programmeurs fédérés. | C |                                                                                                                                                         |
| 1,17 | Obligations d’autorisation | Le MVPD fournit des conditions supplémentaires dans la réponse AuthZ, telles que la journalisation, ou une évaluation de contrôle parental maximale actualisée (OLCA). | C |                                                                                                                                                         |
| 1,18 | Contexte de l’adresse IP | Le MVPD doit transmettre explicitement l’adresse IP. Pour AuthZ, où les appels sont côté serveur, cela donne aux MVPD des informations de suivi sur la provenance de l’utilisateur dans l’appel AuthZ. | C |                                                                                                                                                         |
| 1,19 | Valeur TTL (Token Persistence Time-to-live) Définie dynamiquement | Le MVPD peut définir le TTL du jeton d’authentification Adobe Pass de manière dynamique par le biais des propriétés dans la réponse, de sorte que l’Adobe ne soit pas en boucle pour les modifications TTL. | C- |                                                                                                                                                         |
| 1,20 | Type de périphérique | Le MVPD prend en charge le transfert du type d’appareil dans la requête AuthN ou AuthZ. Cette propriété informe le MVPD de la nature de l’appareil sur lequel le contenu sera consommé, afin qu’il puisse ajuster le TTL du jeton AuthN ou AuthZ pour se conformer à ses propres considérations de sécurité pour l’appareil. | C- | Cela s’avère utile pour la plateforme sans client, où il peut s’avérer difficile d’exposer chaque type d’appareil comme paramètre de configuration du côté de l’authentification Adobe Pass. |



## 2. Fonctions opérationnelles {#operational-features}

| Non | Fonctionnalité | Description | Priorité | Notes |
|-----|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------|
| 2,1 | 24/7 temps de disponibilité | Un accord du MVPD pour s&#39;efforcer de 24/7 le temps et de le mesurer au fil du temps. | A |       |
| 2,2 | Tester Les Informations D’Identification Pour Chaque Programmeur | Le MVPD fournit des comptes de test distincts pour chaque programmeur. | A |       |
| 2,3 | Plan de planification de l’expiration et du renouvellement des certificats | Un plan documenté par MVPD avec un planning pour s’assurer que les certificats SAML sont à jour. | A- |       |
| 2,4 | Latence moyenne inférieure à 250 Ms/Response | Un accord MVPD visant à rechercher une faible latence et à la mesurer au fil du temps. | A- |       |
| 2,5 | Hébergement de son propre logo de sélecteur MVPD | Le MVPD doit héberger son propre logo et il doit être protégé par la mise en cache du réseau de diffusion de contenu. | B |       |
| 2,6 | Plan de réaffectation de l’assistance | Un plan d’escalade documenté par la MVPD, actualisé et révisé au moins trimestriellement. | A |       |
| 2,7 | Plan de protection contre les arrêts | Un projet MVPD co-planifié avec un Adobe sur les mesures de dégradation qui peuvent être utilisées de manière générale si nécessaire. | B |       |
| 2,8 | Maintenir des points de terminaison intermédiaires et de production distincts | Un test d’intégration MVPD qui ne nécessite pas d’usurpation du fichier hôte pour tester l’intégration intermédiaire. | B |       |
| 2,9 | Point de terminaison AQ supplémentaire | Le MVPD conserve une intégration AQ IdP supplémentaire pour le développement commun de nouvelles fonctionnalités.  Cela rendrait moins probable le besoin de demandes spéciales UAT pour le test du certificat de la boutique d’applications. | C |       |





## 3. Fonctions de l’expérience d’authentification {#authn-exp-features}


| Non | Fonctionnalité | Description | Priorité | Notes |
|-----|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 3,1 | Conversion d’authentification au-dessus de l’attente minimale | Le MVPD garantit un taux de conversion minimal pour qu’il soit fonctionnel (5 %) et raisonnable (30 %). | A |                                                                                                                                                           |
| 3,2 | Récupération du mot de passe intégré | Le MVPD fournit un moyen de récupérer les mots de passe intégrés au flux AuthN fédérée. | A |                                                                                                                                                           |
| 3,3 | Enregistrement de compte en ligne | Le MVPD permet de créer un compte intégré au flux AuthN fédérée. | A |                                                                                                                                                           |
| 3,4 | Aide et assistance en ligne | Le MVPD fournit un moyen de fournir de l’aide pendant le flux AuthN fédérée. | A |                                                                                                                                                           |
| 3,5 | Authentification interne par modem | Le MVPD authentifie automatiquement un appareil lorsqu’il se trouve sur le réseau local d’un modèle enregistré (ISP MVPD uniquement). | B | Cette priorité est moindre, car il s&#39;agit d&#39;une optimisation que beaucoup ne peuvent pas encore soutenir et qui présente quelques défis pour limiter les fraudes et contrôler les parents. |
Vous pouvez désormais importer le code du tableau Markdown directement à l’aide de la boîte de dialogue Fichier/Coller les données du tableau... .



## 4. Fonctionnalités d’Analytics {#analytics-features}


| Non | Fonctionnalité | Description | Priorité |
|-----|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 4,1 | Alignement de l’entonnoir de conversion d’authentification | Le MVPD comporte une mesure pour les requêtes AuthN qui commence par la requête AuthN SAML, afin de s’aligner sur les mesures de requête AuthN de l’authentification et du programmeur Adobe Pass. | A |
| 4,2 | Utilisateurs uniques | Utilisateurs authentifiés avec succès et dédupliqués dans un cumul mensuel par jour. | A |
| 4,3 | Comptabilisation des fuseaux horaires | Les rapports comprennent le fuseau horaire du moment où la journée est activée. | A |





## 5. Fonctions d’atténuation de la fraude {#fraud-mitgn-features}


| Non | Fonctionnalité | Description | Priorité | Notes |
|-----|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------|
| 5,1 | Validation de l’abonné vidéo lors de l’authentification | Le MVPD s’assure que l’utilisateur est un abonné vidéo valide pendant le flux AuthN. | A |                                                                                                |
| 5,2 | Limite de taux pour l’authentification / l’autorisation | Le MVPD prend en charge le ralentissement de leurs services web afin de limiter l’utilisation à partir d’un compte utilisateur spécifique, le cas échéant. | B |                                                                                                |
| 5,3 | Identifiant utilisateur persistant avec portée globale à Adobe | Le MVPD s’assure que l’Adobe dispose d’un UserID qui peut être suivi dans les programmeurs pour la fraude. Il n’est pas nécessaire de le fournir directement au client. | B | Spécification OMAP (Online Multimedia Authorization Protocol) et surveillance des utilisateurs réels (RUM). |
| 5,4 | Validation de l’utilisation simultanée | Le MVPD dispose d’un moyen de suivre et de limiter l’utilisation simultanée du compte abonné au-delà d’un seuil d’activité. | B |                                                                                                |

## P1. Fonctionnalités fonctionnelles spécifiques au proxy {#proxy-sp-func-features}

| Non | Description | Priorité | Notes |
|-------|--------------------------------------------------------------------------------|----------|---------------------------------------------------|
| P 1.1 | Proxy chargé de maintenir la précision de la liste à jour des sous-MVPD | A |                                                   |
| P 1.2 | Le proxy fournit un logo de taille appropriée pour chaque sous-MVPD | A | Certaines sous-MVPD actives n’ont pas la bonne taille de logo. |
| P 1.3 | Le proxy fournit une page de connexion de marque appropriée pour chaque sous-MVPD. | A |                                                   |
| P 1.4 | Proxy chargé de cibler des marques de fournisseurs de services spécifiques avec la liste subMVPD | B |                                                   |
| P 1.5 | Le proxy spécifie correctement toutes les propriétés subMVPD (taille d’iFrame, TTL, logo, etc.) | A |                                                   |
| P 1.6 | Le proxy spécifie un entityID distinct | B |                                                   |

## P2. Fonctionnalités opérationnelles du proxy {#proxy-op-features}

| Non | Description | Priorité |
|-------|----------------------------------------------------------------------------------------|----------|
| P 2.1 | La clé API est sécurisée | A |
| P 2.2 | Tester les informations d’identification pour le premier sous-MVPD utilisé dans l’intégration en direct pour chaque nouveau demandeur | A |
| P 2.3 | Les listes subMVPD sont précises et complètes par demandeur | A |
