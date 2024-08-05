---
title: Récupération de la demande d’authentification du partenaire
description: API REST V2 - Récupération de la demande d’authentification du partenaire
source-git-commit: 4598aaa0827b943de83a9e7d847227edf6b0b387
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 1%

---


# Récupération de la demande d’authentification du partenaire {#retrieve-partner-authentication-request}

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
      <td>/api/v2/{serviceProvider}/sessions/sso/{partner}</td>
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
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Domaine d’origine de l’application effectuant la connexion MVPD.
        <br/><br/>
        Si la plateforme de périphérique de diffusion en continu présente des limites en ce qui concerne la fourniture d’une valeur, une application doit reprendre la session d’authentification et fournir une valeur valide.
        <br/><br/>
        Cela sera utilisé dans le cas de scénarios de secours où la réponse indique que l’application en continu doit poursuivre le flux d’authentification de base.
      </td>
      <td><i>required</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        URL de redirection finale vers laquelle navigue l’agent utilisateur une fois le flux d’authentification du MVPD terminé.
        <br/><br/>
        La valeur doit être en codage URL.
        <br/><br/>
        Si la plateforme de périphérique de diffusion en continu présente des limites en ce qui concerne la fourniture d’une valeur, une application doit reprendre la session d’authentification et fournir une valeur valide.
        <br/><br/>
        Cela sera utilisé dans le cas de scénarios de secours où la réponse indique que l’application en continu doit poursuivre le flux d’authentification de base.
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
      <td>200</td>
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
      <td style="background-color: #DEEBFF;"></td>
      <td>
         Objet JSON possédant les attributs suivants :
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Action que l’appareil de diffusion en continu doit effectuer pour terminer le flux d’authentification.
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Valeur</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">partner_profile</td>
                        <td>Le périphérique de diffusion en continu peut utiliser la requête d’authentification de partenaire fournie pour obtenir une réponse d’authentification de partenaire qui peut être utilisée pour récupérer un profil.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">authentifier</td>
                        <td>
                            Lorsque le flux d’authentification unique du partenaire ne peut pas continuer, l’appareil de diffusion en continu peut revenir au flux d’authentification de base.
                            <br/><br/>
                            Le périphérique de diffusion en continu ou un autre périphérique doit ouvrir l’URL fournie dans un agent utilisateur.
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">resume</td>
                        <td>
                            Lorsque le flux d’authentification unique du partenaire ne peut pas continuer, l’appareil de diffusion en continu peut revenir au flux d’authentification de base.
                            <br/><br/>
                            Le périphérique de diffusion en continu ou un autre périphérique doit fournir les paramètres manquants et reprendre la session d’authentification à l’aide du code.
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">autoriser</td>
                        <td>Le périphérique de diffusion en continu peut directement procéder aux flux de décisions.</td>
                     </tr>
                  </table>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  Le type d’interaction que l’appareil de diffusion en continu doit effectuer pour continuer le flux avec l’action spécifiée par l’attribut 'actionName'.
                  <br/><br/>
                  Les valeurs possibles sont les suivantes :
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Valeur</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">direct</td>
                        <td>Le flux se poursuit avec un appel direct à l’URL fournie à l’aide d’un client HTTP disponible pour l’implémentation du client.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">interactif</td>
                        <td>Le flux se poursuit avec une navigation vers l’URL fournie à l’aide d’un agent utilisateur.</td>
                     </tr>
                  </table>
               <td><i>required</i></td>
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
               <td>URL à laquelle l’application cliente doit accéder.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">code</td>
               <td>
                    Code d’authentification pouvant être utilisé sur une application secondaire pour reprendre la session d’authentification.
                    <br/><br/>
                    Ce champ est présent lorsque le flux d’authentification unique du partenaire ne peut pas continuer.
               </td>
               <td>facultatif</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">authenticationRequest</td>
               <td>
                    Demande d’authentification de partenaire à utiliser dans le flux d’authentification avec le partenaire en dehors du système d’authentification Adobe Pass.
                    <br/><br/>
                    Ce champ est présent lorsque le flux d’authentification unique du partenaire peut continuer.
                    <br/><br/>
                    Objet JSON possédant les attributs suivants :
                    <table>
                        <tr>
                            <th style="background-color: #EFF2F7; width: 30%;">Attribut</th>
                            <th style="background-color: #EFF2F7;"></th>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">type</td>
                            <td>
                                Indique le type de protocole pris en charge par le MVPD.
                                <br/><br/>
                                Les valeurs possibles sont les suivantes :
                                <table>
                                    <tr>
                                        <th style="background-color: #EFF2F7; width: 30%;">Valeur</th>
                                        <th style="background-color: #EFF2F7;"></th>
                                    </tr>
                                    <tr>
                                        <td style="background-color: #DEEBFF;">saml</td>
                                        <td>Le MVPD prend en charge le protocole SAML.</td>
                                    </tr>
                                </table>
                            </td>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">requête</td>
                            <td>Requête SAML.</td>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">Attributs</td>
                            <td>Attributs de requête SAML.</td>
                        </tr>
                    </table>
               </td>
               <td>facultatif</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>Identifiant opaque qui peut être utilisé pour le suivi de l’activité des utilisateurs.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Identifiant unique interne associé au fournisseur d’identité lors du processus d’intégration.</td>
               <td>facultatif</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>Identifiant unique interne associé au fournisseur de services lors du processus d’intégration.</td>
               <td><i>required</i></td>
            </tr>
         </table>
      </td>
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

### 1. SSO du partenaire valide activé

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Réponse]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"partner_profile",
   "actionType":"direct",
   "url":"/v2/REF30/profiles/sso/Apple/Cablevision",
   "sessionId":"83c046be-ea4b-4581-b5f2-13e56e69dee9",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30",
   "authenticationRequest":{
      "type":"saml",
      "request":"PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRG...."
   }
}    
```

>[!ENDTABS]

### 2. MVPD dégradé

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Réponse]

```JSON
HTTP/1.1 200 OK
 
{          
   "actionName" : "authorize",
   "actionType" : "direct",
   "url" : "/api/v2/REF30/decisions",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30",
   "sessionId":"14d4f239-e3b1-4a4a-b8b3-6395b968a260"
}
```

>[!ENDTABS]

### 3. Intégration désactivée

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Réponse]

```JSON
HTTP/1.1 403
Content-Type: application/json; charset=utf-8
 
{
    "errors" : [
        {
            "code": "unknown_integration",
            "message": "The integration between the specified programmer and identity provider doesn't exist or it's disabled. Use the TVE Dashboard to register or enable the required integration.",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"        
        } 
    ]
}
```

>[!ENDTABS]

### 4. SSO du partenaire non activé (dans la configuration Pass ou sur VSA) - Basculement vers l’authentification régulière, tous les paramètres requis présents

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status : ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Réponse]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"authenticate",
   "actionType":"interactive",
   "url":"/v2/authenticate/REF30/OKTWW2W",
   "code":"OKTWW2W",
   "sessionId":"748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}  
```

>[!ENDTABS]

### 5. L’authentification unique du partenaire n’est pas activée (dans la configuration Pass ou sur VSA) : reprise de l’authentification régulière, paramètres requis manquants, la session d’authentification doit reprendre.

>[!BEGINTABS]

>[!TAB Requête]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:
        
domainName=adobe.com
```

>[!TAB Réponse]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"resume",
   "actionType":"direct",
   "missingParameters":[
      "redirectUrl"
   ],
   "url":"/v2/REF30/sessions/SB7ZRIO",
   "code":"SB7ZRIO",
   "sessionId":"1476173f-5088-43b8-b7c3-8cf3a185de0a",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}
```

>[!ENDTABS]
