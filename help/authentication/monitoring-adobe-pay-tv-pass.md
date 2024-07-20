---
title: Surveillance de l’authentification Adobe Pass
description: Surveillance de l’authentification Adobe Pass
exl-id: fb000e9d-b5aa-45b1-a914-9e419ec8a4d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Surveillance de l’authentification Adobe Pass {#monitoring-adobe-primetime-authentication}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

Les clients peuvent utiliser [Nagios](http://www.nagios.org) ou d’autres outils pour vérifier si l’authentification Adobe Pass est activée ou désactivée.

## Surveiller les points de terminaison {#monitoring-endpoints}

### Points de terminaison que vous pouvez surveiller {#endpoints-to-monitor}

* Le point de terminaison de configuration pour toutes les plateformes : `https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]` - Il est disponible en HTTP ou HTTPS (selon le choix effectué par le développeur du fournisseur de contenu). Si ce point de terminaison est manquant, cela signifie que votre contenu ne sera pas disponible sur toutes les plateformes et tous les MVPD. Pour l’API REST sans client, nous avons également le point de terminaison suivant : `https://api.auth.adobe.com/adobe-services/config your-config-ID]`.

* Les points de terminaison suivants font partie du SDK Web d’authentification Adobe Pass.  S&#39;il manque, cela signifie que la payTVpass est désactivé pour tous les programmeurs et toutes les propriétés web :

   * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
   * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### Points de terminaison que vous ne devez pas surveiller {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  Vous obtiendrez toujours une erreur 503, car ce point de terminaison nécessite une réponse SAML MVPD.

* Autres points de terminaison de droits - `adobe-services/1.0/authenticate/`, `adobe-services/1.0/deviceShortAuthorize`, `adobe-services/1.0/authorize`

Vous ne pouvez pas surveiller ces points de terminaison car ils ont besoin d’une charge utile pour une réponse appropriée.
