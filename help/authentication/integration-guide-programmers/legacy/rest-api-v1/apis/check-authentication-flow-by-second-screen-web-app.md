---
title: Vérifier le flux d’authentification par application web du deuxième écran
description: Vérifier le flux d’authentification par application web du deuxième écran
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# (Hérité) Vérifier le flux d’authentification par application web du deuxième écran {#check-authentication-flow-by-second-screen-web-app}

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

Cette API doit être utilisée par l’application web de connexion du deuxième écran pour confirmer que l’authentification Adobe Pass a confirmé une connexion réussie à partir de MVPD. Nous vous recommandons d’appeler cette API avant d’afficher un message de réussite indiquant à l’utilisateur final de passer à la console de l’appareil pour poursuivre les workflows.


| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{registration code} | Application Web de connexion | 1. code d’enregistrement </br>    (Composant Chemin d’accès)</br>2.  </br> du demandeur    (Obligatoire) | GET | XML ou JSON contenant les détails de l’erreur en cas d’échec. | 200 - Succès   </br>403 - Interdit |

</br>

| Paramètre d’entrée | Description |
| ----------------- | --------------------------------------------------------------------------------------------- |
| code d&#39;enregistrement | Valeur du code d’enregistrement fournie par l’utilisateur au début du flux d’authentification. |
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |


### Exemple de réponse (en cas d’erreur) {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [Retour à la référence de l’API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
