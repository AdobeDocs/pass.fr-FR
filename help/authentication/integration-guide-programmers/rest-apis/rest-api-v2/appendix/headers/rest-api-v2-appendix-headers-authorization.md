---
title: En-tête - Autorisation
description: API REST V2 - En-tête - Autorisation
exl-id: 86917d7e-ffd9-4d34-8f9c-5a50083f85e6
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 3%

---


# En-tête - Autorisation {#header-authorization}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b>Authorization</b> contient le jeton d’accès `Bearer` requis par l’application cliente pour accéder aux API protégées d’Adobe Pass.

Pour plus d’informations sur le mécanisme d’accès aux API protégées d’Adobe Pass, reportez-vous à la documentation [ Présentation de l’enregistrement client dynamique ](../../../rest-api-dcr/dynamic-client-registration-overview.md).

## Syntaxe {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Autorisation</b> : porteur &lt;access_token&gt;</td>
   </tr>
   <tr>
      <td>Type d’en-tête</td>
      <td>En-tête de requête</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Oui</td>
   </tr>
</table>

## Directives {#directives}

<b>&lt;access_token></b>

La valeur du jeton d’accès est une valeur opaque dont la durée de vie est limitée (par exemple, 24 heures) et qui doit être obtenue auprès d’Adobe Pass, comme décrit dans la documentation de l’API [Récupérer le jeton d’accès](../../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

## Exemples {#examples}

```JSON
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0NmY0MGZiMy01NmJkLTQyYTktOTExYS02YmZmNmEyZmY0
                      MDciLCJuYmYiOjE3MjM1NjE4ODUsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpO
                      mNsaWVudDp2MiIsImV4cCI6MTcyMzU4MzQ4NSwiaWF0IjoxNzIzNTYxODg1fQ.aZUZqwN2fCqNXgX
                      SdKFG9_HcqHjha66G6HmsfTJYcZc12iuLxMu7TT7MbhWVz3kW1jRqgJv8PHhrFSBL5_dgJ1PRSuDg
                      97ZK1secgMKwk46vKZVdtx7LF5t3jGVzQTwN4RqChqyvkW2o67KxVk5xarwJtwB2fwhX_732CYDcv
                      1gWOTLx4xyH5IVvg-P_aImyveG0D-x65I2nOKXaROVvv-kYE6B9OQv_-JBGj72R_yS2AyJQC0R_im
                      0h5S4YvL-c2UZrYK7pvdZq-xAj0uW1wad7PLZjl8yL5CWUz9vzQk2Cmj8adsydjb0u0P3aFrJ0HE9
                      ISqtRvjf4plR1TGWgw6
```
