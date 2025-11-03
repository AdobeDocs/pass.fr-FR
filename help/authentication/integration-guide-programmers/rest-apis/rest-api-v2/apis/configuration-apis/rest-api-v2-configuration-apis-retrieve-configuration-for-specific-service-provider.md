---
title: Récupération de la configuration pour un fournisseur de services spécifique
description: API REST V2 - Récupération de la configuration pour un fournisseur de services spécifique
exl-id: ad7e4c6d-ed96-4ae7-82a9-3c24e5fc9302
source-git-commit: 1c96904f67507ad127c29628963d74a9fb010e99
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 1%

---

# Récupération de la configuration pour un fournisseur de services spécifique {#retrieve-configuration-for-specific-service-provider}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Veillez également à consulter la [FAQ sur l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#configuration-phase-faqs-general).

## Requête {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/api/v2/{serviceProvider}/configuration</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">méthode</td>
      <td>GET</td>
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
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Paramètres de requête</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">profil</td>
      <td>-</td>
      <td>facultatif</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisation</td>
      <td>La génération de la payload du jeton porteur est décrite dans la documentation d’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>La génération de la payload de l’identifiant d’appareil est décrite dans la documentation d’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.</td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La génération de la payload d’informations sur le périphérique est décrite dans la documentation d’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
         <br/><br/>
         Il est vivement recommandé de toujours l’utiliser lorsque la plateforme d’appareil de l’application permet la fourniture explicite de valeurs valides.
         <br/><br/>
         Lorsqu’il est fourni, le serveur principal d’authentification Adobe Pass fusionne implicitement (par défaut) les valeurs définies explicitement avec les valeurs extraites.
         <br/><br/>
         Lorsqu’il n’est pas fourni, le serveur principal de l’authentification Adobe Pass utilise implicitement (par défaut) les valeurs extraites.
      </td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         Adresse IP de l’appareil de diffusion en continu.
         <br/><br/>
         Il est vivement recommandé de toujours l’utiliser pour les implémentations serveur à serveur, en particulier lorsque l’appel est effectué par le service de programmation plutôt que par l’appareil de diffusion en continu.
         <br/><br/>
         Pour les implémentations client à serveur, l’adresse IP de l’appareil de diffusion en continu est envoyée implicitement.
      </td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accepter</td>
      <td>
         Type de média accepté par l’application cliente.
         <br/><br/>
         S’il est spécifié, il doit s’agir de application/json;charset=utf-8.
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
        Le corps de la réponse contient une liste de MVPD ayant une intégration active avec le « serviceProvider ».
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Requête incorrecte</td>
      <td>
        La requête n’est pas valide, le client doit la corriger et réessayer. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Codes d’erreur améliorés</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non Autorisé</td>
      <td>
        Le jeton d’accès n’est pas valide, le client doit obtenir un nouveau jeton d’accès et réessayer. Pour plus d’informations, consultez la documentation <a href="../../../rest-api-dcr/dynamic-client-registration-overview.md"> Présentation de l’enregistrement client dynamique </a> .
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Méthode Non Autorisée</td>
      <td>
        La méthode HTTP n’est pas valide, le client doit utiliser une méthode HTTP autorisée pour la ressource demandée et réessayer. Pour plus d’informations, consultez la section <a href="#request">Requête</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erreur de serveur interne</td>
      <td>
        Un problème est survenu côté serveur. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Codes d’erreur améliorés</a>.
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
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corps</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         JSON contenant une liste d’éléments, chaque élément ayant les attributs suivants :
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">appareil</td>
                <td>Type d’appareil</td>
                <td><i>obligatoire</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">clientType</td>
                <td>Type de client</td>
                <td></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">errorReporting</td>
                <td>Objet</td>
                <td></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">demandeur</td>
                <td>
                    Objet JSON possédant les attributs suivants :
                    <ul>
                        <li><b>id</b><br/>identifiant unique interne associé au fournisseur de services lors du processus d’intégration.</li>
                        <li><b>name</b><br/>Nom commercial (de marque) associé au fournisseur de services lors du processus d’intégration.</li>
                        <li><b>domains</b><br/>La liste des noms de domaine associés à l'authentification Adobe Pass pour représenter le fournisseur de services.</li>
                    </ul>
                </td>
                <td><i>obligatoire</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">mvpds</td>
                <td>
                    Objet JSON possédant les attributs suivants :
                    <ul>
                        <li><b>id</b><br/>identifiant unique interne associé au fournisseur d’identité lors du processus d’intégration.</li>
                        <li><b>displayName</b><br/>nom commercial (de marque) associé au fournisseur d’identité lors du processus d’intégration.</li>
                        <li><b>logoUrl</b><br>URL à partir de laquelle télécharger le logo associé au fournisseur d’identité.</li>
                        <li><b>isTempPass</b><br/>Indicateur qui spécifie si le MVPD est conçu pour fournir la fonctionnalité <a href="/help/premium-workflow/temporary-access/temp-pass-feature.md">TempPass</a>.</li>
                        <li><b>isProxy</b><br/>Indicateur qui spécifie si le MVPD est un MVPD proxy.</li>
                        <li><b>boardingStatus</b><br/>Statut spécifiant si le fournisseur d’identité est intégré par la plateforme d’appareil de diffusion en continu pour les flux d’authentification unique.</li>
                        <li><b>platformMappingId</b><br/>Identifiant unique interne associé au fournisseur d’identité par la plateforme de l’appareil de diffusion en continu pour les flux d’authentification unique.</li>
                        <li><b>enablePlatformServices</b><br/>Indicateur qui spécifie si la configuration du fournisseur d’identité est activée pour la plateforme de l’appareil de diffusion en continu pour les flux d’authentification unique.</li>
                        <li><b>displayInPlatformPicker</b><br/>Indicateur qui spécifie si le fournisseur d’identité peut être affiché dans le sélecteur de plateforme d’appareil de diffusion en continu pour les flux d’authentification unique.</li>
                        <li><b>applyPlatformPermissions</b><br/>indicateur qui spécifie si l’appareil de diffusion en continu doit appliquer les autorisations d’utilisateur fournies par la plateforme pour les flux d’authentification unique.</li>
                    </ul>
                </td>
                <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">temps</td>
               <td></td>
               <td><i>obligatoire</i></td>
            </tr>
         </table>
      </td>
      <td><i>obligatoire</i></td>
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
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corps</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Le corps de la réponse peut fournir des informations d’erreur supplémentaires conformes à la documentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> Codes d’erreur améliorés </a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

## Exemples {#samples}

### &#x200B;1. Récupérer la configuration pour un fournisseur de services spécifique

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/configuration/ HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8
 
{
    "device": "unknown",
    "clientType": "html5",
    "os": "Unknown",
    "requestor": {
        "id": "REF30",
        "name": "Reference site only in 30",
        "domains": [
            {
                "name": "adobe.com",
                "mvpdInitiated": false
            },
            {
                "name": "adobe.io",
                "mvpdInitiated": false
            },
            {
                "name": "adobepass.com",
                "mvpdInitiated": false
            },
            {
                "name": "adobeptime.com",
                "mvpdInitiated": false
            },
            {
                "name": "anvilcreative.com",
                "mvpdInitiated": false
            },
            {
                "name": "testadobe.com",
                "mvpdInitiated": false
            }
        ],
        "mvpds": [
            {
                "id": "AdobePass_SMI",
                "displayName": "Adobe Pass SMI",
                "logoUrl": "https://blogs.adobe.com/conversations/files/2010/08/adobe-logo.jpg",
                "authPerAggregator": false
            },
            {
                "id": "TempPass_TEST40",
                "displayName": "Adobe Temp Pass Test 3 min",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "authPerAggregator": true,
                "isTempPass": true
            },
            {
                "id": "TempPass_TEST44",
                "displayName": "Adobe Temp Pass Test 30 min",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "authPerAggregator": true,
                "isTempPass": true
            },
            {
                "id": "AdobeShibboleth",
                "displayName": "AdobeShibboleth",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/adobe.png"
            },
            {
                "id": "ATTOTT",
                "displayName": "DIRECTV STREAM",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/directvstream.jpg"
            },
            {
                "id": "ElasticSSO",
                "displayName": "ElasticSSO",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "authPerAggregator": false
            },
            {
                "id": "TempPass",
                "displayName": "Temp-Pass",
                "logoUrl": "https://entitlement.auth.adobe.com/entitlement/noLogo.png",
                "passiveAuthnEnabled": false,
                "authPerAggregator": true,
                "isTempPass": true
            },
            {
                "id": "Comcast_SSO_Perf",
                "displayName": "Xfinity Perf",
                "logoUrl": "https://login.comcast.net/static/images/ci/tve/mvpd_comcast_logo112x33.gif",
                "authPerAggregator": true,
                "authPerBrowserSession": true
            }
        ]
    }
}  
```

>[!ENDTABS]
