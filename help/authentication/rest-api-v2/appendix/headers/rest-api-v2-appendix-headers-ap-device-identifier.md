---
title: En-tête - AP-Device-Identifier
description: API REST V2 - En-tête - AP-Device-Identifier
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 3%

---


# En-tête - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b>AP-Device-Identifier</b> contient l’identifiant de l’appareil de diffusion en continu tel qu’il a été créé par l’application cliente.

## Syntaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b> : &lt;type&gt; &lt;identifiant&gt;</td>
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

<b>&lt;type></b>

Type d’identifiant de l’appareil.

Il n’existe qu’un seul type pris en charge, comme illustré ci-dessous.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Type</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>empreinte</td>
      <td>L’identifiant de l’appareil est constitué d’un identifiant opaque et est créé par l’application cliente.</td>
   </tr>
</table>


<b>&lt;identifier></b>

La valeur `Base64-encoded` de l’identifiant de l’appareil.

## Exemple {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```
