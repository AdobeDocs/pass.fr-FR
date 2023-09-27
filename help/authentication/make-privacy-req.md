---
title: Comment effectuer une demande d’accès à des informations personnelles
description: Comment effectuer une demande d’accès à des informations personnelles
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# Comment effectuer une demande d’accès à des informations personnelles {#howto-make-privacy-request}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Identifiants et espaces de noms {#identifier-namespace}

Lors de l’envoi d’une demande d’accès ou de suppression d’informations personnelles, l’application client doit inclure les identifiants suivants :

* **mvpdID** - Identifiant unique du MVPD.
* **userID** - Identifie de manière unique l’utilisateur d’une application de programmeur, mais provient du MVPD. Voir Présentation des ID utilisateur dans la présentation du programmeur.
* **IMSOrgID** - Identifiant de l’organisation du service Adobe Experience Cloud Identity Management, qui identifie de manière unique le client dans Adobe Experience Cloud


Veuillez consulter l’exemple ci-dessous :

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>Les utilisateurs doivent être authentifiés pour pouvoir générer des demandes d’accès à des informations personnelles pour l’authentification Adobe Pass. Dans le cas contraire, les programmeurs doivent trouver d’autres moyens d’extraire l’ID utilisateur MVPD.

## Types de requêtes {#req-type}

L’authentification Adobe Pass prend en charge les demandes d’accès et de suppression.

### Accès {#access-req}

Pour une demande d’accès :

nous vous fournirons un fichier JSON qui contient un résumé du nombre total de demandes d’authentification et d’autorisation créées pour ce titulaire de données.
tous ces événements sont filtrés par client.


**Exemple de requête**

Vous devez charger un fichier JSON avec les identifiants d’authentification Adobe Pass pour lesquels vous envoyez la demande d’accès aux données. Pour voir à quoi ressemble un fichier JSON bien formé, reportez-vous à cet exemple :

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

Vous devez charger un fichier JSON avec les identifiants d’authentification Adobe Pass pour lesquels vous envoyez la demande de suppression de données. Pour voir à quoi ressemble un fichier JSON bien formé, reportez-vous à cet exemple :

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

* nous partageons uniquement un reçu indiquant que les données ont été supprimées, et non un fichier agrégé contenant toutes les données supprimées.
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

## Comment déclencher une requête {#trigger-req}

Deux options permettent aux clients d’envoyer des demandes d’accès à des informations personnelles à Adobe :

* **manuellement** - en utilisant [Interface utilisateur du Privacy Service](#privacy-service-ui)
* **automatiquement** - en utilisant [API PRIVACY SERVICE](#privacy-service-api)

### Utilisation de l’interface utilisateur de Privacy Service {#privacy-service-ui}

A [tutoriel complet](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) sur l’accès et l’utilisation de l’interface utilisateur de Privacy Service sont disponibles en ligne via les services Adobe I/O. En outre, les clients peuvent utiliser ce lien pour accéder à la bibliothèque de vidéos et d’articles sur la réglementation de la confidentialité. Cliquez sur le menu Adobe Experience Cloud et RGPD . Cela ouvre un certain nombre de vidéos - &quot;Comment utiliser l’interface utilisateur en vertu du RGPD&quot; explique comment l’utiliser.

Dans l’interface utilisateur, les clients doivent charger leur propre IMSOrgID et un fichier JSON contenant les détails des demandes en vertu du RGPD pour chaque produit.

### Utilisation de l’API Privacy Service {#privacy-service-api}

Adobe Experience Platform Privacy Service offre une facilitation centralisée et commune des demandes d’accès/de suppression et des demandes d’opposition à la vente des données privées.

La variable **Documentation de l’API Privacy Service** Décrit en détail la manière dont un client Adobe peut s’intégrer à l’API Adobe.

**Visualisez les appels d’API avec Postman (un logiciel tiers gratuit) :**

* [Collection Postman d’API Privacy Service sur GitHub](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Guide vidéo pour la création de l’environnement Postman](https://video.tv.adobe.com/v/28832)
* [Procédure d’importation d’environnements et de collections dans Postman](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**Chemins d’accès aux API :**

* URL de la passerelle PLATFORM : `https://platform.adobe.io/`
* Chemin d’accès de base pour cette API : `/data/core/privacy/jobs`
* Exemple de chemin complet : `https://platform.adobe.io/data/core/privacy/jobs/ping`


**En-têtes requis :**

* Tous les appels nécessitent des en-têtes `Authorization`, `x-gw-ims-org-id`, et `x-api-key`. Pour plus d’informations sur l’obtention de ces valeurs, voir la section **tutoriel sur l’authentification**.
* Toutes les requêtes comportant un payload dans le corps de la requête (comme les appels POST, PUT et PATCH) doivent inclure l’en-tête . `Content-Type` avec la valeur de `application/json`.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
