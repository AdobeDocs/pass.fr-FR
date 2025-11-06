---
title: Référence de l’API REST
description: Référence de l’api REST
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 3%

---

# Référence de l’API REST (héritée) {#rest-api-reference}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Mécanisme de limitation

L’API REST d’authentification Adobe Pass est régie par un [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Formats de réponse {#response-formats}


>[!NOTE]
>
> Les API fournies dans ces services peuvent renvoyer des réponses au format XML ou JSON (pour les API qui renvoient une réponse). Il existe trois manières différentes de spécifier le format de réponse dans la requête :
>
>* Définissez l’en-tête HTTP Accept sur `application/xml` ou `application/json`.
>* Dans la payload de la requête, spécifiez le paramètre `format=xml` ou `format=json`.
>* Appelez le point d’entrée du service web avec l’extension `.xml` ou `.json`. Par exemple, `/regcode.xml` ou `/regcode.json`
>
>Vous pouvez spécifier l’une des méthodes ci-dessus. La spécification de plusieurs méthodes avec des formats en conflit peut entraîner des erreurs ou une sortie indésirable.

## Points d’entrée de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Résumé des services web {#web_srvs_summary}

Le tableau ci-dessous répertorie les services web disponibles pour l’approche sans client. Cliquez sur les points d’entrée des services web pour plus d’informations (exemple de requête et de réponse, paramètres d’entrée, méthodes HTTP, etc.)


| Sr | Point d’entrée de service web | Description | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | Hébergé à | Appelé par |
|-----|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------|
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br>  {requestorId}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | Renvoie le code d’enregistrement et l’URI de page de connexion générés de manière aléatoire | 2 | Service Adobe </br>Reg Code) | Smart Device |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br>  {requestorId}/regcode/ </br>{registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | Retourne l&#39;enregistrement du code d&#39;enregistrement contenant l&#39;UUID du code d&#39;enregistrement, le code d&#39;enregistrement et l&#39;ID de périphérique haché | 8 | Service Adobe </br>Reg Code) | Authentification Adobe Pass |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br>{requestorId}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | Renvoie la liste des fichiers MVPD configurés pour le demandeur | 5 | Adobe </br>Adobe Pass </br>authentication </br>Service | Connexion </br>Web </br>App |
| 4. | [&lt;SP_FQDN>/api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | Lance le processus AuthN en informant l’événement de sélection MVPD. Crée un enregistrement sur la base de données d’authentification, qui est réconcilié lorsqu’une réponse réussie est reçue de MVPD (étape 13) | 7 | Adobe </br>Adobe Pass </br>authentication </br>Service | Connexion </br>Web </br>App |
| 5. | Consommateur d’assertions SAML | Workflow SAML existant entre l’authentification Adobe Pass et MVPD | 13 | Adobe Pass </br>authentication </br>Service | Authentification Adobe Pass |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br>{registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | L’application Web de connexion peut vérifier si le flux de connexion a réussi |                                                                                             | Adobe Pass </br>authentification   </br>Service | Login   </br>Web   </br>App |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Obtient les métadonnées liées au jeton AuthN | 15 | Adobe Pass </br>authentication </br>Service | Smart Device |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br>  {requestorId}/regcode/ </br>{registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md) | Supprime l’enregistrement du code rég. et libère le code rég. pour réutilisation | 16 | Service Adobe </br>Reg Code) | Authentification Adobe Pass |
| 9. | [&lt;SP_FQDN>/api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | Obtient la réponse d’autorisation. | 17 | Adobe Pass </br>authentication </br>Service | Smart Device |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) | Indique si l’appareil possède un jeton AuthN non expiré. |                                                                                             | Adobe Pass </br>authentication </br>Service | Smart Device |
| 11. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Renvoie le jeton AuthN s’il est trouvé. |                                                                                             | Adobe Pass </br>authentication </br>Service | Smart Device |
| 12 | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | Renvoie le jeton AuthZ s’il est trouvé. |                                                                                             | Adobe Pass </br>authentication </br>Service | Smart Device |
| 13. | [&lt;SP_FQDN>/api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | Renvoie le jeton de média court s’il est trouvé - identique à /api/v1/mediatoken |                                                                                             | Adobe Pass </br>authentication </br>Service | Smart Device |
| 14. | [&lt;SP_FQDN>/api/v1/mediatoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | Obtient un jeton de média court |                                                                                             | Adobe Pass </br>authentication </br>Service | Smart Device |
| 15. | [&lt;SP_FQDN>/api/v1/preauthorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) | Récupère la liste des ressources préautorisées |                                                                                             | Adobe Pass </br>authentication </br>Service | Smart Device |
| 16. | [&lt;SP_FQDN>/api/v1/preauthorize/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Récupère la liste des ressources préautorisées |                                                                                             | Adobe Pass </br>authentication </br>Service | Application Web de connexion |
| 17. | [&lt;SP_FQDN>/api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | Supprimer les jetons AuthN et AuthZ du stockage |                                                                                             | Adobe Pass </br>authentification   </br>Service | Smart Device |
| 18. | [&lt;SP_FQDN>/api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Obtient les métadonnées de l’utilisateur une fois le flux d’authentification terminé | S/O | S/O | Smart Device |
| 19. | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Créer un jeton d’authentification pour la passe temporaire ou la passe temporaire promotionnelle | S/O | Adobe Pass </br>authentication </br>Service | Smart Device |


## Sécurité de l’API REST {#security}

Toutes les API REST d’authentification Adobe Pass doivent être appelées à l’aide du protocole HTTPS pour une communication sécurisée. En outre, la plupart des API appelées doivent contenir un jeton d’accès obtenu, comme décrit dans la documentation de l’API [&#x200B; Récupérer le jeton d’accès &#x200B;](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
