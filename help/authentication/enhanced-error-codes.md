---
title: Amélioration des codes d’erreur
description: Amélioration des codes d’erreur
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 87639ad93d8749ae7b1751cd13a099ccfc2636ac
workflow-type: tm+mt
source-wordcount: '2299'
ht-degree: 2%

---

# Amélioration des codes d’erreur {#enhanced-error-codes}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Présentation {#overview}

Ce document décrit la liste des codes d’erreur d’API et des informations d’erreur supplémentaires renvoyés aux applications.

Pour utiliser des codes d’erreur améliorés dans l’application Programmeurs, une demande doit être envoyée à l’équipe d’assistance afin de l’activer avec une modification de configuration.

## Gestion des erreurs de réponse {#response-error-handling}

Dans la plupart des scénarios, l’API d’authentification Adobe Pass inclut des informations d’erreur supplémentaires dans le corps de la réponse afin de fournir **contexte significatif** à la raison pour laquelle une erreur s’est produite et/ou des solutions possibles pour résoudre automatiquement le problème.  *Cependant, dans certains cas spécifiques impliquant des flux d’authentification ou de déconnexion, les services d’authentification Adobe Pass peuvent renvoyer une réponse de HTML ou un corps vide. Pour plus d’informations, consultez la documentation de l’API.*

Bien que certains types d’erreurs puissent être gérés automatiquement (par exemple, une nouvelle tentative de demande d’autorisation en cas d’expiration du réseau ou une nouvelle authentification de l’utilisateur si sa session a expiré), d’autres types peuvent nécessiter des modifications de configuration ou une interaction de l’équipe d’assistance clientèle. Il est important que les programmeurs collectent et fournissent des informations complètes sur les erreurs dans de tels cas.

L’API d’authentification Adobe Pass renvoie des codes d’état HTTP compris entre 400 et 500 pour indiquer les échecs ou les erreurs. Chaque code d’état HTTP a certaines implications :

- Les codes d’erreur 4xx impliquent que l’erreur est générée par le client et que ce dernier doit effectuer un travail supplémentaire pour la corriger (par exemple, obtenir un jeton d’accès avant d’appeler des API protégées ou fournir tout paramètre requis).

- Les codes d’erreur 5xx impliquent que l’erreur est générée par le serveur et que le serveur doit effectuer un travail supplémentaire pour la corriger.

Les informations d’erreur supplémentaires sont incluses dans le champ &quot;erreur&quot; du corps de la réponse.

<table>
<thead>
  <tr>
    <th>Nom</th>
    <th>Type</th>
    <th>Exemple</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>erreur</td>
    <td><i>objet</i></td>
    <td><strong>JSON</strong>
    <br>
    <code>{<br>&nbsp;&nbsp;&nbsp;&nbsp;"status" : 403,<br>&nbsp;&nbsp;&nbsp;&nbsp;"code" : "network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"message" : "Unable to contact your TV provider<br>&nbsp;&nbsp;&nbsp;&nbsp;services",<br>&nbsp;&nbsp;&nbsp;&nbsp;"helpUrl" : "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;"trace" : "12f6fef9-d2e0-422b-a9d7-60d799abe353",<br>&nbsp;&nbsp;&nbsp;&nbsp;"action" : "retry"<br>}
    </code>
    <p>
    <p>
    <strong>XML</strong>
    <br>
    <code>&lt;error&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;status&gt;403&lt;/status&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;code&gt;network_connection_failure&lt;/code&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;message&gt;Unable to contact your TV provider services&lt;/message&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;helpUrl&gt;https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html&lt;/helpUrl&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;trace>12f6fef9-d2e0-422b-a9d7-60d799abe353&lt;/trace&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;action>retry&lt;/action&gt;<br>&lt;/error&gt;
    </code>
    </td>
    <td>Fait référence aux objets de collection ou d’erreur collectés lors de la tentative d’exécution de la requête.</td>
  </tr>
</tbody>
</table>

</br>

Les API Adobe Pass qui gèrent plusieurs éléments (API de préautorisation, etc.) peuvent indiquer si le traitement a échoué pour un élément particulier et si d’autres éléments ont été traités avec succès à l’aide des informations d’erreur au niveau de l’élément. Dans ce cas, la variable ***&quot;error&quot;*** se trouve au niveau de l’élément et le corps de la réponse peut contenir plusieurs ***&quot;errors&quot;*** objets - consultez la documentation de l’API.

</br>

**Exemple avec succès partiel et erreur de niveau élément**

```json
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream2",
            "authorized" : false,
            "error" : {
 
               "status" : 403,
               "code" : "network_connection_failure",
               "message" : "Unable to contact your TV provider services",
               "details" : "",
               "helpUrl" : "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
               "trace" : "8bcb17f9-b172-47d2-86d9-3eb146eba85e",
               "action" : "retry"
            }
 
        }
    ]
}
```

</br>

Chaque objet d’erreur possède les paramètres suivants :

| Nom | Type | Exemple | Restricted | Description |
|---|---|----|:---:|---|
| *status* | *entier* | *403* | &amp;check; | Le code d’état HTTP de réponse comme indiqué dans la norme RFC 7231 (<https://tools.ietf.org/html/rfc7231#section-6>) <ul><li>400 Bad Request</li><li>401 Non autorisé</li><li>403 Forbidden</li><li>404 Introuvable</li><li>405 Méthode non autorisée</li><li>Conflit 409</li><li>410 Gone</li><li>412 Echec de la condition préalable</li><li>429 Too many requests</li><li>Erreur 500 Interval server</li><li>Service 503 indisponible</li></ul> |
| *code* | *string* | *network_connection_failure* | &amp;check; | Code d’erreur d’authentification Adobe Pass standard. La liste complète des codes d’erreur est incluse ci-dessous. |
| *message* | *string* | *Impossible de contacter les services de votre fournisseur de télévision* | | Message lisible par l’utilisateur final. |
| *détails* | *string* | *Votre module d’abonnement n’inclut pas le canal &quot;En ligne&quot;* | | Dans certains cas, un message détaillé est fourni par les points de terminaison d’autorisation MVPD ou par le programmeur via des règles de dégradation. <p> Notez que si aucun message personnalisé n’a été reçu des services partenaires, il se peut que ce champ ne figure pas dans les champs d’erreur. |
| *helpUrl* | *url* | &quot;`http://`&quot; | | URL permettant d’obtenir plus d’informations sur les raisons de cette erreur et les solutions possibles. <p>L’URI représente une URL absolue et ne doit pas être déduite du code d’erreur. Selon le contexte de l’erreur, une autre URL peut être fournie. Par exemple, le même code d’erreur bad_request génère des URL différentes pour les services d’authentification et d’autorisation. |
| *trace* | *string* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* | | Identifiant unique de cette réponse qui peut être utilisé lors du contact de l’assistance pour identifier des problèmes spécifiques dans des scénarios plus complexes. |
| *action* | *string* | *retry* | &amp;check; | Action recommandée pour remédier à la situation : <ul><li> *none* - Malheureusement, il n&#39;y a pas d&#39;action prédéfinie pour résoudre ce problème. Cela peut indiquer un appel incorrect de l’API publique</li><li>*configuration* - Un changement de configuration est nécessaire par le biais du tableau de bord TVE ou en contactant l’assistance. </li><li>*application-registration* - La demande doit s&#39;enregistrer à nouveau. </li><li>*authentication* - L’utilisateur doit s’authentifier ou se reconnecter. </li><li>*authorization* - L’utilisateur doit obtenir l’autorisation de la ressource spécifique. </li><li>*dégradation* - Une forme de dégradation doit être appliquée. </li><li>*retry* - Une nouvelle tentative de requête peut résoudre le problème.</li><li>*retry-after* - Une nouvelle tentative de requête après la période indiquée peut résoudre le problème.</li></ul> |

</br>

**Notes:**

- ***Restricted*** column *indique si la valeur de champ correspondante représente un ensemble fini.* (par exemple, les codes d’état HTTP existants pour &quot;*status*&quot;). Les futures mises à jour de cette spécification pourraient ajouter des valeurs à la liste restreinte, mais ne supprimeront ni ne modifieront les valeurs existantes. Les champs non restreints peuvent généralement contenir n’importe quelle donnée, mais des restrictions peuvent être mises en place pour garantir une taille raisonnable.

- Chaque réponse de l’Adobe contiendra un &quot;Adobe-Request-Id&quot; qui identifie la demande du client dans l’ensemble de nos services HTTP. Le &quot;**trace**&quot; complète cela et doit faire l’objet de rapports ensemble.

## Codes d’état HTTP et d’erreur {#http-status-codes-and-error-codes}

Les incohérences entre les différents codes d’erreur et les codes d’état HTTP associés sont dues aux exigences de compatibilité descendante avec les anciens sdk et les applications ( par exemple *unknown\_application* génère 400 Bad Request tandis que *unknown\_software\_statement* renvoie 401 (non autorisé). La résolution de ces incohérences sera ciblée lors des prochaines itérations.

## Actions et codes d’erreur {#actions-and-error-codes}

Pour la plupart des codes d’erreur, plusieurs actions peuvent être éligibles pour résoudre le problème en question ou plusieurs actions peuvent être requises pour les corriger automatiquement. Nous avons choisi d’indiquer celle avec la probabilité la plus élevée pour corriger l’erreur. La variable **actions** peut être divisé en trois catégories :

1. Ceux qui tentent de corriger le contexte de la requête (reprise, reprise après)
1. Ceux qui tentent de corriger le contexte de l’utilisateur dans l’application (enregistrement-application, authentification, autorisation)
1. Ceux qui tentent de corriger le contexte d&#39;intégration entre une application et un fournisseur d&#39;identité (configuration, dégradation)

Pour la première catégorie (reprise et reprise après), une nouvelle tentative suffit pour résoudre le problème. Dans le cas des API qui gèrent plusieurs éléments, l’application doit répéter la requête et inclure uniquement les éléments avec l’action &quot;reprise&quot; ou &quot;reprise après&quot;. Pour &quot;*retry-after*&quot;, une<u>Retry-After</u>&quot; indique combien de secondes l’application doit attendre avant de répéter la requête.

Pour les 2e et 3e catégories, l’implémentation de l’action réelle dépend fortement des fonctionnalités de l’application. Par exemple, &quot;*dégradation*&quot; peut être mis en oeuvre sous la forme de &quot;passages temporaires de 15 minutes pour permettre aux utilisateurs de lire le contenu&quot; ou d’&quot;outil automatique pour appliquer la dégradation AUTHN-ALL ou AUTHZ-ALL pour son intégration au MVPD spécifié&quot;. Similaire à &quot;*authentication*&quot;peut déclencher une authentification passive (authentification back-channel) sur une tablette et un flux d’authentification plein-écran sur les télévisions connectées. C’est pourquoi nous avons choisi de fournir des URL complètes avec schéma et tous les paramètres.

## Codes d’erreur {#error-codes}

Le tableau des erreurs ci-dessous répertorie les codes d’erreur possibles, les messages associés et les actions possibles.

| Action | Code d’erreur | Code d’état HTTP | Description |
|---|---|---|---|
| **none** | *authorization_denied_by_mvpd* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; lors de la demande d’autorisation pour la ressource spécifiée. |
|  | *authorization_denied_by_parent_control* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; en raison des paramètres de contrôle parental pour la ressource spécifiée. |
|  | *authorization_denied_by_programmer* | 403 | La règle de dégradation appliquée par le programmeur applique une décision &quot;Refuser&quot; pour l’utilisateur actuel. |
|  | *bad_request* | 400 | La requête API n’est pas valide ou n’a pas été correctement formée. Consultez la documentation de l’API pour déterminer les exigences en matière de requête. |
|  | *individualization_service_unavailable* | 503 | La demande a échoué en raison de l’indisponibilité du service d’individualisation. |
|  | *internal_error* | 500 | La demande a échoué en raison d’une erreur du serveur interne. |
|  | *invalid_client_time* | 400 | La date/l’heure/le fuseau horaire de l’ordinateur client n’est pas correctement définie. Cela entraînera probablement des erreurs d’authentification/d’autorisation. |
|  | *invalid_custom_scheme* | 400 | Le schéma personnalisé spécifié utilisé dans l’enregistrement de l’application n’est pas reconnu. Vérifiez la configuration du tableau de bord TVE pour connaître les valeurs de schéma personnalisées appropriées. |
|  | *invalid_domain* | 400 | Le demandeur utilise un domaine non valide. Tous les domaines utilisés par un identifiant de demandeur particulier doivent être placés sur liste blanche par Adobe. |
|  | *invalid_header* | 400 | La requête a échoué car elle contenait un en-tête non valide. Consultez la documentation de l’API pour déterminer les en-têtes valides pour votre requête et s’il existe des limites pour leur valeur. |
|  | *invalid_http_method* | 405 | La méthode HTTP associée à la requête n’est pas prise en charge. Consultez la documentation de l’API pour déterminer les méthodes HTTP prises en charge pour votre requête. |
|  | *invalid_parameter_value* | 400 | La requête a échoué car elle contenait un paramètre ou une valeur de paramètre non valide. Consultez la documentation de l’API pour déterminer les paramètres valides pour votre requête et s’il existe des limites pour leur valeur. |
|  | *invalid_resource_value* | 400 | La requête a échoué car une ressource non valide ou incorrecte a été utilisée. Consultez la documentation de l’API pour déterminer comment les ressources complexes doivent être codées pour votre requête et si leur valeur et/ou leur taille sont limitées. |
|  | *invalid_registration_code* | 404 | Le code d’enregistrement spécifié n’est plus valide ou a expiré. |
|  | *invalid_service_configuration* | 500 | La demande a échoué en raison d’une configuration de service incorrecte. |
|  | *missing_authentication_header* | 400 | La requête a échoué car elle ne contient pas l’en-tête d’authentification requis pour l’API spécifique. |
|  | *missing_resource_mapping* | 400 | Il n’existe aucun mappage correspondant pour la ressource spécifiée. Contactez l’assistance pour corriger le mappage requis. |
|  | *preauthorization_denied_by_mvpd* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; lors de la demande d’autorisation préalable pour la ressource spécifiée. |
|  | *preauthorization_denied_by_programmer* | 403 | Les règles de dégradation appliquées par le programmeur appliquent une décision &quot;Refuser&quot; pour l’utilisateur actuel. |
|  | *registration_code_service_unavailable* | 503 | La demande a échoué car le service de code d’enregistrement n’est pas disponible. |
|  | *service_unavailable* | 503 | La demande a échoué en raison du fait que le service d’authentification ou d’autorisation n’est pas disponible. |
|  | *access_token_unavailable* | 400 | La demande a échoué en raison d’une erreur inattendue lors de la récupération du jeton d’accès. Veuillez consulter la configuration du tableau de bord TVE pour connaître les instructions de logiciels disponibles et les schémas personnalisés enregistrés. |
|  | *unsupported_client_version* | 400 | Cette version du SDK d’authentification Adobe Pass est trop ancienne et n’est plus prise en charge. Veuillez consulter la documentation de l’API pour connaître les étapes nécessaires à la mise à niveau vers la dernière version. |
| **configuration** | *network_required_ssl* | 403 | Il existe un problème de connexion SSL pour le service de partenaire cible. Veuillez contacter l’équipe d’assistance. |
|  | *too_many_resources* | 403 | La demande d’autorisation ou de préautorisation a échoué car trop de ressources ont été interrogées. Veuillez contacter l’équipe d’assistance pour configurer correctement les limites d’autorisation et de préautorisation. |
|  | *unknown_programmer* | 400 | Le programmeur ou le prestataire n&#39;est pas reconnu. Utilisez le tableau de bord TVE pour enregistrer le programmeur spécifié. |
|  | *unknown_application* | 400 | L’application n’est pas reconnue. Utilisez le tableau de bord TVE pour enregistrer l’application spécifiée. |
|  | *unknown_integration* | 400 | L’intégration entre le programmeur spécifié et le fournisseur d’identité n’existe pas. Utilisez le tableau de bord TVE pour créer l’intégration requise. |
|  | *unknown_software_statement* | 401 | L’instruction logicielle associée au jeton d’accès n’est pas reconnue. Contactez l’équipe d’assistance afin de clarifier l’état de la déclaration logicielle. |
| **application-registration** | *access_token_expired* | 401 | Le jeton d’accès a expiré. L’application doit actualiser le jeton d’accès comme indiqué dans la documentation de l’API. |
|  | *invalid_access_token_signature* | 401 | La signature du jeton d’accès n’est plus valide. L’application doit actualiser le jeton d’accès comme indiqué dans la documentation de l’API. |
|  | *invalid_client_id* | 401 | L’identifiant du client associé n’est pas reconnu. L’application doit suivre le processus d’enregistrement de l’application, comme indiqué dans la documentation de l’API. |
| **authentication** | *authentication_session_expiré* | 410 | La session d’authentification actuelle a expiré. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|  | *authentication_session_missing* | 401 | La session d’authentification associée à cette requête n’a pas pu être récupérée. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|  | *authentication_session_invalidate* | 401 | La session d’authentification a été invalidée par le fournisseur d’identité. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|  | *authentication_session_issuer_mismatch* | 400 | La demande d’autorisation a échoué en raison du fait que le MVPD indiqué pour le flux d’autorisation est différent de celui qui a émis la session d’authentification. L’utilisateur doit se réauthentifier auprès du MVPD souhaité pour continuer. |
|  | *authorization_denied_by_hba_policies* | 403 | Le MVPD a renvoyé une décision &quot;Refuser&quot; en raison de stratégies d’authentification basées sur le domicile. L’authentification actuelle a été obtenue à l’aide d’un flux d’authentification domestique, mais l’appareil n’est plus à la maison lors de la demande d’autorisation pour la ressource spécifiée. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer. |
|  | *identity_not_supported_by_mvpd* | 403 | La demande d’autorisation a échoué en raison du fait que l’identité de l’utilisateur n’a pas été reconnue par le MVPD. |
| **authorization** | *authorization_expired* | 410 | L’autorisation précédente pour la ressource spécifiée a expiré. Pour pouvoir continuer, l’utilisateur doit obtenir une nouvelle autorisation. |
|  | *authorization_not_found* | 404 | Aucune autorisation n’a été trouvée pour la ressource spécifiée. Pour pouvoir continuer, l’utilisateur doit obtenir une nouvelle autorisation. |
|  | *device_identifier_mismatch* | 403 | L’identifiant d’appareil spécifié ne correspond pas à l’identification de l’appareil d’autorisation. Pour pouvoir continuer, l’utilisateur doit obtenir une nouvelle autorisation. |
| **retry** | *network_connection_failure* | 403 | Une connexion a échoué avec le service partenaire associé. Une nouvelle tentative de requête peut résoudre le problème. |
|  | *network_connection_timeout* | 403 | Un délai d’expiration de connexion a été atteint avec le service partenaire associé. Une nouvelle tentative de requête peut résoudre le problème. |
|  | *network_received_error* | 403 | Une erreur de lecture s’est produite lors de la récupération de la réponse du service partenaire associé. Une nouvelle tentative de requête peut résoudre le problème. |
|  | *maximum_execution_time_exceeded* | 403 | La demande ne s’est pas terminée dans le délai maximal autorisé. Une nouvelle tentative de requête peut résoudre le problème. |
| **retry-after** | *too_many_requests* | 429 | Trop de requêtes ont été envoyées dans un intervalle donné. L’application peut réessayer la requête après la période suggérée. |
|  | *user_rate_limit_exceeded* | 429 | Trop de requêtes ont été émises par un utilisateur spécifique au cours d’un intervalle donné. L’application peut réessayer la requête après la période suggérée. |
