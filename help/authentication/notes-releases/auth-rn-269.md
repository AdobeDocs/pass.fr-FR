---
title: Notes de mise à jour de l’authentification Adobe Pass 2.69
description: Notes de mise à jour de l’authentification Adobe Pass 2.69
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---

# Notes de mise à jour de l’authentification Adobe Pass 2.69 {#pt-authn-269-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et Web {#server-side-web-clients-269}

* [Numéro de build](#build-number-269)
* [Nouvelles fonctionnalités](#new-features-269)

### Numéro de build {#build-number-269}

Authentification Adobe Pass : adobe-pass-**2.69**
Date de publication : **02/27/2024 - 02/29/2024**

### Nouvelles fonctionnalités {#new-features-269}

#### Divers {#misc}

* Vulnérabilités de sécurité mises en cache.
* Améliorations apportées à la couche de sécurité Réinitialiser le transfert temporaire avec l’enregistrement du client dynamique (DCR).
   * Vous trouverez plus d’informations ici : [Réinitialiser le transfert temporaire](../integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md)
* Améliorations des rapports d’identification de Platform.

#### API REST {#rest-apis}

* Développement en cours des nouvelles API REST.
   * Une prochaine version dédiée introduira de nouveaux points de terminaison et flux, qui seront annoncés dans une notification distincte.
   * La documentation à jour pour l’utilisation de ces nouvelles API est en cours.

#### Tableau de bord TVE {#tve-dashboard}

* Développement en cours vers le nouveau tableau de bord TVE.
   * Une prochaine version dédiée introduira le nouveau tableau de bord TVE, qui sera annoncé dans une notification distincte.
   * La mise à jour de la documentation pour l’utilisation de ce nouveau tableau de bord TVE est en cours.

#### SDK JavaScript 4.7.0 {#js-sdk}

* Suppression de la version 2.0.1 obsolète du SDK JavaScript Access Enabler en raison de vulnérabilités de sécurité.
   * Suivez le lien pour plus de détails : [Notes de mise à jour de l’authentification Adobe Pass JavaScript 4.7.0](authn-rn-javascript-470.md)
