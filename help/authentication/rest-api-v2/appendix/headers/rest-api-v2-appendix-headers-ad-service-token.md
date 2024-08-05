---
title: En-tête - AD-Service-Token
description: API REST V2 - En-tête - AD-Service-Token
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 2%

---


# En-tête - AD-Service-Token {#header-ad-service-token}

## Vue d’ensemble {#overview}

L’en-tête de requête <b>AD-Service-Token</b> contient l’identifiant utilisateur unique `JWS` obtenu à partir d’un service d’identité s’exécutant en dehors des systèmes d’authentification Adobe Pass.

Cet en-tête est conçu pour être utilisé dans les flux activés pour l’authentification unique (SSO) qui utilisent la méthode Service Token.

Pour plus d’informations sur les flux activés pour l’authentification unique (SSO) qui utilisent la méthode du jeton de service, reportez-vous à la documentation [Authentification unique à l’aide des flux de jeton de service](../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-service-token-flows.md).

## Syntaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AD-Service-Token</b> : &lt;identifiant_utilisateur_unique&gt;</td>
   </tr>
   <tr>
      <td>Type d’en-tête</td>
      <td>En-tête de requête</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Non</td>
   </tr>
</table>

## Directives {#directives}

<b>unique_user_identifier</b>

Signature Web JSON (`JWS`) qui est un jeton Web JSON signé (`JWT`) contenant des informations d’identifiant utilisateur uniques.

`JWT` possède les attributs suivants :

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
      <th style="background-color: #EFF2F7;">Description</th>
   </tr>
   <tr>
      <td>iss</td>
      <td>Identifiant unique associé à l’entité qui offre à l’application un service d’identité externe pour obtenir l’authentification unique (SSO).</td>
   </tr>
   <tr>
      <td>sub</td>
      <td>Identifiant unique de l’utilisateur tel que renvoyé par le service d’identité externe.</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>L’audience, qui doit être "Adobe".</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>Le émis à l’horodatage pour le JWT actuel.</td>
   </tr>
   <tr>
      <td>exp</td>
      <td>Horodatage d’expiration du JWT actuel.</td>
   </tr>
</table>

`JWT` doit être signé à l’aide de l’algorithme `SHA256withRSA`.

`JWT` doit être signé avec une clé privée, faisant partie d’une paire de clés privées RSA - clé publique gérée par le service d’identité externe.

La clé publique de cette paire doit être transmise à l&#39;authentification Adobe Pass afin de pouvoir reconnaître les jetons `JWT` signés avec la clé privée mentionnée ci-dessus.

## Exemples {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
