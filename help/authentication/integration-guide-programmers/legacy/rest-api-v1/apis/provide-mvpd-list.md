---
title: Fournir la liste MVPD
description: Fournir la liste MVPD
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 2%

---

# Fournir la liste MVPD {#provide-mvpd-list}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par le [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Points de terminaison de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Renvoie la liste des MVPD configurés pour le demandeur.

| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>Par exemple :</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Authentification Adobe Pass | 1. Demandeur</br>    (Composant Chemin)</br>_2.  deviceType (obsolète)_ | GET | XML ou JSON contenant la liste des MVPD. | 200 |

{style="table-layout:auto"}


| Paramètre d’entrée | Description |
| --------------- | ------------------------------------------------------------- |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| *deviceType* | Type de périphérique. |

{style="table-layout:auto"}

### Exemple de réponse {#sample-response}

Identique à la réponse XML MVPD existante au servlet /config

Remarque : tous les MVPD configurés pour utiliser la fonction SSO de Platform auront les propriétés supplémentaires suivantes dans leur noeud correspondant (JSON/XML) :

* **enablePlatformServices (boolean):** indicateur indiquant si ce MVPD est intégré via la connexion unique à Platform
* **boardingStatus (string):** Indicateur précisant si le MVPD prend entièrement en charge l’authentification unique par plateforme (SUPPORTED) ou si le MVPD apparaît uniquement dans le sélecteur de plateforme (PICKER)
* **displayInPlatformPicker (booléen) :** si ce MVPD apparaît dans le sélecteur de plateforme
* **platformMappingId (string):** l’identifiant de ce MVPD connu par la plateforme
* **requiredMetadataFields (tableau de chaînes) :** les champs de métadonnées utilisateur attendus pour une connexion réussie
