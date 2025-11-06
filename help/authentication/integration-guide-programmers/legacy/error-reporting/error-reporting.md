---
title: Rapport d’erreurs
description: Rapport d’erreurs
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3011'
ht-degree: 2%

---

# Rapport d’erreurs (hérité) {#error-reporting}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Vue d’ensemble {#overview}

Le rapport d’erreur dans l’authentification Adobe Pass est actuellement implémenté de deux manières différentes :

* **Rapports d’erreur avancés** L’implémentateur enregistre un rappel d’erreur dans le cas du [SDK JavaScript AccessEnabler](#accessenabler-javascript-sdk) ou implémente une méthode d’interface nommée « `status` » dans le cas du [SDK iOS/tvOS AccessEnabler](#accessenabler-ios-tvos-sdk) et du [SDK Android AccessEnabler](#accessenabler-android-sdk), afin de recevoir des rapports d’erreur avancés. Les erreurs sont classées en types **Informations**, **Avertissement** et **Erreur**. Ce système de reporting est **asynchrone**, dans la mesure où **il n’existe aucune garantie quant à l’ordre dans lequel les erreurs multiples seront déclenchées**.  Pour plus d’informations sur le système de rapport d’erreur avancé, voir la section [Rapport d’erreur avancé](#advanced-error-reporting).

* **Rapport d’erreur d’origine -** Système de rapport statique dans lequel des messages d’erreur sont transmis à des fonctions de rappel spécifiques en cas d’échec de requêtes spécifiques. Les erreurs sont regroupées en types génériques, d’authentification et d’autorisation. Pour obtenir la liste des erreurs signalées dans le système d’origine, reportez-vous à la section [Rapport d’erreur d’origine](#original-error-reporting).


## Rapports d’erreur avancés {#advanced-error-reporting}

* [SDK JavaScript AccessEnabler](#accessenabler-javascript-sdk)
* [SDK AccessEnabler iOS/tvOS](#accessenabler-ios-tvos-sdk)
* [SDK AccessEnabler Android](#accessenabler-android-sdk)
* [SDK FireOS AccessEnabler](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>L’ancienne API [Rapport d’erreur d’origine](#original-error-reporting) continuera à fonctionner comme avant. Les rapports d’erreur avancés ne rompent pas la fonctionnalité, mais les rapports d’erreur d’origine ne recevront PLUS de mises à jour. Toutes les nouvelles erreurs et mises à jour sont envoyées au système de rapports d’erreurs avancé.

### SDK JavaScript AccessEnabler {#accessenabler-javascript-sdk}

Le nouveau système de rapport d’erreur reste facultatif. Par conséquent, l’implémentateur peut enregistrer explicitement un rappel de gestionnaire d’erreurs pour recevoir des rapports d’erreur avancés. Le système offre la possibilité d’enregistrer et de désenregistrer dynamiquement plusieurs rappels d’erreur. En outre, vous pouvez enregistrer de nouveaux rappels d’erreur dès que le SDK JavaScript AccessEnabler est chargé, sans avoir à effectuer d’autres initialisations (avant d’appeler `setRequestor()`), ce qui signifie que vous pouvez recevoir des rapports avancés sur les erreurs d’initialisation et de configuration.


#### Mise en œuvre {#access-enab-js-imp}

yourErrorHandler(errorData:Object)


Votre fonction de rappel de gestionnaire d’erreurs recevra un seul objet (une carte) avec la structure suivante :

```JavaScript
    {
      errorId: "CFG410",
      level: "error",
      message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

### &#x200B;1. Liaison {#bind}

**`.bind(eventType:String, handlerName:String):void`**

Associe un gestionnaire pour un événement.

**`eventType`** - SEULE la valeur « `errorEvent` » entraîne le déclenchement de rappels de rapports d’erreur avancés par le SDK JavaScript AccessEnabler.

**`handlerName`** - une chaîne spécifiant le nom de la fonction de gestionnaire d’erreurs.


Les deux paramètres de liaison doivent utiliser uniquement des caractères du jeu suivant : `[0-9a-zA-Z][-._a-zA-Z0-9]` ; en d’autres termes, les paramètres doivent commencer par un nombre ou une lettre et ne peuvent ensuite inclure que des tirets, des points, des traits de soulignement et des caractères alphanumériques.  En outre, les paramètres ne peuvent pas dépasser 1 024 caractères.

**Exemple** de gestionnaires d’erreur de liaison :

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

En raison de limitations techniques, vous ne pouvez pas lier une fonction de fermeture ou anonyme. Vous devez spécifier le nom de la méthode dans le deuxième paramètre.


### &#x200B;2. Dissocier {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

Supprime un gestionnaire d&#39;événements précédemment associé.

**`eventType`** - SEULE la valeur « `errorEvent` » entraîne le déclenchement de rappels de rapports d’erreur avancés par le SDK JavaScript AccessEnabler.

**`handlerName`** - Une chaîne spécifiant le nom de la fonction de gestionnaire d’erreurs, si est null ou s’il manque tous les gestionnaires joints pour la `eventType` spécifiée sera supprimée.

Les deux paramètres de liaison doivent utiliser uniquement des caractères du jeu suivant : `[0-9a-zA-Z][-._a-zA-Z0-9]` ; en d’autres termes, les paramètres doivent commencer par un nombre ou une lettre et ne peuvent ensuite inclure que des tirets, des points, des traits de soulignement et des caractères alphanumériques.  En outre, les paramètres ne peuvent pas dépasser 1 024 caractères.

**Exemple** de suppression d’un seul gestionnaire d’erreurs :

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**Exemple** suppression de tous les gestionnaires d’erreur :

`accessEnabler.unbind('errorEvent');`


### SDK AccessEnabler iOS/tvOS {#accessenabler-ios-tvos-sdk}

Le nouveau système de rapport d’erreurs est obligatoire. Par conséquent, l’implémentateur doit explicitement se conformer au nouveau protocole « EntitlementStatus » de l’objectif C. Cette nouvelle approche permet aux programmeurs de recevoir des rapports d’erreur avancés.

#### Mise en œuvre {#accessenab-ios-tvossdk-imp}

Un implémenteur doit se conformer au protocole **EntitlementStatus** suivant :

**EntitlementStatus.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

Votre fonction **status** recevra un seul objet (un `NSDictionary`) avec la structure suivante :

```OBJ-C
    {
          errorId: "CFG410",
          level: "error",
          message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

**1. Déclaration**

```OBJ-C
    @interface MyEntitlementStatusDelegate : NSObject <EntitlementStatus>
```

**2. Implémentation**

```OBJ-C
    @implementation DemoAppAppDelegate     
    // very simple implementation
    - (void)status:(NSDictionary *)statusDict {
        NSLog(@"%@:\n%@", statusDict[@"level"], statusDict);
    }
```


### SDK AccessEnabler Android {#accessenabler-android-sdk}

Le nouveau système de rapport d’erreur est obligatoire, car l’implémentateur doit se conformer explicitement au protocole défini par l’interface `IAccessEnablerDelegate`. Cette nouvelle approche permet aux programmeurs de recevoir des rapports d’erreur avancés.

#### Mise en œuvre {#access-enablr-androidsdk-imp}

Un implémenteur doit gérer la nouvelle méthode `status` à partir de l’interface `IAccessEnablerDelegate`. La fonction **`status`** recevra un seul objet **`AdvancedStatus`** ayant le modèle suivant :

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Exemple**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

### SDK FireOS AccessEnabler {#accessenabler-fireos-sdk}


Le nouveau système de rapport d’erreur est obligatoire, car l’implémentateur doit se conformer explicitement au protocole défini par l’interface `IAccessEnablerDelegate`. Cette nouvelle approche permet aux programmeurs de recevoir des rapports d’erreur avancés.

#### Mise en œuvre {#access-enab-fireos-sdk-}

Un implémenteur doit gérer la nouvelle `status`méthode à partir de l’interface`IAccessEnablerDelegate`. La fonction **`status`** recevra un seul objet **`AdvancedStatus`** ayant le modèle suivant :

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Exemple**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

## Référence des codes d’erreur avancés {#advanced-error-codes-reference}

Le tableau suivant répertorie et décrit les codes d’erreur exposés par la nouvelle API d’erreur, ainsi que les actions suggérées pour les corriger :

| ID | Niveau | Description | Actions du développeur | Actions de l’utilisateur | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL ET AAPL_ERROR | Erreur | Erreur SSO générique d’Apple | L’erreur contient un champ de détails avec l’erreur VSA d’origine. | s.o. | s.o. | Oui | s.o. |
| VSA203 | Infos | L’utilisateur a décidé de se déconnecter de l’application pendant l’authentification suite à une connexion via le SSO de la plateforme. | Demandez/demandez à l’utilisateur de se déconnecter explicitement de Paramètres -> Comptes -> Fournisseur de télévision sur tvOS. <br><br> Demander à l’utilisateur de se déconnecter explicitement de Paramètres -> Fournisseur de télévision sur iOS/iPadOS. | Déconnectez-vous explicitement de Paramètres -> Comptes -> Fournisseur TV sur tvOS. <br> <br> Se déconnecter explicitement de Paramètres -> Fournisseur TV sur iOS/iPadOS | s.o. | Oui | s.o. |
| VSA404 | Infos | L&#39;autorisation du compte d&#39;abonné de la vidéo de l&#39;application est indéterminée. | Incitez les utilisateurs qui refusent de donner l’autorisation d’accéder aux informations d’abonnement en expliquant les avantages de l’expérience utilisateur SSO (authentification unique). | L’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès au fournisseur de télévision) ou à la section Paramètres -> Fournisseur de télévision sur iOS/iPadOS ou Paramètres -> Comptes -> Fournisseur de télévision sur tvOS. | s.o. | Oui | s.o. |
| VSA503 | Infos | Échec de la requête de métadonnées du compte d&#39;abonné vidéo de l&#39;application. | Le point d’entrée MVPD ne répond pas. L’application peut revenir au flux d’authentification normal. | s.o. | s.o. | Oui | s.o. |
| 500 | Erreur | Erreur interne | Utilisez AccessEnablerDebug et examinez les journaux de débogage (sortie console.log) afin de déterminer ce qui s’est passé. | s.o. | Oui | Oui | s.o. |
| SEC403 | Erreur | Erreur de sécurité du domaine. Le demandeur utilise un domaine non valide. Tous les domaines utilisés par un ID de demandeur particulier doivent être whitelistés par Adobe. | - Chargez AccessEnabler uniquement à partir de la liste des domaines autorisés <br> <br> - Contactez Adobe afin de gérer la liste autorisée de domaine pour l’ID de demandeur utilisé <br> <br> - iOS : vérifiez que vous utilisez le certificat correct et que la signature est créée correctement | s.o. | s.o. | Oui | s.o. |
| SEC412 | Attention | [Disponible dans la version 2.5] l’ID d’appareil ne correspond pas. Cela peut se produire chaque fois que la plateforme sous-jacente modifie son identifiant d’appareil. Dans ce cas, les jetons existants seront effacés et l’utilisateur ne sera plus authentifié. Notez que cela se produit légitimement lorsque l’utilisateur utilise le SDK JS et qu’il est en itinérance (sur JS, l’adresse IP du client fait partie de l’identifiant de l’appareil). Sinon, cela peut indiquer une tentative de fraude, c’est-à-dire une tentative de copier des jetons à partir d’un autre appareil. | - Surveillez le nombre d’avertissements. Si elles augmentent sans raison apparente (aucune mise à jour récente du navigateur ; nouveaux systèmes d’exploitation), cela peut être un indicateur de tentatives de fraude.  <br> <br>- Vous pouvez éventuellement informer l’utilisateur qu’il doit se reconnecter. | Reconnectez-vous. | Oui | Oui | Oui à partir de la version 3.2 |
| SEC420 | Erreur | Erreur de sécurité HTTP lors de la communication avec les serveurs d’authentification Adobe Pass. Cette erreur se produit généralement lorsque des usurpations/proxies sont en place. | - Chargez `[https://]{SP_FQDN\}` dans le navigateur et acceptez manuellement les certificats SSL, par exemple, **https://api.auth.adobe.com** ou **https://api.auth-staging.adobe.com** <br> <br>- Marquer les certificats du proxy comme approuvés | Si cela se produit pour un utilisateur régulier, cela indique la possibilité d’une attaque par un homme du milieu. | Oui | Oui | Oui à partir de la version 3.2 |
| CFG100 | Attention | La date / l&#39;heure / le fuseau horaire de l&#39;ordinateur client n&#39;est pas défini correctement. Cela entraînera probablement des erreurs d’authentification/d’autorisation. | - Indiquez à l’utilisateur ou à l’utilisatrice l’heure correcte. <br> <br> Agissez pour empêcher les flux de droits, car ils risquent d’échouer. | Définissez la date/l’heure correcte. | Oui | Oui | Oui à partir de la version 3.2 |
| CFG400 | Erreur | Un ID de demandeur non valide a été fourni. | Le développeur DOIT spécifier un ID de demandeur valide. | s.o. | Oui | Oui | Oui à partir de la version 3.2 |
| CFG404 | Erreur | Les serveurs d’authentification Adobe Pass sont introuvables. Cela peut se produire dans 3 instances : <br><br> - Le développeur a mis en place une usurpation non valide. <br><br> - L&#39;utilisateur a des problèmes de mise en réseau et ne peut pas atteindre les domaines d&#39;authentification Adobe Pass. <br><br> - Les serveurs d’authentification Adobe Pass sont mal configurés. <br><br>  **Remarque :** sur Firefox, CFG400 apparaît à la place de CFG404 (limitation du navigateur) | - Vérification de la mystification. <br><br> - Vérifiez les paramètres réseau/DNS. <br><br> -Informer Adobe. | Vérifiez les paramètres réseau/DNS. | Oui | Oui | Oui à partir de la version 3.2 |
| CFG410 | Erreur | AccessEnabler est trop ancien. | Informez l’utilisateur de vider les caches. | Effacez la mémoire cache du navigateur. | Oui | s.o. | Oui à partir de la version 3.2 |
| CFG5xx | Erreur | Les serveurs d’authentification Adobe Pass rencontrent des erreurs internes. xx peut être n’importe quel nombre. | - Informer l&#39;utilisateur de l&#39;indisponibilité de l&#39;authentification Adobe Pass. <br><br> - Contournement de l’authentification Adobe Pass. <br> <br> - Informez Adobe. | Réessayez plus tard. | Oui | Oui | Oui à partir de la version 3.2 |
| N000 | Infos | L’utilisateur n’est pas authentifié. | s.o. | Connexion. | Oui | Oui | Oui à partir de la version 3.2 |
| N001 | Infos | Une tentative d&#39;authentification passive a été lancée en arrière-plan. Cela se produit pour les fichiers MVPD configurés avec « Authentification par demandeur ». L’utilisateur sera, espérons-le, automatiquement authentifié, mais cela entraînera une baisse des performances à l’initialisation. | Informez éventuellement l’utilisateur ou l’utilisatrice, ou présentez une interface utilisateur qui l’avertit, que « des travaux sont en cours ». | Attendez. | Oui | Oui | Oui à partir de la version 3.2 |
| N003 | Infos | L’utilisateur sélectionne l’option « Autre fournisseur TV » dans le sélecteur MVPD d’Apple. | Le rappel *displayProviderDialog* sera appelé et l’application pourra revenir au flux d’authentification standard. | Sélectionnez MVPD standard et continuez avec l’écran de connexion. | s.o. | Oui | s.o. |
| N004 | Infos | L’utilisateur sélectionne un fournisseur de télévision qui n’est pas pris en charge par le demandeur actuel. | Le rappel *displayProviderDialog* sera appelé et l’application pourra revenir au flux d’authentification standard. | Sélectionnez MVPD standard et continuez avec l’écran de connexion. | s.o. | Oui | s.o. |
| N005 | Infos | Le sélecteur MVPD a été annulé. | s.o. | s.o. | Oui | Oui | Oui à partir de la version 3.2 |
| N010 | Attention | L’utilisateur a été authentifié alors que la règle de dégradation authenticate-all était en place pour le MVPD sélectionné. | Vous pouvez éventuellement informer l’utilisateur qu’il bénéficie d’un accès gratuit « gratuit » en raison de difficultés liées à MVPD. | s.o. | Oui | Oui | Oui à partir de la version 3.2 |
| N011 | Infos | L’utilisateur a été authentifié à l’aide de TempPass. | - Informez l’utilisateur. <br> <br> - Présente éventuellement une liste des fichiers MVPD standard. | Connectez-vous éventuellement à l’aide de votre MVPD standard. | Oui | Oui | Oui à partir de la version 3.2 |
| N111 | Attention | TempPass expiré. | - Informer l’utilisateur. <br> <br> - Présentez une liste des fichiers MVPD standard. <br> <br> - Masquez l’option TempPass. | Connectez-vous avec votre MVPD standard. | Oui | Oui | Oui à partir de la version 3.2 |
| N130 | Erreur | **Jeton d’authentification introuvable sur la session.** Cela peut être dû à l’une des causes suivantes : <br> <br> 1. Les cookies (tiers) du navigateur sont désactivés (ne s’applique pas à la version 4.x d’AccessEnabler JavaScript SDK) <br> <br> 2. Le navigateur a l’option Empêcher le suivi sur plusieurs sites activée (Safari 11+) <br> <br> 3. La session a expiré <br> <br> 4. Le programmeur appelle les API d’authentification avec un <br> de succession incorrect <br> REMARQUE : ce code d’erreur n’est pas disponible pour les flux d’authentification de redirection de page entière. | &#x200B;1. Demander à l’utilisateur d’activer les cookies (tiers) <br> <br> 2. Inviter l’utilisateur à désactiver le suivi intersite <br> <br> 3. Inviter l’utilisateur à réauthentifier <br> <br> 4. API d’appel dans le bon ordre | &#x200B;1. Activer les <br> de cookies (tiers) <br> 2. Désactivation du suivi cross-site <br> <br> 3. Réauthentifier le <br> <br> 4. S/O | Oui | Oui | Oui à partir de la version 3.2 |
| N500 | Erreur | Erreur interne. <br> <br> Remarque : il s’agit de l’erreur d’origine du système d’erreur « Erreur d’authentification générique » et « Erreur d’authentification interne ». Cette erreur finira par être supprimée. | Utilisez AccessEnablerDebug et examinez les journaux de débogage (sortie console.log) afin de déterminer ce qui s’est passé. | s.o. | Oui | Oui | s.o. |
| R401 | Erreur | Une erreur s’est produite lors de la tentative d’obtention d’un jeton d’accès. <br> <br> Remarque : il s’agit d’une erreur irrécupérable. Informez l’utilisateur que l’application n’est pas disponible. | - iOS : vérifiez l’instruction du logiciel et les schémas personnalisés dans votre application. <br> <br> - JavaScript : vérifiez la déclaration du logiciel dans l’application de votre site web. <br> <br> Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible | s.o. | Oui À partir de la version 4.0 | Oui À partir de la version 3.0 | Oui à partir de la version 3.2 |
| R400 | Erreur | L’application n’est pas enregistrée. L&#39;instruction du logiciel n&#39;est pas valide ou a été révoquée. <br> <br> Remarque : il s’agit d’une erreur irrécupérable. Informez l’utilisateur que l’application n’est pas disponible. | - iOS : vérifiez l’instruction du logiciel et les schémas personnalisés dans votre application. <br> <br> - JavaScript : vérifiez la déclaration du logiciel dans l’application de votre site web. <br> <br> Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible | s.o. | Oui À partir de la version 4.0 | Oui À partir de la version 3.0 | Oui à partir de la version 3.2 |
| REG500 | Erreur | Impossible de récupérer le code d&#39;enregistrement à partir du serveur. <br> <br> Remarque : il s’agit d’une erreur irrécupérable. Informez l’utilisateur que l’application n’est pas disponible. | Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible. | s.o. | Oui À partir de la version 4.0 | Oui À partir de la version 3.0 | Oui à partir de la version 3.2 |
| REGCODE | Succès | Application appelée API setSelectedProvider sur la plateforme tvOS. | Demandez/demandez à l’utilisateur d’utiliser un deuxième appareil (écran) pour se connecter à l’aide du code d’enregistrement fourni. | Utilisez le regcode sur un deuxième appareil (écran) pour lancer l’authentification. | s.o. | Oui uniquement pour tvOS | s.o. |
| Z010 | Attention | L’utilisateur était autorisé lorsque la règle de dégradation Authentifier-tous ou Autoriser-tous était en place pour le MVPD sélectionné. | Vous pouvez éventuellement informer l’utilisateur qu’il bénéficie d’un accès gratuit « gratuit » en raison de difficultés liées à MVPD. | s.o. | Oui | Oui | Oui à partir de la version 3.2 |
| Z011 | Infos | L’utilisateur a été autorisé à utiliser TempPass. | Informez éventuellement l’utilisateur | s.o. | Oui | Oui | Oui à partir de la version 3.2 |
| Z100 | Erreur | L’autorisation a échoué, car l’utilisateur ne dispose pas d’un abonnement pour la ressource demandée, ou pour d’autres raisons provenant du MVPD ; par exemple, la vidéo ne correspond pas aux paramètres de contrôle parental pour le compte d’utilisateur. | - Ne pas autoriser la lecture. <br> <br> - Informer l’utilisateur. <br> <br> - La touche &#39;message&#39; du message d&#39;erreur PEUT contenir un message plus détaillé fourni par le MVPD. | s.o. | Oui | Oui | Oui à partir de la version 3.2 |
| Z110 | Erreur | Autorisation refusée en raison de refus MVPD répétés. Tentative de fraude possible ou DOS. | - Ne pas autoriser la lecture. <br> <br> - Informer l’utilisateur. | s.o. | Oui | Oui | Oui à partir de la version 3.2 |
| Z120 | Erreur | Autorisation refusée pour des raisons techniques de communication avec le MVPD. Erreur réseau possible. | - Ne pas autoriser la lecture. <br> <br> - Informez l’utilisateur ou l’utilisatrice que le MVPD a rencontré des problèmes et il ou elle doit réessayer ultérieurement. | Réessayez plus tard. | Oui | Oui | Oui à partir de la version 3.2 |
| Z130 | Erreur | Autorisation refusée car une ressource non valide/incorrecte a été utilisée. | Vérifiez la chaîne de la ressource et corrigez-la. En règle générale, cette erreur est due à un MRSS incorrect ou à l’utilisation d’une chaîne simple au lieu de MRSS. | s.o. | Oui | Oui | Oui à partir de la version 3.2 |
| Z169 | Erreur | Autorisation refusée car la règle de dégradation authzNone a été appliquée à la ressource spécifiée. | Informer l’utilisateur | s.o. | Oui | Oui | Oui à partir de la version 3.2 |
| Z500 | Erreur | Erreur interne. <br> <br> Remarque : il s’agit de l’ancienne « Erreur d’authentification générique » et « Erreur d’authentification interne ». Cette erreur finira par être supprimée. | Utilisez AccessEnablerDebug et examinez les journaux de débogage (sortie console.log) afin de déterminer ce qui s’est passé. | s.o. | Oui | Oui | Oui à partir de la version 3.2 |
| P100 | Erreur | Échec de la préautorisation. Cela est probablement dû à une demande d’autorisation pour trop de ressources. | - N’utilisez PAS plus de ressources que le nombre maximal autorisé. <br> <br> - Contactez l’assistance technique d’authentification d’Adobe Pass pour rechercher/configurer le nombre maximal de ressources autorisées. | s.o. | Oui À partir de la version 3.0 | Oui | Oui à partir de la version 3.2 |
| IS2XX | Erreur | Ces codes d’erreur sont renvoyés lorsque les données de réponse du point d’entrée du serveur d’individualisation ont un format non valide ou qu’elles ne contiennent pas les informations d’individualisation requises. | Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible | s.o. | Oui À partir de la version 3.0 | s.o. | s.o. |
| IS4XX | Erreur | Ces codes d’erreur sont renvoyés en cas d’échec du point d’entrée du serveur d’individualisation 4XX . Il s’agit du code d’état HTTP de la réponse. | Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible | s.o. | Oui À partir de la version 3.0 | s.o. | s.o. |
| IS5XX | Erreur | Ces codes d’erreur sont renvoyés en cas d’échec du point d’entrée du serveur d’individualisation 5XX . Il s’agit du code d’état HTTP de la réponse. | Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible | s.o. | Oui À partir de la version 3.0 | s.o. | s.o. |
| IS0 | Erreur | Ce code est renvoyé lorsque le point d’entrée du serveur d’individualisation n’a pas répondu du tout. Par conséquent, la connexion a expiré | Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible | s.o. | Oui À partir de la version 3.0 | s.o. | s.o. |
| LS011 | Attention | AccessEnabler utilise un stockage volatile en raison de problèmes de LSO/LocalStorage et de problèmes de WebStorage (ou d&#39;indisponibilité). <br> L’authentification/autorisation <br> ne persiste pas au-delà de la page actuelle. À chaque chargement de page, l’utilisateur ou l’utilisatrice doit s’authentifier. Les TTL configurées ne sont pas appliquées lors des rechargements de page. | - Informer l&#39;utilisateur des limitations. <br> <br> - Indiquez à l&#39;utilisateur comment augmenter l&#39;espace de stockage disponible. <br> <br> - Vous pouvez également vous déconnecter pour effacer le stockage. | - Augmentez le stockage. <br> <br> - Déconnectez-vous pour effacer le stockage. | Oui | s.o. | s.o. |

<br>

## Rapport d’erreurs d’origine {#original-error-reporting}

Cette section décrit le système de rapport d’erreurs d’origine, ainsi que les codes d’erreur d’origine. Dans le système de rapport d’erreur d’origine, AccessEnabler transmet les erreurs à ces deux fonctions de rappel : `setAuthenticationStatus()` après un appel à `checkAuthentication()` ; `tokenRequestFailed()`, après l’échec d’un appel à `checkAuthorization()` ou `getAuthorization()`.

Les API de rapport d’erreur et de statut d’origine continuent à fonctionner exactement comme auparavant. Toutefois, les API de rapports d’erreurs d’origine ne seront pas mises à jour à l’avenir. Tous les nouveaux rapports d’erreurs et les mises à jour des anciennes erreurs seront répercutés UNIQUEMENT dans le nouveau [système avancé de rapports d’erreurs](#advanced-error-reporting).


Pour obtenir des exemples d’utilisation du système de rapports d’erreurs d’origine, consultez les fonctions [Référence de l’API JavaScript](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md):[setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) et [tokenRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#token-request-failed-error-msg), [Référence de l’API iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) : [setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setAuthNStatus)et [tokentRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#tokenReqFailed), [Référence de l’API Android](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus) et [tokenRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed).

### Codes d’erreur de rappel d’origine {#original-callback-error-codes}

| **Erreurs génériques** | |
|---|---|
| Erreur interne | Une erreur système s’est produite lors de la tentative de traitement de la demande. |
| Erreur de fournisseur non sélectionné | Se produit lorsque le client annule dans la boîte de dialogue de sélection du fournisseur. |
| Erreur de fournisseur non disponible | Se produit lorsqu&#39;aucun fournisseur n&#39;est disponible. |
|  |  |
| **Erreurs d’authentification** | |
| Erreur d’authentification générique | Renvoyé lorsque la raison n’est pas connue ou ne peut pas être publiée. |
| Erreur d’authentification interne | Une erreur système s’est produite lors de la tentative d’authentification. |
| Erreur Utilisateur non authentifié | L’utilisateur n’est pas authentifié. |
|  |  |
| **Erreurs d’autorisation** |  |
| Erreur d’autorisation générique | Renvoyé lorsque la raison n’est pas connue ou ne peut pas être publiée. |
| Erreur d’autorisation interne | Une erreur système s’est produite lors de la tentative d’autorisation. |
| Erreur Utilisateur non autorisé | Le client n’est pas autorisé à consulter le contenu demandé. |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
