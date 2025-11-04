---
title: En-tête - AP-Visitor-Identifier
description: API REST V2 - En-tête - AP-Visitor-Identifier
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
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

Pour plus d’informations sur l’utilisation de l’ECID dans l’authentification Adobe Pass, reportez-vous à la documentation [&#x200B; Utilisation de l’Experience Cloud ID dans l’authentification Adobe Pass &#x200B;](/help/authentication/integration-guide-programmers/features-standard/analytics/exp-cloud-id-authn.md).

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
