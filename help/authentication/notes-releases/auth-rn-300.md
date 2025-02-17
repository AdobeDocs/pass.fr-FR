---
title: Notes de mise à jour d’Adobe Pass Authentication 3.0
description: Notes de mise à jour d’Adobe Pass Authentication 3.0
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Notes de mise à jour d’Adobe Pass Authentication 3.0 {#authn-300-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-300}

* [Numéro de build](#build-number-300)
* [Présentation de la version](#release-overview-300)

### Numéro de build {#build-number-300}

Authentification Adobe Pass : adobe-pass-**3.0**

Date de publication : **09/10/2024 - 09/12/2024**

### Présentation de la version {#release-overview-300}

#### API REST v2

##### Code

* À partir de la version adobe-pass-**3.0**, les applications clientes nouvelles et existantes peuvent s’intégrer ou migrer vers la nouvelle API REST v2 pour bénéficier des dernières fonctionnalités d’Adobe Pass.

##### Documentation

* Pour commencer avec la nouvelle API REST v2, reportez-vous aux documents suivants :
   * [API REST v2 - API - Présentation](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [API REST v2 - Flux - Présentation](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* Les URL des documents publics de l’API REST v1 ont été modifiées. Reportez-vous aux documents suivants :
   * [API REST v1 - API - Présentation](../integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
   * [API REST v1 - API - Référence](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### Outils

* Pour tester la nouvelle API REST v2, reportez-vous à la page nouvelle authentification Adobe Pass du site web [Adobe Developer](https://developer.adobe.com/adobe-pass).

#### Correctifs

* Correction d’un problème en raison duquel le paramètre d’URL de redirection n’était pas utilisé lorsqu’il était présent dans la requête de déconnexion.
