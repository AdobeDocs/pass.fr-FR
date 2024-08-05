---
title: En-tête - AP-TempPass-Identity
description: API REST V2 - En-tête - AP-TempPass-Identity
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 4%

---


# En-tête - AP-TempPass-Identity {#header-ap-temppass-identity}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

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
