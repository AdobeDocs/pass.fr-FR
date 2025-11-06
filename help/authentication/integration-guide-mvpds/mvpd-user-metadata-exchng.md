---
title: Échange de métadonnées d’utilisateur MVPD
description: Échange de métadonnées d’utilisateur MVPD
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# Échange de métadonnées d’utilisateur MVPD

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro-user-metadata-exchange}

Les MVPD conservent des métadonnées spécifiques à l’utilisateur concernant ses clients, qui sont parfois partagées avec les programmeurs. L’objectif de l’authentification Adobe Pass est de faciliter l’échange de ces « métadonnées utilisateur », mais pas d’appliquer des règles concernant l’échange. Les règles d&#39;échange sont pour que les MVPD travaillent avec leurs partenaires de programmation.

Les types de métadonnées utilisateur actuellement disponibles pour échange sont les suivants :

* Code postal
* Note maximale (Vichip ou MPAA)
* Identifiant utilisateur
* ID de ménage
* Identifiant du canal

Grâce à cette fonctionnalité, les MVPD et les programmeurs peuvent implémenter des cas d’utilisation spéciaux tels que le contrôle parental. Par exemple, un MVPD peut transmettre les données d’évaluation parentale à un programmeur, qui les utilise ensuite pour filtrer les choix de visionnage disponibles pour un utilisateur.

Points clés des métadonnées de l’utilisateur :

* Le MVPD transmet les métadonnées de l’utilisateur à l’application du programmeur pendant les flux d’authentification et d’autorisation
* L’authentification Adobe Pass enregistre les valeurs des métadonnées dans les jetons AuthN et AuthZ
* L’authentification Adobe Pass peut normaliser les valeurs des fichiers MVPD qui fournissent des métadonnées utilisateur dans différents formats
* Certains paramètres peuvent être chiffrés à l’aide de la clé du programmeur
* Les valeurs spécifiques sont mises à disposition par Adobe, via une modification de configuration

>[!NOTE]
>
>Les métadonnées utilisateur sont une extension des métadonnées statiques (TTL de jeton d’authentification, TTL de jeton d’autorisation et ID d’appareil) précédemment disponibles dans l’authentification Adobe Pass.

## Exemples {#example-mvpd-user-metadata-exch}

### Contrôle Parental {#example-parental-control}

Cet exemple illustre l’échange des éléments suivants :

* [Programmeur vers MVPD Metadata Exchange](#progr-mvpd-metadata-exch)

* [Flux d’échange de métadonnées entre MVPD et le programmeur](#mvpd-progr-exchange-flow)

### Programmeur vers MVPD Metadata Exchange {#progr-mvpd-metadata-exch}

Actuellement, l’API de programmation, l’authentification Adobe Pass et les ordonnateurs MVPD ne prennent en charge que l’autorisation au niveau du canal. Le canal est spécifié sous la forme d’une chaîne de texte brut dans l’appel API getAuthorization() du programmeur. Cette chaîne est propagée jusqu’au serveur principal d’autorisation de MVPD :

À partir de l’application ou du site du programmeur, l’utilisateur choisit un MVPD compatible XACML (dans cet exemple, « TNT »). Pour plus d’informations sur XACML, voir [Langage de balisage de contrôle d’accès eXtensible](https://en.wikipedia.org/wiki/XACML){target=_blank}.
L’application du programmeur forme une requête AuthZ qui inclut la ressource et ses métadonnées.  Cet exemple inclut une évaluation MPAA de « pg » dans l’attribut media de l’élément channel :

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

L’authentification Adobe Pass prend en charge une autorisation plus granulaire, jusqu’au niveau de la ressource, lorsqu’elle est prise en charge par le MVPD et le programmeur. La ressource et ses métadonnées sont opaques pour Adobe. L’objectif est d’établir un format standard pour spécifier l’ID de ressource et les métadonnées de manière normalisée, afin d’envoyer les ID de ressource à différents MVPD.

>[!NOTE]
>
>Si l’utilisateur choisit un MVPD compatible uniquement avec les canaux, l’authentification Adobe Pass extrait UNIQUEMENT le titre du canal (« TNT » dans l’exemple ci-dessus) et transmet uniquement le titre au MVPD.

### Flux d’échange de métadonnées entre MVPD et le programmeur {#mvpd-progr-exchange-flow}

L’authentification Adobe Pass repose sur les hypothèses suivantes :

* Le MVPD envoie l’évaluation maximale dans le cadre de la réponse SAML
* Ces informations sont enregistrées dans le jeton d’authentification
* Une API est fournie par l’authentification Adobe Pass pour permettre aux programmeurs de récupérer ces informations
* Les programmeurs implémentent cette fonctionnalité sur leur site ou application (par exemple, pour masquer les vidéos qui dépassent la note maximale pour l’utilisateur)

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

**Normalisation et validation des ressources.** identifiants de ressource peuvent être transmis sous la forme d’une chaîne simple ou d’une chaîne MRSS. Un programmeur peut décider d’utiliser soit le format de chaîne simple, soit le format MRSS, mais il aura besoin d’un accord préalable avec le MVPD afin que le MVPD sache comment traiter cette ressource.

**Identifiant de ressource et spécification de métadonnées.**’authentification Adobe Pass utilise la norme RSS avec l’extension Media RSS pour spécifier une ressource et ses métadonnées. Conjointement avec l’extension Media RSS, l’authentification Adobe Pass prend en charge un large éventail de métadonnées, telles que le contrôle parental (via `<media:rating>`) ou la géolocalisation (`<media:location>`).

L’authentification Adobe Pass peut également prendre en charge la conversion transparente de la chaîne de canal héritée vers la ressource RSS correspondante pour les MVPD qui nécessitent RSS. Dans l’autre sens, l’authentification Adobe Pass prend en charge la conversion de RSS+MRSS en titre de canal brut, pour les fichiers MVPD de canal uniquement.

**L’authentification Adobe Pass garantit une compatibilité ascendante totale avec les intégrations existantes.** En d’autres termes, pour les programmeurs qui utilisent l’authentification au niveau du canal, l’authentification Adobe Pass prend soin de compresser l’identifiant du canal dans le format nécessaire avant de l’envoyer à un MVPD qui comprend ce format. L’inverse s’applique également : si un programmeur spécifie toutes ses ressources dans un nouveau format, l’authentification Adobe Pass convertit le nouveau format en une simple chaîne de canal si elle autorise uniquement sur un MVPD qui effectue une autorisation au niveau du canal.

## Cas D’Utilisation Des Métadonnées Utilisateur {#user-metadata-use-cases}

Les cas d’utilisation sont en constante évolution et se développent à mesure que de plus en plus de MVPD concluent des accords juridiques et ajoutent des fonctionnalités. Vous trouverez ci-dessous des exemples d’utilisation des métadonnées utilisateur.

* [ID d’utilisateur MVPD](#mvpd-user-id)
* [ID de ménage](#household-user-id)
* [Code postal](#zip-code)
* [Évaluation Max (Contrôle Parental)](#max-rating-parental-control)
* [Alignement des canaux](#channel-line-up)

### ID d’utilisateur MVPD {#mvpd-user-id}

* Selon les indications du MVPD
* Pas les informations de connexion réelles de l’utilisateur, car elles sont hachées par le MVPD
* Peut être utilisé pour indiquer des problèmes liés à ou pour des utilisateurs et utilisatrices spécifiques
* Chiffré
* Prise en charge de MVPD : tous les MVPD

### ID d&#39;utilisateur de ménage {#household-user-id}

* Permet d’obtenir des informations de mesure correctes
* Chiffré
* Prise en charge de MVPD : certains MVPD

### Code postal {#zip-code}

* Code postal de facturation de l’utilisateur
* Principalement utilisé pour appliquer les règles de période de gel des événements sportifs
* Peut être fourni avec la réponse AuthZ pour des mises à jour rapides
* Prise en charge de MVPD : certains MVPD

### Évaluation Max (Contrôle Parental) {#max-rating-parental-control}

* Initialement, avec actualisation AuthZ
* Filtrer le contenu en dehors de l’interface utilisateur
* Évaluations MPAA ou VChip
* Prise en charge de MVPD : certains MVPD

### Alignement des canaux {#channel-line-up}

* Les fichiers MVPD peuvent fournir une liste des canaux que l’utilisateur est autorisé à afficher
* Permet de peindre rapidement l’interface utilisateur
* La spécification OLCA l’autorise en tant qu’AttributeStatement dans la réponse AuthN
* Prise en charge des MVPD : certains MVPD

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
