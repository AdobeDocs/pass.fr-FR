---
title: Codes d’erreur améliorés
description: Codes d’erreur améliorés
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 7ac04991289c95ebb803d1fd804e9b497f821cda
workflow-type: tm+mt
source-wordcount: '2696'
ht-degree: 3%

---

# Codes d’erreur améliorés {#enhanced-error-codes}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Les codes d’erreur améliorés représentent une fonctionnalité d’authentification d’Adobe Pass qui fournit des informations d’erreur supplémentaires aux applications clientes intégrées à :

* API REST d&#39;authentification Adobe Pass :
   * [API REST v2](../../rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [API REST v1 (héritée)](../../legacy/rest-api-v1/rest-api-overview.md)
* API de préautorisation des SDK d’authentification Adobe Pass :
   * [SDK JavaScript (hérité) (API de préautorisation)](../../legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
   * [SDK iOS/tvOS (hérité) (API de préautorisation)](../../legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
   * [(Hérité) Android SDK (API de préautorisation)](../../legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)

  _(*) L’API Preauthorize est la est la seule API SDK d’authentification Adobe Pass qui prend en charge les codes d’erreur améliorés_

>[!IMPORTANT]
>
> Les applications qui intègrent l’API REST d’authentification Adobe Pass v2 bénéficieront par défaut des codes d’erreur améliorés sans nécessiter de configuration supplémentaire.
>
> <br/>
>
> Les applications qui intègrent l’API REST d’authentification Adobe Pass v1 ou l’API de préautorisation des SDK ne peuvent bénéficier des codes d’erreur améliorés que si la fonctionnalité est explicitement activée.
>
> <br/>
>
> Pour activer explicitement cette fonctionnalité, créez un ticket via notre [Zendesk](https://adobeprimetime.zendesk.com) et demandez de l’aide à votre gestionnaire de compte technique (TAM).

## Représentation {#enhanced-error-codes-representation}

Les codes d’erreur améliorés peuvent être représentés au format `JSON` ou `XML` selon l’API d’authentification Adobe Pass intégrée et la valeur de l’en-tête « Accept » utilisée (c’est-à-dire `application/json` ou `application/xml`) :

| API d’authentification Adobe Pass | JSON | XML |
|-------------------------------|---------|---------|
| API REST v2 | &check; |         |
| API REST v1 | &check; | &check; |
| API de préautorisation des SDK | &check; |         |

>[!IMPORTANT]
>
> L’authentification Adobe Pass peut communiquer les codes d’erreur améliorés aux applications clientes sous deux formes :
>
> <br/>
>
> * **Informations sur l’erreur de niveau supérieur** : dans ce cas, l’objet ***« error »*** se trouve au niveau supérieur, par conséquent le corps de la réponse ne peut contenir que l’objet ***« error »***.
> * **Informations sur l’erreur au niveau de l’élément** : dans ce cas, l’objet ***« error »*** se trouve au niveau de l’élément. Par conséquent, le corps de la réponse peut contenir un objet ***« error »*** pour tous les éléments qui ont rencontré une erreur lors de la maintenance.
>
> <br/>
>
> Consultez la documentation publique de chaque API d’authentification Adobe Pass intégrée pour déterminer les détails spécifiques à la représentation des codes d’erreur améliorés.

**API REST v2**

Reportez-vous aux réponses HTTP suivantes contenant des exemples de codes d’erreur améliorés représentés comme `JSON` applicables pour l’API REST v2.

>[!BEGINTABS]

>[!TAB API REST v2 - Informations d’erreur au niveau de l’élément (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB API REST v2 - Informations d’erreur de niveau supérieur (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!ENDTABS]

**API REST v1**

Reportez-vous aux réponses HTTP suivantes contenant des exemples de codes d’erreur améliorés représentés sous la forme `JSON` ou `XML` applicables pour l’API REST v1.

>[!BEGINTABS]

>[!TAB API REST v1 - Informations d’erreur au niveau de l’élément (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB API REST v1 - Informations d’erreur de niveau supérieur (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB API REST v1 - Informations d’erreur de niveau supérieur (XML)]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!ENDTABS]

### Structure {#enhanced-error-codes-representation-structure}

Les codes d’erreur améliorés incluent les champs de `JSON` ou les attributs de `XML` suivants avec des exemples :

| Nom | Type | Exemple | Restricted | Description |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *action* | *chaîne* | *aucune* | &check; | L’action recommandée Authentification Adobe Pass qui pourrait remédier à la situation telle que définie dans ce document. <br/><br/> Pour plus d’informations, consultez la section [Action](#enhanced-error-codes-action). |
| *statut* | *entier* | 403 ** | &check; | Le code de statut de la réponse HTTP tel que défini dans le document [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6). <br/><br/> Pour plus d&#39;informations, consultez la section [Statut](#enhanced-error-codes-status). |
| *code* | *chaîne* | *authorization_deny_by_mvpd* | &check; | Code d’identifiant unique d’authentification Adobe Pass associé à l’erreur, tel que défini dans ce document. <br/><br/> Pour plus d’informations, consultez la section [Code](#enhanced-error-codes-code). |
| *message* | *chaîne* | *Le MVPD a renvoyé une décision de refus lors de la demande d’autorisation pour la ressource spécifiée* |            | Message lisible par l’utilisateur ou l’utilisatrice final(e) qui peut s’afficher dans certains cas. <br/><br/> Pour plus d&#39;informations, consultez la section [Gestion des réponses](#enhanced-error-codes-response-handling). |
| *détails* | *chaîne* | *Votre forfait d’abonnement n’inclut pas le canal « En direct »* |            | Le message détaillé qui peut être fourni par un partenaire de services dans certains cas, <br/><br/> Ce champ peut ne pas être présent si le partenaire de services ne fournit pas de message personnalisé. |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | URL de la documentation publique sur l’authentification Adobe Pass qui renvoie à des informations supplémentaires sur la raison de cette erreur et les solutions possibles. <br/><br/> Ce champ contient une URL absolue et ne doit pas être déduit du code d’erreur. Selon le contexte d’erreur, une autre URL peut être fournie. |
| *trace* | *chaîne* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | Identifiant unique de la réponse qui peut être utilisé lors du contact de l’assistance de l’authentification Adobe Pass pour résoudre des problèmes spécifiques. |

>[!IMPORTANT]
>
> La colonne **Restreint** indique si le champ respectif contient une valeur d’un ensemble fini, tandis que les champs non restreints peuvent contenir n’importe quelle donnée.
>
> <br/>
>
> Les futures mises à jour de ce document pourraient ajouter des valeurs aux ensembles finis, mais ne supprimeront ni ne modifieront les ensembles existants.

### Action {#enhanced-error-codes-representation-action}

Les codes d’erreur améliorés incluent un champ « action » qui fournit une action recommandée qui peut remédier à la situation.

Les valeurs possibles du champ « action » sont les suivantes :

| Action | Description | Catégorie |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| aucun | Il n’existe aucune action prédéfinie pour résoudre ce problème, mais dans certains cas, cela peut indiquer un appel incorrect de l’API. | Correction du contexte de la requête. |
| configuration | L’application cliente nécessite une modification de configuration, la plupart du temps effectuée via le tableau de bord Adobe Pass TVE. | Correction du contexte de configuration de l’intégration. |
| application-registration | L’application cliente doit s’enregistrer à nouveau. | Correction du contexte de l’application cliente. |
| authentification | L’application cliente nécessite l’authentification ou la réauthentification de l’utilisateur. | Correction du contexte de l’application cliente. |
| autorisation | L’application cliente requiert l’obtention d’une autorisation pour la ressource spécifiée. | Correction du contexte de l’application cliente. |
| réessayer | L’application cliente doit relancer la requête. | Correction du contexte de la requête. |

_(*) Pour certaines erreurs, plusieurs actions peuvent être des solutions possibles, mais le champ « action » indique celle qui a la plus forte probabilité de corriger l&#39;erreur._

### Etat {#enhanced-error-codes-representation-status}

Les codes d’erreur améliorés incluent un champ « statut » qui indique le code d’état HTTP associé à l’erreur.

Les valeurs possibles pour le champ « statut » sont les suivantes :

| Code | Phrase De Raison |
|------|-----------------------|
| 400 | Requête incorrecte |
| 401 | Non Autorisé |
| 403 | Interdit |
| 404 | Introuvable |
| 405 | Méthode Non Autorisée |
| 410 | Autant |
| 412 | Échec de la condition préalable |
| 500 | Erreur de serveur interne |

Les codes d’erreur améliorés avec un « statut » 4xx apparaissent généralement lorsque l’erreur est générée par le client et, la plupart du temps, cela implique que le client a besoin d’un travail supplémentaire pour la corriger.

Les codes d’erreur améliorés avec un « statut » 5xx apparaissent généralement lorsque l’erreur est générée par le serveur et, la plupart du temps, cela implique que le serveur nécessite davantage de travail pour la corriger.

>[!IMPORTANT]
>
> Dans certains cas, le code de statut de la réponse HTTP est différent du champ « statut » du code d’erreur amélioré, en particulier lors de l’interaction avec une API d’authentification Adobe Pass qui communique les codes d’erreur améliorés en tant qu’informations d’erreur au niveau de l’élément.

### Code {#enhanced-error-codes-representation-code}

Les codes d’erreur améliorés incluent un champ « code » qui fournit un identifiant unique d’authentification Adobe Pass associé à l’erreur.

Les valeurs possibles du champ « code » sont agrégées [ci-dessous](#enhanced-error-codes-list) dans deux listes basées sur l’API d’authentification Adobe Pass intégrée.

## Listes {#enhanced-error-codes-lists}

### API REST v2 {#enhanced-error-codes-lists-rest-api-v2}

Le tableau ci-dessous répertorie les codes d’erreur améliorés qu’une application cliente peut rencontrer lors de son intégration à l’API REST d’authentification Adobe Pass v2.

| Action | Code | Etat | Message |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **aucune** | *invalid_parameter_service_provider* | 400 | La valeur du paramètre du fournisseur de services est manquante ou non valide. |
|                              | *invalid_parameter_mvpd* | 400 | La valeur du paramètre mvpd est manquante ou non valide. |
|                              | *invalid_parameter_code* | 400 | La valeur du paramètre de code est manquante ou non valide. |
|                              | *invalid_parameter_resources* | 400 | La valeur du paramètre de ressources est manquante ou non valide. |
|                              | *invalid_parameter_redirect_url* | 400 | La valeur du paramètre d’URL de redirection est manquante ou non valide. |
|                              | *invalid_parameter_partner* | 400 | La valeur du paramètre du partenaire est manquante ou non valide. |
|                              | *invalid_parameter_saml_response* | 400 | La valeur du paramètre de réponse SAML est manquante ou non valide. |
|                              | *invalid_header_device_info* | 400 | La valeur de l’en-tête des informations sur l’appareil est manquante ou non valide. |
|                              | *invalid_header_device_identifier* | 400 | La valeur de l’en-tête de l’identifiant de l’appareil est manquante ou non valide. |
|                              | *invalid_header_identity_for_temporary_access* | 400 | L’identité de la valeur de l’en-tête d’accès temporaire est manquante ou non valide. |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | La valeur de statut d’accès aux autorisations de l’en-tête de statut du framework du partenaire est absente. |
|                              | *invalid_header_pfs_permission_access_not_determine* | 400 | La valeur de statut d’accès aux autorisations de l’en-tête de statut du framework du partenaire est indéterminée. |
|                              | *invalid_header_pfs_permission_access_not_granted* | 400 | La valeur de statut d’accès d’autorisation de l’en-tête de statut du framework du partenaire n’est pas accordée. |
|                              | *invalid_header_pfs_provider_id_not_determine* | 400 | La valeur de l’identifiant du fournisseur dans l’en-tête de statut du framework du partenaire n’est pas associée à un fichier mvpd connu. |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | La valeur de l’identifiant du fournisseur dans l’en-tête de statut du framework du partenaire ne correspond pas au mvpd envoyé en tant que paramètre. |
|                              | *invalid_header_pfs_provider_info_expired* | 400 | Les informations sur le fournisseur figurant dans l&#39;en-tête de statut du framework du partenaire ont expiré. |
|                              | *invalid_integration* | 400 | L’intégration entre le fournisseur de services spécifié et mvpd n’existe pas ou est désactivée. |
|                              | *invalid_authentication_session* | 400 | La session d’authentification associée à cette requête est manquante ou non valide. |
|                              | *preauthorization_deny_by_mvpd* | 403 | Le MVPD a renvoyé une décision de refus lors de la demande d’autorisation préalable pour la ressource spécifiée. |
|                              | *authorization_deny_by_mvpd* | 403 | Le MVPD a renvoyé une décision « Deny » (Refuser) lors de la demande d’autorisation pour la ressource spécifiée. |
|                              | *authorization_deny_by_parental_controls* | 403 | Le MVPD a renvoyé une décision de refus en raison des paramètres de contrôle parental pour la ressource spécifiée. |
|                              | *autorisation_refusée_par_dégradation_règle* | 403 | Une règle de dégradation est appliquée à l’intégration entre le fournisseur de services spécifié et mvpd, ce qui refuse l’autorisation pour les ressources demandées. |
|                              | *internal_server_error* | 500 | La requête a échoué en raison d’une erreur de serveur interne. |
| **configuration** | *too_many_resources* | 403 | La demande d’autorisation ou de préautorisation a échoué, car trop de ressources ont été interrogées. Contactez l’équipe d’assistance pour configurer correctement les limites d’autorisation et de préautorisation. |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | La configuration du certificat de métadonnées de l’utilisateur est manquante ou non valide. |
|                              | *invalid_configuration_temporary_access* | 500 | La configuration d’accès temporaire n’est pas valide. |
|                              | *invalid_configuration_platform* | 500 | La configuration de la plateforme est manquante ou non valide pour l&#39;intégration. |
|                              | *invalid_configuration_platform_id* | 500 | La configuration de l’ID de plateforme est manquante ou non valide. |
|                              | *invalid_configuration_platform_trait* | 500 | La configuration des caractéristiques de plateforme est manquante ou non valide. |
|                              | *invalid_configuration_platform_category_trait* | 500 | La configuration des caractéristiques de la catégorie de plateforme est manquante ou non valide. |
|                              | *invalid_configuration_platform_services* | 500 | La configuration des services de la plateforme est manquante ou non valide pour l&#39;intégration. |
|                              | *invalid_configuration_mvpd_platform* | 500 | La configuration de la plateforme mvpd est manquante ou non valide pour mvpd et platform. |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | La configuration du statut d’intégration de la plateforme mvpd est manquante ou non valide pour mvpd et platform. |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | La configuration d’échange de profils de plateforme mvpd est manquante ou non valide pour mvpd et platform. |
| **enregistrement-application** | *invalid_access_token_service_provider* | 401 | Le jeton d’accès n’est pas valide en raison d’un fournisseur de services non valide. |
|                              | *invalid_access_token_client_application* | 401 | Le jeton d’accès n’est pas valide en raison d’une application cliente non valide. |
| **authentication** | *authenticated_profile_missing* | 403 | Le profil authentifié associé à cette requête est manquant. |
|                              | *authenticated_profile_expired* | 403 | Le profil authentifié associé à cette demande a expiré. |
|                              | *authenticated_profile_invalidated* | 403 | Le profil authentifié associé à cette demande a été invalidé. |
|                              | *temporary_access_duration_limit_exceeded* | 403 | La limite de durée d’accès temporaire a été dépassée. |
|                              | *temporary_access_resources_limit_exceeded* | 403 | La limite des ressources d&#39;accès temporaire a été dépassée. |
|                              | *authorization_deny_by_hba_policies* | 403 | Le MVPD a renvoyé une décision « Deny » en raison de politiques d’authentification basées sur l’accueil. L’authentification actuelle a été obtenue par le biais d’un flux d’authentification à domicile et l’appareil n’est plus à la maison lors de la demande d’autorisation pour la ressource spécifiée. Pour continuer, l’utilisateur doit s’authentifier à nouveau avec un MVPD pris en charge. |
|                              | *authorization_deny_by_session_invalidated* | 403 | La session d’authentification a été invalidée par le fournisseur d’identité. Pour continuer, l’utilisateur doit s’authentifier à nouveau avec un MVPD pris en charge. |
|                              | *identité_non_reconnue_par_mvpd* | 403 | La demande d’autorisation a échoué en raison du fait que l’identité de l’utilisateur n’a pas été reconnue par le MVPD. |
| **réessayer** | *network_received_error* | 403 | Une erreur de lecture s’est produite lors de la récupération de la réponse du service partenaire associé. Réessayer la requête peut résoudre le problème. |
|                              | *network_connection_timeout* | 403 | Il y a eu un délai d’expiration de connexion avec le service partenaire associé. Réessayer la requête peut résoudre le problème. |
|                              | *maximum_execution_time_exceeded* | 403 | La demande n’a pas abouti dans le délai maximum autorisé. Réessayer la requête peut résoudre le problème. |

### API REST v1 (héritée) {#enhanced-error-codes-lists-rest-api-v1}

Le tableau ci-dessous répertorie les codes d’erreur améliorés qu’une application cliente peut rencontrer lors de son intégration à l’API REST d’authentification Adobe Pass v1.

| Action | Code | Etat | Message |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **aucune** | *invalid_requestor* | 400 | Le paramètre du demandeur est manquant ou non valide. |
|                    | *invalid_device_info* | 400 | Les informations sur l’appareil sont manquantes ou non valides. |
|                    | *invalid_device_id* | 400 | Identifiant d’appareil manquant ou non valide. |
|                    | *missing_resource* | 400, 412 | Le paramètre de ressource est manquant. |
|                    | *malform_authz_request* | 400, 412 | La demande d’autorisation est nulle ou non valide. |
|                    | *preauthorization_deny_by_mvpd* | 403 | Le MVPD a renvoyé une décision de refus lors de la demande d’autorisation préalable pour la ressource spécifiée. |
|                    | *authorization_deny_by_mvpd* | 403 | Le MVPD a renvoyé une décision « Deny » (Refuser) lors de la demande d’autorisation pour la ressource spécifiée. |
|                    | *authorization_deny_by_parental_controls* | 403 | Le MVPD a renvoyé une décision de refus en raison des paramètres de contrôle parental pour la ressource spécifiée. |
|                    | *internal_error* | 400, 405, 500 | La requête a échoué en raison d’une erreur de serveur interne. |
| **configuration** | *intégration_inconnue* | 400, 412 | L&#39;intégration entre le programmeur et le fournisseur d&#39;identité spécifiés n&#39;existe pas. Utilisez le tableau de bord TVE pour créer l’intégration requise. |
|                    | *too_many_resources* | 403 | La demande d’autorisation ou de préautorisation a échoué, car trop de ressources ont été interrogées. Contactez l’équipe d’assistance pour configurer correctement les limites d’autorisation et de préautorisation. |
| **authentication** | *authentication_session_issuer_mismatch* | 400 | La demande d’autorisation a échoué en raison du fait que le MVPD indiqué pour le flux d’autorisation est différent de celui qui a émis la session d’authentification. L’utilisateur doit s’authentifier à nouveau avec le MVPD souhaité pour continuer. |
|                    | *authorization_deny_by_hba_policies* | 403 | Le MVPD a renvoyé une décision « Deny » en raison de politiques d’authentification basées sur l’accueil. L’authentification actuelle a été obtenue à l’aide d’un flux d’authentification à domicile (HBA), mais le périphérique n’est plus à domicile lors de la demande d’autorisation pour la ressource spécifiée. Pour continuer, l’utilisateur doit s’authentifier à nouveau avec un MVPD pris en charge. |
|                    | *authorization_deny_by_session_invalidated* | 403 | La session d’authentification a été invalidée par le fournisseur d’identité. Pour continuer, l’utilisateur doit s’authentifier à nouveau avec un MVPD pris en charge. |
|                    | *identité_non_reconnue_par_mvpd* | 403 | La demande d’autorisation a échoué en raison du fait que l’identité de l’utilisateur n’a pas été reconnue par le MVPD. |
|                    | *authentication_session_invalidated* | 403 | La session d’authentification a été invalidée par le fournisseur d’identité. Pour continuer, l’utilisateur doit s’authentifier à nouveau avec un MVPD pris en charge. |
|                    | *authentication_session_missing* | 403 412 | Impossible de récupérer la session d&#39;authentification associée à cette requête. Pour continuer, l’utilisateur doit s’authentifier à nouveau avec un MVPD pris en charge. |
|                    | *authentication_session_expired* | 403 412 | La session d’authentification actuelle a expiré. Pour continuer, l’utilisateur doit s’authentifier à nouveau avec un MVPD pris en charge. |
|                    | *preauthorization_authentication_session_missing* | 412 | Impossible de récupérer la session d&#39;authentification associée à cette requête. Pour continuer, l’utilisateur doit s’authentifier à nouveau avec un MVPD pris en charge. |
|                    | *preauthorization_authentication_session_expired* | 412 | La session d’authentification actuelle a expiré. Pour continuer, l’utilisateur doit s’authentifier à nouveau avec un MVPD pris en charge. |
| **autorisation** | *authorization_not_found* | 403, 404 | Aucune autorisation n’a été trouvée pour la ressource spécifiée. L’utilisateur ou l’utilisatrice doit obtenir une nouvelle autorisation pour continuer. |
|                    | *authorization_expired* | 410 | L’autorisation précédente pour la ressource spécifiée a expiré. L’utilisateur ou l’utilisatrice doit obtenir une nouvelle autorisation pour continuer. |
| **réessayer** | *network_received_error* | 403 | Une erreur de lecture s’est produite lors de la récupération de la réponse du service partenaire associé. Réessayer la requête peut résoudre le problème. |
|                    | *network_connection_timeout* | 403 | Il y a eu un délai d’expiration de connexion avec le service partenaire associé. Réessayer la requête peut résoudre le problème. |
|                    | *maximum_execution_time_exceeded* | 403 | La demande n’a pas abouti dans le délai maximum autorisé. Réessayer la requête peut résoudre le problème. |

### API de préautorisation des SDK (hérités) {#enhanced-error-codes-lists-sdks-preauthorize-api}

Reportez-vous à la [section](#enhanced-error-codes-list-rest-api-v1) précédente pour connaître les codes d’erreur améliorés qu’une application cliente peut rencontrer lors de son intégration avec l’API de préautorisation des SDK d’authentification Adobe Pass.

## Gestion des réponses {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> Il existe des codes d’erreur améliorés qui peuvent être gérés automatiquement dans le code de l’application cliente, comme retenter une demande d’autorisation en cas de temporisation du réseau ou exiger que l’utilisateur s’authentifie à nouveau lorsque sa session a expiré, mais d’autres types peuvent nécessiter des modifications de configuration ou une interaction de l’équipe d’assistance clientèle d’Adobe Pass Authentication.
>
> <br/>
>
> Par conséquent, il est important de collecter et de fournir des informations d’erreur complètes lors de la création d’un ticket via notre [Zendesk](https://adobeprimetime.zendesk.com), afin de s’assurer que les modifications nécessaires sont apportées avant de lancer la nouvelle application ou la nouvelle fonctionnalité.

En résumé, lors de la gestion des réponses contenant des codes d’erreur améliorés, tenez compte des points suivants :

1. **Indépendant des API qui renvoient l’erreur** : implémentez une logique de gestion des erreurs centralisée qui prend en charge le catalogue complet des codes d’erreur améliorés, quelle que soit l’API qui les génère. Plusieurs codes d’erreur améliorés sont partagés entre les API et doivent être gérés de manière cohérente.

1. **Indépendance par rapport aux informations d’erreur de niveau supérieur et au niveau de l’élément** : gérez les informations d’erreur de niveau supérieur et au niveau de l’élément indépendamment de la manière dont elles sont communiquées, assurez-vous de pouvoir gérer les deux formes de transmission des codes d’erreur améliorés.

1. **Vérifier les deux valeurs de statut** : toujours vérifier le code de statut de la réponse HTTP et le champ « statut » du code d’erreur amélioré. Elles peuvent être différentes et toutes deux fournissent des informations précieuses.

1. **Logique de reprise** : pour les erreurs qui nécessitent une reprise, assurez-vous que les reprises sont limitées (c’est-à-dire 2-3) ou sont effectuées avec une exécution exponentielle afin d’éviter de surcharger le serveur. En outre, dans le cas des API d’authentification Adobe Pass qui gèrent plusieurs éléments à la fois (par exemple, l’API de préautorisation), vous devez inclure dans la demande répétée uniquement les éléments marqués avec « réessayer » et non la liste entière.

1. **Modifications de configuration** : pour les erreurs qui nécessitent des modifications de configuration, assurez-vous que les modifications nécessaires sont effectuées avant de lancer la nouvelle application ou la nouvelle fonctionnalité.

1. **Authentification et autorisation** : en cas d’erreurs liées à l’authentification et à l’autorisation, vous devez inviter l’utilisateur à s’authentifier à nouveau ou à obtenir une nouvelle autorisation, si nécessaire.

1. **Commentaires de l’utilisateur** : facultatif, utilisez les champs « message » et (potentiel) « détails » lisibles par l’utilisateur ou l’utilisatrice pour l’informer du problème. Le message texte « détails » peut être transmis à partir des points d’entrée de préautorisation ou d’autorisation MVPD ou du programmeur lors de l’application des règles de dégradation.
