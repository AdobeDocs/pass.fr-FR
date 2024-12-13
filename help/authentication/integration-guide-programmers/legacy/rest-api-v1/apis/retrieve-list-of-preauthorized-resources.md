---
title: Récupération de la liste des ressources préautorisées
description: Récupération de la liste des ressources préautorisées
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# (Hérité) Récupération de la liste des ressources préautorisées {#retrieve-list-of-preauthorized-resources}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

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

Il existe deux ensembles d’API : l’un pour l’application de diffusion en continu ou le service de programmation, l’autre pour la deuxième application web Screens. Cette page décrit l’API du service de programmation ou de l’application de diffusion en continu.


| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize. | Service de programmation</br></br>ou</br></br>d’application en flux continu | 1. demandeur (obligatoire)</br>2.  deviceId (obligatoire)</br>3.  Liste des ressources (obligatoire)</br>4.  device_info/X-Device-Info (obligatoire)</br>5.  _deviceType_</br> 6  _deviceUser_ (obsolète)</br>7.  _appId_ (obsolète) | GET | XML ou JSON contenant des décisions de pré-autorisation individuelles ou des détails d’erreur. Voir les exemples ci-dessous. | 200 - Succès </br></br> 400 - Requête incorrecte </br></br> 401 - Non autorisé </br></br> 405 - Méthode non autorisée </br></br>412 - Échec de la condition préalable </br></br> 500 - Erreur de serveur interne |


| Paramètre d’entrée | Description |
| --- | --- |
| demandeur | ID de demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Octets d’ID de l’appareil. |
| liste des ressources | Chaîne contenant une liste délimitée par des virgules de resourceId qui identifie le contenu susceptible d’être accessible à un utilisateur ou une utilisatrice et qui est reconnue par les points d’entrée d’autorisation MVPD. |
| device_info/</br></br>X-Device-Info | Informations sur l’appareil de diffusion en continu.</br></br>**Remarque** : cela PEUT être transmis à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de la longueur d’une URL de GET, il DOIT être transmis en tant que X-Device-Info dans l’en-tête http. </br></br>Voir les détails complets dans [Transmettre les informations sur l’appareil et la connexion](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Type d’appareil (par exemple, Roku, PC).</br></br>Si ce paramètre est défini correctement, ESM propose des mesures [ventilées par type d’appareil](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyse puissent être effectués, par exemple, Roku, AppleTV et Xbox.</br></br>Voir, [avantages de l’utilisation du paramètre de type d’appareil sans client dans les mesures de réussite ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Remarque** : le `device_info` remplacera ce paramètre. |
| _deviceUser_ | Identifiant utilisateur de l’appareil. |
| _appId_ | Nom/ID de l’application. </br></br>**Remarque** : device_info remplace ce paramètre. |



### Exemple de réponse {#sample-response}



**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
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
  </resource>
</resources>
```

</br>

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
