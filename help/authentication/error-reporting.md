---
title: Rapport d’erreurs
description: Rapport d’erreurs
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2961'
ht-degree: 1%

---

# Rapport d’erreurs {#error-reporting}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


## Présentation {#overview}

Le rapport d’erreurs dans l’authentification Adobe Pass est actuellement implémenté de deux manières différentes :

* **Rapport d’erreurs avancé** L’implémentateur enregistre un rappel d’erreur en cas de [SDK JavaScript AccessEnabler](#accessenabler-javascript-sdk) ou met en oeuvre une méthode d’interface nommée &quot;`status`&quot; en cas de [SDK AccessEnabler iOS/tvOS](#accessenabler-ios-tvos-sdk) et [SDK Android AccessEnabler](#accessenabler-android-sdk), afin de recevoir des rapports d’erreur avancés. Les erreurs sont classées dans **Informations**, **Avertissement**, et **Erreur** types. Ce système de reporting est **asynchrone**, dans **l’ordre dans lequel plusieurs erreurs seront déclenchées n’est pas garanti.**.  Pour plus d’informations sur le système avancé de reporting des erreurs, voir [Rapport d’erreurs avancé](#advanced-error-reporting) .

* **Rapport d’erreurs initial -** Système de création de rapports statique dans lequel les messages d’erreur sont transmis à des fonctions de rappel spécifiques en cas d’échec de requêtes spécifiques. Les erreurs sont regroupées en types génériques, d’authentification et d’autorisation. Pour obtenir la liste des erreurs signalées dans le système d’origine, reportez-vous au [Rapport d’erreurs d’origine](#original-error-reporting) .


## Rapport d’erreurs avancé {#advanced-error-reporting}

* [SDK JavaScript AccessEnabler](#accessenabler-javascript-sdk)
* [SDK AccessEnabler iOS/tvOS](#accessenabler-ios-tvos-sdk)
* [SDK Android AccessEnabler](#accessenabler-android-sdk)
* [SDK AccessEnabler FireOS](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>Le vieux [Rapport d’erreurs d’origine](#original-error-reporting) L’API continuera à fonctionner comme auparavant, le reporting d’erreur avancé n’interrompt pas la fonctionnalité, mais le reporting d’erreur d’origine ne recevra plus aucune mise à jour. Toutes les nouvelles erreurs et mises à jour se produiront dans le système de reporting avancé des erreurs.

### SDK JavaScript AccessEnabler {#accessenabler-javascript-sdk}

Le nouveau système de création de rapports d’erreurs est facultatif. Par conséquent, l’implémentateur peut enregistrer explicitement un rappel de gestionnaire d’erreurs pour recevoir des rapports d’erreur avancés. Le système offre la possibilité d’enregistrer et d’annuler l’enregistrement dynamique de plusieurs rappels d’erreur. En outre, vous pouvez enregistrer de nouveaux rappels d’erreur dès que le SDK JavaScript AccessEnabler est chargé, sans avoir à effectuer d’autre initialisation (avant d’appeler `setRequestor()`), ce qui signifie que vous pouvez recevoir des rapports avancés sur les erreurs d’initialisation et de configuration.


#### Implémentation {#access-enab-js-imp}

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

### 1. Liaison {#bind}

**`.bind(eventType:String, handlerName:String):void`**

Joint un gestionnaire pour un événement.

**`eventType`** - UNIQUEMENT le &quot;`errorEvent`&quot; entraîne le déclenchement par le SDK JavaScript AccessEnabler de rappels avancés de rapports d’erreur.

**`handlerName`** - chaîne spécifiant le nom de la fonction du gestionnaire d’erreurs.


Les deux paramètres de liaison ne doivent utiliser que les caractères de l’ensemble suivant : `[0-9a-zA-Z][-._a-zA-Z0-9]`, c’est-à-dire que les paramètres doivent commencer par un nombre ou une lettre, et peuvent ensuite inclure uniquement des tirets, des points, des traits de soulignement et des caractères alphanumériques.  En outre, les paramètres ne peuvent pas dépasser 1 024 caractères.

**Exemple** des gestionnaires d’erreurs de liaison :

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

En raison de limitations techniques, vous ne pouvez pas lier une fermeture ou une fonction anonyme. Vous devez spécifier le nom de la méthode dans le deuxième paramètre.


### 2. Délier {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

Supprime un gestionnaire d’événements précédemment joint.

**`eventType`** - SEULEMENT LE &quot;`errorEvent`La valeur &quot;&quot; entraîne le déclenchement du SDK JavaScript AccessEnabler, déclenchant des rappels avancés des rapports d’erreur.

**`handlerName`** - chaîne spécifiant le nom de la fonction du gestionnaire d’erreurs, si est nul ou si tous les gestionnaires associés sont manquants pour la fonction spécifiée `eventType` sera supprimé.

Les deux paramètres de liaison ne doivent utiliser que les caractères de l’ensemble suivant : `[0-9a-zA-Z][-._a-zA-Z0-9]`, c’est-à-dire que les paramètres doivent commencer par un nombre ou une lettre, et peuvent ensuite inclure uniquement des tirets, des points, des traits de soulignement et des caractères alphanumériques.  En outre, les paramètres ne peuvent pas dépasser 1 024 caractères.

**Exemple** de suppression d’un seul gestionnaire d’erreurs :

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**Exemple** suppression de tous les gestionnaires d’erreurs :

`accessEnabler.unbind('errorEvent');`


### SDK AccessEnabler iOS/tvOS {#accessenabler-ios-tvos-sdk}

Le nouveau système de reporting des erreurs est obligatoire. Par conséquent, l’implémentateur doit se conformer explicitement au nouveau protocole Objective C &quot;EntitlementStatus&quot;. Cette nouvelle approche permet aux programmeurs de recevoir des rapports d’erreur avancés.

#### Implémentation {#accessenab-ios-tvossdk-imp}

Un implémentateur doit se conformer aux **EntitlementStatus** protocole :

**EntitlementStatus.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

Votre **status** reçoit un seul objet (une `NSDictionary`) avec la structure suivante :

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


### SDK Android AccessEnabler {#accessenabler-android-sdk}

Le nouveau système de reporting des erreurs est obligatoire, car l’implémentateur doit se conformer explicitement à la variable `IAccessEnablerDelegate` protocole défini par l’interface. Cette nouvelle approche permet aux programmeurs de recevoir des rapports d’erreur avancés.

#### Implémentation {#access-enablr-androidsdk-imp}

Un implémentateur doit gérer la nouvelle `status` de l’interface`IAccessEnablerDelegate`. La variable **`status`** reçoit une seule **`AdvancedStatus`** ayant le modèle suivant :

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

### SDK AccessEnabler FireOS {#accessenabler-fireos-sdk}


Le nouveau système de reporting des erreurs est obligatoire, car l’implémentateur doit se conformer explicitement à la variable `IAccessEnablerDelegate` protocole défini par l’interface. Cette nouvelle approche permet aux programmeurs de recevoir des rapports d’erreur avancés.

#### Implémentation {#access-enab-fireos-sdk-}

Un implémentateur doit gérer la nouvelle `status`de l’interface`IAccessEnablerDelegate`. La variable **`status`** reçoit une seule **`AdvancedStatus`** ayant le modèle suivant :

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

| ID | Niveau | Description | Actions du développeur | Actions des utilisateurs | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL&amp; AAPL_ERROR | Erreur | Erreur générique de connexion unique Apple | L’erreur contient un champ de détails avec l’erreur VSA d’origine. | n/a | n/a | Oui | n/a |
| VSA203 | Infos | L’utilisateur a décidé de se déconnecter de l’application pendant que l’authentification a eu lieu suite à une connexion via la connexion unique à la plateforme. | Demandez à l’utilisateur de se déconnecter explicitement de Paramètres -> Comptes -> Fournisseur de télévision sur tvOS. <br><br> Demandez à l’utilisateur de se déconnecter explicitement de Settings -> TV Provider sur iOS/iPadOS. | Déconnectez-vous explicitement de Paramètres -> Comptes -> Fournisseur de télévision sur tvOS. <br> <br> Déconnexion explicite de Paramètres -> Fournisseur de télévision sur iOS/iPadOS | n/a | Oui | n/a |
| VSA404 | Infos | L’autorisation Application Video Subscriber Account est indéterminée. | Incitez les utilisateurs qui refusent de donner l’autorisation d’accéder aux informations d’abonnement en expliquant les avantages de l’expérience utilisateur de connexion unique (SSO). | L’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès au fournisseur de télévision) ou à la section depuis Paramètres -> Fournisseur de télévision sur iOS/iPadOS ou Paramètres -> Comptes -> Fournisseur de télévision sur tvOS. | n/a | Oui | n/a |
| VSA503 | Infos | Échec de la demande de métadonnées du compte d’abonné vidéo d’application. | Le point de terminaison MVPD ne répond pas. L’application peut basculer vers un flux d’authentification régulier. | n/a | n/a | Oui | n/a |
| 500 | Erreur | Erreur interne | Utilisez AccessEnablerDebug et examinez les journaux de débogage (sortie console.log) afin de déterminer ce qui s’est mal passé. | n/a | Oui | Oui | n/a |
| SEC403 | Erreur | Erreur Domain Security. Le demandeur utilise un domaine non valide. Tous les domaines utilisés par un identifiant de demandeur particulier doivent être placés sur liste blanche par Adobe. | - Charger AccessEnabler uniquement à partir de la liste des domaines autorisés <br> <br> - Contactez l’Adobe afin de gérer la liste blanche des domaines pour l’identifiant du demandeur utilisé. <br> <br> - iOS : vérifiez que vous utilisez le certificat correct et que la signature est correctement créée. | n/a | n/a | Oui | n/a |
| SEC412 | Avertissement | [Disponible dans la version 2.5] L’identifiant de l’appareil ne correspond pas. Cela peut se produire chaque fois que la plateforme sous-jacente modifie son identifiant d’appareil. Dans ce cas, les jetons existants seront effacés et l’utilisateur ne sera plus authentifié. Notez que cela se produit légitimement lorsque l’utilisateur utilise le SDK JS et se déplace en continu (sur JS, l’adresse IP du client fait partie de l’ID de périphérique). Dans le cas contraire, cela pourrait être une indication d’une tentative de fraude, une tentative de copie de jetons à partir d’un autre appareil. | - Surveillez le nombre d’avertissements. S’ils augmentent sans raison apparente (aucune mise à jour récente du navigateur ; nouveaux systèmes d’exploitation), cela peut être un indicateur des tentatives de fraude.  <br> <br>- Si vous le souhaitez, informez l’utilisateur qu’il doit se reconnecter. | Connectez-vous à nouveau. | Oui | Oui | Oui depuis la version 3.2 |
| SEC420 | Erreur | Erreur de sécurité HTTP lors de la communication avec les serveurs d’authentification Adobe Pass. Cette erreur survient généralement lorsque des adresses proxy/usurpation sont en place. | - Chargement `[https://]{SP_FQDN\}` dans le navigateur et acceptez manuellement les certificats SSL, par exemple : **https://api.auth.adobe.com** ou **https://api.auth-staging.adobe.com** <br> <br>- Marquer les certificats du proxy comme étant approuvés | Si cela se produit pour un utilisateur régulier, cela indique une possible attaque de l’homme du milieu ! | Oui | Oui | Oui depuis la version 3.2 |
| CFG100 | Avertissement | La date/l’heure/le fuseau horaire de l’ordinateur client n’est pas correctement définie. Cela entraînera probablement des erreurs d’authentification/d’autorisation. | - Informer l’utilisateur de définir l’heure correcte. <br> <br> Prenez des mesures pour empêcher les flux de droits, car ils vont probablement échouer. | Définissez la date/l’heure correcte. | Oui | Oui | Oui depuis la version 3.2 |
| CFG400 | Erreur | Un identifiant de demandeur non valide a été fourni. | Le développeur DOIT spécifier un identifiant de demandeur valide. | n/a | Oui | Oui | Oui depuis la version 3.2 |
| CFG404 | Erreur | Les serveurs d’authentification Adobe Pass sont introuvables. Cela peut se produire dans 3 cas : <br><br> - Le développeur a mis en place une usurpation non valide. <br><br> - L’utilisateur rencontre des problèmes de mise en réseau et ne peut pas accéder aux domaines d’authentification Adobe Pass. <br><br> - Les serveurs d’authentification Adobe Pass sont mal configurés. <br><br>  **Remarque :** Sous Firefox, CFG400 apparaîtra à la place de CFG404 (limite de navigateur). | - Vérifiez l’usurpation. <br><br> -Vérifiez les paramètres réseau/DNS. <br><br> - Informe l’Adobe. | Vérifiez les paramètres réseau/DNS. | Oui | Oui | Oui depuis la version 3.2 |
| CFG410 | Erreur | AccessEnabler est trop vieux. | Permet d’informer l’utilisateur d’effacer les caches. | Effacez le cache du navigateur. | Oui | n/a | Oui depuis la version 3.2 |
| CFG5xx | Erreur | Les serveurs d’authentification Adobe Pass rencontrent des erreurs internes. xx peut être n’importe quel nombre. | - Informer l’utilisateur de l’indisponibilité de l’authentification Adobe Pass. <br><br> - Contournement de l’authentification Adobe Pass. <br> <br> - Informer l’Adobe. | Essaie plus tard. | Oui | Oui | Oui depuis la version 3.2 |
| N000 | Infos | L’utilisateur n’est pas authentifié. | n/a | Connectez-vous. | Oui | Oui | Oui depuis la version 3.2 |
| N001 | Infos | Une tentative d’authentification passive a été lancée en arrière-plan. Cela se produira pour les MVPD configurés avec &quot;Authentification par demandeur&quot;. Bien que l’utilisateur puisse être authentifié automatiquement, cela entraîne une pénalité de performances lors de l’initialisation. | Vous pouvez éventuellement informer l’utilisateur, ou l’interface utilisateur actuelle qui l’avertit, que &quot;le travail est en cours&quot;. | Attends ! | Oui | Oui | Oui depuis la version 3.2 |
| N003 | Infos | L’utilisateur sélectionne l’option &quot;Autre fournisseur de télévision&quot; dans le sélecteur Apple MVPD. | La variable *displayProviderDialog* est appelée et l’application peut revenir au flux d’authentification standard. | Sélectionnez MVPD normal et continuez avec l’écran de connexion. | n/a | Oui | n/a |
| N004 | Infos | L’utilisateur sélectionne un fournisseur de télévision qui n’est pas pris en charge par le demandeur actuel. | La variable *displayProviderDialog* est appelée et l’application peut revenir au flux d’authentification standard. | Sélectionnez MVPD normal et continuez avec l’écran de connexion. | n/a | Oui | n/a |
| N005 | Infos | Le sélecteur MVPD a été annulé. | n/a | n/a | Oui | Oui | Oui depuis la version 3.2 |
| N010 | Avertissement | L’utilisateur a été authentifié alors que la règle de dégradation &quot;Tout authentifier&quot; était en place pour le MVPD sélectionné. | Si vous le souhaitez, vous pouvez indiquer à l’utilisateur qu’il bénéficie d’un accès gratuit &quot;gratuit&quot; en raison de difficultés liées au MVPD. | n/a | Oui | Oui | Oui depuis la version 3.2 |
| N011 | Infos | L’utilisateur a été authentifié à l’aide de TempPass. | - Informer l’utilisateur. <br> <br> - Vous pouvez éventuellement présenter une liste de MVPD standard. | Connectez-vous éventuellement à votre MVPD ordinaire. | Oui | Oui | Oui depuis la version 3.2 |
| N111 | Avertissement | TempPass expiré. | - Informer l’utilisateur. <br> <br> - Présenter une liste de MVPD standard. <br> <br> - Masquez l’option TempPass . | Connectez-vous avec votre MVPD standard. | Oui | Oui | Oui depuis la version 3.2 |
| N130 | Erreur | **Jeton d’authentification introuvable sur la session.**  Cela peut être dû à l’une des raisons suivantes : <br> <br> 1. Les cookies (tiers) du navigateur sont désactivés (non applicable pour le SDK JavaScript AccessEnabler version 4.x) <br> <br> 2. Le navigateur a activé l’option Empêcher le suivi sur plusieurs sites (Safari 11+) <br> <br> 3. La session a expiré <br> <br> 4. Le programmeur appelle les API d’authentification en succession incorrecte <br> <br> REMARQUE : Ce code d’erreur n’est pas disponible pour les flux d’authentification de redirection de page entière. | 1. Invitez l’utilisateur à activer les cookies (tiers) <br> <br> 2. Demander à l’utilisateur de désactiver le suivi intersite <br> <br> 3. Demander à l’utilisateur de se reconnecter <br> <br> 4. Appeler les API dans le bon ordre | 1. Activation des cookies (tiers) <br> <br> 2. Désactivation du suivi sur plusieurs sites <br> <br> 3. Reconnecter <br> <br> 4. N/A | Oui | Oui | Oui depuis la version 3.2 |
| N500 | Erreur | Erreur interne. <br> <br> Remarque : Il s’agit de l’erreur &quot;Erreur d’authentification générique&quot; et &quot;Erreur d’authentification interne&quot; du système d’erreur d’origine. Cette erreur finira par disparaître progressivement. | Utilisez AccessEnablerDebug et examinez les journaux de débogage (sortie console.log) afin de déterminer ce qui s’est mal passé. | n/a | Oui | Oui | n/a |
| R401 | Erreur | Une erreur s’est produite lors de la tentative d’obtention d’un jeton d’accès. <br> <br> Remarque : il s’agit d’une erreur irrécupérable. Informez l’utilisateur que l’application n’est pas disponible. | - iOS : vérifiez l’instruction logicielle et les schémas personnalisés dans votre application. <br> <br> - JavaScript : vérifiez l’instruction logicielle dans l’application de votre site web. <br> <br> Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible. | n/a | Oui depuis la version 4.0 | Oui depuis la version 3.0 | Oui depuis la version 3.2 |
| R400 | Erreur | L’application n’est pas enregistrée. L’instruction logicielle n’est pas valide ou a été révoquée. <br> <br> Remarque : il s’agit d’une erreur irrécupérable. Informez l’utilisateur que l’application n’est pas disponible. | - iOS : vérifiez l’instruction logicielle et les schémas personnalisés dans votre application. <br> <br> - JavaScript : vérifiez l’instruction logicielle dans l’application de votre site web. <br> <br> Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible. | n/a | Oui depuis la version 4.0 | Oui depuis la version 3.0 | Oui depuis la version 3.2 |
| REG500 | Erreur | Le code d’enregistrement n’a pas pu être récupéré du serveur. <br> <br> Remarque : il s’agit d’une erreur irrécupérable. Informez l’utilisateur que l’application n’est pas disponible. | Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement hors service. | n/a | Oui depuis la version 4.0 | Oui depuis la version 3.0 | Oui depuis la version 3.2 |
| REGCODE | Succès | L’application appelée API setSelectedProvider sur la plateforme tvOS. | Demandez à l’utilisateur d’utiliser un 2e appareil (écran) pour se connecter à l’aide du code d’enregistrement fourni. | Utilisez le regcode sur un deuxième appareil (écran) pour lancer l’authentification. | n/a | Oui uniquement pour tvOS | n/a |
| Z010 | Avertissement | L’utilisateur a été autorisé alors que la règle de dégradation Tout authentifier ou Tout autoriser était en place pour le MVPD sélectionné. | Si vous le souhaitez, vous pouvez indiquer à l’utilisateur qu’il bénéficie d’un accès gratuit &quot;gratuit&quot; en raison de difficultés liées au MVPD. | n/a | Oui | Oui | Oui depuis la version 3.2 |
| Z011 | Infos | L’utilisateur a été autorisé avec TempPass | Avertir éventuellement l’utilisateur | n/a | Oui | Oui | Oui depuis la version 3.2 |
| Z100 | Erreur | L’autorisation a échoué car l’utilisateur n’a pas d’abonnement pour la ressource demandée ou en raison d’autres raisons provenant du MVPD, par exemple, la vidéo ne correspond pas aux paramètres de contrôle parental pour le compte d’utilisateur. | - Ne pas autoriser la lecture. <br> <br> - Informer l’utilisateur. <br> <br> - La clé &#39;message&#39; du message d&#39;erreur PEUT contenir un message plus détaillé fourni par le MVPD. | n/a | Oui | Oui | Oui depuis la version 3.2 |
| Z110 | Erreur | Autorisation refusée en raison de refus répétés de MVPD. Tentative de fraude potentielle pour DOS. | - Ne pas autoriser la lecture. <br> <br> - Informer l’utilisateur. | n/a | Oui | Oui | Oui depuis la version 3.2 |
| Z120 | Erreur | Autorisation refusée pour des raisons techniques lors de la communication avec le MVPD. Erreur réseau possible. | - Ne pas autoriser la lecture. <br> <br> - Informez l’utilisateur que le MVPD a rencontré des difficultés et qu’il doit réessayer ultérieurement. | Essaie plus tard. | Oui | Oui | Oui depuis la version 3.2 |
| Z130 | Erreur | Autorisation refusée car une ressource non valide/incorrecte a été utilisée. | Vérifiez la chaîne de ressource et corrigez-la. En règle générale, cette erreur est due à un MRSS mal formé ou à l&#39;utilisation d&#39;une chaîne simple au lieu d&#39;un MRSS. | n/a | Oui | Oui | Oui depuis la version 3.2 |
| Z169 | Erreur | Autorisation refusée car la règle de dégradation authzNone a été appliquée pour la ressource spécifiée. | Informer l’utilisateur | n/a | Oui | Oui | Oui depuis la version 3.2 |
| Z500 | Erreur | Erreur interne. <br> <br>  Remarque : Il s’agit de l’ancienne erreur &quot;Erreur d’authentification générique&quot; et &quot;Erreur d’authentification interne&quot;. Cette erreur finira par disparaître progressivement. | Utilisez AccessEnablerDebug et examinez les journaux de débogage (sortie console.log) afin de déterminer ce qui s’est mal passé. | n/a | Oui | Oui | Oui depuis la version 3.2 |
| P100 | Erreur | La préautorisation a échoué. Cela est probablement dû à une demande d’autorisation pour un trop grand nombre de ressources. | - N’utilisez PAS plus que le nombre maximal de ressources autorisées. <br> <br> - Contactez la prise en charge de l’authentification Adobe Pass pour rechercher/configurer le nombre maximum de ressources autorisées. | n/a | Oui depuis la version 3.0 | Oui | Oui depuis la version 3.2 |
| IS2XX | Erreur | Ces codes d’erreur sont renvoyés lorsque les données de réponse du point de terminaison du serveur d’individualisation ont un format non valide ou ne contiennent pas d’informations d’individualisation requises. | Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible. | n/a | Oui depuis la version 3.0 | n/a | n/a |
| IS4XX | Erreur | Ces codes d’erreur sont renvoyés en cas d’échec du point de terminaison du serveur d’individualisation 4XX : il s’agit du code d’état HTTP de la réponse. | Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible. | n/a | Oui depuis la version 3.0 | n/a | n/a |
| IS5XX | Erreur | Ces codes d’erreur sont renvoyés en cas d’échec du point de terminaison du serveur d’individualisation 5XX : est le code d’état HTTP de la réponse. | Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible. | n/a | Oui depuis la version 3.0 | n/a | n/a |
| IS0 | Erreur | Ce code est renvoyé lorsque le point de terminaison du serveur d’individualisation n’a pas répondu du tout. Par conséquent, la connexion a expiré. | Ouvrez un ticket à l’aide de Zendesk et informez l’utilisateur que le système est temporairement indisponible. | n/a | Oui depuis la version 3.0 | n/a | n/a |
| LS011 | Avertissement | AccessEnabler utilise un stockage volatile en raison de problèmes de LSO / LocalStorage et de problèmes de WebStorage (ou d&#39;indisponibilité). <br> <br> L’authentification/l’autorisation ne persiste pas au-delà de la page actuelle. À chaque chargement de page, l’utilisateur doit s’authentifier. Les TTL configurés ne sont pas appliqués lors des rechargements de page. | - Informer l’utilisateur des restrictions. <br> <br> - Informer l’utilisateur sur la manière d’augmenter l’espace de stockage disponible. <br> <br> - Vous pouvez également vous déconnecter afin d’effacer le stockage. | - Augmentez le stockage. <br> <br> - Déconnectez-vous afin d’effacer le stockage. | Oui | n/a | n/a |

<br>

## Rapport d’erreurs d’origine {#original-error-reporting}

Cette section décrit le système de reporting des erreurs d’origine, ainsi que les codes d’erreur d’origine. Dans le système de reporting des erreurs d&#39;origine, AccessEnabler transmet les erreurs à ces deux fonctions de rappel : `setAuthenticationStatus()` après un appel à `checkAuthentication()`; `tokenRequestFailed()`, après l’échec d’un appel à la fonction `checkAuthorization()` ou `getAuthorization()`.

Les rapports d’erreur d’origine et les API d’état continuent de fonctionner exactement comme auparavant. Toutefois, à l’avenir, les API de création de rapports d’erreur d’origine ne seront pas mises à jour. Tous les nouveaux rapports d’erreur et mises à jour sur les anciennes erreurs seront UNIQUEMENT répercutés dans la nouvelle [Système de création de rapports d’erreurs avancé](#advanced-error-reporting).


Pour obtenir des exemples d’utilisation du système de reporting des erreurs d’origine, reportez-vous à la section [Référence de l’API JavaScript](/help/authentication/javascript-sdk-api-reference.md):[setAuthenticationStatus()](/help/authentication/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) et [tokenRequestFailed()](/help/authentication/javascript-sdk-api-reference.md#token-request-failed-error-msg) fonctions, [Référence de l’API iOS/tvOS](/help/authentication/iostvos-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/javascript-sdk-api-reference.md#setAuthNStatus)et [tokentRequestFailed()](/help/authentication/javascript-sdk-api-reference.md#tokenReqFailed), [Référence de l’API Android](/help/authentication/android-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/android-sdk-api-reference.md#setAuthNStatus) et [tokenRequestFailed()](/help/authentication/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed).

### Codes d’erreur de rappel d’origine {#original-callback-error-codes}

| **Erreurs génériques** | |
|---|---|
| Erreur interne | Une erreur système s’est produite lors de la tentative de traitement de la requête. |
| Erreur du fournisseur non sélectionné | Se produit lorsque le client annule dans la boîte de dialogue de sélection du fournisseur. |
| Erreur du fournisseur non disponible | Se produit lorsqu’aucun fournisseur n’est disponible. |
|  |  |
| **Erreurs d’authentification** | |
| Erreur d’authentification générique | Renvoyée lorsque la raison n’est pas connue ou ne peut pas être publiée. |
| Erreur d’authentification interne | Une erreur système s’est produite lors de la tentative d’authentification. |
| Erreur de non-authentification de l’utilisateur | L’utilisateur n’est pas authentifié. |
|  |  |
| **Erreurs d’autorisation** |  |
| Erreur d’autorisation générique | Renvoyée lorsque la raison n’est pas connue ou ne peut pas être publiée. |
| Erreur d’autorisation interne | Une erreur système s’est produite lors de la tentative d’autorisation. |
| Erreur de l’utilisateur non autorisé | Le client n’est pas autorisé à afficher le contenu demandé. |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
