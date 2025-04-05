---
title: Récupération du profil pour un fichier mvpd spécifique
description: API REST V2 - Récupération du profil d’un fichier mvpd spécifique
exl-id: ed1abc33-c279-4465-b5a0-b4e5b892076e
source-git-commit: 32c3176fb4633acb60deb1db8fb5397bbf18e2d0
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 1%

---

# Récupération du profil pour un fichier mvpd spécifique {#retrieve-profile-for-specific-mvpd}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Veillez également à consulter la [FAQ sur l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

## Requête {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/api/v2/{serviceProvider}/profiles/{mvpd}</td>
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
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>Identifiant unique interne associé au fournisseur d’identité lors du processus d’intégration.</td>
      <td><i>obligatoire</i></td>
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
      <td><i>obligatoire</i></td>
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
      <td><i>obligatoire</i></td>
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
      <td style="background-color: #DEEBFF;">Adobe-Subject-Token<br/>or<br/>X-Roku-Reserved-Roku-Connect-Token</td>
      <td>
        La génération de la payload d’authentification unique pour la méthode d’identité de Platform est décrite dans la documentation de l’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a> / <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md">X-Roku-Reserved-Roku-Connect-Token</a>.
        <br/><br/>
        Pour plus d’informations sur les flux activés pour l’authentification unique à l’aide d’une identité de plateforme, reportez-vous à la documentation <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md"> Authentification unique à l’aide des flux d’identité de plateforme </a>.
      </td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Jeton de service AD</td>
      <td>
        La génération de la payload d’authentification unique pour la méthode de jeton de service est décrite dans la documentation d’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>.
        <br/><br/>
        Pour plus d’informations sur les flux activés pour l’authentification unique à l’aide d’un jeton de service, reportez-vous à la documentation <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md"> Authentification unique à l’aide de flux de jetons de service </a>.
      </td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        La génération de la payload d’authentification unique pour la méthode Partner est décrite dans la documentation d’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a>.
        <br/><br/>
        Pour plus d’informations sur l’authentification unique activée pour les flux utilisant un partenaire, reportez-vous à la documentation <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md"> Authentification unique à l’aide des flux de partenaire </a>.</td>
      <td>facultatif</td>
    </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td>La génération de la payload de l’identifiant unique de l’utilisateur est décrite dans la documentation d’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md">AP-TempPass-Identity</a>.</td>
      <td>facultatif</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accepter</td>
      <td>
         Type de média accepté par l’application cliente.
         <br/><br/>
         S’il est spécifié, il doit s’agir d’application/json.
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
        Le corps de la réponse contient un mappage de profils valides, qui peut être vide.
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
      <td>403</td>
      <td>Interdit</td>
      <td>
        La durée de vie (TTL) d’accès temporaire a expiré ou le nombre maximal de ressources est dépassé. Le client doit indiquer à l’utilisateur de lancer un flux d’authentification de base à l’aide d’un MVPD standard. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Codes d’erreur améliorés</a>.
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
      <td style="background-color: #DEEBFF;">profils</td>
      <td>
        JSON contenant un mappage de paires clé-valeur.
        <br/><br/>
        L'élément clé est défini par la valeur suivante :
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Valeur</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Identifiant unique interne associé au fournisseur d’identité lors du processus d’intégration.</td>
               <td><i>obligatoire</i></td>
            </tr>
         </table>
         L’élément de valeur est défini par les attributs suivants :
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>Date et heure auxquelles le profil n’est pas valide.</td>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Date et heure après lesquelles le profil n’est plus valide.</td>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">émetteur</td>
               <td>
                  Entité propriétaire du profil.
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <ul>
                    <li><b>mvpd (par exemple, Spectrum, Télévision par câble, etc.)</b><br/>Le profil a été créé suite à : une authentification de base, une authentification unique à l’aide de l’identité de la plateforme ou une authentification unique à l’aide d’un jeton de service.</li>
                    <li><b>Adobe</b><br/>Le profil a été créé suite à : un accès dégradé, un accès temporaire.</li>
                    <li><b>Apple</b><br/>Le profil a été créé suite à : authentification unique à l'aide d'Apple partenaire.</li>
                  </ul>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">type</td>
               <td>
                  Type du profil.
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <ul>
                    <li><b>standard</b><br/>Le profil a été créé suite à : authentification de base.</li>
                    <li><b>degraded</b><br/>Le profil a été créé suite à : un accès dégradé.</li>
                    <li><b>temporaire</b><br/>Le profil a été créé suite à : accès temporaire.</li>
                    <li><b>appleSSO</b><br/>Le profil a été créé suite à : authentification unique à l’aide d’Apple partenaire.</li>
                    <li><b>platformSSO</b><br/>Le profil a été créé suite à : authentification unique à l’aide de l’identité de la plateforme.</li>
                    <li><b>serviceTokenSSO</b><br/>Le profil a été créé suite à : authentification unique à l’aide du jeton de service.</li>
                  </ul>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">attributs</td>
               <td>
                    JSON contenant un mappage de paires clé-valeur.
                    <br/><br/>
                    L’élément clé est défini par les attributs de métadonnées de l’utilisateur et peut être :
                    <ul>
                        <li>Obligatoire, comme 'userID'</li>
                        <li>Non obligatoire, comme « zip », « householdID », « maxRating », etc.</li>
                    </ul>
                    Les valeurs des attributs peuvent être les suivantes :
                    <ul>
                        <li>simple</li>
                        <li>liste</li>
                        <li>carte</li>
                    </ul>
                    Les métadonnées de l’utilisateur sont disponibles une fois le flux d’authentification terminé, mais certains attributs de métadonnées peuvent être mis à jour pendant le flux d’autorisation, selon le MVPD et l’attribut de métadonnées spécifique en question.
               </td>
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
      <td>400, 401, 403, 405, 500</td>
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

### 1. Récupérer le profil pour un mvpd spécifique obtenu par l’authentification de base

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/profiles/Spectrum HTTP/1.1 

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "Spectrum": {
            "notBefore": 1623943955,
            "notAfter": 1623951155,
            "issuer": "Spectrum",
            "type": "regular",
            "attributes": {
                "userID": {
                    "value": "BASE64_value_userId",
                    "state": "plain"
                },
                "householdID": {
                    "value": "BASE64_value_householdId",
                    "state": "plain"
                },
                "zip": {
                    "value": "BASE64_value_zip",
                    "state": "enc"
                }
            }
        }
     }
}
```

>[!ENDTABS]

### 2. Récupérez le profil pour un mvpd spécifique obtenu par l’authentification de base ou l’authentification unique à l’aide de la méthode Jeton de service

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/profiles/AdobeShibboleth HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AD-Service-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJkZDNmYWIyN2NmMjg0ZmU2ZWU0ZDY3ZmExZjY4MzE3YyIsImlzcyI6IkFkb2JlIiw.....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "profiles": {
      "AdobeShibboleth": {
         "notBefore": 1748073636999,
         "notAfter": 1748105173000,
         "issuer": "AdobeShibboleth",
         "type": "serviceTokenSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd1...",
               "state": "plain"
            },
            "userID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd14aTV....",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobeShibboleth",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 3. Récupérer le profil d’un fichier mvpd spécifique obtenu par l’authentification de base ou l’authentification unique à l’aide de la méthode Identité de Platform

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/profiles/AdobePass_SMI HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Adobe-Subject-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiIyMmM4MDU1MjEzMDIwYzhmZGYzOGZkMTI1YWViMzUzYSIsImlzcyI6....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "profiles": {
      "AdobePass_SMI": {
         "notBefore": 1724337476000,
         "notAfter": 1724345252000,
         "issuer": "AdobePass_SMI",
         "type": "platformSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "userID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobePass_SMI",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 4. Récupérer le profil pour le TempPass de base

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/profiles/TempPass_TEST40 HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Réponse - Disponible]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "TempPass_TEST40": {
            "notBefore": 1697718650206,
            "notAfter": 1697718710206,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "expiration_date": {
                    "value": 1697718710206,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB Réponse - Limite De Durée Dépassée]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_duration_limit_exceeded",
    "message": "The temporary access duration limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "authentication"
}
```

>[!TAB Réponse - Configuration non valide]

```HTTPS
HTTP/1.1 500 Internal Server Error

Content-Type: application/json;charset=UTF-8

{
    "status": 500,
    "code": "invalid_configuration_temporary_access",
    "message": "The temporary access configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "configuration"
}
```

>[!ENDTABS]

### 5. Récupérer le profil pour le TempPass promotionnel

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/profiles/flexibleTempPass HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Réponse - Disponible]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "flexibleTempPass": {
            "notBefore": 1697720528524,
            "notAfter": 1697720588524,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "remaining_resources": {
                    "value": 1,
                    "state": "plain"
                },
                "used_assets": {
                    "value": [
                        "res04",
                        "res02",
                        "res03",
                        "res01"
                    ],
                    "state": "plain"
                },
                "expiration_date": {
                    "value": 1697720528524,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB Réponse - Limite De Durée Dépassée]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_duration_limit_exceeded",
    "message": "The temporary access duration limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB Réponse - Limite de ressources dépassée]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_resources_limit_exceeded",
    "message": "The temporary access resources limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "authentication"
}
```

>[!TAB Réponse - Configuration non valide]

```HTTPS
HTTP/1.1 500 Internal Server Error

Content-Type: application/json;charset=UTF-8

{
    "status": 500,
    "code": "invalid_configuration_temporary_access",
    "message": "The temporary access configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB Réponse - Identité Non Valide]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{
    "status": 400,
    "code": "invalid_header_identity_for_temporary_access",
    "message": "The identity for temporary access header value is missing or invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 6. Récupérer le profil d’un fichier mvpd spécifique pendant l’application de la dégradation

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/profiles/${degradedMvpd} HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Réponse - Dégradation AuthNAll]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "${degradedMvpd}": {
            "notBefore": 1697719042666,
            "notAfter": 1697719102666,
            "issuer": "Adobe",
            "type": "degraded",
            "attributes":
                "userID": {
                    "value": "95cf93bcd183214a0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!IMPORTANT]
>
> `95cf93bcd183214a` est un préfixe spécifique à la dégradation.

>[!ENDTABS]
