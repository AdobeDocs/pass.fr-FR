---
title: Service d’authentification unique d’Adobe
description: Découvrez le service SSO d’Adobe Pass qui permet une authentification transparente sur plusieurs appareils et applications.
exl-id: a1ff85d4-f7d2-4dea-b82f-d29730d9012f
source-git-commit: 151c64276377be5ef21bca4c0d3eaa04ac3da495
workflow-type: tm+mt
source-wordcount: '3615'
ht-degree: 2%

---


# Service d’authentification unique d’Adobe {#sso-service}

Ce document décrit les cas d’utilisation, les points d’entrée et l’API pour le service d’authentification unique Adobe.

**Révision actuelle - 1.0.0**

## Portée {#scope}

Le service SSO d’Adobe Pass permet une authentification transparente sur plusieurs appareils et applications, offrant ainsi une expérience utilisateur unifiée tout en maintenant les normes de sécurité et de conformité. Ce service répond au besoin croissant d’authentification sur plusieurs plateformes dans le paysage actuel de la diffusion en continu sur plusieurs appareils.

## Vue d’ensemble {#overview}

### Ce que c&#39;est {#what-it-is}

Une solution d’authentification unique complète qui permet aux utilisateurs de s’authentifier une seule fois et de gérer le transfert de l’authentification de manière informée vers d’autres appareils

### Défis actuels de l’authentification {#current-challenges}

* L’utilisateur doit s’authentifier auprès de chaque service de streaming auquel il est abonné
* L’utilisateur doit s’authentifier séparément sur chaque appareil ou application
* En raison de la difficulté à saisir un mot de passe dans le flux d’authentification sur certaines plateformes, cela peut entraîner une augmentation des taux d’abandon

### Opportunité d’un service SSO Adobe Pass {#opportunity}

* Augmentation du nombre de services de streaming nécessitant leur propre authentification
* Demande croissante d’expériences transparentes entre les appareils
* Augmentation du nombre d’appareils de streaming par foyer
* Nécessité d’une authentification unifiée par foyer

### Avantages commerciaux {#business-benefits}

#### Fournisseurs de contenu {#content-providers}

* **Interactions client accrues** - Une expérience transparente allonge la durée des sessions
* **Friction réduite** - Des barrières d’authentification plus faibles augmentent la consommation de contenu
* **Rétention améliorée** - Une meilleure expérience utilisateur réduit le taux de perte de clientèle
* **Réduction des coûts** - Moins de tickets d’assistance liés aux problèmes d’authentification

#### Utilisateurs finaux {#end-users}

* **Expérience transparente** - Authentifiez-vous une fois, accédez partout
* **Time Savings** - Aucun processus de connexion répété
* **Flexibilité de l’appareil** - Basculez entre les appareils sans interruption
* **Expérience cohérente** - Authentification uniforme sur toutes les plateformes

#### IdP (MVPD, opérateurs de télécommunications, etc.) {#idps}

* Les MVPD peuvent être informés des périphériques supplémentaires à l’aide d’une authentification unique
* **Satisfaction améliorée des utilisateurs** - Expérience d’authentification améliorée
* **Charge d’assistance réduite** - Moins d’appels d’assistance liés à l’authentification
* **Avantage concurrentiel** - Meilleure expérience utilisateur que celle des concurrents

## Cas d’utilisation {#use-cases}

### Mappage d’identité {#identity-mapping}

Le service crée la possibilité de lier un compte D2C et TVE dans la même application.

De plus en plus de services de streaming créent des offres groupées destinées à être vendues à un tiers (MVPD/virtual MVPD/Telco, etc.). Les utilisateurs et utilisatrices peuvent avoir à gérer plusieurs comptes dans la même application. Pour créer une expérience d’authentification transparente, un service doit relier ces comptes pour simplifier l’expérience de connexion.

L’utilisateur s’authentifiera avec son compte D2C, puis s’authentifiera UNE FOIS avec un compte différent (par exemple, MVPD). Grâce à notre service, ces comptes seront liés, ce qui signifie qu’à l’avenir, les authentifications suivantes sur différents appareils ne pourront être effectuées qu’avec le compte D2C.

### SSO entre appareils {#cross-device-sso}

L’authentification sur un appareil connecté à la télévision peut être plus difficile que sur un téléphone mobile. Une bonne expérience utilisateur serait de s’authentifier sur le téléphone, puis de transmettre cette authentification à la télévision intelligente.

## Composants clés {#key-components}

* **API de jeton de service** : composant clé qui gère en toute sécurité le mécanisme d’authentification unique.
* **API List** - L’application peut aider l’utilisateur à comprendre la liste des appareils dans son écosystème
* **API Link** - Application permet à l’utilisateur d’ajouter des appareils supplémentaires dans son écosystème
* **Dissocier l’API** - L’application permet à l’utilisateur de supprimer des appareils de son écosystème

## Cas d’utilisation détaillés {#use-cases-detailed}

### SSO D2C-TVE {#d2c-tve-sso}

Ce cas d’utilisation permet de lier les informations d’identification D2C et TVE(MVPD) sur un appareil et d’exploiter ce profil lié sur d’autres appareils au sein de la même application.

Flux SSO ![D2C-TVE](../../../assets/sso_service_d2c_1.png)

![Flux SSO entre appareils](../../../assets/sso_service_d2c_2.png)

### SSO entre appareils {#cross-device-sso-detailed}

Ce cas d’utilisation permet aux utilisateurs déjà authentifiés sur un appareil de réutiliser le profil authentifié sur la même application D2C ou TVE installée sur d’autres appareils (application requise pour implémenter Adobe Pass REST V2 pour l’authentification et l’autorisation).

## Intégration du service Adobe SSO dans une application D2C-TVE {#integration}

### Étape 1 - Obtenir un identifiant commun {#step-1}

Pour intégrer le service SSO Adobe, une implémentation d’application doit établir un identifiant unique et persistant à utiliser comme attribut SSO d’identifiant commun dans X-SSO-ID. Cela peut être obtenu en authentifiant un utilisateur à l’aide d’un service D2C et en conservant un attribut sur cette authentification.

### Étape 2 - Obtention d’un jeton de service {#step-2}

L’utilisation de l’identifiant commun dans X-SSO-ID dans le point d’entrée POST /serviceToken récupère un jeton JWT signé avec la payload suivante :

```json
{
  "iss": "ssoservicetoken",
  "sub": "unique_common_identifier",
  "nbf": 1758093558,
  "exp": 1758097158,
  "iat": 1758093558
}
```

Le jeton de service comporte les heures d’expiration « iat » - émis à et « exp » - , intervalle dans lequel le jeton de service est valide. En cas d’expiration, un nouveau jeton JWT peut être obtenu à l’aide du point d’entrée GET /serviceToken avec le jeton de service expiré.

### Étape 3 : S’authentifier à l’aide de l’API REST Adobe Pass V2 avec un MVPD TVE {#step-3}

L’authentification avec Adobe Pass doit être mise en œuvre à l’aide du jeton de service : [API REST V2 - Flux de jetons de service d’authentification unique](https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-flows/rest-api-v2-single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows)

### Étape 4 - Lier un autre appareil {#step-4}

Depuis l’application authentifiée, un lien peut être créé pour être utilisé sur un autre appareil à l’aide de l’API /link

```json
{
    "status": "CREATED",
    "code": "228128",
    "notBefore": 1758094617220,
    "notAfter": 1758098217220
}
```

Le « code » est un code de lien de courte durée sous la forme d&#39;une séquence de 6 chiffres que l&#39;utilisateur va introduire dans une application non authentifiée sur un appareil secondaire

### Étape 5 - Récupérer l’authentification SSO {#step-5}

Sur un autre appareil, une fois que l’utilisateur a introduit le code, l’application peut :

* récupérer l’identité à partir du service D2C
* récupération d’un profil MVPD d’Adobe Pass avec l’API REST V2

Le profil MVPD sera valide par l’intermédiaire de la connexion unique pendant la période pendant laquelle l’authentification initiale a été obtenue.

Si MVPD invalide le profil ou si l’utilisateur choisit de se déconnecter de MVPD, l’application utilisant l’API REST Adobe Pass V2 ne disposera plus d’un enregistrement de profil et devra s’authentifier à nouveau auprès de MVPD.

### Étape 6 - Gérer l&#39;authentification SSO sur d&#39;autres appareils {#step-6}

Une application peut obtenir des informations sur tous les autres appareils liés avec le même identifiant commun à l’aide de l’API /list.

À tout moment, un appareil peut être supprimé de l’authentification SSO en annulant sa liaison avec l’API /unlink.

## API {#apis}

### API de jeton de service {#service-token-api}

#### Description {#service-token-description}

L’API de jeton de service peut être utilisée pour demander et gérer des jetons de service qui activent la fonctionnalité d’authentification unique (SSO) entre plusieurs applications ou appareils. Ces jetons de service identifient les profils authentifiés (profils SSO) et sont essentiels à l’établissement et à la maintenance des connexions SSO.

>[!WARNING]
>
>Les jetons de service contiennent des informations d’authentification sensibles. Les applications doivent gérer ces jetons de manière sécurisée et ne jamais les exposer à des tiers non approuvés.

L’API Service Token fournit deux points d’entrée principaux :

* **POST /api/{serviceProvider}/serviceToken** - Obtenez un jeton de service JWS nouvellement créé
* **GET /api/{serviceProvider}/serviceToken** - Actualisez un jeton de service JWS existant.

Si la requête de l’API du jeton de service n’a pas pu être traitée en raison d’une erreur des services d’authentification Adobe Pass, des informations d’erreur supplémentaires sont incluses dans la réponse de l’API.

#### POST - serviceToken {#post-service-token}

##### Requête {#post-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/api/{serviceProvider}/serviceToken</td>
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
      <td>Identifiant du fournisseur pour lequel le jeton est demandé.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisation</td>
      <td>La génération de la payload du jeton porteur est décrite dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>
         La génération de la payload de l’identifiant d’appareil est décrite dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.
         <br/><br/>
         Cet identifiant est utilisé comme identifiant SSO par défaut lorsque X-SSO-ID n'est pas fourni.
      </td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         Les informations sur le périphérique comme spécifié dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-x-device-info">X-Device-Info</a>.
         <br/><br/>
         <b>Fortement recommandé</b> à utiliser lorsque la plateforme d’appareil de l’application permet de fournir explicitement des valeurs valides.
         <br/><br/>
         Le serveur principal de l’authentification Adobe Pass fusionnera les valeurs définies explicitement avec les valeurs extraites implicitement. Si elles ne sont pas fournies, les valeurs extraites par défaut seront utilisées.
      </td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-LINK</td>
      <td>
         Code de lien qui associe cette requête à un profil authentifié existant. Lorsqu’elle est fournie, la réponse inclut un jeton de service pour l’authentification unique avec le profil qui a généré le code du lien.
         <br/><br/>
         Il est généralement utilisé lorsqu’une application ou un appareil secondaire souhaite se connecter à un profil authentifié à partir d’une application ou d’un appareil principal.
      </td>
      <td>obligatoire si x-sso-id n’est pas fourni</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-ID</td>
      <td>
         Identifiant commun sur lequel l’application demande une authentification unique (SSO).
         <br/><br/>
         Lorsqu’il est fourni, cet identifiant est utilisé pour établir un profil SSO commun à tous les appareils et/ou applications.
      </td>
      <td>obligatoire si x-sso-link n’est pas fourni</td>
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

##### Réponse {#post-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Texte</th>
      <th style="background-color: #EFF2F7;">Description</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Créé</td>
      <td>
        Le jeton de service a été généré et renvoyé dans le corps de la réponse.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Requête incorrecte</td>
      <td>
        La requête n’est pas valide, le client doit la corriger et réessayer. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codes d’erreur améliorés</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non Autorisé</td>
      <td>
        Le jeton d’accès n’est pas valide, le client doit obtenir un nouveau jeton d’accès et réessayer. Pour plus d’informations, consultez la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> Présentation de l’enregistrement client dynamique </a> .
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erreur de serveur interne</td>
      <td>
        Un problème est survenu côté serveur. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codes d’erreur améliorés</a>.
      </td>
   </tr>
</table>

###### Succès {#success-post-service-token}

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
      <td style="background-color: #DEEBFF;">statut</td>
      <td>Etat HTTP (ex : « CREATED »)</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Une signature Web JSON (JWS) codée en Base64 contenant le jeton de service. Ce jeton peut être utilisé dans les appels API suivants pour identifier le profil authentifié et activer la fonctionnalité de connexion unique.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoch ms, ou 0 en erreur</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoch ms, ou 0 en erreur</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

###### Erreur {#error-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Etat</td>
      <td>400, 401, 500</td>
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
      <td>Le corps de la réponse peut fournir des informations d’erreur supplémentaires conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Codes d’erreur améliorés </a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

## Exemples {#samples-post-service-token}

### &#x200B;1. Demander un nouveau jeton de service (avec ID SSO)

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-ID: <sso_id>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    Accept: application/json
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### &#x200B;2. Demander un nouveau jeton de service (avec lien SSO)

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-LINK: <link_code>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    User-Agent: <user_agent>
    Accept: application/json
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

#### GET - serviceToken {#get-service-token}

##### Requête {#get-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/api/{serviceProvider}/serviceToken</td>
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
      <td>Identifiant du fournisseur pour lequel le jeton est demandé.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisation</td>
      <td>La génération de la payload du jeton porteur est décrite dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Jeton de service AD</td>
      <td>
         Jeton de service obtenu précédemment qui doit être actualisé.
         <br/><br/>
         Ce jeton doit être valide ou avoir expiré récemment pour être éligible à l’actualisation.
      </td>
      <td><i>obligatoire</i></td>
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

##### Réponse {#get-service-token-response}

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
        Le jeton de service a été actualisé avec succès et renvoyé dans le corps de la réponse.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Requête incorrecte</td>
      <td>
        La requête n’est pas valide, le client doit la corriger et réessayer. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codes d’erreur améliorés</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non Autorisé</td>
      <td>
        Le jeton d’accès ou le jeton de service n’est pas valide, le client doit obtenir un nouveau jeton d’accès ou de service et réessayer. Pour plus d’informations, consultez la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> Présentation de l’enregistrement client dynamique </a> .
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erreur de serveur interne</td>
      <td>
        Un problème est survenu côté serveur. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codes d’erreur améliorés</a>.
      </td>
   </tr>
</table>

###### Succès {#success-get-service-token}

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
      <td style="background-color: #DEEBFF;">statut</td>
      <td>État HTTP (par exemple, « OK »)</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Une signature Web JSON (JWS) codée en Base64 contenant le jeton de service actualisé. Ce jeton peut être utilisé dans les appels API suivants pour identifier le profil authentifié et activer la fonctionnalité de connexion unique.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoch ms, ou 0 en erreur</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoch ms, ou 0 en erreur</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

###### Erreur {#error-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Etat</td>
      <td>400, 401, 500</td>
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
      <td>Le corps de la réponse peut fournir des informations d’erreur supplémentaires conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Codes d’erreur améliorés </a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

## Exemples {#samples-get-service-token}

### &#x200B;1. Demande d’actualisation d’un jeton de service

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### API Link {#link-api}

#### Description {#link-description}

L’API Link peut être utilisée pour demander un code de lien (ou un code QR) qui peut activer l’authentification unique (SSO) entre plusieurs applications ou appareils. Ce code de lien permet aux utilisateurs de connecter une nouvelle application ou un nouvel appareil à un profil authentifié existant (profil SSO), offrant ainsi une expérience SSO transparente entre les applications ou appareils.

L’API Link nécessite la fourniture d’un jeton de service valide dans l’en-tête AD-Service-Token .

Le code de lien généré est généralement affiché aux utilisateurs sur une application ou un périphérique principal et saisi sur une application ou un périphérique secondaire pour établir la connexion SSO. Le code de lien a une période de validité limitée (généralement de 5 à 30 minutes) et est destiné à une utilisation unique.

Si la requête de l’API Link n’a pas pu être traitée en raison d’une erreur des services d’authentification Adobe Pass, des informations d’erreur supplémentaires sont incluses dans le résultat de la réponse de l’API Link.

#### POST - lien {#post-link}

##### Requête {#post-link-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/api/{serviceProvider}/link</td>
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
      <td>Identifiant du fournisseur pour lequel le jeton est demandé.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisation</td>
      <td>La génération de la payload du jeton porteur est décrite dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>La génération de la payload de l’identifiant d’appareil est décrite dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Jeton de service AD</td>
      <td>
         La génération du jeton de service est décrite dans la documentation de l’API Jeton de service .
         <br/><br/>
         Ce jeton de service identifie le profil authentifié pour lequel le code de lien sera généré.
      </td>
      <td><i>obligatoire</i></td>
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

##### Réponse {#post-link-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Texte</th>
      <th style="background-color: #EFF2F7;">Description</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Créé</td>
      <td>
        Le code du lien a été généré avec succès et renvoyé dans le corps de la réponse.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Requête incorrecte</td>
      <td>
        La requête n’est pas valide, le client doit la corriger et réessayer. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codes d’erreur améliorés</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non Autorisé</td>
      <td>
        Le jeton d’accès n’est pas valide, le client doit obtenir un nouveau jeton d’accès et réessayer. Pour plus d’informations, consultez la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> Présentation de l’enregistrement client dynamique </a> .
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erreur de serveur interne</td>
      <td>
        Un problème est survenu côté serveur. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codes d’erreur améliorés</a>.
      </td>
   </tr>
</table>

###### Succès {#success-post-link}

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
      <td style="background-color: #DEEBFF;">statut</td>
      <td>Etat HTTP (ex : « CREATED »)</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">lien</td>
      <td>Code numérique ou alphanumérique court pouvant être utilisé sur une application ou un appareil secondaire pour établir une connexion SSO.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Date et heure (en millisecondes depuis l’époque Unix) auxquelles le code du lien devient valide.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Date et heure (en millisecondes depuis l’époque Unix) auxquelles le code du lien expire.</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

###### Erreur {#error-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Etat</td>
      <td>400, 401, 500</td>
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
      <td>Le corps de la réponse peut fournir des informations d’erreur supplémentaires conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Codes d’erreur améliorés </a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

## Exemples {#samples-post-link}

### &#x200B;1. Demander un code de lien pour un profil authentifié existant

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /api/{serviceProvider}/link HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{            
   "status": "CREATED",
   "code": "123456",
   "notBefore": 1623840000000,
   "notAfter": 1623842700000
}
```

>[!ENDTABS]

### Dissocier l’API {#unlink-api}

#### Description {#unlink-description}

L’API Unlink peut être utilisée pour demander la suppression d’un ou de plusieurs appareils d’un profil authentifié (profil SSO). Cette API permet aux utilisateurs de déconnecter des appareils de leur configuration SSO, ce qui permet de contrôler quels appareils ont accès à leur profil authentifié.

>[!WARNING]
>
>L’API Unlink nécessite la fourniture d’un jeton de service valide dans l’en-tête AD-Service-Token .

Si la demande d’API Unlink n’a pas pu être traitée en raison d’une erreur des services d’authentification Adobe Pass, des informations d’erreur supplémentaires sont incluses dans le résultat de la réponse de l’API Unlink.

#### POST - unlink {#post-unlink}

##### Requête {#post-unlink-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/api/{serviceProvider}/unlink</td>
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
      <td>Identifiant du fournisseur de services.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Paramètres du corps</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">appareils</td>
      <td>
         Tableau d’identifiants d’appareil à dissocier.
         <br/><br/>
         Exemple :<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
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
      <td>La génération de la payload du jeton porteur est décrite dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Type de média accepté pour les ressources en cours d’envoi.
         <br/><br/>
         Il doit s’agir d’application/json.
      </td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>La génération de la payload de l’identifiant d’appareil est décrite dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Jeton de service AD</td>
      <td>
         La génération du jeton de service est décrite dans la documentation de l’API Jeton de service .
         <br/><br/>
         Ce jeton de service identifie le profil authentifié pour lequel les appareils seront dissociés.
      </td>
      <td><i>obligatoire</i></td>
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

##### Réponse {#post-unlink-response}

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
        La dissociation du ou des périphériques demandés de la configuration de l'authentification unique a été effectuée.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Requête incorrecte</td>
      <td>
        La requête n’est pas valide, le client doit la corriger et réessayer. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codes d’erreur améliorés</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non Autorisé</td>
      <td>
        Le jeton d’accès n’est pas valide, le client doit obtenir un nouveau jeton d’accès et réessayer. Pour plus d’informations, consultez la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> Présentation de l’enregistrement client dynamique </a> .
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Méthode Non Autorisée</td>
      <td>
        La méthode HTTP n’est pas valide, le client doit utiliser une méthode HTTP autorisée pour la ressource demandée et réessayer.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erreur de serveur interne</td>
      <td>
        Un problème est survenu côté serveur. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codes d’erreur améliorés</a>.
      </td>
   </tr>
</table>

###### Succès {#success-post-unlink}

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
      <td style="background-color: #DEEBFF;">statut</td>
      <td>Informations sur le résultat de l'opération : « OK »</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">unlinkedDevices</td>
      <td>
         Liste des appareils dont la liaison a été annulée.
         <br/><br/>
         Exemple :<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

###### Erreur {#error-post-unlink}

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
      <td>Le corps de la réponse peut fournir des informations d’erreur supplémentaires conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Codes d’erreur améliorés </a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

## Exemples {#samples-post-unlink}

### &#x200B;1. Demande de dissociation des appareils d’un profil SSO

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### &#x200B;2. Demande de dissociation partielle des appareils

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "unknowndevice",
    "deviceid3"
  ]
}
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### API de liste {#list-api}

#### Description {#list-description}

L’API List peut être utilisée pour obtenir une liste des appareils actuellement connectés à un profil authentifié (profil SSO). Cette API permet aux utilisateurs et aux applications d’afficher les appareils qui font partie de leur configuration SSO, offrant ainsi une visibilité et des fonctionnalités de gestion pour leur expérience authentifiée sur plusieurs appareils.

>[!WARNING]
>
>L’API List nécessite la fourniture d’un jeton de service valide dans l’en-tête AD-Service-Token .

L’API List renvoie des détails sur chaque appareil dans le profil authentifié (profil SSO), y compris des identifiants d’appareil et des métadonnées qui peuvent aider les utilisateurs à reconnaître leurs appareils. Ces informations peuvent être utilisées pour aider les utilisateurs à prendre des décisions éclairées concernant les appareils qui doivent rester dans leur configuration SSO.

Si la requête de l’API List n’a pas pu être traitée en raison d’une erreur des services d’authentification Adobe Pass, des informations d’erreur supplémentaires sont incluses dans le résultat de la réponse de l’API List.

#### GET - liste {#get-list}

##### Requête {#get-list-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/api/{serviceProvider}/list</td>
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
      <td>Identifiant du fournisseur de services.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisation</td>
      <td>La génération de la payload du jeton porteur est décrite dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>La génération de la payload de l’identifiant d’appareil est décrite dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Jeton de service AD</td>
      <td>
         La génération du jeton de service est décrite dans la documentation de l’API Jeton de service .
         <br/><br/>
         Ce jeton de service identifie le profil authentifié pour lequel la liste d’appareils sera récupérée.
      </td>
      <td><i>obligatoire</i></td>
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

##### Réponse {#get-list-response}

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
        La liste des périphériques de la configuration SSO a été récupérée avec succès et renvoyée dans le corps de la réponse.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Requête incorrecte</td>
      <td>
        La requête n’est pas valide, le client doit la corriger et réessayer. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codes d’erreur améliorés</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non Autorisé</td>
      <td>
        Le jeton d’accès n’est pas valide, le client doit obtenir un nouveau jeton d’accès et réessayer. Pour plus d’informations, consultez la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> Présentation de l’enregistrement client dynamique </a> .
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Méthode Non Autorisée</td>
      <td>
        La méthode HTTP n’est pas valide, le client doit utiliser une méthode HTTP autorisée pour la ressource demandée et réessayer.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erreur de serveur interne</td>
      <td>
        Un problème est survenu côté serveur. Le corps de la réponse peut contenir des informations d’erreur conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Codes d’erreur améliorés</a>.
      </td>
   </tr>
</table>

###### Succès {#success-get-list}

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
      <td style="background-color: #DEEBFF;">appareils</td>
      <td>
         JSON contenant un mappage de paires clé-valeur.
         <br/><br/>
         <b>Key:</b> deviceId : payload de l’identifiant d’appareil comme décrit dans la documentation d’en-tête <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>
         <br/><br/>
         <b>Value:</b> attributes - JSON contenant un mappage des attributs de métadonnées de l’appareil, notamment :
         <ul>
            <li>type d’appareil</li>
            <li>plate-forme</li>
            <li>agent utilisateur</li>
            <li>autres métadonnées pertinentes qui permettent d’identifier l’appareil</li>
         </ul>
         Les valeurs des attributs peuvent être simples (chaîne, entier, booléen, etc.)
      </td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

###### Erreur {#error-get-list}

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
      <td>Le corps de la réponse peut fournir des informations d’erreur supplémentaires conformes à la documentation <a href="https://experienceleague.adobe.com/fr/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> Codes d’erreur améliorés </a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

## Exemples {#samples-get-list}

### &#x200B;1. Requête pour répertorier les appareils dans un profil SSO

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {
    "deviceid1": {
      "deviceType": "smartTV",
      "model": "Samsung",
      "os": "Tizen",
      "osVersion": "5.0",
      "lastSeen": 1623840000000,
      "type": "regular"
    },
    "deviceid2": {
      "deviceType": "mobile",
      "model": "iPhone",
      "os": "iOS",
      "osVersion": "14.5",
      "lastSeen": 1623830000000,
      "type": "sso"
    },
    "deviceid3": {
      "deviceType": "tablet",
      "model": "iPad",
      "os": "iPadOS",
      "osVersion": "14.5",
      "lastSeen": 1623820000000,
      "type": "sso"
    }
  }
}
```

>[!ENDTABS]

### &#x200B;2. Demande d’énumération des appareils sans appareils liés

>[!BEGINTABS]

>[!TAB Requête]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Réponse]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {}
}
```

>[!ENDTABS]

## Codes d’erreur {#error-codes}

### Structure de réponse d&#39;erreur {#error-structure}

Toutes les réponses d’erreur incluent les champs suivants :

| Champ | Type | Description |
|:---|:---|:---|
| statut | Entier | Code d’état HTTP (400, 401, 500, etc.) |
| code | String | Code d’erreur lisible par machine |
| message | String | Description lisible par l’utilisateur |
| action | String | Action suggérée pour le client |
| helpUrl | String | Lien vers la documentation |
| trace | String | ID de requête unique (UUID) pour la corrélation |

**Exemple de réponse d’erreur**

```json
{
  "status": "BAD_REQUEST",
  "error": {
    "status": 400,
    "code": "header_missing",
    "message": "Required header is missing",
    "action": "check_headers",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=fr",
    "trace": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```

**Exemple de réponse de succès**

```json
{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIs...",
  "notBefore": 1697500800000,
  "notAfter": 1697587200000
}
```

>[!NOTE]
>
>Les réponses de succès n’incluent PAS de champ d’erreur. Les réponses d’erreur n’incluent PAS les champs de succès.

### Catalogue des codes d’erreur {#error-catalog}

#### Erreurs générales

| code | statut | message | action |
|:---|:---|:---|:---|
| token_invalid | 400 | Le jeton fourni est non valide | get_new_token |
| token_expired | 401 | Le jeton a expiré | get_new_token |
| header_missing | 400 | Un en-tête obligatoire est manquant | check_headers |
| non autorisé | 401 | Accès non autorisé | aucun |
| internal_error | 500 | Une erreur interne s’est produite | aucun |

#### Erreurs de validation

| code | statut | message | action |
|:---|:---|:---|:---|
| request_null | 400 | L’objet de la demande ne peut pas être nul | aucun |

#### Erreurs de jeton de service POST

| code | statut | message | action |
|:---|:---|:---|:---|
| header_missing | 400 | L’en-tête x-sso-id ou x-sso-link est requis pour les requêtes POST | check_headers |
| header_missing | 400 | L’en-tête AP-Device-Identifier est requis pour les requêtes POST | check_headers |

#### Erreurs de jeton de service GET

| code | statut | message | action |
|:---|:---|:---|:---|
| header_missing | 400 | L’en-tête AD-Service-Token est requis pour les requêtes GET. | check_headers |
| header_invalid | 401 | Signature JWT non valide dans AD-Service-Token | get_new_token |
| header_invalid | 401 | Erreur de validation de la signature JWT | get_new_token |
| header_invalid | 401 | L’objet JWT (sous) est manquant ou vide dans AD-Service-Token | get_new_token |
| header_invalid | 401 | Erreur d’extraction de l’objet JWT | get_new_token |

#### Erreurs de validation du lien

| code | statut | message | action |
|:---|:---|:---|:---|
| header_missing | 401 | L’en-tête AD-Service-Token est requis pour les requêtes de lien. | check_headers |
| header_invalid | 401 | Signature JWT non valide dans AD-Service-Token | get_new_token |
| header_invalid | 401 | Erreur de validation de la signature JWT | get_new_token |

#### Annuler le lien des erreurs de validation

| code | statut | message | action |
|:---|:---|:---|:---|
| header_missing | 401 | L’en-tête AD-Service-Token est requis pour les requêtes de dissociation. | check_headers |
| header_invalid | 401 | Signature JWT non valide dans AD-Service-Token | get_new_token |
| header_invalid | 401 | Erreur de validation de la signature JWT | get_new_token |
| header_invalid | 401 | L’objet JWT (sous) est manquant ou vide dans AD-Service-Token | get_new_token |
| request_invalid | 400 | La liste des appareils ne peut pas être nulle ou vide | check_request_body |

#### Liste des erreurs de validation

| code | statut | message | action |
|:---|:---|:---|:---|
| header_missing | 401 | L’en-tête AD-Service-Token est requis pour les requêtes de liste. | check_headers |
| header_invalid | 401 | Signature JWT non valide dans AD-Service-Token | get_new_token |
| header_invalid | 401 | Erreur de validation de la signature JWT | get_new_token |
| header_invalid | 401 | L’objet JWT (sous) est manquant ou vide dans AD-Service-Token | get_new_token |
| header_invalid | 401 | Erreur d’extraction de l’objet JWT | get_new_token |

