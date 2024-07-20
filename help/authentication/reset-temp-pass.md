---
title: Réinitialiser le transfert temporaire
description: Réinitialiser le transfert temporaire
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 28d432891b7d7855e83830f775164973e81241fc
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# Réinitialiser le transfert temporaire {#reset-temp-pass}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.
>
>Pour utiliser l’API Reset Temp Pass, vous devez :
>- demandez à l’équipe de support un relevé logiciel pour votre application enregistrée.
>- Obtention d’un jeton d’accès basé sur l’[ enregistrement du client dynamique](dynamic-client-registration.md)
> 

Pour **réinitialiser un Temp Pass** spécifique, Adobe Pass Authentication fournit aux programmeurs une API web *public* :

- **Environnement :** spécifie le point de terminaison du serveur de passe de télévision payante Adobe qui recevra l’appel réseau de réinitialisation de la passe temporaire. Valeurs possibles : **Préqual** (*mgmt-prequal.auth.adobe.com*), **Version** (*mgmt.auth.adobe.com*) ou **Personnalisé** (réservé pour les tests internes d’Adobe).
- **Jeton d’accès OAuth2 :** le jeton OAuth2 est nécessaire pour autoriser le programmeur pour l’authentification payante Adobe. Un tel jeton peut être obtenu à partir de l’[enregistrement du client dynamique](dynamic-client-registration.md).
- **ID de transmission temporaire :** l’identifiant unique du MVPD de transmission temporaire à réinitialiser.(un programmeur peut utiliser plusieurs MVPD de transmission de température et souhaite réinitialiser un MVPD spécifique)
- **Clé générique :** certains MVPD de transmission de température (c’est-à-dire [pass temporaire de promotion](promotional-temp-pass.md)).

Tous les paramètres ci-dessus (à l’exception de la *clé générique*) sont obligatoires. Voici un exemple de paramètres et de l’appel réseau associé (l’exemple se présente sous la forme d’une commande *curl*) :

- **Environnement :** Version (*mgmt.auth.adobe.com*)
- **Jeton d’accès OAuth2 :** &lt;jeton_accès> à partir de l’ [enregistrement du client dynamique](dynamic-client-registration.md)
- **Identifiant du programmeur :** REF
- **ID de transmission temporaire :** TempPassREF
- **Clé générique :** null (aucune valeur fournie)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

une requête HTTP DELETE sera envoyée au point de terminaison **/reset**, en transmettant le *jeton d’accès OAuth2* dans l’en-tête d’autorisation et les paramètres *ID de périphérique*, *ID de demandeur* et *ID de transmission temporaire (ID MVPD)* comme paramètres.

Si le programmeur fournit une valeur pour la *Clé générique*, un autre appel HTTP sera effectué (cette fois au point de terminaison **/reset/generic**), en transmettant la *Clé générique* à l’intérieur du paramètre de requête *key*.

Par exemple, définissez la *Clé générique* sur un hachage d’adresse électronique (pour
Les MVPD de transmission de température qui prennent en charge ce type de fonctionnalités) généreront le
appel HTTP suivant (le courriel est `user@domain.com` son SHA-256
hash est `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`) :

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Pour **réinitialiser une transmission temporaire spécifique pour tous les appareils**, l’authentification Adobe Pass fournit aux programmeurs une API web *publique* :

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>L’URL ci-dessus remplace l’API de réinitialisation précédente. L’ancienne API reset (v1) n’est plus prise en charge.

- **Protocole :** HTTPS
- **Hôte :**
   - Version : mgmt.auth.adobe.com
   - Préqualification - mgmt-prequal.auth.adobe.com
- **Chemin d’accès :** /reset-tempass/v3/reset
- **Paramètres de requête :** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **En-têtes :** Autorisation : porteur &lt;access_token_here>
- **Réponse :**
   - Succès - HTTP 204
   - Échec :
      - HTTP 400 pour une requête incorrecte
      - HTTP 401 si l’accès est refusé. Le client DOIT demander un nouveau access_token.
      - HTTP 403 si l’ID client n’est plus autorisé à effectuer des requêtes. De nouvelles informations d’identification client doivent être générées.


Par exemple :

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
