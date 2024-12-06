---
title: Réinitialiser le transfert temporaire
description: Réinitialiser le transfert temporaire
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# Réinitialiser le transfert temporaire {#reset-temp-pass}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Avant d’utiliser l’API Reset Temp Pass, assurez-vous que les conditions préalables suivantes sont remplies :
>
> * Récupérez les informations d’identification du client comme décrit dans la documentation de l’API [Récupérez les informations d’identification du client](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenez le jeton d’accès comme décrit dans la documentation de l’API [Récupérer le jeton d’accès](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Pour plus d’informations sur la création d’une application enregistrée et le téléchargement de l’instruction logicielle, consultez la documentation [Aperçu de l’enregistrement du client dynamique](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) .

Pour **réinitialiser un Temp Pass spécifique pour un appareil ou tous les appareils**, Adobe Pass Authentication fournit aux programmeurs une API web *publique* :

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **Protocole :** HTTPS
* **Hôte :**
   * Version : mgmt.auth.adobe.com
   * Préqualification - mgmt-prequal.auth.adobe.com
* **Chemin d’accès :** /reset-tempass/v3/reset
* **Paramètres de requête :** device_id(lorsqu’aucune valeur n’est fournie, par défaut, tous), requestor_id (obligatoire), mvpd_id (obligatoire), appId, deviceUser, environment
* **En-têtes :** Autorisation : porteur &lt;access_token_here>
* **Réponse :**
   * Succès - HTTP 204
   * Échec : xw
      * HTTP 400 pour une requête incorrecte
      * HTTP 401 si l’accès est refusé. Le client DOIT demander un nouveau access_token.
      * HTTP 403 si l’ID client n’est plus autorisé à effectuer des requêtes. De nouvelles informations d’identification client doivent être générées.


Par exemple :

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

Pour **réinitialiser le transfert temporaire flexible pour une clé générique ou toutes les clés**, Adobe Pass Authentication fournit aux programmeurs une API web *publique* :

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **Protocole :** HTTPS
* **Hôte :**
   * Version : mgmt.auth.adobe.com
   * Préqualification - mgmt-prequal.auth.adobe.com
* **Chemin d’accès :** /reset-tempass/v3/reset/generic
* **Paramètres de requête :** clé(si aucune valeur n’est fournie, la valeur par défaut est all), requestor_id (obligatoire), mvpd_id (obligatoire), environnement
* **En-têtes :** Autorisation : porteur &lt;access_token_here>
* **Réponse :**
   * Succès - HTTP 204
   * Échec :
      * HTTP 400 pour une requête incorrecte
      * HTTP 401 si l’accès est refusé. Le client DOIT demander un nouveau access_token.
      * HTTP 403 si l’ID client n’est plus autorisé à effectuer des requêtes. De nouvelles informations d’identification client doivent être générées.


Par exemple, définissez la *Clé générique* sur un hachage d’adresse électronique (pour
Les MVPD de transmission de température qui prennent en charge ce type de fonctionnalités) généreront le
appel HTTP suivant (le courriel est `user@domain.com` son SHA-256
hash est `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`) :

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
