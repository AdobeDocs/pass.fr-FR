---
title: Récupération de la liste des ressources préautorisées par l’application web du deuxième écran
description: Récupération de la liste des ressources préautorisées par l’application web du deuxième écran
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: 1c357b918fa4f6d4b92a9055de018c55ee5861e0
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# (Hérité) Récupérer la liste des ressources préautorisées par l’application web du deuxième écran {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Points d’entrée de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Description {#description}

Une requête à l’Authentification Adobe Pass pour obtenir la liste des ressources préautorisées.

Il existe deux ensembles d’API : l’un pour l’application de diffusion en continu ou le service de programmation, l’autre pour la deuxième application web Screens. Cette page décrit l’API de l’application AuthN.


| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize/{registration code} | Module AuthN | &#x200B;1. code d’enregistrement </br>    (Composant Chemin d’accès)</br>2.  demandeur (obligatoire)</br>3.  resource (obligatoire) | GET | XML ou JSON contenant des décisions de pré-autorisation individuelles ou des détails d’erreur. Voir les exemples ci-dessous. | 200 - Succès </br></br> 400 - Requête incorrecte </br></br> 401 - Non autorisé </br></br> 405 - Méthode non autorisée </br></br>412 - Échec de la condition préalable </br></br> 500 - Erreur de serveur interne |



| Paramètre d’entrée | Description |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| code d&#39;enregistrement | Valeur du code d’enregistrement fournie par l’utilisateur au début du flux d’authentification. |
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| ressource | Chaîne contenant une liste délimitée par des virgules de resourceId qui identifie le contenu susceptible d’être accessible à un utilisateur ou une utilisatrice et qui est reconnue par les points d’entrée d’autorisation MVPD. |


### Exemple de réponse {#sample-response}

**XML:**

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

**JSON:**

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
