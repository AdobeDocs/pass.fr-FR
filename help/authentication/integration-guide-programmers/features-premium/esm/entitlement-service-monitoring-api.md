---
title: API de surveillance du service de droits
description: API de surveillance du service de droits
exl-id: a9572372-14a6-4caa-9ab6-4a6baababaa1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 1%

---

# API de surveillance du service de droits {#entitlement-service-monitoring-api}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Avant d’utiliser l’API de dégradation, assurez-vous que les conditions préalables suivantes sont remplies :
>
> * Récupérez les informations d’identification du client comme décrit dans la documentation de l’API [Récupérez les informations d’identification du client](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenez le jeton d’accès comme décrit dans la documentation de l’API [Récupérer le jeton d’accès](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Pour plus d’informations sur la création d’une application enregistrée et le téléchargement de l’instruction logicielle, consultez la documentation [Aperçu de l’enregistrement du client dynamique](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) .

## Présentation de l’API {#api-overview}

Le service de surveillance des droits (ESM) est implémenté en tant que projet WOLAP (Web-based [Online Analytics Processing](https://en.wikipedia.org/wiki/Online_analytical_processing){target=_blank}). ESM est une API Web de création de rapports d’entreprise générique, soutenue par un entrepôt de données. Il agit comme un langage de requête HTTP qui permet d’effectuer entièrement des opérations OLAP standard.

>[!NOTE]
>
>L’API ESM n’est pas disponible en général. Contactez votre représentant Adobe pour toute question concernant la disponibilité.

L’API ESM fournit une vue hiérarchique des cubes OLAP sous-jacents. Chaque ressource ([dimension](#esm_dimensions) dans la hiérarchie des dimensions, mappée en tant que segment de chemin d’URL) génère des rapports avec (agrégé) [mesures](#esm_metrics) pour la sélection actuelle. Chaque ressource pointe vers sa ressource parente (pour le cumul) et ses sous-ressources (pour l’analyse). Le découpage et la découpe s’effectuent par le biais de paramètres de chaîne de requête en épinglant des dimensions à des valeurs ou des plages spécifiques.

L’API REST fournit les données disponibles dans un intervalle de temps spécifié dans la requête (revenant aux valeurs par défaut si aucune valeur n’est fournie), en fonction du chemin d’accès à la dimension, des filtres fournis et des mesures sélectionnées. La période ne sera pas appliquée aux rapports qui ne contiennent pas de dimensions temporelles (année, mois, jour, heure, minute, seconde).

Le chemin d’accès racine de l’URL du point de terminaison renvoie les mesures agrégées globales dans un seul enregistrement, ainsi que les liens vers les options d’analyse disponibles. La version de l’API est mappée en tant que segment de fin du chemin d’accès URI du point d’entrée. Par exemple, `https://mgmt.auth.adobe.com/esm/v3` signifie que les clients accéderont à WOLAP version 3.

Les chemins d’URL disponibles sont détectables via des liens contenus dans la réponse. Les chemins d’URL valides sont conservés pour mapper un chemin d’accès dans l’arborescence déroulante sous-jacente qui contient des mesures agrégées (pré-). Un chemin dans le formulaire `/dimension1/dimension2/dimension3` reflétera une pré-agrégation de ces trois dimensions (l’équivalent d’un SQL `clause GROUP` BY `dimension1`, `dimension2`, `dimension3`). Si une telle pré-agrégation n’existe pas et que le système ne peut pas la calculer à la volée, l’API renvoie une réponse 404 Not Found.

## Arborescence de défilement {#drill-down-tree}

Les arborescences d’exploration suivantes illustrent les dimensions (ressources) disponibles dans ESM 3.0 pour [Programmers](#progr-dimensions) et [MVPDs](#mvpd-dimensions).


### Dimensions disponibles pour les programmeurs {#progr-dimensions}

#### Jour

![](../../../assets/esm-progr-dimensions-day.png)

#### Heure

![](../../../assets/esm-progr-dimensions-hour.png)

#### Minute

![](../../../assets/esm-progr-dimensions-minute.png)

### Dimensions disponibles pour les MVPD {#mvpd-dimensions}

![](../../../assets/esm-mvpd-dimensions.png)

Une GET au point d’entrée de l’API `https://mgmt.auth.adobe.com/esm/v3` renvoie une représentation contenant :

* Liens vers les chemins d’exploration racine disponibles :

   * `<link rel="drill-down" href="/v3/dimensionA"/>`

   * `<link rel="drill-down" href="/v3/dimensionB"/>`

* Résumé (valeurs agrégées) de toutes les mesures (par défaut
interval, puisqu’aucun paramètre de chaîne de requête n’est fourni, voir ci-dessous).


Suivez un chemin d’accès (étape par étape) :
`/dimensionA/year/month/day/dimensionX` récupère les éléments suivants :
response :

* Liens vers les options d’analyse &quot;`dimensionY`&quot; et &quot;`dimensionZ`&quot;

* Un rapport contenant des agrégats quotidiens pour chaque valeur de `dimensionX`


### Filtres

À l’exception des dimensions de date/heure, toute dimension disponible pour la projection actuelle (chemin de dimension) peut être filtrée en utilisant son nom comme paramètre de chaîne de requête.

Les options de filtrage disponibles sont les suivantes :

* Les filtres **Égal à** sont fournis en définissant le nom de la dimension sur une valeur particulière dans la chaîne de requête.

* Les filtres **IN** peuvent être spécifiés en ajoutant le même paramètre de nom de dimension plusieurs fois avec des valeurs différentes : dimension=valeur1\&amp;dimension=valeur2

* Les filtres **Not equals** doivent utiliser &quot;\!&quot; symbole situé après le nom de la dimension, ce qui entraîne le &quot;\!&quot;.=&#39; &quot;operator&quot;: dimension\!=value

* Les filtres **NOT IN** requièrent le &quot;\!=’ à utiliser plusieurs fois, une fois pour chaque valeur de l’ensemble : dimension\!=value1\&amp;dimension\!=value2&amp;...

Il existe également une utilisation spéciale pour les noms de dimension dans la chaîne de requête : si le nom de dimension est utilisé comme paramètre de chaîne de requête sans valeur, l’API aura pour instruction de renvoyer une projection incluant cette dimension dans le rapport.

### Exemples de requêtes ESM

| *URL* | *Équivalent SQL* |
|---|---|
| /dimension1/dimension2/dimension3?dimension1=value1 | SELECT * de projection WHERE dimension1 = &#39;value1&#39; </br> GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1=valeur1&amp;dimension1=valeur2 | SELECT * de projection WHERE dimension1 IN (&#39;value1&#39;, &#39;value2&#39;) </br> GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=value1 | SELECT * de projection WHERE dimension1 &lt;> &#39;value1&#39; | </br> GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=value1&amp;dimension2!=value2 | SELECT * de projection WHERE dimension1 NOT IN (&#39;value1&#39;, &#39;value2&#39;) | </br> GROUP BY dimension1, dimension2, dimension3 |
| En supposant qu’il n’y ait pas de chemin direct : /dimension1/dimension3 </br> mais qu’il y ait un chemin : /dimension1/dimension2/dimension3 </br> </br> /dimension1?dimension3 | SELECT * de projection GROUP BY dimension1, dimension3 |

>[!NOTE]
>
>Aucune de ces techniques de filtrage ne fonctionnera pour les dimensions `date/time`. Le seul moyen de filtrer les dimensions `date/time` consiste à définir les paramètres de chaîne de requête `start` et `end` (décrits ci-dessous) sur les valeurs requises.

Les paramètres de chaîne de requête suivants ont une signification réservée pour l’API (ils ne peuvent donc pas être utilisés comme noms de dimension, sinon aucun filtrage ne serait possible pour une telle dimension).

### Paramètres de chaîne de requête réservés à l’API ESM

| Paramètre | Facultatif | Description | Valeur par défaut | Exemple |
| --- | ---- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ---- | --- |
| access_token | Oui | Le jeton DCR peut être transmis en tant que jeton porteur d’autorisation standard. | Aucun | access_token=XXXX |
| nom-dimension | Oui | Tout nom de dimension, contenu dans le chemin d’URL actuel ou dans un sous-chemin valide ; la valeur sera traitée comme un filtre égal. Si aucune valeur n’est fournie, la dimension spécifiée sera alors incluse dans la sortie même si elle n’est pas incluse ou adjacente au chemin actuel. | Aucun | someDimension=someValue&amp;someOtherDimension |
| end | Oui | Heure de fin du rapport en millisecondes | Heure actuelle du serveur | end=2024-07-30 |
| format | Oui | Utilisé pour la négociation de contenu (avec le même effet mais une priorité inférieure au chemin &quot;extension&quot; - voir ci-dessous). | Aucun : la négociation du contenu tentera les autres stratégies | format=json |
| limit | Oui | Nombre maximal de lignes à renvoyer | Valeur par défaut signalée par le serveur dans le lien self si aucune limite n’est spécifiée dans la requête. | limit=1500 |
| mesures | Oui | Liste de noms de mesures séparés par des virgules à renvoyer. Elle doit être utilisée à la fois pour filtrer un sous-ensemble des mesures disponibles (afin de réduire la taille de la payload) et pour appliquer l’API afin de renvoyer une projection contenant les mesures demandées (plutôt que la projection optimale par défaut). | Toutes les mesures disponibles pour la projection actuelle seront renvoyées si ce paramètre n’est pas fourni. | metrics=m1,m2 |
| start | Oui | Heure de début du rapport ISO8601 ; le serveur remplira la partie restante si seul un préfixe est fourni : par exemple, start=2024 donnera start=2024-01-01:00:00:00 | Signalé par le serveur dans le lien self : le serveur tente de fournir des valeurs par défaut raisonnables en fonction de la granularité temporelle sélectionnée. | start=2024-07-15 |

La seule méthode HTTP actuellement disponible est GET.

## Codes d’état de l’API ESM {#esm-api-status-codes}

| Code d’état | Expression de motif | Description |
|---|---|---|
| 200 | OK | La réponse contiendra les liens &quot;Cumul&quot; et &quot;Exploration&quot; (le cas échéant). Le rapport sera rendu en tant qu’attribut de la ressource : élément/propriété &quot;rapport&quot; imbriqué. |
| 400 | Requête incorrecte | Le corps de la réponse contient un message texte expliquant ce qui ne va pas avec la requête. </br> </br> Un état de 400 Bad Request est accompagné d’un texte explicatif dans le corps de la réponse (type de média brut/texte) qui fournit des informations utiles concernant l’erreur client. Outre les scénarios triviaux tels que les formats de date non valides ou les filtres appliqués aux dimensions non existantes, le système refusera également de répondre aux requêtes qui nécessitent un volume massif de données à renvoyer ou à agréger à la volée. |
| 401 | Non autorisé | En raison d’une requête qui ne contient pas les en-têtes OAuth appropriés pour authentifier l’utilisateur |
| 403 | Interdit | Indique que la demande n’est pas autorisée dans le contexte de sécurité actuel ; cela se produit lorsque l’utilisateur est authentifié, mais pas autorisé à accéder aux informations demandées. |
| 404 | Introuvable | Se produit lorsqu’un chemin d’URL non valide est fourni avec la requête. Cela ne devrait jamais se produire si le client suit les liens &quot;zoom descendant&quot;/&quot;cumul&quot; fournis avec 200 réponses. |
| 405 | Méthode non autorisée | Indique qu’une méthode non prise en charge a été utilisée dans la requête. Bien que, pour l’heure, seule la méthode GET soit prise en charge, les futures versions pourront autoriser les HEAD ou les OPTIONS. |
| 406 | Non accepté | Indique qu’un type de média non pris en charge a été demandé par le client. |
| 500 | Erreur interne du serveur | &quot;Ça ne devrait jamais arriver&quot; |
| 503 | Service indisponible | Indique une erreur dans l’application ou ses dépendances. |

## Formats de données {#data-formats}

Les données sont disponibles dans les formats suivants :

* JSON (par défaut)
* XML
* CSV
* HTML (à des fins de démonstration)

Les stratégies de négociation de contenu suivantes peuvent être utilisées par les clients (la priorité est donnée par le poste dans la liste - premier élément) :

1. Une &quot;extension de fichier&quot; ajoutée au dernier segment du chemin d’URL : par exemple, `/esm/v3/media-company/year/month/day.xml`. Si l’URL contient une chaîne de requête, l’extension doit être précédée du point d’interrogation : `/esm/v3/media-company/year/month/day.csv?mvpd= SomeMVPD`
1. Un paramètre de chaîne de requête de format : par exemple, `/esm/report?format=json`
1. En-tête HTTP Accept standard : par exemple, `Accept: application/xml`

&quot;extension&quot; et le paramètre de requête prennent en charge les valeurs suivantes :

* xml
* json
* csv
* html

Si aucun type de média n’est spécifié par l’une des stratégies, l’API génère du contenu JSON par défaut.

## Langage de l’application hypertexte {#hypertext-application-language}

Pour JSON et XML, la charge utile sera codée en tant que HAL, comme décrit ici : <http://stateless.co/hal_specification.html>.

Le rapport réel (une balise/propriété imbriquée appelée &quot;rapport&quot;) se compose de la liste réelle d’enregistrements contenant toutes les dimensions et mesures sélectionnées/applicables avec leurs valeurs, codées comme suit :

### JSON

```JSON
 "report": [
  {
    "dimension1": "d1",
    ...
    "metric1": "m1",
    ...
  }, {
    ...
  }
]
```

### XML

```XML
 <report>
  <record dimension1="d1" ... metric1="m1" ... />
  ...
</report
```

Pour les formats XML et JSON, l’ordre des champs (dimensions et mesures) dans un enregistrement n’est pas spécifié, mais cohérent (l’ordre sera le même dans tous les enregistrements). Toutefois, les clients ne doivent pas se fier à un ordre particulier des champs dans un enregistrement.

Le lien de la ressource (le rel &quot;self&quot; dans JSON et l’attribut de ressource &quot;href&quot; dans XML) contient le chemin actuel et la chaîne de requête utilisée pour le rapport intégré. La chaîne de requête affiche tous les paramètres implicites et explicites, de sorte que la charge utile indique explicitement l’intervalle de temps utilisé, les filtres implicites (le cas échéant), etc. Le reste des liens de la ressource contiendra tous les segments disponibles qui peuvent être suivis afin d’analyser en détail les données actives. Un lien de cumul est également fourni, qui pointe vers le chemin parent (le cas échéant). La valeur `href` des liens d’analyse/de cumul contient uniquement le chemin d’accès à l’URL (elle n’inclut pas la chaîne de requête ; le client doit donc l’ajouter si nécessaire). Notez que tous les paramètres de chaîne de requête utilisés (ou implicites) par la ressource actuelle ne s’appliqueront pas aux liens &quot;de cumul&quot; ou &quot;d’exploration&quot; (par exemple, les filtres peuvent ne pas s’appliquer aux sous-ressources ou aux super-ressources).

Exemple (en supposant qu’il existe une mesure unique appelée `clients` et qu’il existe une pré-agrégation pour `year/month/day/...`) :

* https://mgmt.auth.adobe.com/esm/v3/year/month.xml

```XML
   <resource href="/esm/v3/year/month?start=2024-07-20T00:00:00&end=2024-08-20T14:35:21">
   <links>
   <link rel="roll-up" href="/esm/v3/year"/>
   <link rel="drill-down" href="/esm/v3/year/month/day"/>
   </links>
   <report>
   <record month="6" year="2024" clients="205"/>
   <record month="7" year="2024" clients="466"/>
   </report>
   </resource>
```

* https://mgmt.auth.adobe.com/esm/v3/year/month.json

  ```JSON
      {
        "_links" : {
          "self" : {
            "href" : "/esm/v3/year/month?start=2024-07-20T00:00:00&end=2024-08-20T14:35:21"
          },
          "roll-up" : {
            "href" : "/esm/v3/year"
          },
          "drill-down" : {
            "href" : "/esm/v3/year/month/day"
          }
        },
        "report" : [ {
          "month" : "6",
          "year" : "2024",
          "clients" : "205"
        }, {
          "month" : "7",
          "year" : "2024",
          "clients" : "466"
        } ]
      }
  ```

### CSV

Dans le format de données CSV, aucun lien ou autre métadonnées (à l’exception de la ligne d’en-tête) ne sera fourni en ligne ; les métadonnées de sélection seront fournies dans le nom de fichier, selon le modèle suivant :

```CSV
    esm__<start-date>_<end-date>_<filter-values,...>.csv
```

Le fichier CSV contient une ligne d’en-tête, puis les données du rapport sous forme de lignes suivantes. La ligne d’en-tête contient toutes les dimensions suivies de toutes les mesures. L’ordre de tri des données du rapport est reflété dans l’ordre des dimensions. Par conséquent, si les données sont triées par `D1`, puis par `D2`, l’en-tête CSV ressemblera à : `D1, D2, ...metrics...`.

L’ordre des champs dans la ligne d’en-tête reflète l’ordre de tri des données du tableau.


Exemple : https://mgmt.auth.adobe.com/esm/v3/year/month.csv produira un fichier nommé `report__2024-07-20_2024-08-20_1000.csv` avec le contenu suivant :


| Année | Mois | Clients |
| ---- | :---: | ------- |
| 2024 | 6 | 580 |
| 2024 | 7 | 231 |

## Actualisation des données {#data-freshness}

Les réponses HTTP réussies contiennent un en-tête `Last-Modified` qui indique l’heure de la dernière mise à jour du rapport dans le corps. L’absence d’un en-tête Last-Modified indique que les données du rapport sont calculées en temps réel.

En règle générale, les données de granularité grossière sont mises à jour moins fréquemment que les données d’granularité fine (par exemple, les valeurs par minute ou par heure peuvent être plus à jour que les valeurs quotidiennes, en particulier pour les mesures qui ne peuvent pas être calculées en fonction de granularités plus petites, comme les nombres uniques).

## Compression GZIP {#gzip-compression}

Adobe recommande vivement d’activer la prise en charge de gzip dans les clients qui récupèrent les rapports ESM. Cela réduira considérablement la taille de la réponse, ce qui réduit votre temps de réponse. (Le taux de compression des données ESM se situe entre 20 et 30.)

Pour activer la compression gzip dans votre client, définissez l’en-tête `Accept-Encoding:` comme suit :

* Accept-Encoding : gzip, dégonfler


<!--
## Related Information {#related-information}

- [ESM Overview](/help/authentication/entitlement-service-monitoring-overview.md)
- [Degradation API Overview](/help/authentication/degradation-api-overview.md)
- [Understanding Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
