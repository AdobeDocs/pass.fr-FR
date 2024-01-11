---
title: Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.0.0
description: Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.0.0
source-git-commit: 7057aeda34b4fe0d059912ab0a71ea856427654c
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.0.0 {#javascript-sdk-400-release-notes}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Numéro de build {#build-no-javascript-sdk-400}

Authentification Adobe Pass : JavaScript 4.0.0

Date de publication : **07/05/2018**


## Présentation des versions {#overview-javascript-sdk-400}

* Mise en oeuvre de la prise en charge d’un mécanisme d’enregistrement dynamique de client, un mécanisme de sécurité unifié sur toutes les plateformes. La gestion de l’accès à la sécurité sur toutes les plateformes peut être réalisée via le tableau de bord TVE, par le programmeur lui-même.
* Élimine l’utilisation de tous les cookies tiers et de presque tous les cookies propriétaires pour toutes les fonctionnalités (à l’exception de l’individualisation où un cookie propriétaire est toujours utilisé), qui sera supprimé à l’avenir), ce qui le rend plus compatible et à l’abri du besoin avec les stratégies de navigateur émergentes concernant les cookies tiers, ou les cookies en général (par exemple, ITP de Safari). Notez que la version 3.x précédente fonctionne comme prévu pour les flux heureux en utilisant uniquement des cookies propriétaires, mais cela peut changer avec la publication de nouvelles versions de navigateur.
* Prise en charge de la désactivation de la mise en cache pour les contrôles d’autorisation de contrôle en amont.
* Cette version n’est PAS rétrocompatible et est hébergée sous un nouveau chemin : https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js . Pour bénéficier de ce nouveau SDK JS, un processus de migration est nécessaire de la part des programmeurs afin d’enregistrer leurs applications web à l’aide du nouveau mécanisme d’enregistrement du client dynamique et de pointer vers la nouvelle URL du SDK JS.


## Package de version {#rel-pkg-javascript-sdk-400}

L’URL de production est : https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

L’URL d’évaluation est : https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
