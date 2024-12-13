---
title: Fourniture d’une liste MVPD
description: Fourniture d’une liste MVPD
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 2%

---

# (Hérité) Fournir Une Liste MVPD {#provide-mvpd-list}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Points d’entrée de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Renvoie la liste des fichiers MVPD configurés pour le demandeur.

| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>Par exemple :</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Authentification Adobe Pass | 1. Demandeur </br>    (Composant Chemin d’accès)</br>_2.  deviceType (obsolète)_ | GET | XML ou JSON contenant la liste des fichiers MVPD. | 200 |

{style="table-layout:auto"}


| Paramètre d’entrée | Description |
| --------------- | ------------------------------------------------------------- |
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| *deviceType* | Type d’appareil. |

{style="table-layout:auto"}

### Exemple de réponse {#sample-response}

Identique à la réponse XML MVPD existante au servlet /config

Remarque : tous les MVPD configurés pour utiliser la connexion unique de Platform disposent des propriétés supplémentaires suivantes dans leur nœud correspondant (JSON/XML) :

* **enablePlatformServices (booléen) : indicateur** si ce MVPD est intégré via l’authentification unique de Platform
* **boardingStatus (string) : indicateur** qui signale si le MVPD prend entièrement en charge l’authentification unique de Platform (PRIS EN CHARGE) ou si le MVPD apparaît uniquement dans le sélecteur de plateforme (PICKER)
* **displayInPlatformPicker (booléen) :** ce MVPD doit-il apparaître dans le sélecteur de plateforme ?
* **platformMappingId (string) :** l&#39;identifiant de ce MVPD tel qu&#39;il est connu par la plateforme
* **requiredMetadataFields (tableau de chaînes) :** les champs de métadonnées utilisateur qui devraient être disponibles lors d’une connexion réussie
