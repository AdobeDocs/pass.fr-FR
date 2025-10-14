---
title: En-tête - Jeton de service AD
description: API REST V2 - En-tête - Jeton de service AD
exl-id: 856f76fc-cde6-4b3f-81f7-deaa0df015dc
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 1%

---

# En-tête - Jeton de service AD {#header-ad-service-token}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b>AD-Service-Token</b> contient l’identifiant utilisateur unique `JWS` obtenu à partir d’un service d’identités s’exécutant en dehors des systèmes d’authentification Adobe Pass.

Cet en-tête est conçu pour être utilisé dans les flux activés pour l’authentification unique (SSO) qui utilisent la méthode du jeton de service.

Pour plus d’informations sur les flux activés pour l’authentification unique (SSO) utilisant la méthode du jeton de service, reportez-vous à la documentation [&#x200B; Authentification unique à l’aide des flux de jeton de service &#x200B;](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

## Syntaxe {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AD-Service-Token</b> : &lt;unique_user_identifier&gt;</td>
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

La signature web JSON (`JWS`) qui est un jeton web JSON signé (`JWT`) contenant des informations d’identifiant utilisateur uniques.

Le `JWT` possède les attributs suivants :

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
      <th style="background-color: #EFF2F7;">Description</th>
   </tr>
   <tr>
      <td>is</td>
      <td>Identifiant unique associé à l’entité qui offre à l’application un service d’identités externe pour obtenir l’authentification unique (SSO).</td>
   </tr>
   <tr>
      <td>sous-marin</td>
      <td>Identifiant unique de l’utilisateur renvoyé par le service d’identités externe.</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>L’audience, qui doit être « Adobe ».</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>L’horodatage émis pour le JWT actuel.</td>
   </tr>
   <tr>
      <td>exp</td>
      <td>Date et heure d’expiration du JWT actuel.</td>
   </tr>
</table>

La `JWT` doit être signée à l’aide de `SHA256withRSA` algorithme.

Le `JWT` doit être signé avec une clé privée, faisant partie d’une paire de clés privées RSA - clé publique gérée par le service d’identités externe.

La clé publique de cette paire doit être transmise à l’authentification Adobe Pass afin de pouvoir reconnaître les jetons `JWT` signés avec la clé privée mentionnée ci-dessus.

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
