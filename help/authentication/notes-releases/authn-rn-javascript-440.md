---
title: Notes De Mise À Jour De JavaScript 4.4.0 D’Authentification Adobe Pass
description: Notes De Mise À Jour De JavaScript 4.4.0 D’Authentification Adobe Pass
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Notes De Mise À Jour De JavaScript 4.4.0 D’Authentification Adobe Pass {#javascript-sdk-440-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Numéro de build {#build-number-440}

Authentification Adobe Pass : JavaScript 4.4.0

Date De Publication : **06/22/2021**

## Présentation de la version {#release-overview-440}

### Nouvelles fonctionnalités

Autorisation de contrôle en amont

* Nouvelle API de préautorisation : il s’agit du nouvel appel API à utiliser pour notre fonctionnalité d’autorisation de contrôle en amont, qui introduit également un rapport d’erreur amélioré pour le flux de contrôle en amont.
* Cette fonctionnalité est disponible sur demande, car elle a été activée dans la configuration de Primetime Authentication. Veuillez contacter votre TAM pour plus d’informations sur la manière d’activer cette fonctionnalité.
* API checkPreauthorizedResources obsolète.

Identification de la plateforme

* Ajoute un en-tête AP-SDK-Identifier sur tous les appels SDK pour mieux identifier le type et la version de SDK.

Autres

* Améliorations de l’architecture interne.

### Correctifs

* Correction de la condition de concurrence générée lorsque setRequestor et getAuthentication sont appelés simultanément.
* Correction d’un problème qui empêchait le chargement correct des droits sur les environnements d’évaluation.
* Correction d’un problème qui empêchait le flux de déconnexion en arrière-plan de se terminer sur les navigateurs Safari, l’utilisateur semblait toujours authentifié jusqu’à ce qu’une actualisation de page se produise. Un délai d’expiration a été introduit. Il est actuellement défini sur 30 secondes. Par conséquent, s’il n’y a pas de réponse du serveur d’authentification Primetime pendant cette période, le SDK appelle le rappel setAuthenticationStatus.

## Package de version {#release-package-440}

L’URL de production est : https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

L’URL d’évaluation est : https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
