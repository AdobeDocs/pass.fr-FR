---
title: Notes de mise à jour de l’authentification Adobe Pass 2.63
description: Notes de mise à jour de l’authentification Adobe Pass 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# Notes de mise à jour de l’authentification Adobe Pass 2.63 {#pt-authn-263-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et Web {#server-side-web-clients-263}

* [Numéro de build](#build-number)
* [Nouvelles fonctionnalités](#new-features)

### Numéro de build {#build-number-263}

Authentification Adobe Pass : adobe-pass-**2,63**
Date de publication : **09/20/2022 - 09/22/2022**

### Nouvelles fonctionnalités {#new-features-263}

#### Améliorer le mécanisme d&#39;identification des plates-formes {#pf-identification-mech}

* À compter de cette version, nous avons amélioré le mécanisme utilisé pour identifier un appareil et ne dépendra plus de l’implémentation côté client. Cela permet de disposer d’une granularité plus précise lors de l’application des règles métier au niveau de la plateforme et d’une meilleure compréhension des valeurs de trafic dans les rapports ESM.

* Une nouvelle version d’ESM sera bientôt disponible, avec de nouveaux rapports améliorés qui exposent les champs liés à la plateforme.

* Pour plus d’informations sur les modifications prévues, contactez votre TAM.

#### Auto-dégradation du MVPD {#mvpd-self-degradation}

Cette fonctionnalité permet aux distributeurs multicanaux de contourner temporairement leurs propres points de terminaison d’authentification et d’autorisation pour les scénarios de trafic élevé lorsque la charge sur ces points de terminaison respectifs devient trop élevée.


#### Ajout d’un identifiant proxy dans l’en-tête des appels d’autorisation {#add-proxied-id}

Cette fonctionnalité ajoute l’identifiant d’un MVPD de syntaxe proxy dans l’en-tête de l’appel d’autorisation. Cela permet à Synacor de configurer des règles de fonctionnement pour chaque proxy individuel (ex. routage vers différents domaines par MVPD (proxy).


#### Tableau de bord TVE {#tve-dashboard}

Dans cette version, nous avons corrigé un problème en raison duquel authN ou authZ TTL définis au niveau MVPD n’étaient pas correctement calculés dans les rapports de configuration.


#### SDK JavaScript 4.6.0 {#js-sdk}

* Suppression de l’utilisation de `eval` , rendant ainsi le SDK compatible avec la stratégie de sécurité du contenu.
* Correction d’un problème qui empêchait la fin du flux d’authentification lorsque le stockage local du navigateur était explicitement effacé par une application partenaire.
