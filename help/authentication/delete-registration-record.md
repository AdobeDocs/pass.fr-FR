---
title: Supprimer un enregistrement d’enregistrement
description: Supprimer le enregistrement
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---

# Supprimer un enregistrement d’enregistrement {#delete-registration-record}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par le [mécanisme de limitation](/help/authentication/throttling-mechanism.md)

## Points de terminaison de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Description {#delete-record}

Supprime l’enregistrement du code reg et le libère pour réutilisation.

| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Par exemple :</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | Application de diffusion en continu</br></br>ou</br></br>Service de programmation | 1. ID du demandeur </br>    (Composant Chemin)</br>2.  Code d’enregistrement </br>    (composant Chemin) | DELETE | Aucun | 204 |

{style="table-layout:auto"}

</br>

| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| code d&#39;enregistrement | La valeur du code d’enregistrement qui s’afficherait sur le périphérique de diffusion (à saisir dans le flux d’authentification). |

{style="table-layout:auto"}

</br>

### [Retour à la référence de l’API REST](/help/authentication/rest-api-reference.md)
