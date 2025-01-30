---
title: Décisions
description: Décisions
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# Décisions {#decisions}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Les décisions sont générées par l’authentification Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) en fonction des demandes d’autorisation ou de préautorisation MVPD de l’utilisateur ou de l’utilisatrice, déterminant si l’accès au [contenu protégé](#protected-resources) est accordé ou refusé.

Deux types de décisions sont fournis, selon l’API appelée :

* [décisions de préautorisation](#preauthorization-decisions) qui sont des décisions informatives.
* [décisions d’autorisation](#authorization-decisions) qui font autorité.

## Décisions de pré-autorisation {#preauthorization-decisions}

La décision de préautorisation est une décision informative qui permet à l’application cliente d’être informée si le MVPD peut autoriser ou refuser l’accès de l’utilisateur à une [ ressource protégée ](#protected-resources).

L’objectif de la préautorisation (autorisation de contrôle en amont) est de permettre à l’application d’afficher des informations précises sur le contenu que l’utilisateur peut être autorisé à consulter. Pour ce faire, l’interface utilisateur est améliorée à l’aide d’indicateurs, tels que des icônes verrouillées ou déverrouillées, afin de refléter le statut d’accès.

>[!IMPORTANT]
>
> La décision de préautorisation ne doit pas être utilisée de manière faisant autorité pour jouer des ressources, car tel est l’objectif d’une [décision d’autorisation](#authorization-decisions).

L’utilisation de l’API de préautorisation n’est pas obligatoire, l’application cliente peut l’ignorer si elle souhaite présenter un catalogue de ressources sans aucun filtrage.

Si l’application cliente a l’intention d’utiliser cette fonctionnalité, il est important de noter que les décisions de préautorisation ne peuvent être obtenues que pour un nombre limité de ressources par requête API, généralement jusqu’à 5.

>[!IMPORTANT]
> 
> Le nombre maximal de ressources ne peut être augmenté qu’après avoir conclu un accord avec les MVPD et les représentants d’Adobe Pass Authentication. Une fois approuvées, les modifications peuvent être mises en œuvre via le tableau de bord Adobe Pass TVE par un administrateur de votre entreprise ou un représentant Adobe Pass Authentication agissant en votre nom.
> 
> Pour plus d’informations, reportez-vous à la documentation du [Guide d’utilisation des intégrations de tableaux de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

Les MVPD peuvent prendre en charge la préautorisation par le biais de divers mécanismes, chacun ayant des implications distinctes en termes de performances et de nombre maximal de ressources pouvant être traitées dans une seule requête API.

Pour plus d’informations sur les mécanismes existants prenant en charge la préautorisation, reportez-vous à la documentation relative à l’[autorisation de contrôle en amont de MVPD](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md).

>[!IMPORTANT]
>
> Pour les MVPD sans prise en charge complète de l’autorisation de contrôle en amont, l’utilisation de la préautorisation doit être approuvée à l’avance avec les MVPD et les représentants de l’authentification Adobe Pass, car elle peut entraîner des problèmes de performances et des temps de réponse plus lents.

## Décisions d’autorisation {#authorization-decisions}

La décision d’autorisation est une décision faisant autorité qui permet à l’application cliente de se conformer à la décision MVPD d’autoriser ou de refuser l’accès de l’utilisateur à une [ ressource protégée ](#protected-resources).

L’objectif de l’autorisation est de permettre à l’application de lire les ressources demandées par l’utilisateur, après validation des droits avec le MVPD et réception d’un jeton média provenant de l’authentification Adobe Pass.

>[!IMPORTANT]
> 
> L’authentification Adobe Pass recommande aux programmeurs d’utiliser la bibliothèque Vérificateur de jeton multimédia pour valider le jeton multimédia inclus dans une décision d’autorisation, en garantissant un accès sécurisé avant de démarrer le flux vidéo.
> 
> Pour plus d’informations, consultez la documentation [Jetons de média](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md).

L’utilisation de l’API d’autorisation est obligatoire, l’application cliente ne peut pas ignorer cette phase si elle souhaite lire les ressources demandées par l’utilisateur, car elle doit vérifier auprès du MVPD que l’utilisateur a bien le droit de lire le flux avant de le publier.

Il est important de noter que les décisions d’autorisation ne peuvent être obtenues que pour un nombre limité de ressources par requête API, généralement 1.

>[!IMPORTANT]
>
> Le nombre maximal de ressources ne peut être augmenté qu’après avoir conclu un accord avec les MVPD et les représentants d’Adobe Pass Authentication.

## Ressources protégées {#protected-resources}

Les ressources protégées font référence au contenu en flux continu, identifié par des valeurs uniques définies par des accords entre les MVPD et les programmeurs participants.

Les ressources protégées suivent une structure arborescente hiérarchique, chaque niveau offrant une plus grande granularité pour l’autorisation du contenu :

* Réseau
   * Canal
      * Afficher
         * Épisode
            * Ressource

>[!IMPORTANT]
>
> La préautorisation (autorisation de contrôle en amont) est axée sur les ressources au niveau du canal avec des identifiants ayant un format de chaîne simple ou MRSS.
> 
> Nous vous déconseillons d’utiliser des ressources avec des identifiants qui incluent des sections `CDATA` en cas de préautorisation, car elles sont principalement utilisées pour les ressources au niveau des ressources définies par un SMS.

### Identifiant de la ressource {#resource-identifier}

L’identifiant unique de la ressource peut avoir deux formats :

* Format de chaîne simple, tel qu’un identifiant unique pour un canal (marque).
* Format RSS (Media RSS) contenant des informations supplémentaires telles que le titre, les évaluations et les métadonnées de contrôle parental.

Dans le cas d’un identifiant de ressource simple, tel que « REF30 » (supposé représenter un canal), il peut être traduit en identifiant de ressource RSS comme suit :

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

Dans le cas d&#39;un identifiant de ressource plus complexe, l&#39;identifiant de ressource RSS peut inclure des informations d&#39;évaluation supplémentaires comme suit :

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

Les identifiants uniques sont principalement opaques pour l’authentification Adobe Pass. Cependant, des transformateurs peuvent être appliqués en fonction des fonctionnalités et des exigences de MVPD. Si le MVPD ne peut pas reconnaître ou analyser un identifiant de ressource, il renvoie une erreur à l’authentification Adobe Pass, qui transmet ensuite l’erreur à l’application cliente à l’aide d’un [ Code d’erreur amélioré ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

## API REST V2 {#rest-api-v2}

Les décisions de préautorisation peuvent être récupérées à l’aide de l’API suivante :

* [Récupérer des décisions de préautorisation à l’aide de mvpd spécifiques](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

Les décisions d’autorisation peuvent être récupérées à l’aide de l’API suivante :

* [Récupérer des décisions d’autorisation à l’aide de mvpd spécifiques](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Reportez-vous aux sections **Réponse** et **Exemples** des API ci-dessus pour comprendre la structure des décisions de préautorisation et d’autorisation.

Pour plus d’informations sur comment et à quel moment intégrer les API ci-dessus, reportez-vous aux documents suivants :

* [Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
