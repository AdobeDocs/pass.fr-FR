---
title: Préautoriser
description: Préautorisation JavaScript
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1466'
ht-degree: 0%

---

# (Hérité) Autoriser à l’avance {#js-preauthorize}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#preauth-overview}

La méthode API de préautorisation doit être utilisée par les applications pour obtenir des décisions de préautorisation pour une ou plusieurs ressources. La requête d’API de préautorisation doit être utilisée pour les indications de l’interface utilisateur et/ou le filtrage de contenu. Une requête d’API d’autorisation réelle doit être effectuée avant d’autoriser l’accès des utilisateurs et utilisatrices aux ressources spécifiées.

Si une erreur inattendue (par exemple, un problème réseau et un point d’entrée d’autorisation MVPD indisponible) se produit lorsqu’une requête d’API de préautorisation est traitée par les services d’authentification Adobe Pass, une ou plusieurs informations d’erreur distinctes sont incluses pour les ressources affectées dans le résultat de la réponse d’API de préautorisation.

### public preauthorize(request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any>) : void {#preauth-method}

**Description :** cette méthode doit être utilisée par les applications pour obtenir des décisions de préautorisation (informatives) de l’utilisateur authentifié auprès du service d’authentification Adobe Pass afin d’afficher des ressources protégées spécifiques, dans le but principal de décorer l’interface utilisateur de l’application (par exemple, en indiquant le statut d’accès avec des icônes de verrouillage et de déverrouillage).

**Disponibilité :** v4.4.0+

**Paramètres:**

* `PreauthorizeRequest` : objet Builder utilisé pour définir la requête
* `AccessEnablerCallback` : rappel utilisé pour renvoyer la réponse de l’API
* `PreauthorizeResponse` : objet utilisé pour renvoyer le contenu de la réponse API

### classe PreauthorizeRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string[]) : PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* Définit la liste des ressources pour lesquelles vous souhaitez obtenir des décisions de préautorisation.
* Il est obligatoire de le définir pour l’utilisation de l’API de préautorisation.
* Chaque élément de la liste doit être une chaîne représentant la valeur de l’ID de ressource ou le fragment RSS du média qui doit être défini en accord avec le MVPD.
* Cette méthode définit les informations uniquement dans le contexte de l’instance d’objet `PreauthorizeRequestBuilder` actuelle, qui est le destinataire de cet appel de méthode.

* Pour créer un `PreauthorizeRequest` réel, vous pouvez consulter la méthode de `PreauthorizeRequestBuilder` :

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` des ressources. La liste des ressources pour lesquelles vous souhaitez obtenir des décisions de préautorisation.
* `@returns {PreauthorizeRequestBuilder}` Référence à la même instance d’objet `PreauthorizeRequestBuilder`, qui est le destinataire de l’appel de méthode.
* Elle le fait pour permettre la création de chaînes de méthodes.

#### disableFeatures(...features: string[]) : PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* Définit les fonctionnalités qui doivent être désactivées lors de l’obtention de décisions de préautorisation.
* Cette fonction définit les informations uniquement dans le contexte de l’instance d’objet `PreauthorizeRequestBuilder` actuelle, qui est le destinataire de cet appel de fonction.
* Pour créer un `PreauthorizeRequest` réel, vous pouvez jeter un coup d’œil à la fonction de `PreauthorizeRequestBuilder` :

```JavaScript
public func build() -> PreauthorizeRequest
```

* Fonctionnalités `@param {string[]}`. Ensemble de fonctionnalités que vous souhaitez désactiver.
* `@returns` La référence à la même instance d’objet de `PreauthorizeRequestBuilder`, qui est le récepteur de l’appel de fonction.
* Elle le fait pour permettre la création de chaînage de fonctions.

#### build() : PreauthorizeRequest {#preauth-req}

* Crée et récupère la référence d&#39;une nouvelle instance d&#39;objet `PreauthorizeRequest`.
* Cette méthode instancie un nouvel objet `PreauthorizeRequest` chaque fois qu’elle est appelée.
* Cette méthode utilise les valeurs définies à l’avance dans le contexte de l’instance d’objet `PreauthorizeRequestBuilder` actuelle, qui est le destinataire de cet appel de méthode.
* Gardez à l&#39;esprit que cette méthode ne produit pas d&#39;effets secondaires,
* par conséquent, il ne modifie pas l’état du SDK ni l’état de l’instance de l’objet `PreauthorizeRequestBuilder`, qui est le destinataire de cet appel de méthode.
* Cela signifie que les appels successifs de cette méthode pour le même récepteur créeront différentes nouvelles instances d’objet de `PreauthorizeRequest`, mais ayant les mêmes informations, au cas où les valeurs définies sur la `PreauthorizeRequestBuilder` ne seraient pas modifiées entre les appels.
* Si vous n’avez pas besoin de mettre à jour les informations fournies (ressources et mise en cache), vous pouvez réutiliser l’instance PreauthorizeRequest pour plusieurs utilisations de l’API preauthorize.
* `@returns {PreauthorizeRequest}`

### interface AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse(résultat : T); {#on-response-result}

* Rappel de réponse appelé par le SDK lorsque la demande d’API de préautorisation a été remplie.
* Le résultat est un résultat réussi ou un résultat d’erreur contenant un statut.
* `@param {T} result`

#### onFailure(résultat : T); {#on-failure-result}

* Rappel d’échec appelé par le SDK lorsque la requête API de préautorisation n’a pas pu être traitée.
* Le résultat est un résultat d’échec contenant un statut .
* `@param {T} result`

### classe PreauthorizeResponse {#preauth-response-class}

#### statut public : statut ; {#public-status}

* Renvoie : informations supplémentaires sur le statut (état) en cas d’échec.
* Peut contenir une valeur `null`.

#### décisions publiques : décision[]; {#public-decisions}

* Renvoie : liste des décisions de préautorisation. Une décision pour chaque ressource.
* La liste peut être vide en cas d’échec.

### class Status {#class-status}

#### statut public : nombre ; {#public-status-numbr}

* Le code de statut de la réponse HTTP comme documenté dans la RFC 7231.
* Peut être 0 au cas où le `Status` provient du SDK au lieu des services d’authentification Adobe Pass.

#### code public : numéro ; {#public-code-numbr}

* Code d’erreur standard des services d’authentification Adobe Pass.
* Peut contenir une chaîne vide ou une valeur `null`.

#### message public : chaîne ; {#public-msg-string}

* Le message détaillé qui est dans certains cas fourni par les points d’entrée d’autorisation MVPD ou par les règles de dégradation du programmeur.
* Peut contenir une chaîne vide ou une valeur `null`.

#### détails publics : chaîne ; {#public-details-strng}

* Contient un message détaillé qui, dans certains cas, est fourni par les points d’entrée d’autorisation MVPD ou par les règles de dégradation du programmeur.
* Peut contenir une chaîne vide ou une valeur `null`.


#### public helpUrl : chaîne ; {#public-help-url-string}

* URL renvoyant vers plus d’informations sur la raison pour laquelle cet état/cette erreur s’est produit et les solutions possibles.
* Peut contenir une chaîne vide ou une valeur `null`.

#### public trace : chaîne ; {#public-trace-string}

* Identifiant unique de cette réponse. Il peut être utilisé lors du contact avec l’assistance pour identifier des problèmes spécifiques dans des scénarios plus complexes.
* Peut contenir une chaîne vide ou une valeur `null`.

#### action publique : chaîne ; {#public-action-string}

* Action recommandée pour remédier à la situation.
   * **none** : il n’existe malheureusement aucune action prédéfinie pour résoudre ce problème. Cela peut indiquer un appel incorrect de l’API publique
   * **configuration** : une modification de configuration est nécessaire via le tableau de bord TVE ou en contactant l’assistance.
   * **application-registration** : l’application doit s’enregistrer à nouveau.
   * **authentification** : l’utilisateur doit s’authentifier ou s’authentifier à nouveau.
   * **autorisation** : l’utilisateur ou l’utilisatrice doit obtenir une autorisation pour la ressource spécifique.
   * **dégradation** : une forme de dégradation doit être appliquée.
   * **réessayer** : réessayer d’exécuter la requête peut résoudre le problème
   * **retry-after** : réessayer d’exécuter la requête après la période indiquée peut résoudre le problème.
* Peut contenir une chaîne vide ou une valeur `null`.

### décision de classe {#class-decision}

#### id public : chaîne ; {#public-id-string}

* Identifiant de ressource pour lequel la décision a été obtenue.

#### public autorisé : booléen ; {#public-auth-boolean}

* Valeur de l’indicateur indiquant si la décision est réussie ou non.

#### erreur publique : statut ; {#public-error-status}

* Informations de statut (d’état) supplémentaires au cas où une erreur se serait produite. Peut contenir une valeur `null`.

## Exemple d’implémentation client {#client-imp-example}

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

### Scénario 1 : Toutes les ressources demandées ont été autorisées {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Handicapé</td>
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


### Scénario 2 : Certaines ressources demandées ont été autorisées. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Handicapé</td>
    <td>

     »JavaScript
    
    {
    « decisions »: [
    {
    « id »: « RES01 »,
    « authorized »: true
    },
    {
    « id »: « RES02 »,
    « authorized »: false
    },
    {
    « id »: « RES03 »,
    « authorized »: true
    }
    ]
    }
    
     »

</td>
  </tr>

<tr>
    <td>Activé</td>
    <td>

     »JavaScript
    {
    « decisions »: [
    {
    « id »: « RES01 »,
    « authorized »: true
    },
    {
    « id »: « RES02 »,
    « authorized »: false,
    « error »: {
    « status »: 403,
    « code »: « preauthorization_deny_by_mvpd »,
    « message »: « Le MVPD a renvoyé une décision \« Deny\ » lors de la demande de préautorisation pour la ressource spécifiée. »,
    « helpUrl »: « https://experienceleague.adobe.com/docs/primetime/authentication/home.html »,
    « action »: « none »
    }
    },
    {id »: « 
     »,RES03« authorized »: true
    },
    ]
    }
     »
    
     

</td>
  </tr>
</tbody>


### Scénario 3 : Aucune des ressources demandées n’a été autorisée. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Handicapé</td>
    <td>

     »JavaScript
    
    {
    « décisions »: [
    {
    « id »: « RES01 »,
    « authorized »: false
    },
    {
    « id »: « RES02 »,
    « authorized »: false
    },
    {
    « id »: « RES03 »,
    « authorized »: false
    }
    ]
    }
    
     »

</td>
  </tr>

<tr>
    <td>Activé</td>
    <td>

     »JavaScript
    
    {
    « decisions »: [
    {
    « id »: « RES01 »,
    « authorized »: false,
    « error »: {
    « status »: 403,
    « code »: « preauthorization_deny_by_mvpd »,
    « message »: « Le MVPD a renvoyé une décision \« Deny\ » lors de la demande de préautorisation pour la ressource spécifiée. »,
    « Url »: « https://experienceleague.adobe.com/docs/primetime/authentication/home.html »,
    « action »: « none »
    }
    },
    {id »: « 
     »,RES02« authorized »: false,
    « error »: {
    « status »: 403,
    « code »: « preauthorization_deny_by_mvpd »,
    « message »: « Le MVPD a renvoyé une décision \« Deny\ » lors de la demande d&#39;autorisation préalable pour la ressource spécifiée. »,
    « Url »: « https://experienceleague.adobe.com/docs/primetime/authentication/home.html »,
    « action »: « none »
    }
    },
    « id »: « 
     »,
    « authorized »: false,RES03« error »: {
    « status »: 403,
    « code »: « maximum_execution_time_exceeded »,
     
     
    « message »: « La demande n&#39;a pas été effectuée dans le délai maximum autorisé. Réessayer la requête peut résoudre le problème. »,
    « helpUrl »: « https://experienceleague.adobe.com/docs/primetime/authentication/home.html »,
    « action »: « retry »
    }
    }
    ]
    }
    
     »

</td>
  </tr>
</tbody>


### Scénario 4 : requête client incorrecte - aucune ressource spécifiée. {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Désactivé/Activé</td>
    <td>

     »JavaScript
    {
    « status »: {
    « status »: 400,
    « code »: « internal_error »,
    « message »: « La demande a échoué en raison d’une erreur interne. »,
    « details »: « Le paramètre de chaîne obligatoire[] &#39;resource&#39; n’est pas présent »,
    « helpUrl »: « https://experienceleague.adobe.com/docs/primetime/authentication/home.html »,
    « action »: « none »
    },
    « decisions »: []
    }
     »

</td>
  </tr>
</tbody>
</table>

### Scénario 5 : requête client incorrecte - ressources vides spécifiées. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Désactivé/Activé</td>
    <td>

     »JavaScript
    {
    « status »: {
    « status »: 412,
    « code »: « missing_resource »,
    « message »: « The resource parameter is missing »,
    « helpUrl »: « https://experienceleague.adobe.com/docs/primetime/authentication/home.html »,
    « action »: « none »
    },
    « decisions »: []
    }
     »

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

     »JavaScript
    {
    « decisions »: [
    {
    « id »: « RES01 »,
    « authorized »: false,
    « error »: {
    « status »: 403,
    « code »: « network_received_error »,
    « message »: « Une erreur de lecture s’est produite lors de la récupération de la réponse du service partenaire associé. Réessayer la requête peut résoudre le problème. »,
    « helpUrl »: « https://experienceleague.adobe.com/docs/primetime/authentication/home.html »,
    « action »: « retry »
    }
    },
    {
    « id »: « RES02 »,
    « authorized »: false,
    « error »: {
    « status »: 403,
    « code »: « network_received_error »,
    « message »: « Une erreur de lecture s&#39;est produite lors de la récupération de la réponse du service partenaire associé. Réessayer la requête peut résoudre le problème. »,
    « helpUrl »: « https://experienceleague.adobe.com/docs/primetime/authentication/home.html »,
    « action »: « retry »
    }
    }
    ]
    }
     »

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
    <td>Désactivé/Activé</td>
    <td>

     »JavaScript
    {
    « status »: {
    « status »: 0,
    « code »: « authentication_session_missing »,
    « message »: « La session d&#39;authentification associée à cette requête n&#39;a pas pu être récupérée. L’utilisateur doit s’authentifier à nouveau avec un MVPD pris en charge pour continuer. »,
    « action »: « authentication »
    },
    « decisions »: []
    }
    
     »

</td>
  </tr>
</tbody>
</table>



### Scénario 8 : le flux de préautorisation a été appelé avant la fin de l’appel setRequestor

<table>
<thead>
  <tr>
    <th>Indicateur de code d’erreur amélioré</th>
    <th>Réponse</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Désactivé/Activé</td>
    <td>

     »JavaScript
    {
    « status »: {
    « status »: 0,
    « code »: « requestor_not_configured »,
    « message »: « Le demandeur n’est pas encore configuré, ce qui est un prérequis pour l’utilisation d’une API autre que l’API setRequestor. »,
    « action »: « retry »
    },
    « decisions »: []
    }
     »

</td>
  </tr>
</tbody>
</table>
