---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 2.69
description: Notes De Mise À Jour De L’Authentification Adobe Pass 2.69
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 2.69 {#pt-authn-269-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-269}

* [Numéro de build](#build-number-269)
* [Nouvelles fonctionnalités](#new-features-269)

### Numéro de build {#build-number-269}

Authentification Adobe Pass : adobe-pass-**2.69**
Date de publication : **02/27/2024 - 02/29/2024**

### Nouvelles fonctionnalités {#new-features-269}

#### Divers {#misc}

* Correctifs de vulnérabilités de sécurité.
* Améliorations de la couche de sécurité Réinitialiser la passe temporaire avec l’enregistrement client dynamique (DCR).
   * Vous trouverez plus d’informations ici : [Fonction TempPass](../integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
* Améliorations apportées à la création de rapports d’identification de Platform.

#### API REST {#rest-apis}

* Développement en cours des nouvelles API REST.
   * Une prochaine version dédiée introduira de nouveaux points d’entrée et flux, qui seront annoncés dans une notification distincte.
   * Mise à jour de la documentation pour l’utilisation de ces nouvelles API en cours.

#### Tableau de bord TVE {#tve-dashboard}

* Développement en cours du nouveau tableau de bord TVE.
   * Une prochaine version dédiée présentera le nouveau tableau de bord TVE, qui sera annoncé dans une notification distincte.
   * Mise à jour de la documentation pour utiliser ce nouveau tableau de bord TVE en cours.

#### JavaScript SDK 4.7.0 {#js-sdk}

* Suppression de la version 2.0.1 obsolète du SDK JavaScript Access Enabler en raison de failles de sécurité.
   * Suivez le lien pour plus d’informations : [Notes de mise à jour de l’authentification Adobe Pass JavaScript 4.7.0](authn-rn-javascript-470.md)
