---
title: En-tête - Adobe-Objet-Jeton
description: API REST V2 - En-tête - Adobe-Objet-Jeton
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: e5ef8c0cba636ac4d2bda1abe0e121d0ecc1b795
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 2%

---

# En-tête - Adobe-Objet-Jeton {#header-adobe-subject-token}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b> Adobe-Subject-Token</b> contient l’identifiant unique de la plateforme, `JWS` ou `JWE`, obtenu à partir d’un service d’identité ou d’une bibliothèque s’exécutant en dehors des systèmes d’authentification Adobe Pass.

Cet en-tête est conçu pour être utilisé dans les flux activés pour l’authentification unique (SSO) qui utilisent la méthode d’identification de plateforme.

Pour plus d’informations sur les flux activés pour l’authentification unique (SSO) qui utilisent la méthode d’identification de plateforme, reportez-vous à la documentation [Authentification unique à l’aide des flux d’identités de plateforme](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Syntaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe-Subject-Token</b> : &lt;identifiant_plate_unique&gt;</td>
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

Signature Web JSON (`JWS`) ou chiffrement Web JSON (`JWE`) qui est un jeton Web JSON signé ou chiffré (`JWT`) contenant des informations uniques sur l’identifiant de la plateforme.

Elle est disponible pour les plateformes suivantes :

* [Guide pas à pas Amazon SSO (API REST V2)](../../../single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## Exemples {#examples}

Reportez-vous aux exemples décrits pour les plateformes suivantes :

* [Guide pas à pas Amazon SSO (API REST V2)](../../../single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
