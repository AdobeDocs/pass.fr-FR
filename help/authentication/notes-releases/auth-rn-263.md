---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 2.63
description: Notes De Mise À Jour De L’Authentification Adobe Pass 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 2.63 {#authn-263-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-263}

* [Numéro de build](#build-number-263)
* [Présentation de la version](#release-overview-263)

### Numéro de build {#build-number-263}

Authentification Adobe Pass : adobe-pass-**2.63**

Date de publication : **09/20/2022 - 09/22/2022**

### Présentation de la version {#release-overview-263}

#### Améliorer le mécanisme d&#39;identification des plateformes

* À partir de cette version, nous avons amélioré le mécanisme utilisé pour identifier un appareil et ne dépendra plus de l’implémentation côté client. Vous obtiendrez ainsi une granularité plus précise lors de l’application des règles métier au niveau de la plateforme, ainsi qu’une meilleure compréhension des valeurs de trafic dans les rapports ESM.

* Une nouvelle version d’ESM sera bientôt publiée, avec de nouveaux rapports améliorés qui exposent les champs liés à la plateforme.

* Pour plus d’informations sur les modifications prévues, contactez votre équipe.

#### autodégradation de MVPD

Cette fonctionnalité permet aux MVPD de contourner temporairement leurs propres points d’entrée d’authentification et d’autorisation pour les scénarios de trafic élevé lorsque la charge sur ces points d’entrée respectifs devient trop élevée.

#### Ajouter un ID proxy dans l’en-tête des appels d’autorisation

Cette fonction ajoute l’identifiant d’un MVPD proxy Synacor dans l’en-tête de l’appel d’autorisation. Cela permet à Synacor de configurer des règles métier pour chaque proxy individuel (ex. routage vers différents domaines par MVPD proxy).

#### Tableau de bord TVE

Dans cette version, nous avons corrigé un problème en raison duquel les TTL authN ou authZ définies au niveau du MVPD n’étaient pas correctement calculées dans les rapports de configuration.

#### JavaScript SDK 4.6.0

* Suppression de l’utilisation de `eval` fonction, rendant ainsi SDK conforme à la politique de sécurité du contenu.
* Correction d’un problème qui empêchait le flux d’authentification de se terminer avec succès lorsque le stockage local du navigateur était explicitement effacé par une application partenaire.
