---
title: En-tête - AP-Partner-Framework-Status
description: API REST V2 - En-tête - AP-Partner-Framework-Status
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 1%

---


# En-tête - AP-Partner-Framework-Status {#header-ap-partner-framework-status}

## Vue d’ensemble {#overview}

L’en-tête de requête <b>AP-Partner-Framework-Status</b> contient des informations d’état obtenues à partir d’un framework de partenaire afin d’obtenir une authentification unique (SSO).

## Syntaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status{1: &lt;partner_framework_status_information&gt;</b></td>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         Il s’agit d’un attribut obligatoire.
         <br/><br/>
         Les informations d’état des autorisations utilisateur sont renvoyées par la structure du partenaire et traitées par l’application.
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
                  Il s'agit d'une énumération avec les valeurs possibles suivantes :
                  <br/>
                  <ul>
                     <li>grant : l’utilisateur a autorisé l’application à accéder aux informations d’abonnement.</li>
                     <li>deny : l’utilisateur a refusé la demande d’accès aux informations d’abonnement.</li>
                     <li>En attente : l’utilisateur n’a pas encore choisi d’autoriser l’application à accéder aux informations d’abonnement.</li>
                     <li>notDetermined : l’application n’est pas autorisée à accéder aux informations d’abonnement.</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>erreur</td>
               <td>
                  Il s’agit d’un attribut facultatif.
                  <br/><br/>
                  Vous pouvez l’utiliser pour transmettre l’erreur de framework du partenaire au cas où l’une d’elles serait déclenchée lors de l’interrogation des informations d’état des autorisations utilisateur.
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
                        <td>Chaîne qui identifie de manière unique l’erreur telle que définie par la structure du partenaire.</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>Chaîne contenant la description de l’erreur telle que définie par la structure du partenaire.</td>
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
         Les informations d’état de connexion du fournisseur renvoyées par la structure du partenaire et traitées par l’application.
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
                  Il s’agit du mappingId qui identifie le MVPD utilisé pendant le flux d’authentification au niveau de la structure du partenaire.
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  Il s’agit d’un attribut obligatoire.
                  <br/><br/>
                  Il s’agit de la date d’expiration du profil utilisateur authentifié, au cas où l’utilisateur aurait réussi à se connecter à l’aide d’un MVPD pris en charge au niveau de la structure du partenaire.
               </td>
            </tr>
            <tr>
               <td>erreur</td>
               <td>
                  Il s’agit d’un attribut facultatif.
                  <br/><br/>
                  Vous pouvez l’utiliser pour transmettre l’erreur de framework du partenaire au cas où l’une d’elles serait déclenchée lors de l’interrogation des informations d’état de connexion du fournisseur.
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
                        <td>Chaîne qui identifie de manière unique l’erreur telle que définie par la structure du partenaire.</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>Chaîne contenant la description de l’erreur telle que définie par la structure du partenaire.</td>
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
