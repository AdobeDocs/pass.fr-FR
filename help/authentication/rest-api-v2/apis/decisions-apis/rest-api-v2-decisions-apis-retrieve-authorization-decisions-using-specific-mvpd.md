---
title: Récupération des décisions d’autorisation à l’aide de mvpd spécifique
description: API REST V2 - Récupération des décisions d’autorisation à l’aide de mvpd spécifique
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '935'
ht-degree: 1%

---


# Récupération des décisions d’autorisation à l’aide de mvpd spécifique {#retrieve-authorization-decisions-using-specific-mvpd}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

## Requête {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">path</td>
      <td>/api/v2/{serviceProvider}/requests/authorized/{mvpd}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Paramètres de chemin</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identifiant unique interne associé au fournisseur de services lors du processus d’intégration.</td>
      <td><i>required</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>Identifiant unique interne associé au fournisseur d’identité lors du processus d’intégration.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Paramètres du corps</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ressources</td>
      <td>Liste des ressources qui nécessitent une décision MVPD avant de pouvoir être lues.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisation</td>
      <td>La génération du payload du jeton porteur est décrite dans la documentation de l’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a>.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Type de média accepté pour les ressources envoyées.
         <br/><br/>
         Il doit s’agir de application/json.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>La génération de la payload de l’identifiant de l’appareil est décrite dans la documentation de l’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> .</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La génération de la payload d’informations sur l’appareil est décrite dans la documentation de l’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> .
         <br/><br/>
         Il est vivement recommandé de toujours l’utiliser lorsque la plate-forme d’appareil de l’application autorise la spécification explicite de valeurs valides.
         <br/><br/>
         Lorsqu’il est fourni, le serveur principal d’authentification Adobe Pass fusionne implicitement les valeurs définies explicitement avec les valeurs extraites (par défaut).
         <br/><br/>
         Lorsqu’il n’est pas fourni, le serveur principal d’authentification Adobe Pass utilise implicitement les valeurs extraites (par défaut).
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         Adresse IP de l’appareil de diffusion en continu.
         <br/><br/>
         Il est vivement recommandé de toujours l’utiliser pour les implémentations serveur à serveur, en particulier lorsque l’appel est effectué par le service de programmation plutôt que par l’appareil de diffusion en continu.
         <br/><br/>
         Pour les implémentations client/serveur, l’adresse IP du périphérique de diffusion en continu est envoyée implicitement.
      </td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Adobe-Objet-Jeton</td>
      <td>
        La génération de la payload d’authentification unique pour la méthode d’identification de plateforme est décrite dans la documentation de l’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a>.
        <br/><br/>
        Pour plus d’informations sur les flux activés pour l’authentification unique à l’aide d’une identité de plateforme, reportez-vous à la documentation <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">Connexion unique à l’aide des flux d’identité de plateforme</a> .
      </td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        La génération de la payload de connexion unique pour la méthode Service Token est décrite dans la documentation de l’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>.
        <br/><br/>
        Pour plus d’informations sur les flux activés pour l’authentification unique à l’aide d’un jeton de service, reportez-vous à la documentation <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md">Authentification unique à l’aide des flux de jeton de service</a> .
      </td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        La génération de la payload de connexion unique pour la méthode Partner est décrite dans la documentation de l’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a>.
        <br/><br/>
        Pour plus d’informations sur les flux activés pour l’authentification unique à l’aide d’un partenaire, reportez-vous à la documentation <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md">Connexion unique à l’aide des flux de partenaire</a> .</td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td>La génération de la payload de l’identifiant unique de l’utilisateur est décrite dans la documentation de l’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md">AP-TempPass-Identity</a> .</td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accepter</td>
      <td>
         Type de média accepté par l’application cliente.
         <br/><br/>
         S’il est spécifié, il doit s’agir de application/json.
      </td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>Agent utilisateur de l’application cliente.</td>
      <td>facultatif</td>
   </tr>
</table>

## Réponse {#response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Texte</th>
      <th style="background-color: #EFF2F7;">Description</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Le corps de la réponse contient une liste de décisions avec des informations supplémentaires.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Requête incorrecte</td>
      <td>
        La requête n’est pas valide. Le client doit corriger la requête et réessayer. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="../../../enhanced-error-codes.md">Codes d’erreur améliorés</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non autorisé</td>
      <td>
        Le jeton d’accès n’est pas valide, le client doit obtenir un nouveau jeton d’accès et réessayer. Pour plus d’informations, reportez-vous à la documentation <a href="../../../dcr-api/dynamic-client-registration-overview.md">Présentation de l’enregistrement du client dynamique</a> .
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Méthode non autorisée</td>
      <td>
        La méthode HTTP n’est pas valide, le client doit utiliser une méthode HTTP autorisée pour la ressource demandée et réessayer. Pour plus d’informations, reportez-vous à la section <a href="#request">Requête</a> .
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erreur interne du serveur</td>
      <td>
        Le côté serveur a rencontré un problème. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="../../../enhanced-error-codes.md">Codes d’erreur améliorés</a>.
      </td>
   </tr>
</table>

### Succès {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Etat</td>
      <td>200</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corps</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">décisions</td>
      <td>
         JSON contenant une liste d’éléments, chaque élément ayant les attributs suivants :
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">resource</td>
                <td>Identifiant de ressource pour lequel la décision d’autorisation est renvoyée.</td>
                <td><i>required</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">serviceProvider</td>
                <td>Identifiant unique interne associé au fournisseur de services lors du processus d’intégration.</td>
                <td><i>required</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">mvpd</td>
                <td>Identifiant unique interne associé au fournisseur d’identité lors du processus d’intégration.</td>
                <td><i>required</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">authorized</td>
                <td>État de décision de la ressource, qui peut être "true" ou "false".</td>
                <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">source</td>
               <td>
                  Informations sur la source de décision.
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <ul>
                    <li><b>mvpd</b><br/>La décision est émise par le point de terminaison d’autorisation MVPD.</li>
                    <li><b>dégradation</b><br/>La décision est émise à la suite d’un accès dégradé.</li>
                    <li><b>temppass</b><br/>La décision est émise à la suite d’un accès temporaire.</li>
                    <li><b>factice</b><br/>La décision est émise à la suite de la fonctionnalité d’autorisation factice.</li>
                  </ul>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token</td>
               <td>
                  Informations sur le jeton multimédia.
                  <br/><br/>
                  Objet JSON possédant les attributs suivants :
                  <ul>
                    <li><b>notBefore</b><br/>Horodatage avant lequel le jeton multimédia n’est pas valide.</li>
                    <li><b>notAfter</b><br/>Horodatage après lequel le jeton multimédia n’est pas valide.</li>
                    <li><b>serializedToken</b><br/>Jeton multimédia encodé en Base64.</li>
                  </ul>
               <td>facultatif</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>Horodatage avant lequel la décision n’est pas valide.</td>
               <td>facultatif</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Horodatage au bout duquel la décision n’est pas valide.</td>
               <td>facultatif</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">erreur</td>
               <td>L’erreur fournit des informations supplémentaires sur la décision "Refuser" conforme à la documentation <a href="../../../enhanced-error-codes.md">Codes d’erreur améliorés</a>.</td>
               <td>facultatif</td>
            </tr>
         </table>
      </td>
      <td><i>required</i></td>
</table>

### Erreur {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Etat</td>
      <td>400, 401, 405, 500</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corps</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">erreur</td>
      <td>L’erreur fournit des informations supplémentaires conformes à la documentation <a href="../../../enhanced-error-codes.md">Enhanced Error Codes</a>.</td>
      <td><i>required</i></td>
   </tr>
</table>

## Exemples {#samples}

### 1. Récupérer les décisions d’autorisation à l’aide de mvpd standard

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/REF30/decisions/authorize/Cablevision

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Réponse - Disponible]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
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
        }
    ]
}
```

>[!ENDTABS]

### 2. Récupérer les décisions d’autorisation à l’aide d’un laissez-passer temporaire

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/apasstest1/decisions/authorize/TempPass_TEST40 HTTP/1.1

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Réponse - Disponible]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "issuedAt": 1697094207324,
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!TAB Réponse - Démarré]

```JSON
HTTP/1.1 200 OK
Content-Type: applicationjson
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "issuedAt": 1695360527896,
                "notBefore": 1695360527896,
                "notAfter": 1695360707896,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!TAB Réponse - Expiré]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_expired",
                "message": "TempPass has expired.",
                 "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB Réponse - Configuration non valide]

```JSON
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8
 
{
    "status": 500,
    "code": "temppass_invalid_configuration",
    "message": "TempPass configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 3. Récupérer les décisions d’autorisation à l’aide d’un laissez-passer temporaire promotionnel

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/apasstest1/decisions/authorize/flexibleTempPass HTTP/1.1

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Réponse - Disponible]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
    ]
}
```

>[!TAB Réponse - Démarré]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
    ]
}
```

>[!TAB Réponse - Expiré]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_expired",
                "message": "TempPass has expired.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB Réponse - Consommé]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_max_resources_exceeded",
                "message": "Flexible TempPass maximum resources exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB Réponse - Configuration non valide]

```JSON
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8      
 
{
    "status": 500,
    "code": "temppass_invalid_configuration",
    "message": "TempPass configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB Réponse - Identité non valide]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8 
 
{
    "status": 400,
    "code": "temppass_invalid_identity",
    "message": "TempPass is not available for the specified identity.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 4. Récupérer les décisions d’autorisation à l’aide d’un mvpd dégradé

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/REF30/decisions/authorize/degradedMvpd

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30", "apasstest1"]
}
```

>[!TAB Réponse - AuthNAll Degradation]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            }
        }
    ]
}
```

**Remarque :** Dans ce cas, la règle AuthZAll a &quot;channel&quot; : &quot;apasstest1&quot;

>[!TAB Réponse - AuthZAll Degradation]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            }
        }
    ]
}
```

**Remarque :** Dans ce cas, la règle AuthZAll a &quot;channel&quot; : &quot;apasstest1&quot;

>[!TAB Réponse - Dégradation AuthZNone]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
 
    ]
}
```

**Remarque :** Dans ce cas, la règle AuthZNone a &quot;channel&quot; : &quot;apasstest1&quot;

>[!TAB Réponse - Règle de dégradation expirée]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_configuration_change",
                "message": "AuthXAll degradation configuration changed, please try again!",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]
