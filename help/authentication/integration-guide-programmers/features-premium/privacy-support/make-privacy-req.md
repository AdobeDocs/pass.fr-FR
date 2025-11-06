---
title: Comment effectuer une demande d’accès à des informations personnelles
description: Comment effectuer une demande d’accès à des informations personnelles
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# Comment effectuer une demande d’accès à des informations personnelles {#howto-make-privacy-request}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Identifiants et espaces de noms {#identifier-namespace}

Lors de l’envoi d’une demande d’accès à des informations personnelles ou de suppression, l’application cliente doit inclure les identifiants suivants :

* **mvpdID** - Identifiant unique du MVPD.
* **userID** - Identifie de manière unique l’utilisateur de l’application d’un programmeur, mais provient de MVPD. Consultez Présentation des ID utilisateur dans la Présentation du programmeur.
* **IMSOrgID** : ID d’organisation du service Adobe Experience Cloud Identity Management qui identifie de manière unique le client dans le Adobe Experience Cloud


Veuillez vérifier l’exemple ci-dessous :

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>Les utilisateurs doivent être authentifiés pour pouvoir générer des demandes d’accès à des informations personnelles pour l’authentification Adobe Pass. Dans le cas contraire, les programmeurs doivent trouver d’autres moyens d’extraire l’identifiant utilisateur MVPD.

## Types de demandes {#req-type}

L’authentification Adobe Pass prend en charge les demandes d’accès et de suppression.

### Accès {#access-req}

Pour une demande d’accès :

Nous vous renverrons un fichier JSON qui contient un résumé du nombre total de demandes d’authentification et d’autorisation créées pour ce titulaire de données.
tous ces événements sont filtrés par client.


**Exemple de requête**

Vous devez charger un fichier JSON avec les identifiants d’authentification Adobe Pass pour lesquels vous soumettez la demande d’accès aux données. Pour voir à quoi ressemble un fichier JSON bien formé, consultez cet exemple :

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["access"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Exemple de réponse**

```JSON
{
    "jobId": "d9a6b417-f619-4420-82a3-09f61fa8eff3",
    "requestId": "15765127177927284RX-739",
    "userKey": "John Dow",
    "action": "access",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/16/2019 04:11 PM GMT",
    "lastModifiedDate": "12/16/2019 04:15 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/16/2019 04:15 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-16T16:15:23.4Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "6",
                        "numberOfAuthorizationDecisions": "11"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/d9a6b417-f619-4420-82a3-09f61fa8eff3/d9a6b417-f619-4420-82a3-09f61fa8eff3.zip",
    "regulation": "ccpa"
}
```

### Supprimer {#delete-req}

Vous devez charger un fichier JSON avec les identifiants d’authentification Adobe Pass pour lesquels vous soumettez la demande de suppression de données. Pour voir à quoi ressemble un fichier JSON bien formé, consultez cet exemple :

**Exemple de requête**

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["delete"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Exemple de réponse**

Pour une demande de suppression :

* nous partageons uniquement un accusé de réception indiquant que les données ont été supprimées, et non un fichier agrégé contenant toutes les données supprimées.
* le reçu inclus dans la réponse contient un résumé du nombre total de jetons d’authentification et d’autorisation trouvés pour ce titulaire de données.

```JSON
{
    "jobId": "aab380d1-a0cd-4a0d-ba95-2649ee90c063",
    "requestId": "15759883098453100RX-074",
    "userKey": "John Dow",
    "action": "delete",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/10/2019 02:31 PM GMT",
    "lastModifiedDate": "12/10/2019 02:34 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/10/2019 02:34 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-10T14:34:55.274Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "2",
                        "numberOfAuthorizationDecisions": "3"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/aab380d1-a0cd-4a0d-ba95-2649ee90c063/aab380d1-a0cd-4a0d-ba95-2649ee90c063.zip",
    "regulation": "ccpa"
}
```

## Déclenchement d’une requête {#trigger-req}

Il existe deux options permettant aux clients d’envoyer des demandes d’accès à des informations personnelles à Adobe :

* **manuellement** - en utilisant l’interface utilisateur de [Privacy Service](#privacy-service-ui)
* **automatiquement** - en utilisant l’API [Privacy Service](#privacy-service-api)

### En utilisant l’interface utilisateur de Privacy Service {#privacy-service-ui}

Un [tutoriel complet](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) sur l’accès et l’utilisation de l’interface utilisateur de Privacy Service est disponible en ligne via les services Adobe I/O. De plus, les clients peuvent utiliser ce lien pour accéder à la bibliothèque de vidéos et d’articles sur les réglementations de confidentialité. Cliquez sur le menu Adobe Experience Cloud et RGPD . Un certain nombre de vidéos s’ouvrent : la rubrique « Comment faire pour l’interface utilisateur RGPD » explique comment l’utiliser.

Dans l’interface utilisateur , les clients doivent charger leur propre IMSOrgID et un fichier JSON contenant les détails des requêtes relatives au RGPD pour chaque produit.

### En utilisant l’API Privacy Service {#privacy-service-api}

Adobe Experience Platform Privacy Service fournit une facilitation commune et centralisée des demandes d’accès/de suppression et des demandes d’opposition à la vente de données privées.

La documentation de l’API Privacy Service **** explique en détail comment un client Adobe peut s’intégrer à l’API Adobe.

**Visualiser les appels d’API avec Postman (un logiciel tiers gratuit) :**

* [Collection de Postman de l’API Privacy Service sur GitHub](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Guide vidéo pour la création de l’environnement Postman](https://video.tv.adobe.com/v/28832)
* [Procédure d’importation d’environnements et de collections dans Postman](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**Chemins d’accès aux API :**

* URL de la passerelle PLATFORM : `https://platform.adobe.io/`
* Chemin de base de cette API : `/data/core/privacy/jobs`
* Exemple de chemin complet : `https://platform.adobe.io/data/core/privacy/jobs/ping`


**En-têtes requis :**

* Tous les appels nécessitent les en-têtes `Authorization`, `x-gw-ims-org-id` et `x-api-key`. Pour plus d’informations sur l’obtention de ces valeurs, consultez le **tutoriel sur l’authentification**.
* Toutes les requêtes avec une payload dans le corps de la requête (tels que les appels POST, PUT et PATCH) doivent inclure la `Content-Type` d’en-tête avec une valeur de `application/json`.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
