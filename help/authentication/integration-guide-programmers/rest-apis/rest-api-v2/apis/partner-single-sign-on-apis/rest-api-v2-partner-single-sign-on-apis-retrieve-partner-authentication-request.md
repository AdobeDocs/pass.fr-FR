---
title: Récupérer la demande d’authentification du partenaire
description: API REST V2 - Récupération de la demande d’authentification du partenaire
exl-id: 52d8a8e9-c176-410f-92bc-e83449278943
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '1219'
ht-degree: 0%

---

# Récupérer la demande d’authentification du partenaire {#retrieve-partner-authentication-request}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Requête {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/api/v2/{serviceProvider}/sessions/sso/{partner}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">méthode</td>
      <td>POSTER</td>
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
      <td style="background-color: #DEEBFF;">partenaire</td>
      <td>Nom du partenaire (par exemple, Apple) qui fournit la structure d’authentification unique intégrée aux flux d’authentification Adobe Pass.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Paramètres du corps</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Domaine d’origine de l’application qui effectue la connexion à MVPD.
        <br/><br/>
        Si la plateforme de l’appareil de diffusion en continu présente des limites dans la fourniture d’une valeur, une application doit reprendre la session d’authentification et fournir une valeur valide.
        <br/><br/>
        Cela sera utilisé dans le cas de scénarios de secours où la réponse indique que l’application de diffusion en continu doit poursuivre le flux d’authentification de base.
      </td>
      <td><i>obligatoire</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        URL de redirection finale vers laquelle l’agent utilisateur accède une fois le flux d’authentification pour MVPD terminé.
        <br/><br/>
        La valeur doit être encodée en URL.
        <br/><br/>
        Si la plateforme de l’appareil de diffusion en continu présente des limites dans la fourniture d’une valeur, une application doit reprendre la session d’authentification et fournir une valeur valide.
        <br/><br/>
        Cela sera utilisé dans le cas de scénarios de secours où la réponse indique que l’application de diffusion en continu doit poursuivre le flux d’authentification de base.
      </td>
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
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Type de média accepté pour les ressources en cours d’envoi.
         <br/><br/>
         Il doit s’agir de application/x-www-form-urlencoded.
      </td>
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
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        La génération de la payload d’authentification unique pour la méthode Partner est décrite dans la documentation d’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a>.
        <br/><br/>
        Pour plus d’informations sur l’authentification unique activée pour les flux utilisant un partenaire, reportez-vous à la documentation <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md"> Authentification unique à l’aide des flux de partenaire </a>.</td>
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
        Le corps de la réponse contient des informations sur les actions suivantes nécessaires pour effectuer l’authentification.
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
         Objet JSON possédant les attributs suivants :
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Action que l’appareil de diffusion en continu doit effectuer pour terminer le flux d’authentification.
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <ul>
                    <li><b>partner_profile</b><br/>L’appareil de diffusion en continu peut utiliser la requête d’authentification de partenaire fournie afin d’obtenir une réponse d’authentification de partenaire qui peut être utilisée pour récupérer un profil.</li>
                    <li><b>authentification</b><br/>Lorsque le flux d’authentification unique du partenaire ne peut pas continuer, l’appareil de diffusion en continu peut revenir au flux d’authentification de base.<br/>L’appareil de diffusion en continu ou un autre appareil doit ouvrir l’URL fournie dans un agent utilisateur.</li>
                    <li><b>resume</b><br/>Lorsque le flux d’authentification unique du partenaire ne peut pas continuer, l’appareil de diffusion en continu peut revenir au flux d’authentification de base.<br/>L’appareil de diffusion en continu ou un autre appareil doit fournir les paramètres manquants et reprendre la session d’authentification à l’aide du code .</li>
                    <li><b>authorize</b><br/>L’appareil de diffusion en continu peut traiter directement les flux de décisions.</li>
                  </ul>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  Type d’interaction que l’appareil de diffusion en continu doit effectuer pour continuer le flux avec l’action spécifiée par l’attribut « actionName ».
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <ul>
                    <li><b>interactif</b><br/>Le flux se poursuit avec une navigation vers l’URL fournie à l’aide d’un agent utilisateur.</li>
                    <li><b>direct</b><br/>Le flux se poursuit par un appel direct à l’URL fournie à l’aide d’un client HTTP disponible pour l’implémentation du client.</li>
                  </ul>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">reasonType</td>
               <td>
                  Type de raison qui explique le « actionName ».
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <ul>
                    <li><b>none</b><br/>L’application cliente est requise pour continuer à s’authentifier.</li>
                    <li><b>authentifié</b><br/>L’application cliente est déjà authentifiée par le biais de flux d’accès de base.</li>
                    <li><b>degraded</b><br/>L’application cliente est déjà authentifiée par le biais de flux d’accès dégradés.</li>
                    <li><b>authenticatedSSO</b><br/>L’application cliente est déjà authentifiée par le biais de flux d’accès à authentification unique.</li>
                    <li><b>pfs_fallback</b><br/>L’application cliente doit revenir au flux d’authentification de base en raison d’une valeur d’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> manquante ou non valide.</li>
                    <li><b>configuration_fallback</b><br/>L’application cliente doit revenir au flux d’authentification de base en raison de la configuration d’authentification unique du partenaire sur le serveur principal d’Adobe Pass.</li>
                  </ul>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>
                    Paramètres manquants qui doivent être fournis pour terminer le flux d’authentification de base.
                    <br/><br/>
                    Ce champ est présent lorsque le flux d’authentification unique du partenaire ne peut pas continuer.
               </td>
               <td>facultatif</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>URL dans laquelle l’application cliente doit naviguer.</td>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">code</td>
               <td>
                    Code d’authentification qui peut être utilisé sur une application secondaire pour reprendre la session d’authentification.
                    <br/><br/>
                    Ce champ est présent lorsque le flux d’authentification unique du partenaire ne peut pas continuer.
               </td>
               <td>facultatif</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">authenticationRequest</td>
               <td>
                    Requête d’authentification du partenaire à utiliser dans le flux d’authentification avec le partenaire en dehors du système d’authentification Adobe Pass.
                    <br/><br/>
                    Ce champ est présent lorsque le flux d’authentification unique du partenaire peut continuer.
                    <br/><br/>
                    Objet JSON possédant les attributs suivants :
                    <ul>
                        <li><b>type</b><br/>Indique le type de protocole pris en charge par le MVPD (SAML uniquement).</li>
                        <li><b>request</b><br/>Requête SAML.</li>
                        <li><b>attributes</b><br/>Attributs de requête SAML.</li>
                    </ul>
               </td>
               <td>facultatif</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>Identifiant opaque pouvant être utilisé pour le suivi de l’activité des utilisateurs et utilisatrices.</td>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Identifiant unique interne associé au fournisseur d’identité lors du processus d’intégration.</td>
               <td>facultatif</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>Identifiant unique interne associé au fournisseur de services lors du processus d’intégration.</td>
               <td><i>obligatoire</i></td>
            </tr>
         </table>
      </td>
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

### 1. Récupérer la demande d’authentification du partenaire

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "partner_profile",
    "actionType": "direct",
    "reasonType": "none",
    "url": "/api/v2/REF30/profiles/sso/Apple",
    "sessionId": "83c046be-ea4b-4581-b5f2-13e56e69dee9",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "authenticationRequest": {
        "type": "saml",
        "request": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRG....",
        "attributesNames": ["uid", "NameID", "uniqueId"]
   }
}    
```

>[!ENDTABS]

### 2. Récupérez la demande d’authentification du partenaire, mais la dégradation est appliquée

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiJHtkZWdyYWRlZE12cGR9IiwKICAgICAgImV4cGlyYXRpb25EYXRlIiA6ICIyMDI1NDMwNjM2MDAwIgogICAgfQp9
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authorize",
    "actionType": "direct",
    "reasonType": "degraded",
    "url": "/api/v2/REF30/decisions/authorize/${degradedMvpd}",
    "sessionId": "14d4f239-e3b1-4a4a-b8b3-6395b968a260",
    "mvpd": "${degradedMvpd}",
    "serviceProvider": "REF30"
}
```

>[!ENDTABS]

### 3. Récupérer la demande d’authentification du partenaire, mais revient au flux d’authentification de base en raison d’une valeur d’en-tête AP-Partner-Framework-Status manquante ou non valide

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImRlbmllZCIKICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHt9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authenticate",
    "actionType": "interactive",
    "reasonType": "pfs_fallback",
    "url": "/api/v2/authenticate/REF30/OKTWW2W",
    "code": "OKTWW2W",
    "sessionId": "748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]

### 4. Récupérez la demande d’authentification du partenaire, mais retourne au flux d’authentification de base en raison de la configuration d’authentification unique du partenaire sur le serveur principal d’Adobe Pass

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authenticate",
    "actionType": "interactive",
    "reasonType": "configuration_fallback",
    "url": "/api/v2/authenticate/REF30/OKTWW2W",
    "code": "OKTWW2W",
    "sessionId": "748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]

### 5. Récupérer la demande d’authentification du partenaire, mais revient au flux d’authentification de base en raison de paramètres manquants

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:
        
domainName=adobe.com
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "resume",
    "actionType": "direct",
    "reasonType": "none",
    "missingParameters": [
          "redirectUrl"
    ],
    "url": "/api/v2/REF30/sessions/SB7ZRIO",
    "code": "SB7ZRIO",
    "sessionId": "1476173f-5088-43b8-b7c3-8cf3a185de0a",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]
