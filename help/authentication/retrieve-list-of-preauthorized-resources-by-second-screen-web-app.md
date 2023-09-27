---
title: Récupération de la liste des ressources préautorisées par une application web de deuxième écran
description: Récupération de la liste des ressources préautorisées par une application web de deuxième écran
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 0%

---

# Récupération de la liste des ressources préautorisées par une application web de deuxième écran {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Points de terminaison de l’API REST {#clientless-endpoints}

&lt;reggie_fqdn>:

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Demande d’authentification Adobe Pass pour obtenir la liste des ressources préautorisées.

Il existe deux ensembles d’API : un ensemble pour l’application de diffusion en continu ou le service de programmation et un ensemble pour l’application web du deuxième écran. Cette page décrit l’API de l’application AuthN.


| Point d’entrée | Appelé  </br>Par | Entrée   </br>Paramètres | HTTP  </br>Méthode | Réponse | HTTP  </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/preautoriser/{code d’enregistrement} | Module AuthN | 1. code d’enregistrement  </br>    (composant Chemin)</br>2.  demandeur (obligatoire)</br>3.  liste de ressources (obligatoire) | GET | XML ou JSON contenant des décisions de préautorisation ou des détails d’erreur individuels. Voir les exemples ci-dessous. | 200 - Succès</br></br>400 - Mauvaise requête</br></br>401 - Non autorisé</br></br>405 - Méthode non autorisée  </br></br>412 - Echec de la précondition</br></br>500 - Erreur interne du serveur |



| Paramètre d’entrée | Description |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| code d&#39;enregistrement | La valeur du code d’enregistrement fournie par l’utilisateur au début du flux d’authentification. |
| demandeur | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| liste de ressources | Chaîne contenant une liste délimitée par des virgules de resourceIds qui identifie le contenu pouvant être accessible à un utilisateur et reconnu par les points de terminaison d’autorisation MVPD. |


### Exemple de réponse {#sample-response}

**XML :**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4`
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
    <resource>
        <id>TestStream1</id>
        <authorized>true</authorized>
    </resource>
    <resource>
        <id>TestStream2</id>
        <authorized>false</authorized>  
        <error>
            <status>403</status>
            <code>authorization_denied_by_mvpd</code>
            <message>User not authorized</message>
            <details>Your subscription package does not include the "TestStream3" channel.</details>
            <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
            <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
            <action>none</action>
        </error>
    <resource>
</resources>
```

**JSON :**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
