---
title: En-tête - AP-TempPass-Identity
description: API REST V2 - En-tête - AP-TempPass-Identity
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 6%

---


# En-tête - AP-TempPass-Identity {#header-ap-temppass-identity}

## Vue d’ensemble {#overview}

L’en-tête de requête <b>AP-TempPass-Identity</b> contient les informations d’identité de l’utilisateur utilisées pour obtenir le TempPass promotionnel.

## Syntaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-TempPass-Identity</b> : &lt;user_identity_information&gt;</td>
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

<b>&lt;user_identity_information></b>

La valeur `Base64-encoded` sur les informations d’identité de l’utilisateur associées à l’utilisateur final auquel un accès temporaire promotionnel doit être accordé.

## Exemples {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

AP-TempPass-Identity: eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
