---
title: Transmet temporaire promouvant
description: Transmet temporaire promouvant
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 0%

---

# Transmet temporaire promouvant {#promotional-temp-pass}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Résumé des fonctionnalités {#feature-summary}

La transmission temporaire promotionnelle permet aux programmeurs d’offrir un accès temporaire à leur contenu protégé aux utilisateurs qui n’ont pas d’informations d’identification de compte avec un MVPD.

La vignette promotionnelle temporaire est conçue pour être utilisée pour exécuter des campagnes promotionnelles lorsqu’un utilisateur, après avoir fourni des informations d’identification valides (par exemple, une adresse électronique) au programmeur, pourra utiliser une variable **nombre prédéfini de titres VOD différents pour une période prédéfinie**.

>[!IMPORTANT]
>
>Adobe ne stocke aucune information d’identification personnelle (PII). Par conséquent, le programmeur doit définir un hachage sur les informations fournies par l’utilisateur unique sur les API d’authentification Adobe Pass.

La vignette promotionnelle temporaire est créée sur le dessus de la balise [Temp Pass](/help/authentication/temp-pass.md) , ce qui signifie qu’elle inclut toutes les fonctionnalités de transmission de temporaire.

Une fois le nombre maximal de titres VOD prédéfinis ou la période prédéfinie dépassée, cet utilisateur ne pourra plus accéder au contenu sur le même appareil ou en utilisant les mêmes informations d’identification de l’utilisateur (par exemple, adresse électronique) tant que les jetons d’autorisation ne seront pas effacés du serveur d’authentification Adobe Pass.

>[!NOTE]
>
>Temp Pass fait partie du package de workflow Premium. Contactez votre représentant commercial Adobe Pass si vous souhaitez utiliser cette fonctionnalité.

## Comparaison des transferts temporaires et des transferts temporaires de conversion {#tp-ptp-comparison}

| Temp Pass | Transmet temporaire promouvant |
|----------------------------------|----------------------------------------------------------------------------------------|
| Accès au contenu <ul><li>en fonction du temps</li></ul> | Accès au contenu <ul><li>en fonction du temps</li><li>en fonction du nombre de ressources</li></ul> |
| Sécurité d’accès basée sur <ul><li>identifiant de l’appareil</li></ul> | Sécurité basée sur <ul><li>identifiant de l’appareil</li><li>hachage des informations d’identifiant utilisateur fournies (par exemple, l’e-mail) ;</li></ul> |
| API d’erreur client disponible | API d’erreur client disponible |
| Réinitialisation/purge disponible | Réinitialisation/purge disponible |

## Détails des fonctionnalités {#feature-details}

Cette fonctionnalité permet aux utilisateurs d’accéder au contenu promotionnel à partir d’un appareil spécifique (téléphone et tablette) après avoir fourni des informations uniques, telles que l’adresse électronique, dans l’application du programmeur.

Le programmeur fournira un hachage sur les informations d’identification personnelles de l’utilisateur sur les API d’authentification et d’autorisation. Ce hachage sera utilisé avec l’identifiant de l’appareil pour générer une clé unique permettant d’identifier l’utilisateur et l’appareil.

En fonction de l’identifiant de l’appareil et des informations fournies par l’utilisateur, et en suivant la logique ci-dessous, l’authentification Adobe Pass détermine si l’utilisateur est dans un nouvel essai ou dans un autre :

* nouveau hachage sur les informations fournies par l’utilisateur (par exemple, e-mail), nouvel ID d’appareil => nouvelle version d’évaluation
* hachage existant sur les informations fournies par l’utilisateur (par exemple, e-mail), nouvel ID d’appareil => version d’évaluation existante (avec le hachage existant sur les informations fournies par l’utilisateur (par exemple, e-mail) ;
* nouveau hachage sur les informations fournies par l’utilisateur (par exemple, e-mail), ID d’appareil existant => version d’évaluation existante (avec ID d’appareil existant)
* hachage existant sur les informations fournies par l’utilisateur (par exemple, e-mail), ID d’appareil existant => version d’évaluation existante ;

>[!NOTE]
>La validation et le hachage des informations fournies par l’utilisateur sont gérés par le programmeur, et non par Adobe.

**La fonction de transfert temporaire promotionnel peut être configurée en fonction des propriétés suivantes :**

* Clé d’informations fournie par l’utilisateur (par exemple, courrier électronique)
* Nombre de ressources que l’utilisateur est autorisé à consommer
* TTL : période pendant laquelle l’utilisateur est autorisé à consommer le nombre configuré de ressources.

### Métadonnées utilisateur {#user-metadata}

Pour faciliter la mise en oeuvre de l’application du programmeur, les **les informations de métadonnées utilisateur sont exposées ;** sur la vignette temporaire promotionnelle, avec les clés correspondantes (pour activer les clés, contactez tve-support@adobe.com) :

* **restes_resources**: nombre de ressources restantes que l’utilisateur actuel est autorisé à consommer
* **used_assets**: liste des ressources que l’utilisateur actuel a déjà consommées.
* **expiration_date**: date d’expiration de l’utilisateur actuel

### Comment le temps de visionnage est-il calculé ? {#compute-viewing-time}

La durée de validité d’une transmission temporaire n’est pas corrélée au temps qu’un utilisateur passe à visionner le contenu sur l’application du programmeur. Lors de la demande d’autorisation initiale de l’utilisateur par le biais d’une transmission temporaire, un délai d’expiration est calculé en ajoutant l’heure de requête actuelle initiale au délai d’activation (durée) spécifié par le programmeur.

### Authentification et autorisation {#authn-authz}

Pour les flux de transmission temporaire promotionnelle, l’authentification et l’autorisation ne communiquent pas avec un MVPD réel, **toutes les demandes d’autorisation réussiront** tant que toutes ces conditions sont remplies :

* jetons d’autorisation valides pour les ressources spécifiées
* nombre de **used_assets** est inférieur à la limite définie par le programmeur
* **expiration_date** est située après la date courante.

### Comportement de contrôle en amont {#preflight-beh}

Lorsqu’une demande de contrôle en amont ou de préautorisation est effectuée pour un MVPD de transmission temporaire promotionnelle, la réponse de contrôle en amont correspondante renvoyée contiendra la liste complète des ressources de la demande de contrôle en amont comme contrôle en amont réussi.

La logique sous-jacente à cela est la suivante : les conditions d’autorisation de transmission temporaire de conversion reposent sur la limite de temps et de nombre de ressources plutôt que sur des ressources spécifiques. Plus précisément, tant que la contrainte temporelle est respectée et tant que la limite de ressources n’est pas dépassée, les ressources appelées sont autorisées.

### SSO {#sso}

La connexion unique n’est pas activée pour les instances de la transmission temporaire promotionnelle, qui est configurée avec l’option &quot;Authentification par demandeur&quot; activée. Cela signifie que lorsque l’utilisateur passe de l’application A à une autre application B, toutes deux intégrées avec la même passe temporaire promotionnelle, l’utilisateur ne se connecte pas automatiquement.

### Déconnexion {#logout}

Tous les jetons d’un appareil sont supprimés lors de la déconnexion. Par conséquent, le passage de la transmission temporaire de conversion à un MVPD standard sélectionné par l’utilisateur ne doit pas dépendre de cette mise en oeuvre. Il est recommandé d’utiliser la variable `setSelectedProvider(null)` afin d’effacer l’état de l’application, puis de redémarrer le flux d’authentification, qui offre une meilleure expérience utilisateur.

### Diagramme de flux de transmission temporaire promu {#promo-tempass-flowdia}

![Diagramme de flux de transmission temporaire promu](assets/promo-temp-pass-flow.png)

*Figure : Flux de transmission de la température promotionnelle*

## Mise en oeuvre de la transmission temporaire de conversion {#impl-promo-tempass}

La transmission temporaire promotionnelle requiert les fonctionnalités côté client suivantes :

* **Informations d’identifiant utilisateur, par exemple propagation de l’adresse électronique** (envoi de l’adresse électronique de l’utilisateur sur les flux d’authentification et d’autorisation). Le courrier électronique est requis par l’authentification Adobe Pass pour lier les jetons d’authentification et d’autorisation (comme dans le cas de la fonction `device_ID`, obligatoire pour tous les appels).
* **Authentification forcée** - permettant au programmeur de forcer un flux d’authentification lorsque l’utilisateur est déjà authentifié. Cette fonctionnalité est nécessaire pour forcer l’actualisation des métadonnées utilisateur (clé de métadonnées utilisateur). **used_assets** contient le nombre de ressources disponibles) à chaque démarrage de l’application. Comme l’utilisateur peut se connecter à plusieurs appareils, les métadonnées utilisateur présentes sur l’appareil au démarrage de l’application ne sont pas fiables. Nous devons les mettre à jour afin de refléter l’état actuel de cet utilisateur spécifique (identifié par son adresse électronique).


>[!IMPORTANT]
>L’authentification forcée est possible uniquement sur iOS et Android.
>L’authentification Adobe Pass ne dispose pas d’un mécanisme intégré pour arrêter la diffusion en continu libre après les X minutes. L’authentification Adobe Pass cessera d’être **authorization** et **short media** jetons une fois que l’utilisateur a utilisé les ressources Y gratuites. C’est aux programmeurs de restreindre l’accès une fois que la passe temporaire de promotion arrive à expiration.

## Sécurité {#security}

>[!IMPORTANT]
>Adobe ne stocke aucune information d’identification personnelle (PII). Par conséquent, le programmeur doit définir un hachage sur les informations fournies par l’utilisateur unique sur les API d’authentification Adobe Pass.

**Hachage des informations d’identifiant utilisateur**

Adobe recommande d’utiliser la variable **SHA-2** la famille ou son **SHA-256**, **SHA-512** sur les données avant leur envoi à Adobe.

Par exemple : **SHA-256** over **&quot;user@domain.com&quot;** is **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**.

## Réinitialiser ou purger la transmission temporaire de conversion {#reset-promo-tempass}

Certaines règles de fonctionnement nécessitent une purge régulière de la transmission temporaire promotionnelle. Pour ce faire, l’authentification Adobe Pass fournit aux programmeurs une *public* API web, décrite ci-dessous :

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>Protocole : **https**</li><li>Hôte :<ul><li>Version : **mgmt.auth.adobe.com**</li><li>Préqualification : **mgmt-prequal.auth.adobe.com**</li></ul></li><li>Chemin : **/resettempass/v2/reset**</li><li>Paramètres de requête : **device_id=all&amp;requestor_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>headers : ApiKey : **1232293681726481**</li> <li>Réponse :<ul><li>Succès : **HTTP 204**</li><li>Échec : **HTTP 400** pour les requêtes incorrectes, **HTTP 401** si ApiKey n’est pas spécifié, **HTTP 403** si ApiKey n’est pas valide</li></ul></li></ul> |

Outre les exigences de purge de la transmission temporaire, la transmission temporaire de conversion utilise le hachage sur les informations d’identifiant utilisateur envoyées sous la forme **generic_data** lors de l’authentification et de l’autorisation pour la purge.

Le hachage sera envoyé, et non l’intégralité du fichier JSON :

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### Clients pris en charge {#supported-clients}

| Clients d’authentification Adobe Pass | Transmet temporaire promouvant | Outil Réinitialiser | Prend en charge le code de réponse dédié / l’erreur client |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| Activateur d’accès JS | OUI | OUI | OUI (à partir de la version 3.0.0) |
| IOS client natif | OUI | OUI | OUI (à partir de la version 1.10) |
| Android du client natif | OUI | OUI | OUI |
| API sans client | OUI | OUI | NON |


## Limites {#limitations}

Cette section décrit les limites qui s’appliquent à la mise en oeuvre actuelle de la transmission temporaire de conversion.

### Sans client {#lim-clientless}

**Appareils dynamiques sans identifiant d’appareil unique**

Toutes les applications d’appareils intelligents ne sont pas en mesure de fournir un identifiant d’appareil unique. En l’absence d’un tel identifiant, l’authentification Adobe Pass peut utiliser l’UUID généré par le service de code d’enregistrement Adobe comme identifiant de périphérique unique. Cela signifie que lorsque l’utilisateur se déconnecte, les jetons d’authentification et d’autorisation sont supprimés. Une fois que l’utilisateur tente de s’authentifier à nouveau, cette fois-ci avec différentes informations utilisateur (par exemple, un e-mail), il pourra autoriser à nouveau. Adobe recommande d’ajouter un flux d’interface utilisateur qui ne permettra pas à un utilisateur de &quot;tromper&quot; le système et d’ajouter une logique pour déterminer s’il s’agit d’un nouvel utilisateur demandant un essai ou d’un essai existant.

**Réinitialisation/purge de la transmission temporaire**

Réinitialiser le transfert de température pour les sans client n’est pas disponible dans les cas spécifiques d’Xbox360 et d’Xbox One, car ces plateformes nécessitent une analyse supplémentaire de l’identifiant de périphérique, ce qui n’est pas possible dans l’outil Réinitialiser le transfert de température .

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->
