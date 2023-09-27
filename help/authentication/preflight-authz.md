---
title: Autorisation de contrôle en amont
description: Autorisation de contrôle en amont
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 0%

---

# Autorisation de contrôle en amont {#preflight-authorization}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## Présentation {#overview}

Cette fonctionnalité fournit un contrôle d’autorisation léger pour plusieurs ressources. Cette vérification légère a pour but de décorer l’interface utilisateur (par exemple, en indiquant l’état d’accès avec les icônes de verrouillage et de déverrouillage). L’autorisation de contrôle en amont est aussi légère et efficace que possible, de sorte qu’un seul appel API génère l’état d’autorisation d’une liste de ressources. Notez que cette fonction n’a aucune autorité en ce qui concerne l’autorisation d’une ressource.

A `getAuthorization(resource)` ou `checkAuthorization(resource)` L’appel doit toujours être effectué avant d’autoriser la lecture.

L’autorisation de contrôle en amont fournit également la prise en charge d’un autre cas d’utilisation, dans lequel le programmeur doit demander l’autorisation de plusieurs ID de ressource afin de permettre la lecture d’un élément du contenu multimédia. Le programmeur peut effectuer un contrôle en amont initial sur les ressources requises, et selon la réponse, peut échouer tôt si les conditions d’exploitation ne sont pas remplies.

Pour obtenir la liste des MVPD qui prennent en charge l’autorisation de contrôle en amont, voir [Autorisation de contrôle en amont MVPD](/help/authentication/mvpd-preflight-authz.md#preflight_support_list) page.

>[!NOTE]
>
> Veuillez noter que l’utilisation de cette fonctionnalité pour les distributeurs multicanaux de programmes audiovisuels qui ne disposent pas d’une prise en charge complète de l’autorisation de contrôle en amont doit être acceptée au préalable avec les Adobes et les distributeurs multicanaux de programmes audiovisuels. L’utilisation de l’autorisation de contrôle en amont sur ces MVPD relève du &quot;pire scénario&quot; décrit [here](/help/authentication/mvpd-preflight-authz.md#intro) et peut entraîner des problèmes de performances et un temps de réponse lent. </br>
> Notez également que l’utilisation de l’autorisation de contrôle en amont avec **plus de 5 ressources doivent être explicitement acceptées par Adobe.**.

## API AccessEnabler Preflight {#AE_pre_api}

AccessEnabler expose une paire API/fonction de rappel pour implémenter l’autorisation de contrôle en amont. L’appel API prend un seul paramètre constitué d’une liste de ressources. La fonction de rappel utilise un seul paramètre qui représente les ressources autorisées réelles. Les exemples suivants sont en ActionScript, mais les appels sont disponibles dans toutes les versions d’AccessEnabler.

### checkPreauthorizedResources(Array:resources):void {#checkPreauthRes}

Appelez cette fonction sur l’objet AccessEnabler pour demander l’état d’autorisation d’une liste de ressources.

Le paramètre resources est la liste des ressources pour laquelle l&#39;autorisation doit être vérifiée. Chaque élément de la liste doit être une chaîne représentant l’ID de ressource. L’ID de ressource est soumis aux mêmes limites que l’ID de ressource dans la variable `getAuthorization()` , c’est-à-dire qu’il est convenu de la valeur établie entre le programmeur et le MVPD, ou un fragment RSS multimédia. Notez que l’authentification Adobe Pass ne gère aucune ressource, à l’exception d’une fine couche de médiation qui peut transformer les formats de ressource en fonction de ce que le MVPD prend réellement en charge.

### preauthorizedResources(Array:authorizedResources) {#preauthRes}

Il s’agit d’une fonction de rappel qui doit être implémentée dans l’application de couche supérieure du programmeur. AccessEnabler appelle cette fonction après calcul de la liste des ressources autorisées.


### Exemple JS

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## Informations d’implémentation {#details}

- [Contrôle en amont à l’aide de ChannelID](#preflight_using_channelID)
- [POST/Préautorisation](#post)
- [Stockage](#storage)

L’appel API tente de trouver une liste mise en cache des ressources autorisées pour l’utilisateur actuel dans l’espace de stockage local du client. En l’absence de liste mise en cache, un appel HTTPS est effectué aux serveurs AdobePass pour récupérer la liste.

Le mécanisme de mise en cache améliore les temps de performances des appels suivants en ignorant complètement l’appel réseau. En outre, la liste mise en cache peut être renseignée d’avance dans le cadre du processus d’authentification.  (Pour plus d’informations sur la configuration de ce scénario, voir [Intégration de l’autorisation Preflight](/help/authentication/authz-usecase.md#preflight_authz_int) dans la section Autorisation du guide d’intégration MVPD).

En outre, la liste des ressources mises en cache peut éventuellement être utilisée pour optimiser le flux d’autorisation, dans le sens où, s’il existe une liste mise en cache de ressources, `checkAuthorization()` peut le vérifier avant d’effectuer un appel réseau. Si la ressource ne figure pas dans la liste des ressources préautorisées, la vérification peut échouer sans avoir à appeler les serveurs d’authentification Adobe Pass.


### Contrôle en amont à l’aide de ChannelID {#preflight_using_channelID}

À partir de la version 2.4.1 de l’authentification Adobe Pass, le flux de contrôle en amont fonctionne comme suit :

1. Lors de l’authentification, l’authentification Adobe Pass lit la variable `channelIID` de la réponse SAML du MVPD et utilise cette valeur pour définir la variable `authorizedResources` dans le jeton d’authentification.
1. Dans le `checkPreauthorizedResources()` fonction d’API, l’authentification Adobe Pass vérifie si la variable `authorizedResources` est défini.
1. Si la variable `authorizedResources` est défini, l’authentification Adobe Pass lit cette valeur et effectue une intersection entre la liste de ressources et la propriété `authorizedResources` et la liste des ressources reçues de `checkPreauthorizedResources()` .  Le résultat de cette intersection est la liste finale des ressources préautorisées.
1. Si la variable `authorizedResources` n’est pas défini, exécutez le flux mis en oeuvre précédemment, dans lequel la liste des ressources reçues d’ `checkPreauthorizedResources()` est transmis au servlet PreAuthorizationServlet. Ce servlet effectue les appels d’autorisation aux points de terminaison MVPD et renvoie la liste des ressources préautorisées.

### Exemple de contrôle en amont avec ChannelID

L’exemple ci-dessous illustre un exemple de ligne de canal. Veuillez noter que le nom de l’attribut utilisé pour envoyer la liste des canaux peut différer d’un MVPD à un autre :

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


La variable `authorizedResources` à partir de l’élément authentication apparaît comme suit :

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

Le programmeur exécute la variable `checkPreauthorizedResources()` appel API, transmission de la liste de paramètres suivante :</span>

- &quot;MSNBC
- &quot;FBN&quot;
- TruTV
- &quot;fbc-fox&quot;

L’implémentation actuelle de Preflight effectue l’intersection avec la liste des ressources de la `authorizedResources` et renvoie cette liste :

- &quot;MSNBC
- &quot;FBN&quot;
- TruTV



**Remarque :** Notez que l’intersection n’est pas sensible à la casse.



### POST/Préautorisation {#post}

Cet appel est effectué automatiquement par AccessEnabler lorsqu’il n’existe aucune liste de ressources, autorisée, mise en cache.


#### Requête {#req}

| Paramètre | Type | Obligatoire | Description |
| --- | --- | --- | --- |
| `authentication_token` | string | OUI | Jeton d’authentification. |
| `resource_id` | string | OUI | Une seule ressource. Cela peut être spécifié plusieurs fois, une fois pour chaque élément du tableau de ressources fourni dans la variable `checkPreauthorizedResources()` appel API. |


**Remarque :** Le nombre maximal de ressources demandées peut être configuré.
**La valeur par défaut maximale est 5.**


#### Réponse {#resp}

La réponse renvoyée par le servlet de préautorisation a le format suivant :

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### Stockage {#storage}

Liste des ressources préautorisées que AccessEnabler obtient du fournisseur de services. Cette liste de ressources :

- est stocké avec les jetons AuthN et AuthZ ;
- est valide tant que l’utilisateur se trouve sur le même site web ou jusqu’à l’expiration du jeton AuthN ;
- Est récupéré à nouveau chaque fois que l’utilisateur arrive sur un nouveau site web intégré d’authentification Adobe Pass

Chaque entrée contient l’ID de ressource pour lequel l’utilisateur est préautorisé.

Par exemple :


| ID de ressource | Autorisé |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


Cette liste est appelée &quot;cache de préautorisation&quot;.

#### Flux {#flow}

1. L’application/le site du programmeur crée une `checkPreauthorizedResources(resourceList)` appelez .
1. AccessEnabler vérifie le jeton d&#39;authentification pour les ressources autorisées :
   1. Si le jeton d’authentification contient des ressources autorisées, cette liste fait autorité et aucun appel ne doit être effectué pour obtenir ces informations. Les ressources de resourceList sont recherchées dans la liste des ressources autorisées sur le jeton d’authentification et seules celles qui ont été trouvées sont renvoyées par la variable `preauthorizedResources()` rappel.
   1. Si le jeton d’authentification NE contient PAS de ressources autorisées - `resourceList` est comparé à la liste des ressources dans le cache de préautorisation.
      1. Si la liste contient les mêmes ressources, cela signifie qu’un appel au serveur a déjà été effectué et que la réponse se trouve déjà dans le cache de préautorisation. Seules les ressources autorisées seront renvoyées par la variable `preauthorizedResources()` rappel.
      1. Si la liste NE contient PAS les mêmes ressources, le client doit effectuer un appel au serveur pour obtenir l’état d’autorisation des ressources dans resourceList. La réponse sera récupérée et stockée dans le cache de préautorisation, en remplaçant complètement les anciennes ressources. Seules les ressources autorisées seront renvoyées par la variable `preauthorizedResources()` rappel.


#### Récupération de liste {#listRetrieve}

Chaque fois que `checkPreauthorizedResources()` est appelée, la liste des ressources à vérifier pour l’autorisation est comparée au cache de préautorisation. Si la liste contient le même ensemble de ressources, aucun appel au fournisseur de services n’est effectué, car toutes les ressources nécessaires pour déclencher la variable `preauthorizedResources()` Les rappels se trouvent déjà dans le cache.


#### logout() {#logout}

Le cache de préautorisation est vidé lors de la déconnexion.


## Dépendances {#depends}

Les performances de l’API Preflight dépendent des mises en oeuvre MVPD spécifiques.  Pour connaître les options d’implémentation, voir [Intégration de l’autorisation Preflight](/help/authentication/authz-usecase.md#preflight_authz_int) dans la section Autorisation du guide d’intégration MVPD.


## Sécurité {#security}

Les API clientes sont disponibles pour tous les programmeurs.

L’implémentation utilise HTTPS comme transport, mais pour un appel plus léger, aucune mesure de sécurité supplémentaire n’est appliquée (pas de signature, pas de FAXS).

**Attention :** N’utilisez PAS cette API de manière autoritaire pour déterminer si un utilisateur doit se voir accorder l’accès à une ressource protégée. L’objectif de cette API est la décoration de l’interface utilisateur et/ou le contrôle en amont des décisions professionnelles. La variable `getAuthorization()` et `checkAuthorization()` Les appels doivent toujours être effectués avant d’autoriser la lecture.


## Compatibilité {#compat}

Cette fonctionnalité est prise en charge dans toutes les versions d’AccessEnabler : AS, JS, AIR, iOS, Android, Xbox (dans le flux AuthN 2e écran).

L’autorisation de contrôle en amont ne prend pas en charge l’autorisation préalable des ressources qui contiennent des sections CDATA. Le système de contrôle en amont actuel se concentre sur la prise en charge du filtrage au niveau des canaux. Les ressources avec des sections CDATA sont probablement des ressources au niveau des ressources. Le contrôle en amont prend en charge les `mrss` ressources pour la préautorisation au niveau du canal, à condition qu’elles ne contiennent pas de CDATA.

## Intégration À D’Autres Fonctionnalités {#integ_w_other_features}

Une optimisation possible peut se produire sur le flux d’autorisation afin d’échouer rapidement si la ressource ne figure pas dans la liste des ressources préautorisées.

Dans cette optimisation, le point d’entrée preflight du serveur s’intègre au mécanisme de dégradation.

Si une règle &quot;AuthN All&quot; est en place pour le MVPD et le demandeur, le point de terminaison preflight reflétera simplement toutes les ressources reçues dans la requête.

La même chose est vraie si &quot;AuthZ All&quot; est activé pour au moins une des ressources présentes dans la requête.

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
