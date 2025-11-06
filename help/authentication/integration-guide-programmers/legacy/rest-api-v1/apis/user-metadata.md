---
title: Métadonnées utilisateur
description: Métadonnées utilisateur
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# Métadonnées utilisateur (héritées) {#user-metadata}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Points d’entrée de l’API REST {#clientless-endpoints}

`<REGGIE_FQDN>` :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>` :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Récupérez les métadonnées que MVPD a partagées à propos de l’utilisateur authentifié.


| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | Service de programmation</br></br>ou</br></br>d’application en flux continu | &#x200B;1. demandeur</br>2.  deviceId (obligatoire)</br>3.  device_info/X-Device-Info (obligatoire)</br>4.  deviceType</br>5.  deviceUser (obsolète)</br>6.  appId (obsolète) | GET | XML ou JSON contenant des métadonnées utilisateur ou des détails d’erreur en cas d’échec. | 200 - Succès<p>404 - Aucune métadonnée trouvée<p>412 - Jeton AuthN non valide (par exemple, jeton expiré) |


| Paramètre d’entrée | Description |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’ID de l’appareil. |
| device_info/<p>X-Device-Info | Informations sur l’appareil de diffusion en continu.</br></br> **Remarque :** cela PEUT être transmis à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations sur la longueur d’une URL GET, il DOIT être transmis en tant que X-Device-Info dans l’en-tête http. </br></br> Voir les détails complets dans [Transmettre les informations sur l’appareil et la connexion](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Type d’appareil (par exemple Roku, PC).</br></br> Si ce paramètre est défini correctement, ESM propose des mesures [ventilées par type d’appareil](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#progr-filter-metrics) lors de l’utilisation de Clientless, de sorte que différents types d’analyse puissent être effectués pour Roku, AppleTV, Xbox, etc.</br></br> Voir [Avantages de l’utilisation du paramètre de type d’appareil sans client dans les mesures Pass](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **Remarque :** l’`device_info` remplace ce paramètre. |
| _deviceUser_ | Identifiant utilisateur de l’appareil.</br></br> **Remarque :** s’il est utilisé, `deviceUser` doit avoir les mêmes valeurs que dans la requête [Créer un code d’enregistrement](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | Nom/ID de l’application. </br></br> **Remarque :** l’`device_info` remplace ce paramètre. S’il est utilisé, `appId` doit avoir les mêmes valeurs que dans la requête [Créer un code d’enregistrement](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

>[!NOTE]
> 
>Les informations de métadonnées utilisateur doivent être disponibles une fois le flux d’authentification terminé. Toutefois, elles peuvent être mises à jour dans le flux d’autorisation, selon le MVPD et le type de métadonnées.




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

À la racine de l’objet se trouvent trois nœuds :

* *updated* : spécifie un horodatage UNIX qui représente la dernière fois que les métadonnées ont été mises à jour. Cette propriété est définie initialement par le serveur lors de la génération des métadonnées pendant la phase d’authentification. Les appels suivants (après la mise à jour des métadonnées) entraînent une incrémentation de l’horodatage.
* *data* : contient les valeurs réelles des métadonnées.
* *encrypted* : tableau répertoriant les propriétés chiffrées. Pour déchiffrer une valeur de métadonnées spécifique, le programmeur doit effectuer un décodage Base64 sur les métadonnées, puis appliquer un déchiffrement RSA sur la valeur obtenue, à l’aide de sa propre clé privée (Adobe chiffre les métadonnées sur le serveur à l’aide du certificat public du programmeur).

En cas d’erreur, le serveur renvoie un objet XML ou JSON spécifiant un message d’erreur détaillé.

Pour plus d’informations, voir [Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

[Retour à la référence de l’API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
