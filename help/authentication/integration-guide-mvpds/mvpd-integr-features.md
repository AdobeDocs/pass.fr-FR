---
title: Fonctionnalités d’intégration de MVPD
description: Fonctionnalités d’intégration de MVPD
exl-id: fcd65940-9a86-49b2-9d52-9031fb763338
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1733'
ht-degree: 3%

---

# Fonctionnalités d’intégration de MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#mvpd-int-features-overview}

L’authentification Adobe Pass prend en charge les normes émergentes pour TV Everywhere. Il est conforme à la spécification **CableLabs OLCA (Online Content Access)**, qui fournit les exigences techniques et l&#39;architecture pour la diffusion de vidéo à un client de télévision payante à partir de sources en ligne. En juin 2011, Adobe a participé au projet de test interopt conjoint de CableLabs et a réussi le processus de test pour la mise en œuvre d&#39;un fournisseur de services.  L&#39;Adobe est également un membre actif du **consortium technique de l&#39;authentification ouverte (OATC)** et participe à plusieurs des projets de rédaction de spécifications des sous-comités dans le cadre de cet organisme.

Après avoir décrit la conformité de l’authentification Adobe Pass à l’OLCA et la participation de l’Adobe à l’OATC, il est également important de noter que l’authentification Adobe Pass est en fait « indépendante du protocole ».  Mais à ce stade de l’ère TVE, l’authentification Adobe Pass est définitivement orientée vers les normes OLCA.  Les normes définissent les moyens actuellement convenus pour les différents acteurs du TVE (programmeurs, MVPD et fournisseurs de services) de mettre en œuvre les fonctionnalités du TVE. La plupart de ces fonctionnalités sont répertoriées dans les tableaux ci-dessous, avec des liens vers des pages connexes qui fournissent des détails et des exemples de mise en œuvre des fonctionnalités.

Les informations contenues dans les tableaux sont destinées à orienter le processus d’intégration de l’authentification Adobe Pass vers des fonctionnalités cohérentes dans toutes les intégrations MVPD. La colonne de priorité classe les fonctionnalités en A, B et C :

* A - Il s’agit de fonctionnalités « indispensables » pour le déploiement initial d’une intégration.
* B - Il s’agit d’améliorations importantes de l’intégration initiale, à ajouter après le déploiement initial.
* C - Il s’agit d’autres améliorations de l’intégration qui peuvent être mises en œuvre après les exigences « B ».


## 1. Fonctionnalités principales {#core-func-features}


| Non. | Fonctionnalité | Description | Priorité | Notes |
|------|---------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1,1 | [Connexion initiée par le programmeur](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md) | La visionneuse sélectionne le MVPD et lance le flux d’authentification (AuthN) à partir du site web ou de l’application de la marque de programmeur. | A+ |                                                                                                                                                          |
| 1,2 | [Autorisation basée sur les canaux](/help/authentication/integration-guide-mvpds/authz-usecase.md) | Une fois authentifié, l’autorisation (AuthZ) peut se produire en arrière-plan, avec la transmission d’un identifiant de canal réseau uniquement et d’un identifiant utilisateur au MVPD. | A+ |                                                                                                                                                          |
| 1,3 | Persistance de l’ID utilisateur | Le MVPD fournit un ID d’utilisateur persistant et obscurci. | A |                                                                                                                                                          |
| 1,4 | [Prise en charge de l’authentification unique](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-support.md) | L’observateur se connecte avec le MVPD sur le site d’une marque, puis peut accéder à une autre marque et ne pas être invité à se reconnecter. La session AuthN est partagée entre les marques. La prise en charge de la connexion unique concerne les sites web et les applications mobiles/pour appareils.  Pour les fichiers MVPD, il faut renvoyer un ID utilisateur ou un autre jeton utilisateur qui peut être utilisé pour les authentifications entre les marques. | A |                                                                                                                                                          |
| 1,5 | Expérience utilisateur de connexion optimisée pour les appareils (UX) | Le MVPD prend en charge le redimensionnement de l’écran de connexion afin qu’il s’adapte aux dimensions de l’appareil utilisé par la vue. | A |                                                                                                                                                          |
| 1,6 | Logo par défaut du sélecteur de MVPD | Le MVPD fournit une URL vers un logo par défaut aux dimensions appropriées (112 x 33 pixels). | A | Le logo doit être hébergé par le MVPD et mis en cache par le réseau CDN. |
| 1,7 | [Définition de la portée du fournisseur de services](/help/authentication/integration-guide-mvpds/serv-provider-scoping.md) | MVPD prend en charge la transmission de l’identifiant de marque (valeur du demandeur) dans la requête AuthN. | A- | Cela active une expérience de connexion spécifique au fournisseur de services. |
| 1,8 | [Autorisation de canaux multiples](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#preflight-multich-authz) | Le MVPD prend en charge un programmeur qui envoie plusieurs canaux dans une seule demande d’autorisation. | B+ |                                                                                                                                                          |
| 1,9 | Authentification basée sur iFrame ou sur un pop-up JS | Le programmeur peut intégrer le flux de connexion dans une expérience iFrame ou pop-up au lieu d’une redirection HTTP. | B |                                                                                                                                                          |
| 1,10 | [ Déconnexion initiée par le programmeur ](/help/authentication/integration-guide-mvpds/usecase-mvpd-logout.md) | La visionneuse dispose d’une session authentifiée sur le site ou l’application du programmeur et avec le MVPD. La visionneuse peut lancer une déconnexion fédérée à partir du site du programmeur, et la déconnexion efface également la session sur le portail MVPD. | B | Garantit que les ordinateurs partagés sont mieux protégés contre les utilisations abusives du point de vue du programmeur. |
| 1,11 | [Message d’erreur d’autorisation personnalisée](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) | Le MVPD transmet sa propre chaîne d’erreur personnalisée, qui est appropriée pour que le site fédéré ou l’application du programmeur s’affiche pour l’utilisateur. | B | Active les scénarios de montée en gamme |
| 1,12 | **Métadonnées utilisateur sur la réponse d’authentification** | La réponse AuthN MVPD peut inclure des métadonnées d’utilisateur qui servent d’astuce pour la personnalisation de l’expérience de l’utilisateur pendant le flux de droits. Cette exigence active les indications de contrôle parental du MVPD au programmeur. | B- |                                                                                                                                                          |
| 1,13 | Authentification initiée par MVPD | La visionneuse termine une session AuthN réussie sur le portail MVPD, puis accède au site TVE du programmeur. L’utilisateur n’est pas invité à sélectionner le MVPD et est automatiquement authentifié. | B- |                                                                                                                                                          |
| 1,14 | Définition de la portée de l’ID utilisateur | L’ID d’utilisateur MVPD doit se présenter sous deux formes : une étendue aux programmeurs et l’autre étendue à l’ensemble de l’Adobe pour la fraude.  Cela permet à l’Adobe de partager l’ID utilisateur MVPD à l’échelle du programmeur sans chiffrement ni obscurcissement supplémentaires. | C |                                                                                                                                                          |
| 1,15 | Autorisation basée sur les ressources | Une fois l’authentification terminée, l’authentification peut se produire en arrière-plan, en transmettant des données structurées qui peuvent inclure le réseau, l’affichage, la ressource, l’évaluation du contrôle parental, etc., si nécessaire. Cela active le contrôle parental sur chaque appel AuthZ du programmeur au MVPD. | C |                                                                                                                                                          |
| 1,16 | Déconnexion initiée par MVPD | La visionneuse dispose d’une session authentifiée sur le site ou l’application du programmeur et avec le MVPD. La visionneuse peut lancer une déconnexion fédérée à partir du site de MVPD qui efface également la session sur tous les sites de programmation fédérés. | C |                                                                                                                                                          |
| 1,17 | Obligations D&#39;Autorisation | Le MVPD fournit des conditions supplémentaires dans la réponse AuthZ, telles que la journalisation ou une évaluation maximale du contrôle parental actualisée (OLCA). | C |                                                                                                                                                          |
| 1,18 | Contexte de l’adresse IP | Le MVPD requiert la transmission explicite de l’adresse IP. Pour AuthZ, où les appels sont côté serveur, cela donne au MVPD des informations de suivi des fraudes sur l’origine de l’utilisateur lors de l’appel AuthZ. | C |                                                                                                                                                          |
| 1,19 | Valeur de durée de vie (TTL) de persistance du jeton définie de manière dynamique | Le MVPD peut définir la durée de vie du jeton d’authentification Adobe Pass de manière dynamique par le biais des propriétés de la réponse, de sorte que l’Adobe soit hors de la boucle pour les modifications de durée de vie. | C- |                                                                                                                                                          |
| 1,20 | Type d’appareil | Le MVPD prend en charge la transmission du type d’appareil dans la requête AuthN ou AuthZ. Cette propriété informe le MVPD de la nature de l’appareil sur lequel le contenu sera utilisé, afin qu’il puisse ajuster la durée de vie du jeton AuthN ou AuthZ pour qu’il soit conforme à ses propres considérations de sécurité pour l’appareil. | C- | Cela s’avère utile pour la plateforme sans client, où il peut être difficile d’exposer chaque type d’appareil en tant que paramètre de configuration du côté de l’authentification Adobe Pass. |



## 2. Caractéristiques Opérationnelles {#operational-features}

| Non | Fonctionnalité | Description | Priorité | Notes |
|-----|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------|
| 2,1 | Disponibilité 24h/24 et 7j/7 | Un accord MVPD pour s’efforcer de gagner du temps 24h/24 et 7j/7 et mesurer cela au fil du temps. | A |       |
| 2,2 | Tester Les Informations D’Identification Pour Chaque Programmeur | Le MVPD fournit des comptes de test distincts pour chaque programmeur. | A |       |
| 2,3 | Plan de calendrier d’expiration et de renouvellement de certificat | Un plan documenté par MVPD avec un planning pour s’assurer que les certificats SAML sont à jour. | A- |       |
| 2,4 | Latence Moyenne Inférieure À 250 Ms/Réponse | Un accord MVPD pour lutter contre la faible latence et la mesurer à long terme. | A- |       |
| 2,5 | Hébergement de son propre logo de sélecteur de MVPD | Le MVPD doit héberger son propre logo et il doit être protégé par la mise en cache du réseau CDN. | B |       |
| 2,6 | Plan d’escalade de l’assistance | Un plan d’escalade documenté par MVPD qui est tenu à jour et examiné au moins une fois par trimestre. | A |       |
| 2,7 | Plan de protection contre les pannes | Un coplan MVPD avec l&#39;Adobe sur les mesures de dégradation qui peuvent être utilisées de manière générale si nécessaire. | B |       |
| 2,8 | Tenir à jour des points d’entrée d’évaluation et de production distincts | Test d’intégration MVPD qui ne nécessite pas d’usurpation des fichiers hôtes pour tester l’intégration d’évaluation. | B |       |
| 2,9 | Point d’entrée QA supplémentaire | Le MVPD conserve une intégration QA IdP supplémentaire pour le développement conjoint de nouvelles fonctionnalités.  En prenant en charge cela, il serait moins probable que nous ayons besoin de requêtes spéciales UAT pour les tests de certificat d’app store. | C |       |





## 3. Fonctionnalités de l’expérience d’authentification {#authn-exp-features}


| Non | Fonctionnalité | Description | Priorité | Notes |
|-----|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 3,1 | Conversion De L’Authentification Au-Dessus De L’Attente Minimale | Le MVPD garantit un taux de conversion minimal pour se révéler fonctionnel (5 %) et raisonnable (30 %). | A |                                                                                                                                                           |
| 3,2 | Récupération de mot de passe en ligne | Le MVPD permet de récupérer les mots de passe en ligne dans le flux AuthN fédéré. | A |                                                                                                                                                           |
| 3,3 | Enregistrement de compte intégré | Le MVPD permet de créer un compte intégré au flux d’authentification fédérée. | A |                                                                                                                                                           |
| 3,4 | Aide/assistance en ligne | Le MVPD permet d’apporter une aide pendant le flux d’authentification fédérée. | A |                                                                                                                                                           |
| 3,5 | Authentification à domicile par modem | Le MVPD authentifie automatiquement un appareil lorsqu’il se trouve sur le réseau local d’un modèle enregistré (MVPD du FAI uniquement). | B | Il s’agit d’une priorité inférieure, car il s’agit d’une optimisation que beaucoup ne peuvent pas encore prendre en charge et qui présente certains défis en matière d’atténuation des fraudes et de contrôle parental |

Vous pouvez désormais importer le code du tableau Markdown directement à l’aide de la boîte de dialogue Fichier/Coller les données du tableau... .


## 4. Fonctionnalités d’Analytics {#analytics-features}


| Non | Fonctionnalité | Description | Priorité |
|-----|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 4,1 | Alignement de l’entonnoir de conversion d’authentification | Le MVPD dispose d’une mesure pour les requêtes AuthN qui commence par la requête AuthN SAML afin de s’aligner sur les mesures Authentification Adobe Pass et Requête AuthN du programmeur . | A |
| 4,2 | Utilisateurs uniques | Utilisateurs qui ont été authentifiés avec succès et dédupliqués dans un cumul mensuel sur plusieurs jours. | A |
| 4,3 | Comptabilisation des fuseaux horaires | Les rapports incluent le fuseau horaire du changement de jour. | A |





## 5. Caractéristiques de réduction de la fraude {#fraud-mitgn-features}


| Non | Fonctionnalité | Description | Priorité | Notes |
|-----|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------|
| 5,1 | Validation des abonnés vidéo lors de l’authentification | Le MVPD garantit que l’utilisateur est un abonné vidéo valide pendant le flux AuthN. | A |                                                                                                |
| 5,2 | Limitation du débit pour l’authentification/autorisation | MVPD prend en charge la limitation de ses services web pour limiter l’utilisation d’un compte utilisateur spécifique, le cas échéant. | B |                                                                                                |
| 5,3 | ID d’utilisateur persistant avec étendue globale à Adobe | Le MVPD garantit qu’Adobe dispose d’un ID d’utilisateur qui peut être suivi par les programmeurs pour les cas de fraude. Il n’est pas nécessaire de le fournir directement au client. | B | Spécification du protocole OMAP (Online Multimedia Authorization Protocol) et surveillance réelle des utilisateurs. |
| 5,4 | Validation de l’utilisation simultanée | Le MVPD dispose d’un moyen permettant de suivre et de limiter l’utilisation simultanée du compte d’abonné au-delà d’un seuil professionnel. | B |                                                                                                |

## P1. Fonctionnalités Spécifiques Au Proxy {#proxy-sp-func-features}

| Non | Description | Priorité | Notes |
|-------|--------------------------------------------------------------------------------|----------|---------------------------------------------------|
| P 1.1 | Proxy responsable de la maintenance de l&#39;exactitude de la liste à jour des sousMVPD | A |                                                   |
| P 1.2 | Proxy fournit un logo de taille appropriée pour chaque sous-MVPD | A | Certains sous-MVPD actifs n’ont pas la bonne taille de logo |
| P 1,3 | Le proxy fournit une page de connexion de marque appropriée pour chaque sous-MVPD | A |                                                   |
| P 1,4 | Proxy responsable pour cibler les marques de fournisseurs de services spécifiques avec la liste subMVPD | B |                                                   |
| P 1,5 | Le proxy spécifie correctement toutes les propriétés subMVPD (taille d’iFrame, TTL, logo, etc.) | A |                                                   |
| P 1,6 | Le proxy spécifie un entityID distinct | B |                                                   |

## P2. Fonctionnalités opérationnelles du proxy {#proxy-op-features}

| Non | Description | Priorité |
|-------|----------------------------------------------------------------------------------------|----------|
| P 2.1 | La clé API est sécurisée. | A |
| P 2.2 | Tester les informations d’identification du premier sous-MVPD utilisé dans l’intégration en direct pour chaque nouveau demandeur | A |
| P 2.3 | Les listes subMVPD sont exactes et complètes par demandeur | A |
