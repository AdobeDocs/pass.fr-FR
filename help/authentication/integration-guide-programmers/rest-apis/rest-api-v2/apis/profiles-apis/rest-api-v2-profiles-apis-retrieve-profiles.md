---
title: Récupération des profils
description: API REST V2 - Récupération de profils
exl-id: 72922aa8-95ca-48dc-8523-e335802fc366
source-git-commit: ebe0a53e3ba54c2effdef45c1143deea0e6e57d3
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 1%

---

# Récupération des profils {#retrieve-profiles}

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
      <td>/api/v2/{serviceProvider}/profiles</td>
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
               <td>Date et heure, en millisecondes, avant lesquelles le profil n’est pas valide.</td>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Date et heure en millisecondes au-delà desquelles le profil n’est plus valide.</td>
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
                    <li><b>Apple</b><br/>Le profil a été créé suite à : authentification unique à l'aide d'Apple partenaire.</li>
                  </ul>
               </td>
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

### &#x200B;1. Récupérer les profils obtenus par l’intermédiaire de l’authentification de base

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/profiles HTTP/1.1
 
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
        "Cablevision": {
            "notBefore": 1623943955,
            "notAfter": 1623951155,
            "issuer": "Cablevision",
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
        },
        "Spectrum": {
            "notBefore": 1623943955,
            "notAfter": 1623951155,
            "issuer": "Spectrum",
            "type": "regular",
            "attributes": {
                "userID": {
                    "value": "BASE64_value_userId",
                    "state": "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]

### &#x200B;2. Récupérez les profils obtenus par authentification de base ou authentification unique à l’aide de la méthode Jeton de service

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/profiles HTTP/1.1
 
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
      },
      "Spectrum": {
         "notBefore": 1623943955,
         "notAfter": 1623951155,
         "issuer": "Spectrum",
         "type": "regular",
         "attributes": {
            "userID": {
               "value": "BASE64_value_userId",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### &#x200B;3. Récupérez les profils obtenus par authentification de base ou authentification unique à l’aide de la méthode d’identification Platform

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/profiles HTTP/1.1
 
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
      },
      "Cablevision": {
         "notBefore": 1623943955,
         "notAfter": 1623951155,
         "issuer": "Spectrum",
         "type": "regular",
         "attributes": {
            "userID": {
               "value": "BASE64_value_userId",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]
