---
title: Notes De Mise À Jour De L’Authentification Adobe Pass 2.64.1
description: Notes De Mise À Jour De L’Authentification Adobe Pass 2.64.1
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# Notes De Mise À Jour De L’Authentification Adobe Pass 2.64.1 {#authn-264-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-2641}

* [Numéro de build](#build-number-2641)
* [Présentation de la version](#release-overview-2641)

### Numéro de build {#build-number-2641}

Authentification Adobe Pass : adobe-pass-**2.64.1**

Date de publication : **01/31/2023 - 02/02/2023**

### Présentation de la version {#release-overview-2641}

Cette version ajoute les éléments suivants :

* La possibilité de bloquer les réponses d&#39;authentification non sollicitées des MVPD où le paramètre « in_response_to » est absent de l&#39;assertion SAML.
* Amélioration de la validation du paramètre redirect_url afin de se conformer aux exigences de sécurité.
* L&#39;invention concerne une journalisation améliorée des informations sur le dispositif pour des deuxièmes demandes d&#39;authentification d&#39;écran, utilisant des informations fournies par le dispositif initial.
