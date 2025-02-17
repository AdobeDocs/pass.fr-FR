---
title: Notes de mise à jour de l’authentification Adobe Pass 2.66
description: Notes de mise à jour de l’authentification Adobe Pass 2.66
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%

---

# Notes de mise à jour de l’authentification Adobe Pass 2.66 {#authn-266-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et clients web {#server-side-web-clients-266}

* [Numéro de build](#build-number-266)
* [Présentation de la version](#release-overview-266)

### Numéro de build {#build-number-266}

Authentification Adobe Pass : adobe-pass-**2.66.0.1**

Date de publication : **07/11/2023 - 07/13/2023**

### Présentation de la version {#release-overview-266}

Avec cette version, nous avons poursuivi les mises à jour internes pour la nouvelle API REST.

#### Correctifs

* Correction du flux de déconnexion pour les MVPD SAML, où le paramètre RelayState était manquant dans la demande de déconnexion. Nous allons cibler les mises à jour de configuration après la publication afin de restaurer le flux de déconnexion pour les MVPD affectées.
* Ajout de la possibilité de mettre à jour les certificats SSL dans notre configuration pour les points d’entrée d’autorisation SOAP.
* Correction d’un problème dans lequel des données incorrectes étaient consignées dans le champ Programmeur de certains rapports ESM.
