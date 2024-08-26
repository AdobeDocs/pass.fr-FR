---
title: Récupération des informations d’identification client
description: API d’enregistrement du client dynamique - Récupération des informations d’identification du client
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 1%

---


# Récupération des informations d’identification client {#retrieve-client-credentials}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API d’enregistrement de client dynamique est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

## Requête {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">path</td>
      <td>/o/client/register</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Paramètres du corps</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">software_statement</td>
      <td>
            L’instruction logicielle associée à l’application enregistrée créée et téléchargée à partir du <a href="https://console.auth.adobe.com/">tableau de bord Adobe Pass TVE</a>.
            <br/><br/>
            La gestion des applications enregistrées est décrite dans la documentation <a href="../dynamic-client-registration-overview.md">Présentation de l’enregistrement du client dynamique</a> .
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirect_uri</td>
      <td>L’URI de redirection associé à l’emplacement où navigue l’agent utilisateur une fois le flux d’authentification terminé.</td>
      <td>facultatif</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
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
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La génération de la payload d’informations sur l’appareil est décrite dans la documentation <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
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
               <td style="background-color: #DEEBFF;">client_id</td>
               <td>Chaîne d’identifiant de l’application cliente.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_secret</td>
               <td>Chaîne secrète de l’application cliente.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_id_issue_at</td>
               <td>Heure à laquelle l’identifiant de l’application cliente a été émis.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">redirect_uris</td>
               <td>Tableau de chaînes URI de redirection que l’application cliente peut utiliser dans les flux basés sur la redirection.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">grant_types</td>
               <td>Chaînes de type d’octroi que la demande client peut utiliser pour le point de terminaison du jeton client.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">portées</td>
               <td>Chaînes de portée définissant les API d’authentification Adobe Pass que l’application cliente peut utiliser.</td>
               <td><i>required</i></td>
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
      <td>400</td>
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
                    La demande n’est pas valide pour l’une des raisons suivantes :
                    <ul>
                        <li>Un paramètre requis n’est pas associé à la requête.</li>
                        <li>La requête inclut une valeur de paramètre non prise en charge.</li>
                        <li>La requête répète un paramètre.</li>
                        <li>La requête est incorrecte.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_redirect_uri</td>
               <td>La requête inclut une valeur pour l’URI de redirection non valide.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_software_statement</td>
               <td>La requête inclut une valeur pour l’instruction logicielle non valide.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unapproved_software_statement</td>
               <td>La requête inclut une valeur pour l’instruction logicielle qui n’est pas approuvée pour l’utilisation par le serveur d’authentification Adobe Pass.</td>
            </tr>
         </table>
      </td>
      <td><i>required</i></td>
   </tr>
</table>

## Exemples {#samples}

### Récupération des informations d’identification client {#samples-retrieve-client-credentials}

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /o/client/register HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/json
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

{
    "software_statement": "eyJhbGciOiJSUzI1NiJ9.
        eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
        bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
        aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
        GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
        zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
        5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
        fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
        U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
        IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
    "redirect_uri": "adobepass://com.programmer"  
 }
```

>[!TAB Réponse - Succès]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
    "client_id": "s6BhdRkqt3",
    "client_secret": "t7AkePiru4",
    "redirect_uris": [
        "app://com.programmer.adobe#sdasdsadas"
    ],
    "grant_types": [
        "client_credentials"
    ],
    "scopes": [
        "api:client:v2"
    ],
    "client_id_issued_at": 1723227212
}
```

>[!TAB Réponse - Erreur]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
