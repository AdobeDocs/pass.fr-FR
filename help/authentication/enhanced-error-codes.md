---
title: Amélioration des codes d’erreur
description: Amélioration des codes d’erreur
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '2593'
ht-degree: 3%

---

# Amélioration des codes d’erreur {#enhanced-error-codes}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Les codes d’erreur améliorés représentent une fonctionnalité d’authentification Adobe Pass qui fournit des informations d’erreur supplémentaires aux applications clientes intégrées à :

* API REST d’authentification Adobe Pass :
   * [API REST v1](./rest-api-overview.md)
   * [API REST v2](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
* API de préautorisation des SDK d’authentification Adobe Pass :
   * [SDK JavaScript (API de préautorisation)](./preauthorize-api-javascript-sdk.md)
   * [SDK iOS/tvOS (API de préautorisation)](./preauthorize-api-ios-tvos-sdk.md)
   * [SDK Android (API de préautorisation)](./preauthorize-api-android-sdk.md)

  _(*) L’API de préautorisation est la seule API du SDK d’authentification Adobe Pass qui fournit la prise en charge des codes d’erreur améliorés._

>[!IMPORTANT]
>
> Les applications qui intègrent l’API REST d’authentification Adobe Pass v2 bénéficieront par défaut des codes d’erreur améliorés sans nécessiter de configuration supplémentaire.
>
> <br/>
>
> Les applications qui intègrent l’API REST d’authentification Adobe Pass v1 ou l’API de préautorisation des SDK ne peuvent bénéficier de codes d’erreur améliorés que si la fonctionnalité est explicitement activée.
>
> <br/>
>
> Pour activer explicitement cette fonctionnalité, créez un ticket par l’intermédiaire de notre [Zendesk](https://adobeprimetime.zendesk.com) et demandez de l’aide à votre gestionnaire de compte technique (TAM).

## Représentation {#enhanced-error-codes-representation}

Les codes d’erreur améliorés peuvent être représentés au format `JSON` ou `XML` en fonction de l’API d’authentification Adobe Pass intégrée et de la valeur d’en-tête &quot;Accepter&quot; utilisée (c’est-à-dire `application/json` ou `application/xml`) :

| API d’authentification Adobe Pass | JSON | XML |
|-------------------------------|---------|---------|
| API REST v1 | &amp;check; | &amp;check; |
| API REST v2 | &amp;check; |         |
| API de préautorisation des SDK | &amp;check; |         |

>[!IMPORTANT]
>
> L’authentification Adobe Pass peut communiquer des codes d’erreur améliorés aux applications clientes sous deux formes :
>
> <br/>
>
> * **Informations sur l’erreur de niveau supérieur** : dans ce cas, l’objet ***&quot;error&quot;*** se trouve au niveau supérieur. Par conséquent, le corps de la réponse ne peut contenir que l’objet ***&quot;error&quot;***.
> * **Informations sur l’erreur au niveau de l’élément** : dans ce cas, l’objet ***&quot;error&quot;*** se trouve au niveau de l’élément. Par conséquent, le corps de la réponse peut contenir un objet ***&quot;error&quot;*** pour tous les éléments qui ont rencontré une erreur lors du service.
>
> <br/>
>
> Consultez la documentation publique pour chaque API d’authentification Adobe Pass intégrée afin de déterminer les spécificités de représentation des codes d’erreur améliorés.

Reportez-vous aux réponses HTTP suivantes contenant des exemples de codes d’erreur améliorés représentés par `JSON` ou `XML`.

>[!BEGINTABS]

>[!TAB API REST v1 - Informations sur l’erreur de niveau supérieur (JSON)]

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

>[!TAB API REST v1 - informations d’erreur au niveau de l’élément (JSON)]

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
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB API REST v2 - Informations sur l’erreur de niveau supérieur (JSON)]

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

>[!TAB API REST v2 - Informations sur l’erreur au niveau de l’élément (JSON)]

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
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!ENDTABS]

Les codes d’erreur améliorés incluent les champs `JSON` ou les attributs `XML` suivants :

| Nom | Type | Exemple | Restricted | Description |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *action* | *string* | *retry* | &amp;check; | L’authentification Adobe Pass a recommandé une action qui peut corriger la situation telle que définie dans ce document. <br/><br/> Pour plus de détails, reportez-vous à la section [Action](#enhanced-error-codes-action) . |
| *status* | *integer* | *403* | &amp;check; | Code d’état de réponse HTTP tel que défini dans le document [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6). <br/><br/> Pour plus d’informations, reportez-vous à la section [Status](#enhanced-error-codes-status) . |
| *code* | *string* | *network_connection_failure* | &amp;check; | Code d’identifiant unique d’authentification Adobe Pass associé à l’erreur tel que défini dans ce document. <br/><br/> Pour plus d’informations, reportez-vous à la section [Code](#enhanced-error-codes-code) . |
| *message* | *string* | *Impossible de contacter les services de votre fournisseur de télévision* |            | Message lisible par l’utilisateur qui peut s’afficher dans certains cas à l’utilisateur final. <br/><br/> Pour plus d’informations, reportez-vous à la section [Gestion de la réponse](#enhanced-error-codes-response-handling) . |
| *details* | *string* | *Votre module d&#39;abonnement n&#39;inclut pas le canal &quot;En ligne&quot;* |            | Message détaillé qui peut être fourni par un partenaire de services dans certains cas, <br/><br/> Ce champ peut ne pas être présent si le partenaire de services ne fournit pas de message personnalisé. |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | URL de documentation publique de l’authentification Adobe Pass qui renvoie à des informations supplémentaires sur les raisons de cette erreur et les solutions possibles. <br/><br/> Ce champ contient une URL absolue et ne doit pas être déduit du code d’erreur. Selon le contexte de l’erreur, une autre URL peut être fournie. |
| *trace* | *string* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | Identifiant unique de la réponse qui peut être utilisé lors du contact de la prise en charge de l’authentification Adobe Pass pour résoudre des problèmes spécifiques. |

>[!IMPORTANT]
>
> La colonne **Restricted** indique si le champ respectif contient une valeur d’un ensemble fini, tandis que les champs non restreints peuvent contenir n’importe quelle donnée.
>
> <br/>
>
> Les futures mises à jour de ce document pourront ajouter des valeurs aux ensembles finis, mais ne supprimeront ni ne modifieront les ensembles existants.

### Action {#enhanced-error-codes-representation-action}

Les codes d’erreur améliorés incluent un champ &quot;action&quot; qui fournit une action recommandée susceptible de remédier à la situation.

Les valeurs possibles pour le champ &quot;action&quot; sont les suivantes :

| Action | Description | Catégorie |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| none | Il n’existe pas d’action prédéfinie pour résoudre ce problème, mais dans certains cas, cela peut indiquer un appel incorrect de l’API. | Corrigez le contexte de la requête. |
| configuration | L’application cliente nécessite un changement de configuration, effectué la plupart du temps via le tableau de bord Adobe Pass TVE. | Correction du contexte de configuration de l’intégration. |
| application-registration | L’application cliente doit à nouveau s’enregistrer. | Correction du contexte de l’application cliente. |
| authentication | L’application cliente nécessite l’authentification ou la réauthentification de l’utilisateur. | Correction du contexte de l’application cliente. |
| authorization | L’application cliente doit obtenir l’autorisation de la ressource spécifiée. | Correction du contexte de l’application cliente. |
| retry | L’application cliente doit réessayer la requête. | Corrigez le contexte de la requête. |

_(*) Pour certaines erreurs, plusieurs actions peuvent être des solutions possibles, mais le champ &quot;action&quot; indique celle avec la plus grande probabilité de corriger l&#39;erreur._

### Etat {#enhanced-error-codes-representation-status}

Les codes d’erreur améliorés incluent un champ &quot;status&quot; qui indique le code d’état HTTP associé à l’erreur.

Les valeurs possibles pour le champ &quot;status&quot; sont les suivantes :

| Code | Reason-Phrase |
|------|-----------------------|
| 400 | Requête incorrecte |
| 401 | Non autorisé |
| 403 | Interdit |
| 404 | Introuvable |
| 405 | Méthode non autorisée |
| 410 | Gone |
| 412 | Echec de la condition |
| 500 | Erreur interne du serveur |

Les codes d’erreur améliorés avec un &quot;statut&quot; 4xx s’affichent généralement lorsque l’erreur est générée par le client et la plupart du temps, cela implique que le client nécessite un travail supplémentaire pour la corriger.

Les codes d’erreur améliorés avec un &quot;statut&quot; 5xx s’affichent généralement lorsque l’erreur est générée par le serveur et la plupart du temps, cela implique que le serveur nécessite un travail supplémentaire pour la corriger.

>[!IMPORTANT]
>
> Dans certains cas, le code d’état de la réponse HTTP diffère du champ &quot;État&quot; du code d’erreur amélioré, en particulier lors de l’interaction avec une API d’authentification Adobe Pass qui communique des codes d’erreur améliorés en tant qu’informations d’erreur au niveau de l’élément.

### Code {#enhanced-error-codes-representation-code}

Les codes d’erreur améliorés incluent un champ &quot;code&quot; qui fournit un identifiant unique d’authentification Adobe Pass associé à l’erreur.

Les valeurs possibles du champ &quot;code&quot; sont agrégées [ci-dessous](#enhanced-error-codes-list) dans deux listes basées sur l’API d’authentification Adobe Pass intégrée.

## Listes {#enhanced-error-codes-lists}

### API REST v1 {#enhanced-error-codes-lists-rest-api-v1}

Le tableau ci-dessous répertorie les codes d’erreur améliorés possibles qu’une application cliente peut rencontrer lors de son intégration à l’API REST d’authentification Adobe Pass v1.

| Action | Code | Etat | Message |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **none** | *invalid_requestor* | 400 | Le paramètre du demandeur est absent ou non valide. |
|                    | *invalid_device_info* | 400 | Les informations de l’appareil sont manquantes ou non valides. |
|                    | *invalid_device_id* | 400 | L’identifiant de l’appareil est absent ou non valide. |
|                    | *missing_resource* | 400, 412 | Le paramètre de ressource est manquant. |
|                    | *malformed_authz_request* | 400, 412 | La demande d’autorisation est nulle ou non valide. |
|                    | *preauthorization_denied_by_mvpd* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; lors de la demande d’autorisation préalable pour la ressource spécifiée. |
|                    | *authorization_denied_by_mvpd* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; lors de la demande d’autorisation pour la ressource spécifiée. |
|                    | *authorization_denied_by_parental_control* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; en raison des paramètres de contrôle parental pour la ressource spécifiée. |
|                    | *internal_error* | 400, 405, 500 | La demande a échoué en raison d’une erreur du serveur interne. |
| **configuration** | *unknown_integration* | 400, 412 | L’intégration entre le programmeur spécifié et le fournisseur d’identité n’existe pas. Utilisez le tableau de bord TVE pour créer l’intégration requise. |
|                    | *too_many_resources* | 403 | La demande d’autorisation ou de préautorisation a échoué car trop de ressources ont été interrogées. Veuillez contacter l’équipe d’assistance pour configurer correctement les limites d’autorisation et de préautorisation. |
| **authentication** | *authentication_session_issuer_mismatch* | 400 | La demande d’autorisation a échoué en raison du fait que le MVPD indiqué pour le flux d’autorisation est différent de celui qui a émis la session d’authentification. L’utilisateur doit se réauthentifier auprès du MVPD souhaité pour continuer. |
|                    | *authorization_denied_by_hba_policies* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; en raison de stratégies d’authentification basées sur le domicile. L’authentification actuelle a été obtenue à l’aide d’un flux d’authentification domestique, mais l’appareil n’est plus à la maison lors de la demande d’autorisation pour la ressource spécifiée. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|                    | *authorization_denied_by_session_invalidate* | 403 | La session d’authentification a été invalidée par le fournisseur d’identité. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|                    | *identity_not_known_by_mvpd* | 403 | La demande d’autorisation a échoué en raison du fait que l’identité de l’utilisateur n’a pas été reconnue par le MVPD. |
|                    | *authentication_session_invalidate* | 403 | La session d’authentification a été invalidée par le fournisseur d’identité. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|                    | *authentication_session_missing* | 403, 412 | La session d’authentification associée à cette requête n’a pas pu être récupérée. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|                    | *authentication_session_expirée* | 403, 412 | La session d’authentification actuelle a expiré. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|                    | *preauthorization_authentication_session_missing* | 412 | La session d’authentification associée à cette requête n’a pas pu être récupérée. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|                    | *preauthorization_authentication_session_expirée* | 412 | La session d’authentification actuelle a expiré. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
| **authorization** | *authorization_not_found* | 403, 404 | Aucune autorisation n’a été trouvée pour la ressource spécifiée. Pour pouvoir continuer, l’utilisateur doit obtenir une nouvelle autorisation. |
|                    | *authorization_expirée* | 410 | L’autorisation précédente pour la ressource spécifiée a expiré. Pour pouvoir continuer, l’utilisateur doit obtenir une nouvelle autorisation. |
| **retry** | *network_received_error* | 403 | Une erreur de lecture s’est produite lors de la récupération de la réponse du service partenaire associé. Une nouvelle tentative de requête peut résoudre le problème. |
|                    | *network_connection_timeout* | 403 | Un délai d’expiration de connexion a été atteint avec le service partenaire associé. Une nouvelle tentative de requête peut résoudre le problème. |
|                    | *maximum_execution_time_exceeded* | 403 | La demande ne s’est pas terminée dans le délai maximal autorisé. Une nouvelle tentative de requête peut résoudre le problème. |

### API de préautorisation des SDK {#enhanced-error-codes-lists-sdks-preauthorize-api}

Reportez-vous à la [section](#enhanced-error-codes-list-rest-api-v1) précédente pour connaître les éventuels codes d’erreur améliorés qu’une application cliente peut rencontrer lors de son intégration avec l’API de préautorisation des SDK d’authentification Adobe Pass.

### API REST v2 {#enhanced-error-codes-lists-rest-api-v2}

Le tableau ci-dessous répertorie les codes d’erreur améliorés possibles qu’une application cliente peut rencontrer lors de son intégration à l’API REST d’authentification Adobe Pass v2.

| Action | Code | Etat | Message |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **none** | *invalid_parameter_service_provider* | 400 | La valeur du paramètre du fournisseur de services est absente ou non valide. |
|                              | *invalid_parameter_mvpd* | 400 | La valeur du paramètre mvpd est absente ou non valide. |
|                              | *invalid_parameter_code* | 400 | La valeur du paramètre de code est absente ou non valide. |
|                              | *invalid_parameter_resources* | 400 | La valeur du paramètre resources est absente ou non valide. |
|                              | *invalid_parameter_redirect_url* | 400 | La valeur du paramètre d’URL de redirection est absente ou non valide. |
|                              | *invalid_parameter_partner* | 400 | La valeur du paramètre partenaire est absente ou non valide. |
|                              | *invalid_parameter_saml_response* | 400 | La valeur du paramètre de réponse SAML est absente ou non valide. |
|                              | *invalid_header_device_info* | 400 | La valeur de l’en-tête des informations sur le périphérique est manquante ou non valide. |
|                              | *invalid_header_device_identifier* | 400 | La valeur de l’en-tête de l’identifiant de l’appareil est manquante ou non valide. |
|                              | *invalid_header_identity_for_temporaire_access* | 400 | L’identité de la valeur d’en-tête d’accès temporaire est manquante ou non valide. |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | La valeur d’état d’accès aux autorisations de l’en-tête d’état de la structure partenaire n’est pas présente. |
|                              | *}invalid_header_pfs_permission_access_not_détermination{1* | 400 | La valeur de l’état d’accès aux autorisations de l’en-tête de l’état de la structure du partenaire est indéterminée. |
|                              | *invalid_header_pfs_permission_access_not_authorized* | 400 | La valeur d’état d’accès aux autorisations de l’en-tête d’état de la structure partenaire n’est pas accordée. |
|                              | *invalid_header_pfs_provider_id_not_Determin* | 400 | La valeur de l’identifiant du fournisseur de l’en-tête de statut de la structure du partenaire n’est pas associée à un mvpd connu. |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | La valeur de l’ID de fournisseur de l’en-tête d’état de la structure du partenaire ne correspond pas au paramètre mvpd envoyé. |
|                              | *invalid_integration* | 400 | L’intégration entre le fournisseur de services spécifié et mvpd n’existe pas ou est désactivée. |
|                              | *invalid_authentication_session* | 400 | La session d’authentification associée à cette requête est manquante ou non valide. |
|                              | *preauthorization_denied_by_mvpd* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; lors de la demande d’autorisation préalable pour la ressource spécifiée. |
|                              | *authorization_denied_by_mvpd* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; lors de la demande d’autorisation pour la ressource spécifiée. |
|                              | *authorization_denied_by_parental_control* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; en raison des paramètres de contrôle parental pour la ressource spécifiée. |
|                              | *authorization_denied_by_dégradation_rule* | 403 | Une règle de dégradation est appliquée à l’intégration entre le fournisseur de services spécifié et mvpd, qui refuse l’autorisation pour les ressources demandées. |
|                              | *internal_server_error* | 500 | La demande a échoué en raison d’une erreur du serveur interne. |
| **configuration** | *too_many_resources* | 403 | La demande d’autorisation ou de préautorisation a échoué car trop de ressources ont été interrogées. Veuillez contacter l’équipe d’assistance pour configurer correctement les limites d’autorisation et de préautorisation. |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | La configuration du certificat de métadonnées utilisateur est manquante ou non valide. |
|                              | *invalid_configuration_temporaire_access* | 500 | La configuration de l’accès temporaire n’est pas valide. |
|                              | *invalid_configuration_platform* | 500 | La configuration de la plateforme est manquante ou non valide pour l’intégration. |
|                              | *invalid_configuration_platform_id* | 500 | La configuration de l’ID de plateforme est manquante ou non valide. |
|                              | *invalid_configuration_platform_trait* | 500 | La configuration des caractéristiques de la plateforme est manquante ou non valide. |
|                              | *invalid_configuration_platform_category_trait* | 500 | La configuration des caractéristiques de la catégorie de plateforme est manquante ou non valide. |
|                              | *invalid_configuration_platform_services* | 500 | La configuration des services Platform est manquante ou non valide pour l’intégration. |
|                              | *invalid_configuration_mvpd_platform* | 500 | La configuration de la plateforme mvpd est manquante ou non valide pour mvpd et platform. |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | La configuration de statut d’intégration de la plateforme mvpd est manquante ou non valide pour mvpd et platform. |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | La configuration de l’exchange de profil de la plateforme mvpd est manquante ou non valide pour mvpd et platform. |
| **application-registration** | *invalid_access_token_service_provider* | 401 | Le jeton d’accès n’est pas valide en raison d’un fournisseur de services non valide. |
|                              | *invalid_access_token_client_application* | 401 | Le jeton d’accès n’est pas valide en raison d’une application cliente non valide. |
| **authentication** | *authenticated_profile_missing* | 403 | Le profil authentifié associé à cette requête est manquant. |
|                              | *authenticated_profile_expirée* | 403 | Le profil authentifié associé à cette requête a expiré. |
|                              | *authenticated_profile_invalidate* | 403 | Le profil authentifié associé à cette requête a été invalidé. |
|                              | *temporaire_access_duration_limit_exceeded* | 403 | La limite de durée d’accès temporaire a été dépassée. |
|                              | *temporaire_access_resources_limit_exceeded* | 403 | La limite des ressources d’accès temporaires a été dépassée. |
|                              | *authorization_denied_by_hba_policies* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; en raison de stratégies d’authentification basées sur le domicile. L’authentification actuelle a été obtenue par le biais d’un flux d’authentification basé sur l’accueil, mais l’appareil n’est plus à la maison lors de la demande d’autorisation pour la ressource spécifiée. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|                              | *authorization_denied_by_session_invalidate* | 403 | La session d’authentification a été invalidée par le fournisseur d’identité. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|                              | *identity_not_known_by_mvpd* | 403 | La demande d’autorisation a échoué en raison du fait que l’identité de l’utilisateur n’a pas été reconnue par le MVPD. |
| **retry** | *network_received_error* | 403 | Une erreur de lecture s’est produite lors de la récupération de la réponse du service partenaire associé. Une nouvelle tentative de requête peut résoudre le problème. |
|                              | *network_connection_timeout* | 403 | Un délai d’expiration de connexion a été atteint avec le service partenaire associé. Une nouvelle tentative de requête peut résoudre le problème. |
|                              | *maximum_execution_time_exceeded* | 403 | La demande ne s’est pas terminée dans le délai maximal autorisé. Une nouvelle tentative de requête peut résoudre le problème. |

## Gestion des réponses {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> Il existe des codes d’erreur améliorés qui peuvent être gérés automatiquement dans le code de l’application cliente, comme une nouvelle tentative de demande d’autorisation en cas de dépassement de délai du réseau ou une nouvelle authentification de l’utilisateur lorsque sa session a expiré, mais d’autres types peuvent nécessiter des modifications de configuration ou une interaction de l’équipe d’assistance clientèle d’Adobe Pass Authentication.
>
> <br/>
>
> Par conséquent, il est important de collecter et de fournir des informations d’erreur complètes lors de la création d’un ticket à l’aide de notre [Zendesk](https://adobeprimetime.zendesk.com), pour vous assurer que les modifications nécessaires sont effectuées avant de lancer la nouvelle application ou la nouvelle fonctionnalité.

En résumé, lorsque vous gérez des réponses contenant des codes d’erreur améliorés, tenez compte des points suivants :

1. **Vérifiez les deux valeurs d’état** : vérifiez toujours le code d’état de la réponse HTTP et le champ &quot;status&quot; du code d’erreur amélioré. Elles peuvent différer et elles fournissent toutes deux des informations précieuses.

1. **Agnostique par rapport aux informations d’erreur de niveau supérieur par rapport à l’élément** : gérez les informations d’erreur de niveau supérieur et d’élément indépendamment de la manière dont elles sont communiquées, assurez-vous que vous pouvez gérer les deux formes de transmission des codes d’erreur améliorés.

1. **Retry logic** : pour les erreurs qui nécessitent une nouvelle tentative, assurez-vous que les reprises sont effectuées avec un backoff exponentiel afin d’éviter une surcharge du serveur. En outre, dans le cas des API d’authentification Adobe Pass qui gèrent plusieurs éléments à la fois (par exemple, API de préautorisation), vous devez inclure dans la requête répétée uniquement les éléments marqués par &quot;reprise&quot; et non la liste entière.

1. **Modifications de configuration** : pour les erreurs qui nécessitent des modifications de configuration, assurez-vous que les modifications nécessaires sont effectuées avant de lancer la nouvelle application ou la nouvelle fonctionnalité.

1. **Authentification et autorisation** : pour les erreurs liées à l’authentification et à l’autorisation, vous devez inviter l’utilisateur à se réauthentifier ou à obtenir une nouvelle autorisation, si nécessaire.

1. **Commentaires de l’utilisateur** : facultatif, utilisez les champs &quot;message&quot; et (potentiel) &quot;détails&quot; lisibles par l’utilisateur pour informer l’utilisateur du problème. Le message texte &quot;détails&quot; peut être transmis à partir des points de terminaison de préautorisation ou d’autorisation MVPD ou du programmeur lors de l’application des règles de dégradation.
