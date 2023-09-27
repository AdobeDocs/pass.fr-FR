---
title: Notes de mise à jour de l’authentification Adobe Pass 2.66
description: Notes de mise à jour de l’authentification Adobe Pass 2.66
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Notes de mise à jour de l’authentification Adobe Pass 2.66 {#authn-266-rn}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :

## Clients côté serveur et Web {#server-side-web-clients-266}

* [Numéro de build](#build-number-266)
* [Présentation des versions](#release-overview-266)

### Numéro de build {#build-number-266}

Authentification Adobe Pass : adobe-pass-**2.66.0.1**
Date de publication : **07/11/2023 - 07/13/2023**

### Présentation des versions {#release-overview-266}

Avec cette version, nous avons continué les mises à jour internes de la nouvelle API REST.

#### Correctifs {#release-overview-bugfixes-266}

* Correction du flux de déconnexion des MVPD basés sur SAML, où le paramètre RelayState était absent de la requête de déconnexion. Nous ciblerons les mises à jour de configuration après la version pour restaurer le flux de déconnexion des MVPD concernés.
* Ajout de la fonctionnalité de mise à jour des certificats SSL dans notre configuration pour les points de terminaison d’autorisation SOAP.
* Correction d’un cas de coin en raison duquel des données incorrectes étaient consignées dans le champ Programmeur dans certains rapports ESM.
