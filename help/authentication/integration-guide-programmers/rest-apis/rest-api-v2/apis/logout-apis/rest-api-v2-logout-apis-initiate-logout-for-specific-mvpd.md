---
title: Lancer la déconnexion pour un fichier mvpd spécifique
description: API REST V2 - Lancement de la déconnexion pour un fichier mvpd spécifique
exl-id: 2482de87-b3d4-4ea8-bd4a-25bf10017e01
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '1025'
ht-degree: 1%

---

# Lancer la déconnexion pour un fichier mvpd spécifique {#initiate-logout-for-specific-mvpd}

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
      <td>/api/v2/{serviceProvider}/logout/{mvpd}</td>
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
      <th style="background-color: #EFF2F7;">Paramètres de requête</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        URL de redirection finale vers laquelle l’agent utilisateur accède une fois le flux de déconnexion pour MVPD terminé.
        <br/><br/>
        La valeur doit être encodée en URL.
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
      <td style="background-color: #DEEBFF;">AP-Visitor-Identifier</td>
      <td>
        La génération de la payload de l’identifiant visiteur est décrite dans la documentation d’en-tête <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md">AP-Visitor-Identifier</a>.
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
        Le corps de la réponse contient une liste d’actions que le client doit effectuer pour terminer le flux de déconnexion pour chaque profil supprimé.
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
      <td style="background-color: #DEEBFF;">déconnexions</td>
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
         </table>
         L’élément de valeur est défini par les attributs suivants :
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Action que l’appareil de diffusion en continu doit effectuer pour terminer le flux de déconnexion.
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <ul>
                    <li><b>logout</b><br/>L’appareil de diffusion en continu doit ouvrir l’URL fournie dans un agent utilisateur.<br/>Cette action s’applique aux scénarios suivants : déconnexion de MVPD avec un point d’entrée de déconnexion.</li>
                    <li><b>partner_logout</b><br/>L’appareil de diffusion en continu doit informer l’utilisateur qu’il doit également se déconnecter au niveau du partenaire (système).<br/>Cette action s’applique aux scénarios suivants : déconnectez-vous de MVPD lorsque le type de profil est « appleSSO ».</li>
                    <li><b>complete</b><br/>L’appareil de diffusion en continu n’a pas besoin d’effectuer d’actions suivantes.<br/>Cette action s’applique aux scénarios suivants : déconnexion de MVPD sans point d’entrée de déconnexion (fonctionnalité de déconnexion factice), déconnexion lors d’un accès dégradé, déconnexion lors d’un accès temporaire.</li>
                    <li><b>non valide</b><br/>L’appareil de diffusion en continu n’a pas besoin d’effectuer d’actions ultérieures.<br/>Cette action s’applique aux scénarios suivants : déconnectez-vous de MVPD lorsqu’aucun profil valide n’est trouvé.</li>
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
                    <li><b>interactive</b><br/>Ce type s’applique aux valeurs suivantes de l’attribut « actionName » : <b>logout</b>.</li>
                    <li><b>partner_interactive</b><br/>Ce type s’applique aux valeurs suivantes de l’attribut « actionName » : <b>partner_logout</b>.</li>
                    <li><b>none</b><br/>Ce type s’applique aux valeurs suivantes de l’attribut « actionName » : <b>complete</b>, <b>invalid</b>.</li>
                  </ul>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Identifiant unique interne associé au fournisseur d’identité lors du processus d’intégration.</td>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>
                  URL utilisée pour effectuer le flux de déconnexion avec le point d’entrée MVPD.
                  <br/><br/>
                  Ce paramètre n’est pas présent pour les valeurs suivantes de l’attribut « actionName » :
                  <ul>
                    <li><b>terminé</b></li>
                    <li><b>invalide</b></li>
                  </ul>
               </td>
               <td>facultatif</td>
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

### &#x200B;1. Lancer la déconnexion pour un mvpd spécifique ayant un point d’entrée de déconnexion

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/logout/Cablevision?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

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
    "logouts": {
        "Cablevision": {
            "actionName": "logout",
            "actionType": "interactive",
            "mvpd": "Cablevision",
            "url": "https://sp.auth.adobe.com/adobe-services/logout?noflash=true&mso_id=Cablevision&requestor_id=REF30&redirect_url=http%3A%2F%2Fadobe.com"
        }
    }
}
```

>[!ENDTABS]

### &#x200B;2. Lancer la déconnexion pour un mvpd spécifique sans point d’entrée de déconnexion

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/logout/Dish?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

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
    "logouts": {
        "Dish": {
            "actionName": "complete",
            "actionType": "none",
            "mvpd": "Dish"
       }
    }
}
```

>[!ENDTABS]

### &#x200B;3. Lancez une déconnexion unique pour un mvpd spécifique, y compris les profils obtenus par l’authentification unique à l’aide de la méthode Jeton de service.

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
>
> * MVPD dispose d’un point d’entrée de déconnexion.

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/logout/Spectrum?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AD-Service-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJZemsxTXpNMk4yWXRZMk0wTWkwME1X .....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "logouts": {
      "Spectrum": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Spectrum",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Spectrum&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### &#x200B;4. Lancez une déconnexion unique pour un mvpd spécifique, y compris les profils obtenus par l’authentification unique à l’aide de la méthode d’identité de Platform

>[!IMPORTANT]
>
> Hypothèses
>
> <br/>
>
> * MVPD dispose d’un point d’entrée de déconnexion.

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/logout/Comcast_SSO?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Adobe-Subject-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJObUZtWmpjek5XVXROVFJoWWkwME5ERmlMV0V6Wm .....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "logouts": {
      "Comcast_SSO": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Comcast_SSO",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Comcast_SSO&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### &#x200B;5. Déconnectez-vous d’une mvpd spécifique, y compris des profils obtenus par authentification unique à l’aide du partenaire (Apple).

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/logout/Cablevision?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

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
   "logouts": {
      "Cablevision": {
         "actionName": "partner_logout",
         "actionType": "partner_interactive",
         "mvpd": "Cablevision"
      }
   }
}
```

>[!ENDTABS]

### &#x200B;6. Déclenchez la déconnexion d’un fichier mvpd spécifique pendant l’application de la dégradation

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/REF30/logout/${degradedMvpd}?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

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
    "logouts": {
        "ATTOTT": {
            "actionName": "complete",
            "actionType": "none",
            "mvpd": "${degradedMvpd}",
        }
    }
}
```

>[!ENDTABS]

### &#x200B;7. Déconnectez-vous du TempPass de base ou promotionnel (non requis)

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/v2/logout/REF30/TempPass_5mins?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

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
    "logouts": {
        "TempPass_5mins": {
            "actionName": "complete",
            "actionType": "none",
            "mvpd": "TempPass_5mins"
        }
    }
}
```

>[!ENDTABS]
