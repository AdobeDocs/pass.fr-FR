---
title: Supprimer enregistrement d'enregistrement
description: Supprimer l'enregistrement
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 2%

---

# (Hérité) Supprimer l’enregistrement d’enregistrement {#delete-registration-record}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

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


## Description {#delete-record}

Supprime l’enregistrement du code rég. et libère le code rég. pour réutilisation.

| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Par exemple :</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | Service de programmation</br></br>ou</br></br>d’application en flux continu | 1. ID du demandeur </br>    (Composant Chemin d’accès)</br>2.  Code d’enregistrement </br>    (Composant Chemin) | DELETE | Aucun | 204 |

{style="table-layout:auto"}

</br>

| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| code d&#39;enregistrement | Valeur du code d’enregistrement qui s’afficherait sur l’appareil de diffusion en continu (à saisir dans le flux d’authentification). |

{style="table-layout:auto"}

</br>

### [Retour à la référence de l’API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
