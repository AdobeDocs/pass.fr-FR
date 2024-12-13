---
title: Préautorisation de l’API iOS/tvOS
description: Préautorisation de l’API iOS/tvOS
exl-id: 79c596a4-0e38-4b6c-bb85-f97c6af45ed8
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 0%

---

# (Hérité) Autoriser à l’avance {#preauthorize}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

L’API de préautorisation peut être utilisée pour obtenir une décision de préautorisation pour une ou plusieurs ressources, de cette manière l’application peut implémenter des indications de l’interface utilisateur et/ou le filtrage de contenu.

>[!IMPORTANT]
>
>L’API d’autorisation **doit** doit être utilisée avant d’accorder à l’utilisateur l’accès aux ressources spécifiées.

Si le résultat de la réponse de l’API de préautorisation contient une ou plusieurs ressources avec une décision de préautorisation refusée, des informations d’erreur supplémentaires peuvent être incluses **(voir la note ci-dessous)** pour chaque ressource affectée.

>[!IMPORTANT]
>
>La fonctionnalité de rapport d’erreur améliorée qui ajoute des informations d’erreur supplémentaires pour les décisions de préautorisation refusées est disponible sur demande, car elle doit être activée du côté de la configuration de l’authentification Adobe Pass.

Au cas où la demande de préautorisation de l’API n’a pas pu être traitée en raison d’une erreur de SDK d’authentification Adobe Pass ou au cas où une erreur de services d’authentification Adobe Pass se produit, une information d’erreur supplémentaire (indépendamment de la configuration ci-dessus) et aucune ressource ne seront incluses dans le résultat de la réponse de préautorisation de l’API.

</br>

## `- (void) preauthorize:(nonnull PreauthorizeRequest *)request didCompleteWith:(nonnull AccessEnablerCallback<PreauthorizeResponse *> *)callback;`


**Disponibilité :** v3.6.0+

**Paramètres:**

- PreauthorizeRequest : objet de la requête utilisé pour transmettre le contenu de la requête API.
- AccessEnablerCallback : objet de rappel utilisé pour renvoyer la réponse de l’API ;
- PreauthorizeResponse : objet de réponse utilisé pour renvoyer le contenu de la réponse de l’API


</br>

## `class PreauthorizeRequest`{#androidpreauthorizerequest}

### **class PreauthorizeRequest.Builder**

```
    ///
    /// Sets the `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// Each element in the list must be a `String` representing either the resource ID value or the media RSS fragment which must be agreed with the MVPD.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    ///
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter resources: The `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
    public func setResources(resources: [String]) -> PreauthorizeRequest.Builder

 

    ///
    /// Sets the features which you want to have them disabled when obtaining preauthorization decisions.
    ///
    /// The list of available features are provided by `PreauthorizeRequest.Feature` enumeration. All features are enabled by default.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    ///
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter features: The set of features which you want to have them disabled.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
    public func disableFeatures(features: Set<PreauthorizeRequest.Feature>) -> PreauthorizeRequest.Builder

 

    ///
    /// Creates and retrieves the reference of a new `PreauthorizeRequest` object instance.
    ///
    /// This function instantiates a new `PreauthorizeRequest` object every time it is called.
    ///
    /// This function uses the values set in advance in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// Bear in mind that this function does not produce any side effects, therefore it does not alter the state of the SDK or the state of the `Builder` object instance which is the receiver of this function call.
    ///
    /// It means that successive calls of this function for the same receiver will create different new `PreauthorizeRequest` object instances, but having the same information, in case the values set to the `Builder` where not modified between the calls.
    ///
    /// In case you do not need to update any of the provided information (resources, features, etc.) you may reuse the `PreauthorizeRequest` instance for multiple uses of the `preauthorize` API.
    ///
    /// - Returns: The reference to a new `PreauthorizeRequest` object instance.
    ///
    public func build() -> PreauthorizeRequest
```


## **enum PreauthorizeRequest.Feature**

```
    ///
    /// This feature controls whether to use the information from the AccessEnabler SDK cache or to bypass it and
    /// rely on Adobe Pass server information via a network call.
    ///
    LOCAL_CACHE

    ///
    /// This feature controls whether to use the information from the Adobe Pass server cache or to bypass it and
    /// rely on MVPD server information via a network call.
    ///
    REMOTE_CACHE
```

</br>

## `interface AccessEnablerCallback<PreauthorizeResponse>` {#accessenablercallback}

```
    /// Response callback called by the SDK when the preauthorize API request was fulfilled. The result is either a successful or an error result containing a status.
    public func onResponse(result: PreauthorizeResponse)


    /// Failure callback called by the SDK when the preauthorize API request could not be serviced. The result is a failure result containing a status. 
    public func onFailure(result: PreauthorizeResponse)
```

</br>


## `class PreauthorizeResponse` {#preauthorizeresponse}

```
    ///
    /// - Returns: Additional status (state) information in case of error or failure.
    ///   Might hold a `nil` value.
    ///
    public Status getStatus()

    ///
    /// - Returns: The list of preauthorization decisions. One decision for each resource.
    ///            The list might be empty in case of error or failure.
    ///
    public List<Decision> getDecisions()
```

### Exemples :

Cette section met en évidence la structure JSON de certains objets PreauthorizeResponse possibles.

>[!IMPORTANT]
>
>Les fichiers JSON présentés dans les exemples suivants sont accessibles uniquement par le biais des classes de modèles présentées dans ce document. Vous ne pourrez pas accéder aux propriétés de ces fichiers JSON autrement que par le biais des méthodes publiques.

>[!IMPORTANT]
>
>La liste des erreurs supplémentaires possibles récupérées par le biais de la fonctionnalité de rapport d’erreur améliorée est documentée dans [Rapport d’erreur avancé](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

#### Succès

Toutes les ressources demandées font l&#39;objet d&#39;une décision positive de préautorisation

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": true 
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


Une ou plusieurs ressources ont une décision de préautorisation refusée et la fonctionnalité de rapport d’erreur améliorée n’est pas activée dans la configuration de l’authentification Adobe Pass

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": false,
                 
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


Une ou plusieurs ressources se voient refuser la décision de préautorisation et la fonctionnalité de rapport d’erreur améliorée est activée dans la configuration de l’authentification Adobe Pass

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": false,
                "error" : {
                   "status" : 403,
                   "code" : "authorization_denied_by_mvpd",
                   "message" : "User not authorized",
                   "details" : "Your subscription package does not include the "TestStream3" channel.",
                   "helpUrl" : "https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                   "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
                   "action" : "none"
                }
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


#### Erreur



Les services d’authentification Adobe Pass rencontrent une erreur lors de la maintenance de la requête API de préautorisation

```JSON
    {
        "resources": [],
        "status": {
            "status": 400,
            "code" : "bad_request",
            "message": "Missing required parameter : deviceId",
            "details": "",
            "helpUrl" : "https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "trace" : "9f115e1c-0158-4a41-8805-9f68923f3646",
            "action" : "none"
        }
    }
```


#### Échec

Le SDK d’authentification Adobe Pass rencontre une erreur lors du traitement de la requête d’API de préautorisation

```JSON
    {
        "status": 0,
        "code": "requestor_not_configured",
        "message": "The requestor is not yet configured which is a prerequisite for using any API apart from the setRequestor API.",
        "action": "retry"
    }
    
    {
        "status": 0,
        "code": "authentication_session_expired",
        "message": "The current authentication session has expired. The user must re-authenticate with a supported MVPD in order to continue.",
        "action": "authentication"
    }
    
    {
        "status": 0,
        "code": "authentication_session_missing",
        "message": "The authentication session associated with this request could not be retrieved. The user must re-authenticate with a supported MVPD in order to continue.",
        "action": "authentication"
    }
    
    {
        "status": 0,
        "code": "access_token_unavailable",
        "message": "The request failed due to an unexpected error while retrieving the access token. Please check the validity of your software statement and custom scheme.",
        "action": "none"
    }
    
    {
        "status": 0,
        "code": "server_response_format_unknown",
        "message": "The request failed due to an unknown server response format. Please capture the device console logs and contact support.",
        "action": "none"
    }
    
    {
        "status": 0,
        "code": "network_error",
        "message": "The request failed due to a network error. If the issue persists over several retries, please capture the device console logs and contact support.",
        "action": "none"
    }
```

</br>

## **statut de la classe** {#status}

```
    ///
    /// - Returns: The HTTP response status code as documented in RFC 7231.
    ///            Might be 0 in case the `Status` comes from the SDK instead of Adobe Pass Authentication services.
    ///
    public int getStatus()

    ///
    /// - Returns: The standard Adobe Pass Authentication services error code.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getCode()

    ///
    /// - Returns: The human readable message which can be displayed to the end user.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getMessage()

    ///
    /// - Returns: The detailed message which in some cases is provided by the MVPD authorization endpoints or by Programmer degradation rules.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getDetails()

    ///
    /// - Returns: The URL that links to more information about why this state/error occurred and possible solutions.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getHelpUrl()

    ///
    /// - Returns: The unique identifier for this response, which can be used when contacting support to identify specific issues in more complex scenarios.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getTrace()

    ///
    /// - Returns: The recommended action to remediate the situation.
    ///             - none: Unfortunately there is no predefined action to remediate this issue. This might indicate an improper invocation of the public API
    ///             - configuration: A configuration change is needed through TVE dashboard or by contacting support.
    ///             - application-registration: The application must register itself again.
    ///             - authentication: The user must authenticate or re-authenticate.
    ///             - authorization: The user must obtain authorization for the specific resource.
    ///             - degradation: Some form of degradation should be applied.
    ///             - retry: Retrying the request might solve the issue
    ///             - retry-after: Retrying the request after the indicated period of time might solve the issue.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getAction()
```

<br>

## **décision de classe** {#decision}

```
    ///
    /// This is a getter function.
    ///
    /// - Returns: The resource id for which the decision was obtained.
    ///
    public Status getId()

    ///
    /// This is a getter function.
    ///
    /// - Returns: The value of the flag indicating if the decision is successful or not.
    ///
    public boolean isAuthorized()

    ///
    /// This is a getter function.
    ///
    /// - Returns: Additional status (state) information in case some error has occurred.
    ///            Might hold a `nil` value.
    ///
    public Status getError()
```

</br>


## **Exemple de code** {#sample}

```
let resources: [String] = ["resource_1", "resource_2", "resource_3"];

let disabledFeatures: Set<PreauthorizationRequest.Feature> = [PreauthorizationRequest.Feature.LOCAL_CACHE];

// Build the Preauthorization API request using the builder from PreauthorizationRequest class`

let request: PreauthorizationRequest = PreauthorizationRequest.Builder()

                  .setResources(resources: resources)


                  .disableFeatures(features: disabledFeatures)  // It is **optional** to disable features. If not used all features are enabled by default.

                  .build();

// Build the AccessEnablerCallback by providing the constructor two callbacks for onResponse and onFailure handling  
func onResponseCallback(result: PreauthorizeResponse) -> Void {  //
TODO };

func onFailureCallback(result: PreauthorizeResponse) -> Void {

// TODO

};

let callback: AccessEnablerCallback<PreauthorizeResponse> = AccessEnablerCallback<PreauthorizeResponse>(onResponse: onResponseCallback, onFailure: onFailureCallback);

// Use the preauthorize API
accessEnabler.preauthorize(request, callback);
```
