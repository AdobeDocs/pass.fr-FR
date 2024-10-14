---
title: Préautoriser
description: Préautoriser JavaScript
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 6c328eb2c635a1d76fc7dae8148a4de291c126e0
workflow-type: tm+mt
source-wordcount: '1465'
ht-degree: 0%

---

# Préautoriser {#js-preauthorize}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#preauth-overview}

La méthode API de préautorisation doit être utilisée par les applications pour obtenir des décisions de préautorisation pour une ou plusieurs ressources. La demande de préautorisation d’API doit être utilisée pour les conseils d’interface utilisateur et/ou le filtrage de contenu. Une demande d’API d’autorisation réelle doit être effectuée avant d’autoriser l’accès de l’utilisateur aux ressources spécifiées.

Si une erreur inattendue (par exemple, un problème de réseau et un point de terminaison d’autorisation MVPD non disponible) se produit lorsqu’une demande d’API de préautorisation est traitée par les services d’authentification Adobe Pass, alors une ou plusieurs informations d’erreur distinctes seront incluses pour les ressources affectées dans le cadre du résultat de la réponse de l’API de préautorisation.

### public preauthorized(request: PreauthorizedRequest, callback: AccessEnablerCallback&lt;any>): void {#preauth-method}

**Description :** Cette méthode doit être utilisée par les applications pour obtenir des décisions de préautorisation (informatives) de l’utilisateur authentifié du service d’authentification Adobe Pass afin d’afficher des ressources protégées spécifiques, dans le but principal de décorer l’interface utilisateur de l’application (par exemple, indiquer l’état d’accès avec des icônes de verrouillage et de déverrouillage).

**Disponibilité :** v4.4.0+

**Paramètres :**

* `PreauthorizeRequest` : objet créateur utilisé pour définir la requête
* `AccessEnablerCallback` : rappel utilisé pour renvoyer la réponse de l’API
* `PreauthorizeResponse` : objet utilisé pour renvoyer le contenu de réponse de l’API

### class PreauthorizedRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string[]): PreauthorizedRequestBuilder {#set-res-preath-req-buildr}

* Définit la Liste des ressources pour lesquelles vous souhaitez obtenir des décisions de préautorisation.
* Il est obligatoire de le définir pour l’utilisation de l’API de préautorisation.
* Chaque élément de la liste doit être une chaîne représentant soit la valeur de l’ID de ressource, soit le fragment RSS du média qui doit être accepté avec le MVPD.
* Cette méthode définit les informations uniquement dans le contexte de l’instance d’objet `PreauthorizeRequestBuilder` actuelle, qui est le récepteur de cet appel de méthode.

* Pour créer un `PreauthorizeRequest` réel, vous pouvez consulter la méthode `PreauthorizeRequestBuilder` :

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` ressources. Liste des ressources pour lesquelles vous souhaitez obtenir des décisions de préautorisation.
* `@returns {PreauthorizeRequestBuilder}` Référence à la même instance d’objet `PreauthorizeRequestBuilder`, qui est le récepteur de l’appel de méthode.
* Cela permet de créer un chaînage de méthodes.

#### disableFeatures(...features: string[]): PreauthorizedRequestBuilder {#disabl-featres-preauth-req-buildr}

* Définit les fonctionnalités dont vous souhaitez qu’elles soient désactivées lors de l’obtention des décisions de préautorisation.
* Cette fonction définit les informations uniquement dans le contexte de l’instance d’objet `PreauthorizeRequestBuilder` actuelle, qui est le récepteur de cet appel de fonction.
* Pour créer un `PreauthorizeRequest` réel, vous pouvez consulter la fonction `PreauthorizeRequestBuilder` :

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` fonctionnalités. Ensemble de fonctionnalités que vous souhaitez désactiver.
* `@returns` Référence à la même instance d’objet `PreauthorizeRequestBuilder`, qui est le récepteur de l’appel de fonction.
* Il effectue cette opération afin de permettre la création d’un chaînage de fonctions.

#### build(): PreauthorizedRequest {#preauth-req}

* Crée et récupère la référence d’une nouvelle instance d’objet `PreauthorizeRequest`.
* Cette méthode instancie un nouvel objet `PreauthorizeRequest` chaque fois qu’il est appelé.
* Cette méthode utilise les valeurs définies à l’avance dans le contexte de l’instance d’objet `PreauthorizeRequestBuilder` actuelle, qui est le récepteur de cet appel de méthode.
* Gardez à l&#39;esprit que cette méthode ne produit aucun effet secondaire,
* par conséquent, il ne modifie pas l’état du SDK ni l’état de l’instance d’objet `PreauthorizeRequestBuilder`, qui est le récepteur de cet appel de méthode.
* Cela signifie que les appels successifs de cette méthode pour le même récepteur créeront de nouvelles instances d’objet `PreauthorizeRequest` différentes, mais avec les mêmes informations, au cas où les valeurs définies sur le `PreauthorizeRequestBuilder` ne seraient pas modifiées entre les appels.
* Si vous n’avez pas besoin de mettre à jour les informations fournies (ressources et mise en cache), vous pouvez réutiliser l’instance PreallowRequest pour plusieurs utilisations de l’API de préautorisation.
* `@returns {PreauthorizeRequest}`

### interface AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse(result : T); {#on-response-result}

* Rappel de réponse appelé par le SDK lorsque la requête API de préautorisation a été satisfaite.
* Le résultat obtenu est un résultat réussi ou une erreur contenant un état.
* `@param {T} result`

#### onFailure(result: T); {#on-failure-result}

* Rappel d’échec appelé par le SDK lorsque la demande d’API de préautorisation n’a pas pu être traitée.
* Le résultat est un résultat d’échec contenant un état.
* `@param {T} result`

### class PreauthorizedResponse {#preauth-response-class}

#### statut public : statut ; {#public-status}

* Renvoie : informations d’état supplémentaires en cas d’échec.
* Peut contenir une valeur `null`.

#### décisions publiques : Décision[] ; {#public-decisions}

* Renvoie : liste des décisions de préautorisation. Une décision pour chaque ressource.
* La liste peut être vide en cas d’échec.

### class Status {#class-status}

#### statut public : nombre ; {#public-status-numbr}

* Le code d’état de réponse HTTP, comme indiqué dans la norme RFC 7231.
* Peut être égal à 0 si `Status` provient du SDK au lieu des services d’authentification Adobe Pass.

#### code public : nombre; {#public-code-numbr}

* Code d’erreur des services d’authentification Adobe Pass standard.
* Peut contenir une chaîne vide ou une valeur `null`.

#### message public : string; {#public-msg-string}

* Message détaillé qui dans certains cas est fourni par les points d’entrée d’autorisation MVPD ou par les règles de dégradation des programmeurs.
* Peut contenir une chaîne vide ou une valeur `null`.

#### public details : string; {#public-details-strng}

* Contient un message détaillé qui dans certains cas est fourni par les points d’entrée d’autorisation MVPD ou par les règles de dégradation des programmeurs.
* Peut contenir une chaîne vide ou une valeur `null`.


#### public helpUrl: string; {#public-help-url-string}

* URL qui renvoie à des informations supplémentaires sur la raison de cet état/erreur et les solutions possibles.
* Peut contenir une chaîne vide ou une valeur `null`.

#### trace publique : string; {#public-trace-string}

* L’identifiant unique de cette réponse, qui peut être utilisé pour contacter l’assistance afin d’identifier des problèmes spécifiques dans des scénarios plus complexes.
* Peut contenir une chaîne vide ou une valeur `null`.

#### action publique : string; {#public-action-string}

* L’action recommandée pour remédier à la situation.
   * **none** : il n’existe malheureusement pas d’action prédéfinie pour résoudre ce problème. Cela peut indiquer un appel incorrect de l’API publique
   * **configuration** : un changement de configuration est nécessaire par le biais du tableau de bord TVE ou en contactant l’assistance.
   * **application-registration** : l’application doit s’enregistrer à nouveau.
   * **authentication** : l’utilisateur doit s’authentifier ou se reconnecter.
   * **authorization** : l’utilisateur doit obtenir l’autorisation de la ressource spécifique.
   * **dégradation** : une forme de dégradation doit être appliquée.
   * **retry** : une nouvelle tentative de requête peut résoudre le problème.
   * **retry-after** : une nouvelle tentative de requête après la période indiquée peut résoudre le problème.
* Peut contenir une chaîne vide ou une valeur `null`.

### Classe Decision {#class-decision}

#### public id: string; {#public-id-string}

* ID de ressource pour lequel la décision a été obtenue.

#### public autorisé : booléen ; {#public-auth-boolean}

* La valeur de l’indicateur indiquant si la décision a réussi ou non.

#### erreur publique : Status; {#public-error-status}

* Informations supplémentaires sur l’état en cas d’erreur. Peut contenir une valeur `null`.

## Exemple de mise en oeuvre client {#client-imp-example}

```JavaScript
let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## Exemples de scénarios {#scenario-examples}

### Scénario 1 : toutes les ressources demandées ont été autorisées {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Désactivé</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### Scénario 2 : certaines ressources demandées ont été autorisées. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Désactivé</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;Decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    ,
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: 03&quot;,
    &quot;authorized&quot;: true
    }
    ]
    
    
    &quot;

</td>
  </tr>

<tr>
    <td>Activé</td>
    <td>

    &quot;JavaScript
    {
    &quot;Decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    ,
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 3,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;Le MVPD a renvoyé une décision \&quot;Refuser\&quot; lors de la demande de préautorisation pour la ressource spécifiée.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }}
    }},
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    ,
    ]
    }
    
    &quot;

</td>
  </tr>
</tbody>


### Scénario 3 : aucune des ressources demandées n’a été autorisée. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Désactivé</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;Decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false
    ,
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: 03&quot;,
    &quot;authorized&quot;: false
    }
    ]
    
    
    &quot;

</td>
  </tr>

<tr>
    <td>Activé</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;Decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;The MVPD return une décision \&quot;Refuser\&quot; lors de la demande de préautorisation pour la ressource spécifiée.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    ,
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
     &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;Le MVPD a renvoyé une décision \&quot;Refuser\&quot; lors de la demande de pré-autorisation pour la ressource spécifiée.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot; action&quot;: &quot;none&quot;
    
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;maximum_execution_time_exceeded&quot;, 1}&quot;message&quot; : &quot;la demande n’a pas été effectuée dans le délai maximal autorisé. 
     Une nouvelle tentative de la requête peut résoudre le problème.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    
    
    ]
    
    
    &quot;

</td>
  </tr>
</tbody>


### Scénario 4 : demande client incorrecte - aucune ressource spécifiée. {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Désactivé/activé</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 400,
    &quot;code&quot;: &quot;internal_error&quot;,
    &quot;message&quot;: &quot;La requête a échoué en raison d’une erreur interne.&quot;,
    &quot;details&quot;: &quot;Required String[] parameter ’resource’ is not present&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;, 8} &quot;action&quot;: &quot;none&quot;
    ,
    &quot;requests&quot;: []
    
    &quot;

    
</td>
  </tr>
</tbody>
</table>

### Scénario 5 : demande client incorrecte - ressources vides spécifiées. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Désactivé/activé</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 412,
    &quot;code&quot;: &quot;missing_resource&quot;,
    &quot;message&quot;: &quot;The resource parameter is missing&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;Decisions&quot;: []
    }}
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scénario 6 : erreur réseau. {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Activé</td>
    <td>

    &quot;JavaScript
    {
    &quot;Decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;Une erreur de lecture s’est produite lors de la récupération de la réponse le service partenaire associé. Une nouvelle tentative de la requête peut résoudre le problème.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    
    ,
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot; : &quot;network_received_error&quot;,
    &quot;message&quot; : &quot;Une erreur de lecture s’est produite lors de la récupération de la réponse du service partenaire associé. Une nouvelle tentative de la requête peut résoudre le problème.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    
    
    ]
    
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scénario 7 : le flux de préautorisation a été appelé sans session AuthN valide.

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Désactivé/activé</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;authentication_session_missing&quot;,
    &quot;message&quot;: &quot;La session d’authentification associée à cette requête n’a pas pu être récupérée. L’utilisateur doit se réauthentifier avec un MVPD pris en charge pour continuer.&quot;,
    &quot;action&quot;: &quot;authentication&quot;
    ,
    &quot;décisions&quot;: []
    }
    
    &quot;

</td>
  </tr>
</tbody>
</table>



### Scénario 8 : le flux de préautorisation a été appelé avant que l’appel setRequestor ne soit terminé

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Désactivé/activé</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;requestor_not_configured&quot;,
    &quot;message&quot;: &quot;Le demandeur n’est pas encore configuré, ce qui est une condition préalable à l’utilisation d’une API autre que l’API setRequestor.&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }}},
    &quot;Decisions&quot;:]
    
    &quot;

</td>
  </tr>
</tbody>
</table>
