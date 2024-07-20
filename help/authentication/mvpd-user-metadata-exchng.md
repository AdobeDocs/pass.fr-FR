---
title: Exchange de métadonnées utilisateur MVPD
description: Exchange de métadonnées utilisateur MVPD
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# Exchange de métadonnées utilisateur MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro-user-metadata-exchange}

Les MVPD gèrent des métadonnées spécifiques à l’utilisateur sur leurs clients qui, dans certains cas, sont partagées avec des programmeurs. Le but de l’authentification Adobe Pass est de négocier un exchange de ces &quot;métadonnées utilisateur&quot;, mais pas d’appliquer des règles concernant l’exchange. Les règles d’exchange sont que les MVPD travaillent avec leurs partenaires programmeurs.

Les types de métadonnées utilisateur disponibles pour exchange sont actuellement les suivants :

* Code postal
* Note maximale (VChip ou MPAA)
* Identifiant utilisateur
* Identifiant du foyer
* Identifiant de canal

Grâce à cette fonctionnalité, les distributeurs multicanaux de programmes et les programmeurs peuvent mettre en oeuvre des cas d’utilisation spécifiques tels que le contrôle parental. Par exemple, un MVPD peut transmettre des données d’évaluation parentale à un programmeur, qui les utilise ensuite pour filtrer les choix d’affichage disponibles pour un utilisateur.

Points clés des métadonnées utilisateur :

* Le MVPD transmet les métadonnées utilisateur à l’application du programmeur pendant les flux d’authentification et d’autorisation.
* L’authentification Adobe Pass enregistre les valeurs des métadonnées dans les jetons AuthN et AuthZ.
* L’authentification Adobe Pass peut normaliser les valeurs des MVPD qui fournissent des métadonnées utilisateur dans différents formats.
* Certains paramètres peuvent être chiffrés à l’aide de la clé du programmeur
* Des valeurs spécifiques sont disponibles par Adobe, via un changement de configuration.

>[!NOTE]
>
>Les métadonnées utilisateur sont une extension des métadonnées statiques (jeton d’authentification TTL, jeton d’autorisation TTL et ID de périphérique) précédemment disponibles dans l’authentification Adobe Pass.

## Exemples {#example-mvpd-user-metadata-exch}

### Contrôle parental {#example-parental-control}

Cet exemple illustre l’échange des éléments suivants :

* [Programmeur vers l’Exchange de métadonnées MVPD](#progr-mvpd-metadata-exch)

* [Flux d’Exchange MVPD vers les métadonnées de programmeur](#mvpd-progr-exchange-flow)

### Programmeur vers l’Exchange de métadonnées MVPD {#progr-mvpd-metadata-exch}

Actuellement, l’API de programmation, l’authentification Adobe Pass et les agents d’autorisation MVPD ne prennent en charge que l’autorisation au niveau du canal. Le canal est spécifié sous la forme d’une chaîne de texte brut dans l’appel API getAuthorization() du programmeur. Cette chaîne est propagée jusqu’au serveur principal d’autorisation du MVPD :

Dans l’application ou le site du programmeur, l’utilisateur choisit un MVPD compatible XACML (dans cet exemple, &quot;TNT&quot;). Pour plus d’informations sur XACML, voir [eXtensible Access Control Markup Language](https://en.wikipedia.org/wiki/XACML){target=_blank}.
L’application du programmeur forme une requête AuthZ qui inclut la ressource et ses métadonnées.  Cet exemple inclut une note MPAA de &quot;pg&quot; dans l’attribut media de l’élément channel :

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

L’authentification Adobe Pass prend en charge une autorisation plus granulaire, jusqu’au niveau de la ressource, lorsqu’elle est prise en charge par le MVPD et le programmeur. La ressource et ses métadonnées sont opaques à Adobe. L’objectif est d’établir un format standard pour spécifier l’ID de ressource et les métadonnées de manière normalisée, afin d’envoyer des ID de ressource à différents MVPD.

>[!NOTE]
>
>Si l’utilisateur choisit un MVPD compatible uniquement avec les canaux, l’authentification Adobe Pass extrait UNIQUEMENT le titre du canal (&quot;TNT&quot; dans l’exemple ci-dessus) et transmet uniquement le titre au MVPD.

### Flux d’Exchange MVPD vers les métadonnées de programmeur {#mvpd-progr-exchange-flow}

Adobe Pass Authentication émet les hypothèses suivantes :

* Le MVPD envoie l’évaluation maximale dans le cadre de la réponse SAML.
* Ces informations sont enregistrées dans le cadre du jeton d’authentification.
* Une API est fournie par l’authentification Adobe Pass pour permettre aux programmeurs de récupérer ces informations
* Les programmeurs implémentent cette fonctionnalité sur leur site ou application (par exemple, pour masquer les vidéos dont la note est supérieure à la note maximale de l’utilisateur).

```XML
<saml:Assertion ID="pfxec5f92e0-8589-3fc3-c708-f4fb8e2fad59"
                 IssueInstant="2010-07-20T10:05:41Z" Version="2.0"
                 xmlns:xs="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:AttributeStatement>
        <saml:Attribute
                Name="MaxTVRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">tv-ma</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute
                Name="MaxMovieRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">nc-17</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```

### Notes {#notes-mvpd-progr-metadata-exch-flow}

**Normalisation et validation des ressources.** Les ID de ressource peuvent être transmis sous forme de chaîne simple ou de chaîne MRSS. Un programmeur peut décider d&#39;utiliser le format de chaîne simple ou le MRSS, mais il aura besoin d&#39;un accord préalable avec le MVPD pour que le MVPD sache comment traiter cette ressource.

**ID de ressource et spécification des métadonnées.** L’authentification Adobe Pass utilise la norme RSS avec l’extension Media RSS pour spécifier une ressource et ses métadonnées. Avec l’extension Media RSS, l’authentification Adobe Pass prend en charge une grande variété de métadonnées, telles que les contrôles parentaux (via `<media:rating>`) ou la géolocalisation (`<media:location>`).

L’authentification Adobe Pass peut également prendre en charge la conversion transparente de la chaîne de canal héritée vers la ressource RSS correspondante pour les MVPD qui nécessitent RSS. Dans l’autre sens, l’authentification Adobe Pass prend en charge la conversion du titre RSS+MRSS en titre de canal brut, pour les MVPD de canal uniquement.

**L’authentification Adobe Pass assure une compatibilité descendante complète avec les intégrations existantes.** En d’autres termes, pour les programmeurs qui utilisent l’authentification au niveau du canal, l’authentification Adobe Pass prend soin de regrouper l’identifiant du canal dans le format nécessaire avant de l’envoyer à un MVPD qui comprend ce format. L’inverse s’applique également : si un programmeur spécifie toutes ses ressources dans un nouveau format, l’authentification Adobe Pass convertit le nouveau format en une chaîne de canal simple si l’autorisation est effectuée sur un MVPD qui ne fait que l’autorisation au niveau du canal.

## Cas d’utilisation des métadonnées utilisateur {#user-metadata-use-cases}

Les cas d’utilisation sont en constante évolution et se développent à mesure que de plus en plus de distributeurs multicanaux prennent des dispositions légales et ajoutent des fonctionnalités. Vous trouverez ci-dessous des exemples d’utilisation des métadonnées utilisateur.

* [Identifiant utilisateur MVPD](#mvpd-user-id)
* [Identifiant du foyer](#household-user-id)
* [Code postal](#zip-code)
* [Évaluation max. (contrôle parental)](#max-rating-parental-control)
* [Liaison de canal](#channel-line-up)

### Identifiant utilisateur MVPD {#mvpd-user-id}

* Tel que fourni par le MVPD
* Informations de connexion réelles de l’utilisateur, telles qu’elles sont hachées par le MVPD
* Peut être utilisé pour indiquer des problèmes avec ou pour des utilisateurs spécifiques
* Chiffré
* Prise en charge MVPD : tous les MVPD

### Identifiant utilisateur du foyer {#household-user-id}

* Permet d’obtenir de bonnes informations sur les mesures
* Chiffré
* Prise en charge MVPD : certains MVPD

### Code postal {#zip-code}

* Code postal de facturation de l’utilisateur
* Principalement utilisé pour appliquer les règles de période de gel des événements sportifs
* Peut être fourni avec la réponse AuthZ pour les mises à jour rapides.
* Prise en charge MVPD : certains MVPD

### Évaluation max. (contrôle parental) {#max-rating-parental-control}

* AuthN initialement, plus l’actualisation AuthZ
* Filtrage du contenu en dehors de l’interface utilisateur
* Scores MPAA ou VChip
* Prise en charge MVPD : certains MVPD

### Liaison de canal {#channel-line-up}

* Les MVPD peuvent fournir une liste des canaux que l’utilisateur est autorisé à afficher.
* Permet d’effectuer un aperçu rapide de l’interface utilisateur.
* La spécification OLCA permet cela sous la forme d’une instruction AttributeStatement dans la réponse AuthN.
* Prise en charge des MVPD : certains MVPD

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
