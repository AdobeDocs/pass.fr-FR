---
title: Récupérer le jeton d’accès
description: API d’enregistrement client dynamique - Récupération du jeton d’accès
exl-id: 23287acf-5d56-46f0-b65e-79bf7d667708
source-git-commit: 110e8519d6c042cc38de3fbefcd34297b6edcfad
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 1%

---

# Récupérer le jeton d’accès {#retrieve-access-token}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API d’enregistrement client dynamique est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Requête {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/o/client/token</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">méthode</td>
      <td>POSTER</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Paramètres du corps</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            Chaîne d’identifiant de l’application cliente.
            <br/><br/>
            Pour plus d’informations sur l’obtention de la chaîne d’identifiant client, reportez-vous à la documentation de l’API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Récupération des informations d’identification client</a>.
      </td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_secret</td>
      <td>
            Chaîne secrète de l’application cliente.
            <br/><br/>
            Pour plus d’informations sur l’obtention de la chaîne secrète client, reportez-vous à la documentation de l’API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Récupération des informations d’identification client</a>.
      </td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">grant_type</td>
      <td>
            Chaîne de type d’octroi (par exemple, « client_credentials ») que l’application cliente peut utiliser pour le point d’entrée du jeton client.
            <br/><br/>
            Pour plus d’informations sur l’obtention de la chaîne de type d’octroi, consultez la documentation de l’API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Récupération des informations d’identification du client</a>.
      </td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
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
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La génération de la payload d’informations sur le périphérique est décrite dans la documentation de <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
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

### Succès {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Etat</td>
      <td>201</td>
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
               <td style="background-color: #DEEBFF;">id</td>
               <td>Identifiant opaque pouvant être utilisé pour le suivi de l’activité des utilisateurs et utilisatrices.</td>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>Valeur du jeton d’accès que l’application cliente doit utiliser pour l’en-tête Autorisation .</td>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>Heure en millisecondes à laquelle le jeton d’accès a été émis.</td>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">expires_in</td>
               <td>Délai en secondes avant l’expiration du jeton d’accès.</td>
               <td><i>obligatoire</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>Type de jeton (par exemple, « porteur »).</td>
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
      <td>400</td>
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
      <td style="background-color: #DEEBFF;">erreur</td>
      <td>
        Les valeurs possibles sont les suivantes :
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Valeur</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_request</td>
               <td>
                    La requête n’est pas valide pour l’une des raisons suivantes :
                    <ul>
                        <li>Il manque un paramètre obligatoire à la requête.</li>
                        <li>La requête inclut une valeur de paramètre non prise en charge (autre que le type d’octroi).</li>
                        <li>La requête répète un paramètre.</li>
                        <li>La requête comprend plusieurs informations d’identification.</li>
                        <li>La requête utilise plusieurs mécanismes pour authentifier le client.</li>
                        <li>La requête est incorrecte.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>Les informations d’identification du client ne sont pas valides. Le client doit obtenir de nouvelles informations d’identification du client et réessayer. Pour plus d’informations, consultez la documentation de l’API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Récupération des informations d’identification du client</a> .</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">authorized_client</td>
               <td>L’application cliente n’est pas autorisée à utiliser ce type d’octroi d’autorisation.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unsupported_grant_type</td>
               <td>Le type d’octroi d’autorisation n’est pas pris en charge par le serveur d’autorisation.</td>
            </tr>
         </table>
      </td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

## Exemples {#samples}

### Récupérer le jeton d’accès {#samples-retrieve-access-token}

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /o/client/token HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
    
Body:
    
client_id=s6BhdRkqt3&client_secret=t7AkePiru4&grant_type=client_credentials
```

>[!TAB Réponse - Succès]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
  "id": "a932f8f0-210a-41a4-b2a8-377751f6b76f",  
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "created_at": 1752148106221,
  "expires_in": 21600,
  "token_type": "bearer"
}
```

>[!TAB Réponse - Erreur]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
