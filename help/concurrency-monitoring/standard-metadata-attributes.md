---
title: Attributs de métadonnées standard
description: Attributs de métadonnées standard
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 4%

---

# Attributs de métadonnées standard {#std-metadata-attributes}

Cette page vise à fournir une liste exhaustive des attributs de métadonnées que le service de surveillance simultanée peut traiter et qui peuvent être utilisés comme base pour les politiques qui peuvent être mises en œuvre. Les attributs de métadonnées standard peuvent être classés comme suit :

* Attributs inclus par conception (envoyés à chaque appel d’initialisation de session, car ils sont requis dans le chemin d’accès à l’URL). Aucun appel valide ne peut être effectué sans ces valeurs.
* Attributs de métadonnées : valeurs qui doivent être transmises en tant que données de formulaire lors de l’appel d’initialisation de la session (si les politiques du serveur principal le demandent).

## Attributs requis par la conception {#attr-req-by-design}

L’API de surveillance de concurrence force les clients à envoyer les valeurs suivantes dans le cadre de tout appel d’initialisation valide : [appels d’initiation de session](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| Nom du champ | Exemple de valeur | Où l’utiliser | Obtenu à partir de |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | En-tête d’autorisation | Ticket Zendesk à l’intégration |
| mvpdName | Sample_MVPD | Chemin URI | Authentification Adobe Pass à partir du point d’entrée de configuration lorsque l’utilisateur sélectionne le MVPD |
| accountId | 12345 | Chemin URI | Métadonnées amontUserID de l’authentification Adobe Pass après la connexion de l’utilisateur [Métadonnées utilisateur amontUserID - Authentification Adobe Pass](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## Attributs de métadonnées {#metadata-attr}

Les champs du tableau ci-dessous peuvent être utilisés par les programmeurs et les MVPD pour créer des politiques qui seront implémentées dans la surveillance de simultanéité.

Avec [API v2.0](http://docs.adobeptime.io/cm-api-v2/), si l’un de ces attributs est requis par les politiques définies, une tentative d’initialisation de session sans cet attribut entraînera une requête incorrecte de 400.


| Entité | Nom de l’attribut | Type de données | Description | Référence externe (ex : EIDR, OATC) | Exemple de valeur | Règles de validation |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Société du média | programmerName | chaîne | Nom du programmeur |                                                   | ProgrammeurX |                                                                                   |
| Ressource | canal | chaîne | La chaîne TV |                                                   | ChannelY |                                                                                   |
|                 | assetId | chaîne | Titre « convivial » ou lisible par le consommateur à présenter pour ce contenu | [Référence des champs de données EIDR 2.0](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | Ben-Hur |                                                                                   |
|                 | type | énumération | Valeur décrivant le type général de contenu représenté par TveItem. Les valeurs énumérées incluent : film broadcastÉpisode nonDiffusionMusique d&#39;épisodePrix vidéoMontrer clip concert conférence newsÉvénement sportifBande-annonce d&#39;événement | [Pratique recommandée pour les flux de métadonnées OATC](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | Le champ doit correspondre à l’un des éléments de l’énumération . |
|                 | contentType | chaîne | Ce champ détermine si le contenu demandé est en ligne ou VOD. | S/O | live, vod | live ou vod |
|                 | genre | chaîne | Genre du contenu diffusé en continu. Décrit le type de programmation général | [Flux de métadonnées OATC recommandé ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} pratique | Comédie | Type de genre valide |
|                 | durée | nombre | Durée de l’élément média en secondes | [Pratique recommandée pour les flux de métadonnées OATC](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | souche de numéros |
| Appareil/Navigateur | deviceId | chaîne | Identifiant d’appareil unique. | [Propriétés de Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | chaîne | Nom convivial de cet appareil. |                                                   | IPad de Joe |                                                                                   |
|                 | marketingName | chaîne | Nom marketing (ou nom convivial) d’un appareil | [Propriétés de Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | nom marketing valide |
|                 | mobileDevice | booléen | True si l&#39;appareil est destiné à être utilisé en déplacement | [Propriétés de Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | chaîne | Nom du modèle de l’appareil, du navigateur ou de tout autre composant | [Propriétés de Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | tablette, téléphone, xbox. décodeur | nom de modèle d’appareil valide |
|                 | osName | chaîne | Système d’exploitation que l’appareil exécute | [Device Atlas - Valeurs de propriétés prédéfinies du système d’exploitation](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, Autre Remarque : Vous devez être connecté avec un nom d&#39;utilisateur et un mot de passe dans Device Atlas pour voir les valeurs de propriété | la valeur attendue correspond à l’une des valeurs des propriétés prédéfinies de Device Atlas |
|                 | browserName | chaîne | Nom ou type du navigateur sur l’appareil | [Device Atlas - Valeurs de propriété prédéfinies du navigateur](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | Nom ou type du navigateur sur l’appareil.  Remarque : vous devez être connecté avec un nom d’utilisateur et un mot de passe dans Device Atlas pour afficher les valeurs de propriété | la valeur attendue correspond à l’une des valeurs des propriétés prédéfinies de Device Atlas |
|                 | browserVersion | chaîne | Version du navigateur sur l’appareil | [Propriétés de Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | Version du navigateur sur l’appareil |                                                                                   |
| Application | applicationName | chaîne | Nom de l’application « convivial » ou lisible par le consommateur | S/O | Sample_Application |                                                                                   |
|                 | applicationId | chaîne | Identifiant d’application qui identifie de manière unique une application cliente. | S/O | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | chaîne | Plateforme native de l’application | S/O | ios, android |                                                                                   |
|                 | applicationVersion | chaîne | Cette valeur peut être utilisée à des fins d’analyse | S/O | 1.0, 2.0 |                                                                                   |
| Objet | accountId | chaîne | Identifiant de compte de l’objet Surveillance d’accès simultané (dans le champ d’application du MVPD) | S/O | compte-test |                                                                                   |
|                 | ContractType | chaîne | premium, de base. Les clients sont libres de l’ajouter en tant que métadonnées personnalisées et de l’utiliser dans leurs propres domaines | S/O | premium, de base |                                                                                   |
| Utilisateur | name | chaîne | Certains fichiers MVPD fournissent des informations relatives à l’utilisateur spécifique qui lit du contenu. | S/O |                                                                                                                                                         |                                                                                   |
|                 | hba | booléen | Indique si l’utilisateur tente de lancer le flux à partir de son emplacement d’origine | S/O | true, false | vrai ou faux |
| Lieu | continent | chaîne | Continent d’où provient l’ID d’appareil qui envoie la demande de lecture | S/O | Amérique du Nord | nom de continent valide |
|                 | pays | chaîne | Pays d’où provient l’ID d’appareil qui envoie la demande de lecture | S/O | USA | nom de pays valide |
|                 | état | chaîne | État d’où provient l’ID d’appareil qui envoie la demande de lecture | S/O | CA | nom d’état valide |
|                 | ville | chaîne | Ville d’où provient l’ID d’appareil qui envoie la demande de lecture | S/O | Cupertino | nom de ville valide |
|                 | zipcode | nombre | Code postal d’où provient l’ID d’appareil qui envoie la demande de lecture | S/O | 95014 | code postal valide |
| Stream | streamId | chaîne | Générée par le service CM, et non sous le contrôle du client. Utilisé implicitement lorsque des règles de type maxstreams sont définies. | S/O | S/O | S/O |
|                 | streamCDN | chaîne | indique le réseau CDN à partir duquel le flux a été récupéré | S/O | S/O | S/O |

## Exemples d’utilisation des attributs de métadonnées pour la création de politiques {#examples-metadata-attr}

Les champs de métadonnées standard peuvent être utilisés pour définir des politiques côté serveur en fonction de leurs valeurs de champ :

* Vous pouvez configurer une politique pour qu’elle s’applique uniquement à des valeurs de champ spécifiques (par exemple, une politique iOS dédiée : où `osType` est `iOS`)
* Vous pouvez limiter le nombre de valeurs distinctes pour un champ donné. Voici quelques exemples :
   * pas plus de X appareils distincts : `HAVING DISTINCT COUNT(deviceId) <= 2`
   * pas plus de X codes postaux distincts : `HAVING DISTINCT COUNT(zipcode) <= 3`
* Vous pouvez limiter le nombre de flux actifs par valeur de champ. Voici quelques exemples :
   * pas plus de X flux actifs pour un seul type d’appareil : `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * pas plus de X flux actifs pour les flux de contenu en direct : `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

Contactez l’équipe de surveillance de l’accès simultané en [créant un ticket dans Zendesk](mailto:tve-support@adobe.com) et indiquez les politiques que vous souhaitez mettre en œuvre.

Vous trouverez d’autres exemples de politiques et de livres de cookie d’intégration dans les sections suivantes :

* [Point de décision de politique](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [Console d’API - Surveillance d’Adobe simultané](http://docs.adobeptime.io/cm-api-v2/)
