---
title: En-tête - AP-Visitor-Identifier
description: API REST V2 - En-tête - AP-Visitor-Identifier
exl-id: b4f8e2a1-9c7d-4e3a-8f5b-2d1c6e9a4b7f
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 4%

---


# En-tête - AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b>AP-Visitor-Identifier</b> contient les `ECID` requises par l’application cliente pour identifier de manière unique un visiteur dans les solutions Adobe Experience Cloud.

Pour plus d’informations sur l’utilisation de l’ECID dans l’authentification Adobe Pass, reportez-vous à la documentation [&#x200B; Utilisation de l’Experience Cloud ID dans l’authentification Adobe Pass &#x200B;](../../../../features-premium/analytics/exp-cloud-id-authn.md).

## Syntaxe {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Visitor-Identifier</b> : &lt;visitor_identifier&gt;</td>
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

## Exemples {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
