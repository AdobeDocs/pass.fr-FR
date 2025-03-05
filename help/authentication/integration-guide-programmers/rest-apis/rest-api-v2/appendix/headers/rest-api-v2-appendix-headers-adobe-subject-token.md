---
title: 'En-Tête : Adobe-Subject-Token.'
description: API REST V2 - En-tête - Adobe-Subject-Token
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 2%

---

# En-Tête : Adobe-Subject-Token. {#header-adobe-subject-token}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b>Adobe-Subject-Token</b> contient l’identifiant unique de plateforme `JWS` ou `JWE` obtenu à partir d’un service d’identités ou d’une bibliothèque s’exécutant en dehors des systèmes d’authentification Adobe Pass.

Cet en-tête est conçu pour être utilisé dans les flux activés pour l’authentification unique (SSO) qui utilisent la méthode d’identification Platform.

Pour plus d’informations sur les flux activés pour l’authentification unique (SSO) utilisant la méthode d’identité de Platform, reportez-vous à la documentation [ Authentification unique à l’aide des flux d’identité de Platform ](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Syntaxe {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Jeton-Objet-Adobe</b> : &lt;unique_platform_identifier&gt;</td>
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

<b>unique_platform_identifier</b>

La signature web JSON (`JWS`) ou le chiffrement web JSON (`JWE`) qui est un jeton web JSON signé ou chiffré (`JWT`) contenant des informations d’identifiant de plateforme uniques.

Cette option est disponible pour les plateformes suivantes :

* [Manuel de l’authentification unique Amazon (API REST V2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## Exemples {#examples}

Consultez les exemples tels que décrits pour les plateformes suivantes :

* [Manuel de l’authentification unique Amazon (API REST V2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
