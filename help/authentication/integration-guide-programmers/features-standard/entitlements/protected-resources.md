---
title: Ressources protégées
description: Ressources protégées
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Ressources protégées {#protected-resources}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Les ressources protégées représentent le contenu que les utilisateurs peuvent diffuser et sont identifiées par des valeurs uniques établies par des accords entre les MVPD et les programmeurs participants.

Les ressources protégées suivent une structure arborescente hiérarchique, chaque niveau offrant une plus grande granularité pour l’autorisation du contenu :

* Réseau
   * Canal
      * Afficher
         * Épisode
            * Ressource

## Identifiants des ressources protégées {#identifiers}

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

## API REST V2 {#rest-api-v2}

Les API d’authentification Adobe Pass récupérant les décisions de préautorisation et d’autorisation nécessitent que l’application cliente inclue les identifiants des ressources protégées en tant que paramètres.

Les décisions de préautorisation et d’autorisation peuvent être récupérées à l’aide des API suivantes :

* [Récupérer des décisions de préautorisation à l’aide de mvpd spécifiques](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Récupérer des décisions d’autorisation à l’aide de mvpd spécifiques](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Reportez-vous aux sections **Requête** et **Exemples** des API ci-dessus pour comprendre le format requis pour fournir les identifiants uniques des ressources protégées.

Les identifiants uniques sont opaques pour l’authentification Adobe Pass, car ils sont transmis directement au MVPD. Si le MVPD ne peut pas reconnaître ou analyser un identifiant de ressource, il renvoie une erreur à l’authentification Adobe Pass, qui renvoie ensuite l’erreur à l’application cliente à l’aide d’un [ Code d’erreur amélioré ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

Pour plus d’informations sur comment et à quel moment intégrer les API ci-dessus, reportez-vous aux documents suivants :

* [Flux de préautorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
