---
title: Récupération du profil à l’aide de la réponse d’authentification du partenaire
description: API REST V2 - Récupération du profil à l’aide de la réponse d’authentification du partenaire
source-git-commit: 4598aaa0827b943de83a9e7d847227edf6b0b387
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 2%

---


# Récupération du profil à l’aide de la réponse d’authentification du partenaire {#retrieve-profile-using-partner-authentication-response}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> La mise en oeuvre de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/throttling-mechanism.md) .

## Requête {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">path</td>
      <td>/api/v2/{serviceProvider}/profiles/sso/{partner}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Paramètres de chemin</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identifiant unique interne associé au fournisseur de services lors du processus d’intégration.</td>
      <td><i>required</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">partenaire</td>
      <td>Nom du partenaire (par exemple, Apple) qui fournit la structure de connexion unique intégrée aux flux d’authentification Adobe Pass.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Paramètres du corps</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">SAMLResponse</td>
      <td>
        Réponse d’authentification du partenaire contenant les métadonnées utilisateur nécessaires pour créer et enregistrer un profil de partenaire.
        <br/><br/>
        La valeur doit être en codage Base64 et en codage URL ultérieure.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisation</td>
      <td>La génération de la charge utile du jeton porteur est décrite dans la documentation <a href="../../../dynamic-client-registration-api.md">Enregistrement dynamique du client</a>.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Type de média accepté pour les ressources envoyées.
         <br/><br/>
         Il doit être application/x-www-form-urlencoded.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>La génération de la payload de l’identifiant de l’appareil est décrite dans la documentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> .</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La génération de la payload d’informations sur l’appareil est décrite dans la documentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
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
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        La génération de la payload de connexion unique pour la méthode Partner est décrite dans la documentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> .
        <br/><br/>
        Pour plus d’informations sur les flux activés pour l’authentification unique à l’aide d’un partenaire, reportez-vous à la documentation <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-partner-flows.md">Connexion unique à l’aide des flux de partenaire</a> .</td>
      <td>facultatif</td>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">Code</th>
      <th style="background-color: #EFF2F7; width: 20%;">Texte</th>
      <th style="background-color: #EFF2F7;">Description</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Créé</td>
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
        Le jeton d’accès n’est pas valide, le client doit obtenir un nouveau jeton d’accès et réessayer. Pour plus d’informations, reportez-vous à la documentation <a href="../../../dynamic-client-registration-api.md">Enregistrement du client dynamique</a> .
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">En-têtes</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">Corps</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">profils</td>
      <td>
         JSON contenant une carte de paires clé-valeur.
         <br/><br/>
         L’élément clé est défini par la valeur suivante :
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Valeur</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Identifiant unique interne associé au fournisseur d’identité lors du processus d’intégration.</td>
               <td><i>required</i></td>
            </tr>
         </table>
         L’élément value est défini par les attributs suivants :
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
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
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Valeur</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">Apple</td>
                        <td>
                            Le profil a été créé suite à :
                            <ul>
                                <li>Authentification unique à l’aide d’Apple partenaire</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               </td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">type</td>
               <td>
                  Type du profil.
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Valeur</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">appleSSO</td>
                        <td>
                            Le profil a été créé suite à :
                            <ul>
                                <li>Authentification unique à l’aide d’Apple partenaire</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               </td>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">Corps</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">erreur</td>
      <td>L’erreur fournit des informations supplémentaires conformes à la documentation <a href="../../../enhanced-error-codes.md">Enhanced Error Codes</a>.</td>
      <td><i>required</i></td>
   </tr>
</table>

## Exemples {#samples}

### 1. Apple SSO activé et réponse SAML valide

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/REF30/profiles/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB Réponse]

```JSON
HTTP/1.1 200 OK
 
{
    "profiles" : {
        "Cablevision" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Apple",
            "type" : "appleSSO",
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
                }       
            }
        }
     }
}  
```

>[!ENDTABS]

### 2. Profil Apple SSO pour une intégration dégradée

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/REF30/profiles/sso/Apple HTTP/1.1
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB Réponse]

```JSON
HTTP/1.1 200 OK
 
{
   "profiles":{
      "WOW":{
         "notBefore":1706636062704,
         "notAfter":1706696062704,
         "issuer":"Adobe",
         "type":"degraded",
         "attributes":{
            "userID":{
               "value":"95cf93bcd183214ac9e4433153cb8a9d180a463128c0a5d26f202e8c",
               "state":"plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 3. Flux SSO Apple lorsque la réponse SAML n’est pas valide

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/REF30/profiles/sso/Apple HTTP/1.1 
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:
        
SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB Réponse]

```JSON
HTTP/1.1 403 OK
Content-Type: application/json; charset=utf-8
    
{
    "errors" : [
        {
            "code": "invalid_mvpd_response",
            "message": "The saml mvpd response is not valid",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"        
        } 
    ]
}   
```

>[!ENDTABS]
