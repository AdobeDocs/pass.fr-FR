---
title: Avantages de l’utilisation du paramètre deviceType sans client dans les mesures d’authentification Adobe Pass
description: Avantages de l’utilisation du paramètre deviceType sans client dans les mesures d’authentification Adobe Pass
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---

# Avantages (hérités) de l’utilisation du paramètre deviceType sans client dans les mesures d’authentification Adobe Pass {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Contexte

Bien que facultatif, le paramètre `deviceType` à partir de l’API Clientless, s’il est présent, est utilisé dans les mesures d’authentification d’Adobe Pass exposées par le biais de la [surveillance du service de droit](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md).

Étant donné que la connexion entre le paramètre `deviceType` et ses **avantages** sur les mesures d’authentification Adobe Pass n’a pas été déclarée au départ, cette note technique a pour but d’ajouter plus d’informations à leur sujet.

## Explication

Le paramètre `deviceType` était présent dans l’API sans client depuis la première version, mais ses implications sur les mesures d’authentification Adobe Pass ont été ajoutées dans une version plus récente.



>[!IMPORTANT]
>
>Si le paramètre `deviceType` est défini correctement, il présente les **avantages** suivants dans la surveillance du service de droit : il offre des mesures [ventilées par type d’appareil](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) lors de l’utilisation de Clientless, de sorte que différents types d’analyse puissent être effectués pour Roku, AppleTV, Xbox, etc.


Pour plus d’informations sur l’API Entitlement Service Monitoring, reportez-vous à l’arborescence [analyse en profondeur](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md#drill-down_tree) qui illustre les [dimensions](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#esm_dimensions) (ressources) disponibles dans ESM 2.0.

>[!NOTE]
>
>Le contenu de cette note technique a également été ajouté à l’[API sans client](#clientless_device_type).




## Mise en œuvre

Pour tirer pleinement parti des mesures d’authentification d’Adobe Pass, deux types d’[API sans client](#web_srvs_summary) sont actuellement utilisés et doivent être définis avec les `deviceType` appropriés :

1. Les API qui ont `regcode` comme paramètre obligatoire et utiliseront le paramètre `deviceType` qui a été défini lors de la création du `regcode`, avec l’appel API suivant :
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. Les API qui ont le `deviceType` comme paramètre facultatif :
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=« s1 »>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

Nous vous recommandons d’utiliser le paramètre `deviceType` et de transmettre le type d’appareil sans client correct pour toutes les API.
