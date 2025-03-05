---
title: En-tête - AP-Partner-Framework-Status
description: API REST V2 - En-tête - AP-Partner-Framework-Status
exl-id: f589d948-e23e-43d4-81c2-8db0e7a40e93
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 1%

---

# En-tête - AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b>AP-Partner-Framework-Status</b> contient des informations de statut obtenues à partir d’un framework de partenaire afin d’obtenir l’authentification unique (SSO).

## Syntaxe {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b> : &lt;partner_framework_status_information&gt;</td>
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

<b>&lt;partner_framework_status_information></b>

La valeur `Base64-encoded` de l’élément JSON contenant les attributs suivants :

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         Il s’agit d’un attribut obligatoire.
         <br/><br/>
         Les informations de statut des autorisations utilisateur renvoyées par le framework du partenaire et traitées par l’application.
         <br/><br/>
         Il s’agit d’un élément JSON avec les attributs suivants :
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessStatus</td>
               <td>
                  Il s’agit d’un attribut obligatoire.
                  <br/><br/>
                  Il s’agit d’une énumération avec les valeurs possibles suivantes :
                  <br/>
                  <ul>
                     <li><b>authorized</b><br/>L’utilisateur a autorisé l’application à accéder aux informations d’abonnement.</li>
                     <li><b>refusé</b><br/>L’utilisateur a refusé que l’application accède aux informations d’abonnement.</li>
                     <li><b>pending</b><br/>L'utilisateur n'a pas encore choisi d'autoriser l'application à accéder aux informations d'abonnement.</li>
                     <li><b>notDefined</b><br/>L’application n’est pas autorisée à accéder aux informations d’abonnement.</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>erreur</td>
               <td>
                  Il s’agit d’un attribut facultatif.
                  <br/><br/>
                  Elle peut être utilisée pour transmettre l’erreur de framework du partenaire au cas où l’une d’elles serait déclenchée lors de la demande d’informations sur le statut des autorisations utilisateur.
                  <br/><br/>
                  Il s’agit d’un élément JSON avec les attributs suivants :
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>code</td>
                        <td>Chaîne qui identifie de manière unique l’erreur telle que définie par le framework du partenaire.</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>Chaîne contenant la description de l’erreur telle que définie par le framework du partenaire.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         Il s’agit d’un attribut obligatoire.
         <br/><br/>
         Informations sur le statut de connexion du fournisseur renvoyées par le framework du partenaire et traitées par l’application.
         <br/><br/>
         Il s’agit d’un élément JSON avec les attributs suivants :
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  Il s’agit d’un attribut obligatoire.
                  <br/><br/>
                  Il s’agit de mappingId qui identifie le MVPD utilisé pendant le flux d’authentification au niveau du framework du partenaire.
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  Il s’agit d’un attribut obligatoire.
                  <br/><br/>
                  Il s’agit de la date d’expiration du profil utilisateur authentifié, au cas où l’utilisateur s’est connecté avec succès à l’aide d’un MVPD pris en charge au niveau du framework du partenaire.
               </td>
            </tr>
            <tr>
               <td>erreur</td>
               <td>
                  Il s’agit d’un attribut facultatif.
                  <br/><br/>
                  Elle peut être utilisée pour transmettre l’erreur de framework du partenaire au cas où l’une d’elles serait déclenchée lors de la demande des informations de statut de connexion du fournisseur.
                  <br/><br/>
                  Il s’agit d’un élément JSON avec les attributs suivants :
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>code</td>
                        <td>Chaîne qui identifie de manière unique l’erreur telle que définie par le framework du partenaire.</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>Chaîne contenant la description de l’erreur telle que définie par le framework du partenaire.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## Exemples {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
