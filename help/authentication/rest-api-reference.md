---
title: Référence de l’API REST
description: Référence de l’api REST
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 3%

---

# Référence de l’API REST {#rest-api-reference}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Mécanisme de ralentissement

L’API REST d’authentification Adobe Pass est régie par une [Mécanisme de ralentissement](/help/authentication/throttling-mechanism.md).

## Formats de réponse {#response-formats}


>[!NOTE]
>
> Les API fournies dans ces services peuvent renvoyer des réponses au format XML ou JSON (pour les API qui renvoient une réponse). Il existe 3 manières différentes de spécifier le format de réponse dans la requête :
>
>* Définissez l’en-tête HTTP Accept sur `application/xml` ou `application/json`.
>* Dans le payload de la requête, spécifiez le paramètre . `format=xml` ou `format=json`.
>* Appel du point d’entrée du service Web avec extension `.xml` ou `.json`. Par exemple : `/regcode.xml` ou `/regcode.json`
>
>Vous pouvez spécifier l’une des méthodes ci-dessus. La spécification de plusieurs méthodes avec des formats en conflit peut entraîner des erreurs ou une sortie indésirable.

## Points de terminaison de l’API REST {#clientless-endpoints}

&lt;reggie_fqdn>:

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Résumé des services web {#web_srvs_summary}

Le tableau ci-dessous répertorie les services web disponibles pour l’approche sans client. Cliquez sur les points de terminaison des services web pour plus d’informations (exemple de requête et de réponse, paramètres d’entrée, méthodes HTTP, etc.)


| Sr | Point de terminaison du service Web | Description | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | Hébergé à | Appelé par |
| --- | --- | --- | --- | --- | --- |
| 1. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode](/help/authentication/registration-code-request.md) | Renvoie le code d’enregistrement généré de manière aléatoire et l’URI de page de connexion | 2 | Adobe  </br>Service Reg Code | Appareil dynamique |
| 2. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode/  </br>  {registrationCode}](/help/authentication/return-registration-record.md) | Renvoie l’enregistrement du code d’enregistrement contenant le code d’enregistrement UUID, le code d’enregistrement et l’ID d’appareil haché. | 8 | Adobe  </br>Service Reg Code | Authentification Adobe Pass |
| 3. | [&lt;sp_fqdn>/api/v1/config/  </br>  {requestorId}](/help/authentication/provide-mvpd-list.md) | Renvoie la liste des MVPD configurés pour le demandeur | 5 | Adobe  </br>Adobe Pass  </br>authentication  </br>Service | Connexion  </br>Web  </br>Application |
| 4. | [&lt;sp_fqdn>/api/v1/authenticate](/help/authentication/initiate-authentication.md) | Lance le processus AuthN en informant l’événement de sélection MVPD. Crée un enregistrement sur la base de données d’authentification, qui est réconcilié lorsqu’une réponse réussie est reçue de MVPD (étape 13) | 7 | Adobe  </br>Adobe Pass  </br>authentication  </br>Service | Connexion  </br>Web  </br>Application |
| 5. | Consommateur d’assertion SAML | Processus SAML existant entre l’authentification Adobe Pass et MVPD | 13 | Adobe Pass  </br>authentication  </br>Service | Authentification Adobe Pass |
| 6. | [&lt;sp_fqdn>/api/v1/checkauthn/  </br>  {registrationCode}](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | L’application Web de connexion peut vérifier si le flux de connexion tenté a réussi. |     | Adobe Pass  </br>authentication   </br>Service | Connexion   </br>Web   </br>Application |
| 7. | [&lt;sp_fqdn>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Obtient les métadonnées associées au jeton AuthN | 15 | Adobe Pass  </br>authentication  </br>Service | Appareil dynamique |
| 8. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode/  </br>  {registrationCode}](/help/authentication/delete-registration-record.md) | Supprime l’enregistrement du code reg et le libère pour réutilisation. | 16 | Adobe  </br>Service Reg Code | Authentification Adobe Pass |
| 9. | [&lt;sp_fqdn>/api/v1/authorized](/help/authentication/initiate-authorization.md) | Obtient la réponse d’autorisation. | 17 | Adobe Pass  </br>authentication  </br>Service | Appareil dynamique |
| 10. | [&lt;sp_fqdn>/api/v1/checkauthn](/help/authentication/check-authentication-token.md) | Indique si le périphérique dispose d’un jeton AuthN non expiré. |     | Adobe Pass  </br>authentication  </br>Service | Appareil dynamique |
| 11 | [&lt;sp_fqdn>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Renvoie le jeton AuthN s’il est trouvé. |     | Adobe Pass  </br>authentication  </br>Service | Appareil dynamique |
| 12. | [&lt;sp_fqdn>/api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | Renvoie le jeton AuthZ s’il est trouvé. |     | Adobe Pass  </br>authentication  </br>Service | Appareil dynamique |
| 13. | [&lt;sp_fqdn>/api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | Renvoie le jeton de média court s’il est trouvé - identique à /api/v1/mediatoken |     | Adobe Pass  </br>authentication  </br>Service | Appareil dynamique |
| 14. | [&lt;sp_fqdn>/api/v1/mediatoken](/help/authentication/obtain-short-media-token.md) | Obtient un jeton de média court |     | Adobe Pass  </br>authentication  </br>Service | Appareil dynamique |
| 15. | [&lt;sp_fqdn>/api/v1/preauthorized](/help/authentication/retrieve-list-of-preauthorized-resources.md) | Récupère la liste des ressources préautorisées |     | Adobe Pass  </br>authentication  </br>Service | Appareil dynamique |
| 16. | [&lt;sp_fqdn>/api/v1/preautoriser/{code}](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Récupère la liste des ressources préautorisées |     | Adobe Pass  </br>authentication  </br>Service | Application Web de connexion |
| 17. | [&lt;sp_fqdn>/api/v1/logout](/help/authentication/initiate-logout.md) | Supprimer les jetons AuthN et AuthZ du stockage |     | Adobe Pass  </br>authentication   </br>Service | Appareil dynamique |
| 18. | [&lt;sp_fqdn>/api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | Obtient les métadonnées utilisateur une fois le flux d’authentification terminé | S/O | S/O | Appareil dynamique |
| 19. | [&lt;sp_fqdn>/api/v1/authenticate/freepreview](/help/authentication/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Création d’un jeton d’authentification pour Temp Pass ou Promoted Temp Pass | S/O | Adobe Pass  </br>authentication  </br>Service | Appareil dynamique |


## Sécurité de l’API REST {#security}

Toutes les API sans client d’authentification Adobe Pass doivent être appelées à l’aide du protocole HTTPS pour une communication sécurisée. En outre, la plupart des API appelées doivent contenir un jeton d’accès fourni par [Enregistrement du client dynamique](/help/authentication/dynamic-client-registration.md).
