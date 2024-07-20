---
title: Gestion de l’enregistrement du client dynamique
description: Gestion de l’enregistrement du client dynamique
exl-id: 2c3ebb0b-c814-4b9e-af57-ce1403651e9e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 0%

---

# Gestion de l’enregistrement du client dynamique {#dynamic-client-registration-management}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

Avec l’adoption généralisée de [onglets personnalisés Android Chrome](https://developer.chrome.com/multidevice/android/customtabs){target_blanck} et du [contrôleur de vue Apple Safari](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target_blanck} dans les applications de nos clients, nous mettons à jour le flux d’authentification de l’utilisateur dans l’authentification Adobe Pass. Plus précisément, nous ne pouvons plus atteindre l’objectif de maintenance de l’état afin que le flux de l’agent utilisateur d’authentification d’un abonné MVPD puisse être suivi entre les redirections. Cela était auparavant effectué à l’aide de cookies HTTP. Cette limitation est le pilote pour commencer à migrer toutes les API vers OAuth 2.0 [RFC6749](https://tools.ietf.org/html/rfc6749){target_blanck}.

Grâce à cette mise à jour, les clients d’authentification Adobe deviennent des clients OAuth 2.0 et un serveur d’autorisation OAuth 2.0 personnalisé est déployé pour répondre aux besoins du service d’authentification Adobe Pass.

Pour que les applications clientes utilisent l’autorisation OAuth 2.0, le serveur doit s’enregistrer dynamiquement afin d’obtenir des informations spécifiques (informations d’identification du client) pour pouvoir interagir avec celle-ci. Dans le cadre du processus d’enregistrement, le client doit présenter un ensemble de métadonnées intégrées au point de terminaison d’enregistrement du client.

Ces métadonnées sont communiquées sous la forme d’une instruction logicielle, qui contient un &quot;software_id&quot; pour permettre à notre serveur d’autorisations de mettre en corrélation différentes instances d’une application à l’aide de la même instruction logicielle.

Une **instruction logicielle** est un jeton Web JSON (JWT) qui affirme les valeurs de métadonnées du logiciel client en tant que lot. Lorsqu’elle est présentée au serveur d’autorisations dans le cadre d’une demande d’enregistrement du client, l’instruction logicielle doit être signée numériquement ou au format MAC à l’aide de la signature web JSON (JWS).

Vous trouverez une explication plus détaillée des instructions logicielles et de leur fonctionnement dans la documentation officielle [RFC7591](https://tools.ietf.org/html/rfc7591).

L’instruction logicielle doit être déployée avec l’application sur l’appareil de l’utilisateur.

Avant cette mise à jour, nous avions deux mécanismes permettant aux applications d’effectuer des appels vers l’authentification Adobe Pass :

* les clients basés sur un navigateur sont enregistrés via la [liste de domaines](/help/authentication/programmer-overview.md#reg-and-init) autorisée
* les clients d’applications natives, tels que les applications iOS et Android, sont enregistrés par le biais du mécanisme **signed requestor**


Grâce au mécanisme d’autorisation d’enregistrement du client, vous devez ajouter vos applications au tableau de bord TVE.

Pour qu’un client puisse commencer à mettre en oeuvre le nouveau SDK Android et le prochain SDK iOS, il a besoin d’une déclaration logicielle. Une instruction logicielle identifie une application créée dans le tableau de bord TVE.

Suivez les étapes des sections ci-dessous pour créer une application enregistrée dans le tableau de bord TVE.

## Création d’une application enregistrée {#create_app}

Vous pouvez créer une application enregistrée de deux manières différentes dans le tableau de bord TVE :

* [Niveau du programmeur](#prog-level) : permet de créer une application enregistrée et de la lier à n’importe quel ou tous les canaux du programmeur.

* [Niveau de canal](#channel-level) : permet de créer une application enregistrée liée de manière permanente à ce canal seule.

### Création d’une application enregistrée au niveau du programmeur {#prog-level}

Accédez à l’onglet **Programmeurs** > **Applications enregistrées** .

![](assets/reg-app-progr-level.png)

Dans l&#39;onglet Applications enregistrées , cliquez sur **Ajouter une nouvelle application**. Renseignez les champs obligatoires de la nouvelle fenêtre.

Comme illustré ci-dessous, les champs que vous devez renseigner sont les suivants :

* **Nom de l’application** : nom de l’application

* **Attribué au canal** : nom de votre canal, t</span> auquel cette application est liée. Le paramètre par défaut dans le masque déroulant est **Tous les canaux.** L’interface vous permet de sélectionner un canal ou tous les canaux.

* **Version de l’application** : par défaut, cette valeur est définie sur &quot;1.0.0&quot;, mais nous vous encourageons vivement à la modifier avec votre propre version de l’application. Si vous décidez de modifier la version de votre application, il est recommandé de la refléter en créant une nouvelle application enregistrée.

* **Plateformes d’application** : plateformes avec lesquelles l’application doit être liée. Vous avez la possibilité de les sélectionner toutes ou plusieurs valeurs.

* **Noms de domaine** : domaines avec lesquels l’application doit être liée. Les domaines de la liste déroulante sont une sélection unifiée de tous les domaines de tous vos canaux. Vous avez la possibilité de sélectionner plusieurs domaines dans la liste. La signification des domaines est les URL de redirection [RFC6749](https://tools.ietf.org/html/rfc6749). Dans le processus d’enregistrement du client, l’application cliente peut demander à être autorisée à utiliser une URL de redirection pour finaliser le flux d’authentification. Lorsqu’une application cliente demande une URL de redirection spécifique, elle est validée par rapport aux domaines placés sur la liste blanche de cette application enregistrée associée à l’instruction logicielle.


![](assets/new-reg-app.png)


Après avoir rempli les champs avec les valeurs appropriées, vous devez cliquer sur &quot;Terminé&quot; pour que l’application soit enregistrée dans la configuration.

Notez qu’il n’existe **aucune option pour modifier une application déjà créée**. Si un élément créé ne répond plus aux exigences , une nouvelle application enregistrée doit être créée et utilisée avec l’application cliente dont elle remplit les exigences.


### Enregistrement d’une nouvelle application au niveau du canal {#channel-level}

Pour créer une application enregistrée au niveau du canal, accédez au menu &quot;Canaux&quot; et choisissez celle pour laquelle vous souhaitez créer une application. Ensuite, après avoir accédé à l’onglet &quot;Applications enregistrées&quot;, cliquez sur le bouton &quot;Ajouter une nouvelle application&quot;.

![](assets/reg-new-app-channel-level.png)

Comme illustré ci-dessous, ce qui est légèrement différent ici , par rapport à la même action effectuée au niveau du programmeur, est la liste déroulante &quot;Canaux attribués&quot; qui n’est pas activée. Il n’existe donc pas d’option pour lier l’application enregistrée à une autre que le canal actuel.

![](assets/new-reg-app-channel.png)

## Lister des applications {#list-reg-app}

Après avoir créé l’application enregistrée, il est possible d’obtenir une instruction logicielle pour présenter le serveur d’autorisation dans le cadre d’une demande.

Pour ce faire, accédez au programmeur ou au canal pour lequel les applications enregistrées ont été créées, où elles sont répertoriées.

Comme illustré ci-dessous , chaque entrée de la liste est identifiée par un nom, une version et des symboles pour les plateformes auxquelles elle a été liée.

![](assets/reg-app-list.png)

Pour chacun d’eux, vous pouvez :

* [Affichage](#view)
* [Téléchargement d’une instruction logicielle](#download-statement)

### Affichage d’une application enregistrée {#view}

Dans la liste des applications, le fait de choisir l’une d’elles et de cliquer sur le bouton &quot;Afficher&quot; affiche les détails utilisés lors de sa création. Comme mentionné précédemment, il n’existe aucune option pour modifier quoi que ce soit.


![](assets/view-reg-app.png)


### Télécharger le relevé logiciel {#download-statement}

Cliquer sur le bouton &quot;Télécharger&quot; dans l’entrée de liste pour laquelle une instruction logicielle est nécessaire génère un fichier texte. Ce fichier contiendra quelque chose de similaire à l’exemple de sortie ci-dessous.


![](assets/download-software-statement.png)

Le nom du fichier est identifié de manière unique en lui ajoutant un préfixe &quot;software_statement&quot; et l’horodatage actuel.

Veuillez noter que, pour la même application enregistrée, différentes instructions logicielles seront reçues chaque fois que l’utilisateur cliquera sur le bouton de téléchargement, mais cela n’invalide pas les instructions logicielles précédemment obtenues pour cette application. Cela se produit car ils sont générés sur place, par requête d’action.

Il existe une **limitation** concernant l’action de téléchargement. Si une instruction logicielle est requise en cliquant sur le bouton &quot;Télécharger&quot; peu après la création de l’application enregistrée et que celle-ci n’a pas encore été enregistrée et que le fichier json de configuration n’a pas été synchronisé , le message d’erreur suivant s’affiche au bas de la page.

![](assets/error-sw-statement-notready.png)

Cette opération encapsule un code d’erreur HTTP 404 Not Found reçu du noyau, car l’identifiant de l’application enregistrée n’a pas encore été propagé et le noyau n’en a aucune connaissance.

Après la création de l’application enregistrée, la solution consiste à attendre au plus 2 minutes que la configuration soit synchronisée. Après cela, le message d’erreur ne sera plus reçu et le fichier texte contenant l’instruction logicielle sera disponible au téléchargement.

Pour plus d’informations sur le fonctionnement du processus de bout en bout, ou pour mieux comprendre comment les requêtes sont exécutées et quelles réponses attendre, voir le lien dans les informations connexes ci-dessous, ainsi que d’autres liens utiles.

<!--
## Related Information {#related}

* [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
* [TVE Dashboard User Guide](/help/authentication/tve-dashboard-user-guide.md)
-->

## Démonstration des fonctionnalités {#tutorial}

Regardez [ce webinaire](https://my.adobeconnect.com/pzkp8ujrigg1/) qui donne plus de contexte sur les fonctionnalités et contient une démonstration sur la gestion des instructions logicielles à l’aide du tableau de bord TVE et sur la manière de tester les instructions générées à l’aide d’une application de démonstration fournie par Adobe dans le cadre du SDK Android.
