---
title: Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.4.0
description: Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.4.0
source-git-commit: 7057aeda34b4fe0d059912ab0a71ea856427654c
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.4.0 {#javascript-sdk-440-release-notes}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Numéro de build {#build-no-javascript-sdk-440}

Authentification Adobe Pass : JavaScript 4.4.0

Date de publication : **06/22/2021**


## Présentation des versions {#overview-javascript-sdk-440}

### Nouvelles fonctionnalités {#new-features-javascript-sdk-440}

Autorisation de contrôle en amont

* Nouvelle API de préautorisation : il s’agit du nouvel appel d’API à utiliser pour notre fonctionnalité d’autorisation de contrôle en amont, qui introduit également un reporting d’erreurs amélioré pour le flux de contrôle en amont.
* Cette fonctionnalité est disponible sur demande, car elle doit être activée sur la configuration de l’authentification Primetime. Pour plus d’informations sur l’activation de cette fonctionnalité, contactez votre TAM.
* Obsolète l’API checkPreauthorizedResources.

Identification de la plateforme

* Ajoute l’en-tête AP-SDK-Identifier sur tous les appels SDK pour mieux identifier le type et la version du SDK.

Autres

* Améliorations architecturales internes.


### Correctifs {#bug-fixes-javascript-sdk-440}

* Correction de la condition de concurrence générée lorsque setRequestor et getAuthentication sont appelés simultanément.
* Correction d’un problème qui empêchait le chargement correct des droits dans les environnements d’évaluation.
* Correction d’un problème qui empêchait l’achèvement du flux de déconnexion en arrière-plan dans les navigateurs Safari. L’utilisateur semblait donc toujours authentifié jusqu’à l’actualisation de la page. Un délai d’expiration a été introduit, actuellement défini sur 30 secondes. Par conséquent, si aucune réponse du serveur d’authentification Primetime n’est fournie au cours de cette période, le SDK appelle le rappel setAuthenticationStatus .

## Package de version {#rel-pkg-javascript-sdk-440}

L’URL de production est : https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

L’URL d’évaluation est : https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
