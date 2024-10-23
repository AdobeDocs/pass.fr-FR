---
title: Page d’enregistrement
description: Page d’enregistrement
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 3c44f1dfbbb5b9ec31f13e267dc691e14dd2ddfa
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 1%

---

# Page d’enregistrement {#registration-page}

## Points de terminaison de l’API REST {#clientless-endpoints}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par le [mécanisme de limitation](/help/authentication/throttling-mechanism.md)

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## Description {#create-reg-code-svc}

Renvoie le code d’enregistrement généré de manière aléatoire et l’URI de page de connexion.

| Point d’entrée | Appelé <br> | Entrée   <br>Paramètre | Méthode HTTP <br> | Réponse | Réponse HTTP <br> |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>Par exemple :<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Application de diffusion en continu<br>ou<br>Service de programmation | 1. demandeur <br>    (Composant Chemin)<br>2.  deviceId (Hashed)   <br>    (Obligatoire)<br>3.  device_info/X-Device-Info (obligatoire)<br>4.  mvpd (facultatif)<br>5.  ttl (facultatif)<br> | POST | XML ou JSON contenant un code d’enregistrement et des informations ou des détails d’erreur en cas d’échec. Voir les exemples ci-dessous. | 201 |

{style="table-layout:auto"}

| Paramètre d’entrée | Type | Description |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autorisation | Valeur d’en-tête <br> : porteur &lt;access_token> | Jeton d’accès DCR |
| Accepter | Valeur d’en-tête <br> : application/json | indiquer le type de contenu que le client doit être en mesure de comprendre ; |
| demandeur | Paramètre de requête | Identifiant du demandeur du programmeur pour lequel cette opération est valide. |
| deviceId | Paramètre de requête | Octets d’identifiant de l’appareil. |
| device_info/<br>X-Device-Info | device_info: Body <br> X-Device-Info: Header | Informations sur les périphériques de diffusion en continu.<br>**Remarque** : cette variable peut être transmise à device_info en tant que paramètre d’URL, mais en raison de la taille potentielle de ce paramètre et des limitations de longueur d’une URL de GET, elle doit être transmise sous la forme X-Device-Info dans l’en-tête http. <br>Pour plus d&#39;informations, reportez-vous à la section [Transmission des informations de périphérique et de connexion](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | Paramètre de requête | Identifiant MVPD pour lequel cette opération est valide. |
| ttl | Paramètre de requête | Durée de vie de ce regcode en secondes.<br>**Remarque** : la valeur maximale autorisée pour ttl est de 36 000 secondes (10 heures). Des valeurs plus élevées entraînent une réponse HTTP 400 (mauvaise requête). Si `ttl` n’est pas renseigné, l’authentification Adobe Pass définit une valeur par défaut de 30 minutes. |
| _deviceType_ | Paramètre de requête | Obsolète, ne doit pas être utilisée. |
| _deviceUser_ | Paramètre de requête | Obsolète, ne doit pas être utilisée. |
| _appId_ | Paramètre de requête | Obsolète, ne doit pas être utilisée. |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Adresse IP du périphérique de diffusion en continu**
><br>
>Pour les mises en oeuvre client-serveur, l’adresse IP du périphérique en flux continu est implicitement envoyée avec cet appel.  Pour les implémentations serveur à serveur, où l’appel **regcode** est effectué en tant que service de programmation et non comme périphérique de diffusion en continu, l’en-tête suivant est nécessaire pour transmettre l’adresse IP du périphérique de diffusion en continu :
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>où `<streaming\_device\_ip>` est l’adresse IP publique de l’appareil en flux continu.
><br><br>
>Exemple :<br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### JSON de réponse


#### Exemples JSON de code d’enregistrement

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| Nom de l’élément | Description |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| id | UUID généré par le service de code d’enregistrement |
| code | Code d’enregistrement généré par le service de code d’enregistrement |
| demandeur | Identifiant du demandeur |
| mvpd | Identifiant Mvpd |
| généré | Horodatage de création du code d’enregistrement (en millisecondes depuis le 1er janvier 1970 GMT) |
| expires | Horodatage de l’expiration du code d’enregistrement (en millisecondes depuis le 1er janvier 1970 GMT) |
| deviceId | ID d’appareil unique de base64 |
| info:deviceId | Type de périphérique Base64 |
| info:deviceInfo | Base64 Informations sur les périphériques normalisées reposant sur les informations reçues de User-Agent, X-Device-Info ou device_info |
| info:userAgent | Agent utilisateur envoyé par l’application |
| info:originalUserAgent | Agent utilisateur envoyé par l’application |
| info:authorizationType | OAUTH2 pour les appels utilisant DCR |
| info:sourceApplicationInformation | Informations sur l’application telles que configurées dans DCR |

{style="table-layout:auto"}


<br>

### Exemple de réponse JSON de message d’erreur (#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

