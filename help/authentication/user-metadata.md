---
title: Métadonnées utilisateur
description: Métadonnées utilisateur
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# Métadonnées utilisateur {#user-metadata}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par le [mécanisme de limitation](/help/authentication/throttling-mechanism.md)

## Points de terminaison de l’API REST {#clientless-endpoints}

`<REGGIE_FQDN>` :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>` :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Récupérez les métadonnées que MVPD a partagées à propos de l’utilisateur authentifié.


| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | Application de diffusion en continu</br></br>ou</br></br>Service de programmation | 1. demandeur</br>2.  deviceId (obligatoire)</br>3.  device_info/X-Device-Info (obligatoire)</br>4.  deviceType</br>5.  deviceUser (obsolète)</br>6.  appId (obsolète) | GET | XML ou JSON contenant des métadonnées utilisateur ou des détails d’erreur en cas d’échec. | 200 - Succès<p>404 - Aucune métadonnée trouvée<p>412 - Jeton AuthN non valide (par exemple, jeton expiré) |


| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’identifiant de l’appareil. |
| device_info/<p>X-Device-Info | Informations sur les périphériques de diffusion en continu.</br></br> **Remarque :** Il peut s’agir de transférer device_info comme paramètre d’URL. Toutefois, en raison de la taille potentielle de ce paramètre et des limitations de longueur d’une URL de GET, il doit être transmis en tant que X-Device-Info dans l’en-tête http. </br></br> Consultez les détails complets dans la section [Transmission des informations de périphérique et de connexion](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Type d’appareil (par exemple, Roku, PC).</br></br> Si ce paramètre est défini correctement, ESM offre des mesures [ ventilées par type d’appareil ](/help/authentication/entitlement-service-monitoring-overview.md#progr-filter-metrics) lors de l’utilisation de Clientless, de sorte que différents types d’analyses puissent être effectués pour Roku, AppleTV, Xbox, etc.</br></br> Voir [Avantages de l’utilisation d’un paramètre de type d’appareil sans client dans Pass metrics](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **Remarque :** `device_info` remplace ce paramètre. |
| _deviceUser_ | L&#39;identifiant de l&#39;utilisateur de l&#39;appareil.</br></br> **Remarque :** Si utilisé, `deviceUser` doit avoir les mêmes valeurs que dans la requête [Create Registration Code](/help/authentication/registration-code-request.md) . |
| _appId_ | ID/nom de l’application. </br></br> **Remarque :** `device_info` remplace ce paramètre. S’il est utilisé, `appId` doit avoir les mêmes valeurs que dans la requête [Create Registration Code](/help/authentication/registration-code-request.md) . |

>[!NOTE]
> 
>Les informations de métadonnées utilisateur doivent être disponibles une fois le flux d’authentification terminé, mais peuvent être mises à jour sur le flux d’autorisation, en fonction du MVPD et du type de métadonnées.




## Exemple de réponse {#sample-response}

Après un appel réussi, le serveur répond avec un objet XML (par défaut) ou JSON avec une structure similaire à celle présentée ci-dessous :


```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
              zip: ["12345", "34567"],
              maxRating: { 
                  "MPAA": "PG-13",
                  "VCHIP": "TV-Y", 
                  "URL": "http://exam.pl/e/manage/ratings"
                         },
              householdID: "3456",
              userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
              channelID: ["channel-1", "channel-2"]
              }
    }
```

À la racine de l’objet, il y a trois noeuds :

* *updated* : spécifie un horodatage UNIX qui représente la dernière fois que les métadonnées ont été mises à jour. Cette propriété est définie initialement par le serveur lors de la génération des métadonnées pendant la phase d’authentification. Les appels suivants (une fois les métadonnées mises à jour) génèrent un horodatage incrémenté.
* *data* : contient les valeurs réelles des métadonnées.
* *encrypted* : un tableau répertoriant les propriétés chiffrées. Pour déchiffrer une valeur de métadonnées spécifique, le programmeur doit effectuer un décodage Base64 sur les métadonnées, puis appliquer un déchiffrement RSA sur la valeur obtenue, en utilisant sa propre clé privée (l’Adobe chiffre les métadonnées sur le serveur à l’aide du certificat public du programmeur).

En cas d’erreur, le serveur renvoie un objet XML ou JSON spécifiant un message d’erreur détaillé.

Pour plus d’informations, voir [Métadonnées utilisateur](/help/authentication/user-metadata-feature.md).

[Retour à la référence de l’API REST](/help/authentication/rest-api-reference.md)
