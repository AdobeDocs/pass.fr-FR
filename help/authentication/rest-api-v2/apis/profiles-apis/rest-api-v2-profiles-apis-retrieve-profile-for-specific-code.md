---
title: Récupération du profil pour un code spécifique
description: API REST V2 - Récupération du profil pour un code spécifique
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 2%

---


# Récupération du profil pour un code spécifique {#retrieve-profile-for-specific-code}

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
      <td>/api/v2/{serviceProvider}/profiles/code/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
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
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">code</td>
      <td>Code d’authentification obtenu après la création de la session d’authentification sur l’appareil de diffusion en continu.</td>
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
        Le corps de la réponse contient un mappage des profils valides, qui peut être vide.
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
      <td style="background-color: #DEEBFF;">profils</td>
      <td>
        JSON contenant une carte de paires clé-valeur.
        <br/><br/>
        L’élément clé est défini par la valeur suivante :
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Valeur</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Identifiant unique interne associé au fournisseur d’identité lors du processus d’intégration.</td>
               <td><i>required</i></td>
            </tr>
         </table>
         L’élément value est défini par les attributs suivants :
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>Horodatage avant lequel le profil n’est pas valide.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Horodatage au bout duquel le profil n’est pas valide.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">issuer</td>
               <td>
                  Entité propriétaire du profil.
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <ul>
                    <li><b>mvpd (par exemple, Spectrum, Cablevision, etc.)</b><br/>Le profil a été créé suite à : authentification de base.</li>
                    <li><b>Adobe</b><br/>Le profil a été créé suite à : accès dégradé, accès temporaire.</li>
                  </ul>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">type</td>
               <td>
                  Type du profil.
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <ul>
                    <li><b>normal</b><br/>Le profil a été créé suite à : authentification de base.</li>
                    <li><b>degraded</b><br/>Le profil a été créé à la suite de : accès dégradé.</li>
                    <li><b>temporaire</b><br/>Le profil a été créé suite à : accès temporaire.</li>
                  </ul>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Attributs</td>
               <td>
                    Liste des attributs de métadonnées utilisateur.
                    <br/><br/>
                    Ces attributs peuvent être :
                    <ul>
                        <li>Obligatoire, comme "userId"</li>
                        <li>Non obligatoire, comme "zip", "householdId", "maxRating", etc.</li>
                    </ul>
                    Les valeurs des attributs peuvent être les suivantes :
                    <ul>
                        <li>simple</li>
                        <li>list</li>
                        <li>map</li>
                    </ul>
               </td>
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

### 1. Récupération des profils authentifiés existants et valides sur un périphérique secondaire après une authentification de base

>[!BEGINTABS]

>[!TAB Requête]

```JSON
GET /api/v2/REF30/profiles/Cablevision/XTC98W
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
```

>[!TAB Réponse]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles" : {
        "Cablevision" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Cablevision",
            "type" : "regular",
            "attributes" : {
                "userId" : {
                    "value" : "BASE64_value_userId",
                    "state" : "plain"
                },
                "householdId" : {
                    "value" : "BASE64_value_householdId",
                    "state" : "plain"
                },
                "zip" : {
                    "value" : "BASE64_value_zip",
                    "state" : "enc"
                },
                "parental-controls" : {
                    "value" : BASE64_value_parental-controls,
                    "state" : "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]
