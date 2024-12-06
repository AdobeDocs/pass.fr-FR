---
title: Notes de mise à jour de l’authentification Adobe Pass 3.0
description: Notes de mise à jour de l’authentification Adobe Pass 3.0
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Notes de mise à jour de l’authentification Adobe Pass 3.0 {#pt-authn-300-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et Web {#server-side-web-clients-300}

* [Numéro de build](#build-number-300)
* [Présentation des versions](#release-overview-300)

### Numéro de build {#build-number-2651}

Authentification Adobe Pass : adobe-pass-**3.0**
Date de publication : **09/10/2024 - 09/12/2024**

### Nouvelles fonctionnalités {#new-features-300}

#### API REST v2 {#rest-apis}

##### Code

* Depuis la version adobe-pass-**3.0**, les applications clientes nouvelles et existantes peuvent intégrer ou migrer vers la nouvelle API REST v2 pour bénéficier des dernières fonctionnalités d’Adobe Pass.

##### Documentation

* Pour commencer avec la nouvelle API REST v2, consultez les documents suivants :
   * [API REST v2 - API - Présentation](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [API REST v2 - Flux - Aperçu](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* Les URL des documents publics de l&#39;API REST v1 ont été modifiées. Reportez-vous aux documents suivants :
   * [API REST v1 - API - Présentation](../integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md)
   * [API REST v1 - API - Référence](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### Outils

* Pour essayer la nouvelle API REST v2, reportez-vous à la nouvelle page d’authentification Adobe Pass du site web [Adobe Developer](https://developer.adobe.com/adobe-pass).

### Correctifs {#bug-fixes-300}

* Correction d’un problème en raison duquel le paramètre d’URL de redirection n’était pas utilisé lorsqu’il était présent dans la requête de déconnexion.
