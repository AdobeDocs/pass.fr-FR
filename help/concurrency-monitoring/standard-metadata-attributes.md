---
title: Attributs de métadonnées standard
description: Attributs de métadonnées standard
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 0%

---


# Attributs de métadonnées standard {#std-metadata-attributes}

Cette page vise à fournir une liste exhaustive des attributs de métadonnées que le service de surveillance de la simultanéité peut traiter et qui peuvent être utilisés comme base pour les stratégies qui peuvent être mises en oeuvre. Les attributs de métadonnées standard peuvent être classés comme suit :

* Attributs inclus par conception (envoyés à chaque appel d’initialisation de session, car ils sont requis dans le chemin d’accès à l’URL). Aucun appel valide ne peut être effectué sans ces valeurs.
* Attributs de métadonnées : valeurs qui doivent être transmises en tant que données de formulaire lors de l’appel d’initialisation de session (si les stratégies du serveur principal requièrent leurs valeurs).

## Attributs requis par la conception {#attr-req-by-design}

L’API de surveillance de la simultanéité force les clients à envoyer les valeurs suivantes dans le cadre de tout appel d’initialisation valide : [appels de lancement de session](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| Nom du champ | Exemple de valeur | Où l’utiliser | Obtenu à partir de |
|-------------|---------------------------|--------------------|------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | En-tête d’autorisation | ticket Zendesk lors de l’intégration |
| mvpdName | Sample_MVPD | chemin URI | Authentification Adobe Pass à partir du point de terminaison de configuration lorsque l’utilisateur sélectionne le MVPD |
| accountId | 12345 | chemin URI | Authentification Adobe Pass métadonnées amontUserID après connexion de l’utilisateur [Métadonnées utilisateur amontUserID - Authentification Adobe Pass](/help/authentication/user-metadata-feature.md) |


## Attributs de métadonnées {#metadata-attr}

Les champs du tableau ci-dessous peuvent être utilisés par les programmeurs et les MVPD pour créer des stratégies qui seront mises en oeuvre dans la surveillance de la simultanéité.

Avec [API v2.0](http://docs.adobeptime.io/cm-api-v2/), si l’un de ces attributs est requis par les stratégies définies, une tentative d’initialisation de session sans cet attribut générera une requête 400 incorrecte.


| Entité | Nom de l’attribut | Type de données | Description | Référence externe (ex : EIDR, OATC) | Exemple de valeur | Règles de validation |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Media Company | programmerName | string | Nom du programmeur |                                                   | ProgrammerX |                                                                                   |
| Ressource | channel | string | Chaîne TV |                                                   | ChannelY |                                                                                   |
|                 | assetId | string | Titre &quot;convivial&quot; ou lisible par le client à présenter pour ce contenu | [Référence des champs de données EIDR 2.0](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | Ben-Hur |                                                                                   |
|                 | type | enumeration | Une valeur décrivant le type général de contenu représenté par TveItem. Les valeurs énumérées incluent : movie broadcastEpisode nonBroadcastEpisode musicVideo AwardsShow clip Concerconférence newsEvent sportingEvent trailer | [Flux de métadonnées OATC - Pratique recommandée](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | Le champ doit correspondre à l&#39;un des éléments de l&#39;énumération |
|                 | contentType | string | Ce champ détermine si le contenu demandé est actif ou VOD | N/A | live, vod | live or vod |
|                 | genre | string | Genre du contenu diffusé en continu. Décrit le type de programmation général | [Flux de métadonnées OATC recommandé](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} Pratique | Comédie | Type de genre valide |
|                 | durée | nombre | Durée de l’élément multimédia en secondes | [Flux de métadonnées OATC - Pratique recommandée](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | séquence de nombres |
| Appareil/navigateur | deviceId | string | Identifiant d’appareil unique. | [Propriétés Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | string | Nom convivial de cet appareil. |                                                   | IPad de Joe |                                                                                   |
|                 | marketingName | string | Nom marketing (ou nom convivial) d’un appareil | [Propriétés Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | IPHONE 6 | nom marketing valide |
|                 | mobileDevice | boolean | True si l’appareil est destiné à être utilisé lors du déplacement | [Propriétés Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | string | Nom du modèle de l’appareil, du navigateur ou d’un autre composant. | [Propriétés Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | tablette, téléphone, xbox. set-top box | nom de modèle de périphérique valide |
|                 | osName | string | Système d’exploitation en cours d’exécution du périphérique | [Device Atlas - Valeurs de propriété prédéfinies du système d’exploitation](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, Autre Remarque : vous devez être connecté avec un nom d’utilisateur et un mot de passe dans Device Atlas pour afficher les valeurs de propriété. | la valeur attendue est l’une des valeurs des propriétés prédéfinies de Device Atlas. |
|                 | browserName | string | Nom ou type de navigateur sur l’appareil | [Device Atlas : valeurs de propriété prédéfinies du navigateur](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | Nom ou type du navigateur sur l’appareil.  Remarque : vous devez être connecté avec un nom d’utilisateur et un mot de passe dans Device Atlas pour afficher les valeurs de propriété. | la valeur attendue est l’une des valeurs des propriétés prédéfinies de Device Atlas. |
|                 | browserVersion | string | Version du navigateur sur l’appareil | [Propriétés Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | Version du navigateur sur l’appareil |                                                                                   |
| Application | applicationName | string | Nom &quot;convivial&quot; ou lisible par le client de l’application | N/A | Sample_Application |                                                                                   |
|                 | applicationId | string | ID d’application qui identifie de manière unique une application cliente. | N/A | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | string | Plateforme native de l’application | N/A | ios, android |                                                                                   |
|                 | applicationVersion | string | Cette valeur peut être utilisée à des fins d’analyse. | N/A | 1.0, 2.0 |                                                                                   |
| Objet | accountId | string | ID de compte du sujet de la surveillance de la simultanéité (dans le cadre du MVPD) | N/A | test-account |                                                                                   |
|                 | contractType | string | premium, de base. Les clients sont libres de l’ajouter en tant que métadonnées personnalisées et de l’utiliser dans leurs propres domaines | N/A | premium, de base |                                                                                   |
| Utilisateur | name | string | Certains MVPD fournissent des informations relatives à l’utilisateur spécifique qui lit du contenu. | N/A |                                                                                                                                                         |                                                                                   |
|                 | hba | boolean | Indique si l’utilisateur tente de lancer le flux à partir de son emplacement d’accueil. | N/A | true, false | true ou false |
| Emplacement | continent | string | Le continent d’où provient l’identifiant de l’appareil qui envoie la requête de lecture | N/A | Amérique du Nord | Nom de continent valide |
|                 | country | string | Pays d’où provient l’identifiant de l’appareil qui envoie la requête de lecture. | N/A | USA | nom du pays valide |
|                 | state | string | L’état d’origine de l’identifiant de l’appareil qui envoie la requête de lecture | N/A | CA | nom d’état valide |
|                 | city | string | Ville d’où provient l’identifiant de l’appareil qui envoie la requête de lecture | N/A | Cuba | nom de ville valide |
|                 | zipcode | nombre | Code postal d’où provient l’identifiant de l’appareil qui envoie la requête de lecture. | N/A | 95014 | code postal valide |
| Diffusion | streamId | string | Générée par le service CM, et non sous le contrôle du client. Est utilisé implicitement lorsque des règles de type maxstream sont définies. | N/A | N/A | N/A |
|                 | streamCDN | string | indique le réseau de diffusion de contenu à partir duquel le flux a été récupéré. | N/A | N/A | N/A |

## Exemples d’utilisation d’attributs de métadonnées pour la création de stratégies {#examples-metadata-attr}

Les champs de métadonnées standard peuvent être utilisés pour définir des stratégies côté serveur en fonction de leurs valeurs de champ :

* Vous pouvez configurer une stratégie de sorte qu’elle s’applique uniquement à des valeurs de champ spécifiques (par exemple, une stratégie iOS dédiée : où `osType` is `iOS`)
* Vous pouvez limiter le nombre de valeurs distinctes pour un champ donné. Voici quelques exemples :
   * pas plus de X appareils distincts : `HAVING DISTINCT COUNT(deviceId) <= 2`
   * pas plus de X codes postaux distincts : `HAVING DISTINCT COUNT(zipcode) <= 3`
* Vous pouvez limiter le nombre de diffusions actives par valeur de champ. Voici quelques exemples :
   * pas plus de X diffusions actives pour un seul type d’appareil : `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * pas plus de X diffusions actives pour les flux de contenu en direct : `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

Contactez l’équipe de surveillance de la simultanéité par [création d’un ticket dans Zendesk](mailto:tve-support@adobe.com) et indiquent les stratégies que vous souhaitez avoir mises en oeuvre.

Vous trouverez d’autres exemples de stratégies et de livres de cookie d’intégration dans les rubriques suivantes :

* [Point de décision politique](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [Console API - Surveillance de la simultanéité des Adobes](http://docs.adobeptime.io/cm-api-v2/)
