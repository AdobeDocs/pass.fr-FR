---
title: Avantages du paramètre deviceType sans client dans les mesures d’authentification Adobe Pass
description: Avantages du paramètre deviceType sans client dans les mesures d’authentification Adobe Pass
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Avantages du paramètre deviceType sans client dans les mesures d’authentification Adobe Pass {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Contexte

Bien que facultatif, le paramètre `deviceType` de l’API sans client, lorsqu’il est présent, est utilisé dans les mesures d’authentification Adobe Pass qui sont exposées par le biais de la [surveillance du service de droits](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md).

Étant donné que la connexion entre le paramètre `deviceType` et ses **avantages** sur les mesures d’authentification Adobe Pass n’a pas été initialement indiquée, la portée de cette note technique est d’ajouter plus d’informations à leur sujet.

## Explication

Le paramètre `deviceType` était présent dans l’API sans client depuis la première version, mais ses implications sur les mesures d’authentification Adobe Pass ont été ajoutées dans une version plus récente.



>[!IMPORTANT]
>
>Si le paramètre `deviceType` est correctement défini, il dispose du **avantage** suivant dans la surveillance du service de droits : il offre des mesures [ ventilées par type d’appareil ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyses puissent être effectués pour Roku, AppleTV, Xbox, etc.


Pour plus d’informations sur l’API de surveillance du service de droits, reportez-vous à l’ [arborescence descendante,](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md#drill-down_tree) qui illustre les [dimensions](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#esm_dimensions) (ressources) disponibles dans ESM 2.0.

>[!NOTE]
>
>Le contenu de cette note technique a également été ajouté à l’ [API sans client](#clientless_device_type).




## Implémentation

Pour tirer pleinement parti des mesures d’authentification Adobe Pass, il existe deux types d’[ API sans client](#web_srvs_summary) qui sont actuellement utilisés et qui doivent avoir la valeur `deviceType` correcte :

1. Les API qui ont `regcode` comme paramètre requis et utiliseront le paramètre `deviceType` défini lors de la création de `regcode`, avec l’appel API suivant :
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. API dont le paramètre `deviceType` est facultatif :
   - [\&lt;SP\_FQDN\>/api/v1/checkauthen](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/autoriser](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preautoriser](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

Nous vous recommandons d’utiliser le paramètre `deviceType` et de transmettre le type d’appareil sans client correct pour toutes les API.
