---
title: Notes de mise à jour d’Adobe Pass Authentication 2.64.1
description: Notes de mise à jour d’Adobe Pass Authentication 2.64.1
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Notes de mise à jour d’Adobe Pass Authentication 2.64.1 {#authn-264-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et Web {#server-side-web-clients-2641}

* [Numéro de build](#build-number-2641)
* [Présentation des versions](#release-overview-2641)

### Numéro de build {#build-number-2641}

Authentification Adobe Pass : adobe-pass-**2.64.1**
Date de publication : **01/31/2023 - 02/02/2023**

### Présentation des versions {#release-overview-2641}

Cette version ajoute les éléments suivants :

* La possibilité de bloquer les réponses d’authentification non sollicitées des MVPD où le paramètre &quot;in_response_to&quot; est absent de l’assertion SAML.
* Amélioration de la validation du paramètre redirect_url afin de se conformer aux exigences de sécurité.
* Amélioration de la journalisation des informations sur l’appareil pour les demandes d’authentification du deuxième écran, à l’aide des informations fournies par l’appareil initial.
