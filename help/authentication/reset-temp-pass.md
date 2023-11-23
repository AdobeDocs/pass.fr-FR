---
title: Réinitialiser le transfert temporaire
description: Réinitialiser le transfert temporaire
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Réinitialiser le transfert temporaire {#reset-temp-pass}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.
>
>Pour utiliser l’API Reset Temp Pass, vous devez :
>- demandez à l’équipe de support un relevé logiciel pour votre application enregistrée.
>- obtenir un jeton d’accès en fonction de [Enregistrement du client dynamique](dynamic-client-registration.md)
> 

Pour **réinitialisation d’une transmission temporaire spécifique**, l’authentification Adobe Pass fournit aux programmeurs une *public* API web :

- **Environnement :** indique le point d’entrée du serveur de passe de la télévision payante Adobe qui recevra l’appel réseau de réinitialisation de la transmission temporaire. Valeurs possibles : **Préqual** (*mgmt-prequal.auth.adobe.com*), **Version** (*mgmt.auth.adobe.com*) ou **Personnalisé** (réservé aux tests internes d’Adobe).
- **Jeton d’accès OAuth2 :** le jeton OAuth2 est nécessaire pour autoriser le programmeur pour l’authentification payante par Adobe. Un tel jeton peut être obtenu à partir de [Enregistrement du client dynamique](dynamic-client-registration.md).
- **ID de transmission temporaire :** l’identifiant unique du MVPD de transmission temporaire à réinitialiser.(un programmeur peut utiliser plusieurs MVPD de transmission de température et souhaite réinitialiser un MVPD spécifique)
- **Clé générique :** quelques MVPD de transmission temporaire (c.-à-d. [Passe temporaire promotionnelle](promotional-temp-pass.md)).

Tous les paramètres ci-dessus (sauf le *Clé générique*) sont obligatoires. Voici un exemple de paramètres et de l’appel réseau associé (l’exemple se présente sous la forme d’une commande *curl*) :

- **Environnement :** Version (*mgmt.auth.adobe.com*)
- **Jeton d’accès OAuth2 :** &lt;access_token> de [Enregistrement du client dynamique](dynamic-client-registration.md)
- **Identifiant du programmeur :** REF
- **ID de transmission temporaire :** TempPassREF
- **Clé générique :** null (aucune valeur fournie)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

une requête HTTP DELETE sera envoyée à la variable **/reset** point d’entrée, transmission de *Jeton d’accès OAuth2* dans l’en-tête d’autorisation et dans la variable *ID de périphérique*, *Identifiant du demandeur* et *ID de transfert temporaire (MVPD ID)* comme paramètres.

Si le programmeur fournit une valeur pour la variable *Clé générique*, un autre appel HTTP sera effectué (cette fois à la fonction **/reset/generic** (point de terminaison), transmission de la variable *Clé générique* dans la variable *key* paramètre de requête .

Par exemple, la définition de la variable *Clé générique* à un hachage d’adresse électronique (pour les MVPD de transmission temporaire qui prennent en charge ce type de fonctionnalités) générera l’appel HTTP suivant (le courriel est `user@domain.com` son hachage SHA-256 est `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`) :

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Pour **réinitialisation d’une transmission temporaire spécifique pour tous les appareils**, l’authentification Adobe Pass fournit aux programmeurs une *public* API web :

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>L’URL ci-dessus remplace l’API de réinitialisation précédente. L’ancienne API reset (v1) n’est plus prise en charge.

- **Protocole :** HTTPS
- **Hôte :**
   - Version : mgmt.auth.adobe.com
   - Préqualification - mgmt-prequal.auth.adobe.com
- **Chemin :** /resettempass/v3/reset
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
