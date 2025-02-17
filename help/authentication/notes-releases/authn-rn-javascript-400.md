---
title: Notes De Mise À Jour De JavaScript 4.0.0 D’Authentification Adobe Pass
description: Notes De Mise À Jour De JavaScript 4.0.0 D’Authentification Adobe Pass
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Notes De Mise À Jour De JavaScript 4.0.0 D’Authentification Adobe Pass {#javascript-sdk-400-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Numéro de build {#build-number-400}

Authentification Adobe Pass : JavaScript 4.0.0

Date De Publication : **07/05/2018**

## Présentation de la version {#release-overview-400}

* Mise en œuvre de la prise en charge du mécanisme d’enregistrement client dynamique, un mécanisme de sécurité unifié sur toutes les plateformes. La gestion de l&#39;accès de sécurité sur les différentes plateformes peut être effectuée via le tableau de bord TVE, par le programmeur lui-même.
* Élimine l’utilisation de tous les cookies tiers et de presque tous les cookies propriétaires pour toutes les fonctionnalités (à l’exception de l’individualisation où un cookie propriétaire est toujours utilisé - sera supprimé à l’avenir), ce qui le rend plus compatible et durable avec les nouvelles politiques de navigateur autour des cookies tiers, ou des cookies en général (par exemple, . ITP de Safari). Notez que la version 3.x précédente fonctionne comme prévu pour les flux heureux en utilisant uniquement des cookies propriétaires, mais cela peut changer avec la publication de nouvelles versions de navigateur.
* Prise en charge de la désactivation de la mise en cache pour les contrôles d’autorisation en amont.
* Cette version n’est PAS rétrocompatible et est hébergée sous un nouveau chemin : https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js . Pour bénéficier de ce nouveau SDK JS, un processus de migration est nécessaire de la part des programmeurs afin d’enregistrer leurs applications web à l’aide du nouveau mécanisme d’enregistrement client dynamique et de pointer vers la nouvelle URL de SDK JS.

## Package de version {#release-package-400}

L’URL de production est : https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

L’URL d’évaluation est : https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
