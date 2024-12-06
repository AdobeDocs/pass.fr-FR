---
title: Vérification du flux d’authentification par application web de deuxième écran
description: Vérification du flux d’authentification par application web de deuxième écran
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Vérification du flux d’authentification par application web de deuxième écran {#check-authentication-flow-by-second-screen-web-app}

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

Cette API doit être utilisée par la seconde application web de connexion à l’écran pour confirmer que l’authentification Adobe Pass a reconnu la connexion réussie de MVPD. Nous vous recommandons d’appeler cette API avant d’afficher un message de réussite à l’utilisateur final qui lui indique de passer à la console de l’appareil pour continuer les workflows.


| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{code d’enregistrement} | Application Web de connexion | 1. code d&#39;enregistrement </br>    (Composant Chemin)</br>2.  requestor </br>    (obligatoire) | GET | XML ou JSON contenant les détails d’erreur en cas d’échec. | 200 - Succès   </br>403 - Interdit |

</br>

| Paramètre d’entrée | Description |
| ----------------- | --------------------------------------------------------------------------------------------- |
| code d&#39;enregistrement | La valeur du code d’enregistrement fournie par l’utilisateur au début du flux d’authentification. |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |


### Exemple de réponse (en cas d’erreur) {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [Retour à la référence de l’API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
