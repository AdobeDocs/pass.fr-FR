---
title: 'En-Tête : X-Roku-Reserved-Roku-Connect-Token'
description: API REST V2 - En-tête - X-Roku-Reserved-Roku-Connect-Token
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 2%

---

# En-Tête : X-Roku-Reserved-Roku-Connect-Token {#header-x-roku-reserved-roku-connect-token}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b>X-Roku-Reserved-Roku-Connect-Token</b> contient l’identifiant unique de plateforme `JWS` ou `JWE` obtenu à partir d’un service d’identités ou d’une bibliothèque s’exécutant en dehors des systèmes d’authentification Adobe Pass.

Cet en-tête est conçu pour être utilisé dans les flux activés pour l’authentification unique (SSO) qui utilisent la méthode d’identification Platform.

Pour plus d’informations sur les flux activés pour l’authentification unique (SSO) utilisant la méthode d’identité de Platform, reportez-vous à la documentation [ Authentification unique à l’aide des flux d’identité de Platform ](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Syntaxe {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Roku-Reserved-Roku-Connect-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

* [Guide pas à pas de Roku SSO (API REST V2)](../../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)
