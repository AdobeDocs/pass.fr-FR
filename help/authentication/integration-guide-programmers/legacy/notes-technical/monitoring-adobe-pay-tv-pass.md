---
title: Surveillance de l’authentification Adobe Pass
description: Surveillance de l’authentification Adobe Pass
exl-id: fb000e9d-b5aa-45b1-a914-9e419ec8a4d9
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# (Hérité) Surveillance De L’Authentification Adobe Pass {#monitoring-adobe-primetime-authentication}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Introduction {#intro}

Les clients peuvent utiliser [Nagios](http://www.nagios.org) ou d’autres outils pour vérifier si l’authentification Adobe Pass est activée ou désactivée.

## Surveillance des points d’entrée {#monitoring-endpoints}

### Points d’entrée que vous pouvez surveiller {#endpoints-to-monitor}

* Point d’entrée de configuration pour toutes les plateformes : `https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]` - Il est disponible via HTTP ou HTTPS (selon le choix effectué par le développeur du fournisseur de contenu). Si ce point d’entrée est manquant, cela signifie que votre contenu ne sera pas disponible sur toutes les plateformes et tous les MVPD. Pour l’API REST sans client, nous disposons également du point d’entrée suivant : `https://api.auth.adobe.com/adobe-services/config your-config-ID]`.

* Les points d’entrée suivants font partie du SDK web d’authentification Adobe Pass.  S’il est manquant, cela signifie que le pay-TVpass est indisponible pour tous les programmeurs et toutes les propriétés web :

   * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
   * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### Points d’entrée que vous ne devez pas surveiller {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  Vous obtiendrez toujours une erreur 503, car ce point d’entrée nécessite une réponse SAML MVPD dessus.

* Autres points d’entrée de droit - `adobe-services/1.0/authenticate/`, `adobe-services/1.0/deviceShortAuthorize`, `adobe-services/1.0/authorize`

Vous ne pouvez pas surveiller ces points d’entrée, car ils ont besoin d’une payload pour une réponse appropriée.
