---
title: Guide d’utilisation du tableau de bord TVE Primetime
description: Guide d’utilisation du tableau de bord TVE Primetime
exl-id: 6f7f7901-db3a-4c68-ac6a-27082db9240a
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '5524'
ht-degree: 0%

---

# Guide d’utilisation du tableau de bord Primetime TVE (hérité) {#tve-db-user-guide}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Introduction {#tve-db-intro}

[[!DNL Adobe] Tableau de bord TVE (TVE Dashboard)](https://console.auth.adobe.com/) est un tableau de bord en libre-service destiné aux utilisateurs travaillant pour des sociétés de médias (programmeurs) qui ont une relation d’affaires avec l’équipe du produit Authentification Adobe Pass.

Contactez votre gestionnaire de compte technique (TAM) pour obtenir l’accès. Pour obtenir l’accès, vous devez configurer deux nouveaux groupes d’utilisateurs dans votre organisation Adobe Marketing Cloud :

* TVE Dashboard Read-Write - Les membres de ce groupe ont des droits complets sur toutes les sections modifiables du tableau de bord
* TVE Dashboard Read-Only : les membres de ce groupe n&#39;ont que des droits d&#39;affichage sur l&#39;ensemble du tableau de bord


Avant d’aborder en détail ce guide d’utilisation, nous vous recommandons de consulter les ressources suivantes afin de bien comprendre les flux et les fonctionnalités fournis par l’équipe du produit Authentification Adobe Pass et de vous familiariser avec les termes utilisés dans le présent document :

* [Document Technique TVE](/help/authentication/kickstart/technical-paper.md)
* [Guide De Démarrage Rapide Du Programmeur](/help/authentication/kickstart/programmer-kickstart-guide.md)

Dans les sections suivantes de ce guide d&#39;utilisation, vous découvrirez comment administrer différents paramètres pour les canaux de votre entreprise, les programmeurs ou les intégrations entre les canaux et les MVPD (Multichannel Video Program Distributors).

>[!IMPORTANT]
>Le tableau de bord TVE offre la possibilité de basculer entre un Workspace de base et un avancé. Vous pouvez le faire en appuyant sur l’icône dans le coin supérieur droit. Le Workspace avancé est destiné aux utilisateurs disposant de solides connaissances techniques ainsi que d’une connaissance avancée des fonctionnalités offertes par l’équipe du produit Authentification Adobe Pass .

![Espaces de travail du tableau de bord TVE](../../../assets/tve-dashboard/old-tve-dashboard/tve-basic-advanced-workspace.png)

*Figure 1 : liste déroulante « De base / Workspace avancé » du tableau de bord Adobe Primetime TVE*

## Environnements {#authn-environments}

Selon les tâches qu’un utilisateur ou une utilisatrice peut être amené(e) à effectuer, il ou elle peut être amené(e) à basculer entre les environnements d’authentification Adobe Pass. Pour plus d’informations sur les environnements d’authentification Adobe Pass, consultez le document suivant : [Présentation des environnements d’authentification Adobe Pass](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md).

Le tableau de bord TVE fournit deux environnements nommés Préqualification et Version, chacun disposant de deux profils nommés Évaluation et Production, comme illustré ci-dessous :

* [Évaluation préalable](https://console-prequal.auth-staging.adobe.com/)
* [Production Prequal](https://console-prequal.auth.adobe.com/)
* [Évaluation des versions](https://console.auth-staging.adobe.com/)
* [Version de production](https://console.auth.adobe.com/)

Pour passer d’un environnement à l’autre, l’utilisateur peut cliquer sur l’environnement souhaité, représenté par l’entrée dans l’élément déroulant illustré ci-dessous :

![Liste déroulante Environnements de tableau de bord TVE](../../../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

*Figure 2 : liste déroulante Environnements du tableau de bord Adobe Pass TVE*

>[!IMPORTANT]
>
>Il est très important de noter que lorsque vous apportez des modifications administratives à votre configuration de l’authentification Adobe Pass via le tableau de bord TVE, nous vous conseillons vivement de suivre l’ordre ci-dessous afin d’assurer un fonctionnement correct.

Pour apporter des modifications administratives à votre configuration d’authentification Adobe Pass via le tableau de bord TVE :

* Effectuez les modifications dans [Évaluation des versions et validez-les](http://sp.auth-staging.adobe.com/apitest/api.html).
* Effectuez les modifications dans [Prequal Production et validez-les](http://sp.auth-staging.adobe.com/apitest/api.html).
* Effectuez les modifications dans [Release Production et validez-les](http://sp.auth-staging.adobe.com/apitest/api.html).

>[!IMPORTANT]
>
>Pour que les modifications administratives soient mises en ligne, les utilisateurs doivent accéder à la section « Vérifier et envoyer les modifications » en sélectionnant le bouton , qui s’affichera dans la partie inférieure gauche de la barre latérale, afin de vérifier les modifications, d’ajouter une description pour les modifications nouvellement créées et de confirmer la mise à jour de la configuration en sélectionnant la « Configuration push ».

![Tve Dashboard review an push notification](../../../assets/tve-dashboard/old-tve-dashboard/tve-review-push-notifications.png)

*Figure 3 : révision du tableau de bord Adobe Primetime TVE et notification push des modifications*

## Sections {#sections}

Les utilisateurs travaillant pour des sociétés de médias (programmeurs) peuvent accéder aux sections suivantes du tableau de bord TVE à partir de la barre latérale :

* **Canaux** - Contient les paramètres liés aux fournisseurs de contenu
* **Programmeurs** - Contient les paramètres liés à l’organisation parent agrégeant un ou plusieurs **canaux**
* **Intégrations** - contient les paramètres liés à l’intégration entre **Canaux** et **MVPD**
* **MVPDs** : contient les paramètres liés aux **MVPDs** disponibles.
* **Rapports** : contient des données agrégées pour trois types de rapports : TTL AuthN, TTL AuthZ, SSO.
* **Journal des modifications** - Contient les dernières modifications appliquées à la configuration du tableau de bord TVE

![Sections du tableau de bord TVE](../../../assets/tve-dashboard/old-tve-dashboard/tve-dashboard-sections.png)

*Figure 4 : sections Tableau de bord Adobe Primetime TVE*

### Canaux {#tve-db-channels-section}

Cette section permet d’afficher et de modifier les paramètres des canaux disponibles ou d’en créer un. Cliquer sur l’un des canaux disponibles renvoie un écran avec les onglets suivants :

* **Données de canal**
   * **Identifiant du canal** - L’identifiant unique du canal utilisé dans notre système, également appelé « identifiant du demandeur ».
   * **Nom d’affichage** - Nom commercial de la chaîne.
* **Paramètres généraux**
   * **Configuration d’Analytics** - Configurez les événements d’authentification Adobe Pass à transférer vers Adobe Analytics. Veuillez contacter l’Adobe pour plus d’informations sur la manière dont l’identifiant de suite de rapports (RSID) doit être configuré avant d’activer cette fonctionnalité.
* **Certificats**

  Contient la liste des certificats utilisés dans le flux d’authentification avec leur organisation d’émission, leur date d’émission et leur date d’expiration. Ces certificats servent de clés privées/publiques et sont utilisés à des fins de validation.
* **Domaines**

  Contient la liste des domaines à partir desquels le canal respectif communiquera avec l’authentification Adobe Pass.
* **Intégrations**

  Contient la liste des intégrations avec les MVPD disponibles, ainsi que l’état de chaque intégration qui peut être activée ou non. Pour accéder à la page Intégration , cliquez sur une entrée spécifique.
* **Applications enregistrées**

  Contient la liste des enregistrements d’applications. Pour plus d’informations, consultez le document [Dynamic Client Registration Management](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

* **Schémas personnalisés**

  Contient la liste des schémas personnalisés. Pour plus d’informations, consultez les sections [Enregistrement de l’application iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md) et [Gestion de l’enregistrement client dynamique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)

#### Ajouter/Supprimer des domaines {#add-delete-domains}

Pour lancer le processus d’ajout d’un nouveau domaine pour le canal sélectionné, vous devez cliquer sur le bouton « Ajouter un nouveau domaine » sous la liste Domaines . Vous créez ainsi une entrée de domaine dans laquelle vous pouvez spécifier le nom de domaine. Si un domaine plus générique existe déjà dans la liste des domaines, vous ne devriez pas ajouter de nouveau sous-domaine.

![Ajouter un nouveau domaine à une section de canal sélectionnée](../../../assets/tve-dashboard/old-tve-dashboard/add-domain-to-channel-sec.png)

*Image : onglet Domaines dans les canaux*

#### Créer une application enregistrée au niveau du canal {#create-registered-application-channel-level}

Pour créer une application enregistrée au niveau d’un canal, accédez au menu « Canaux » et choisissez celui pour lequel vous souhaitez créer une application. Ensuite, après avoir accédé à l&#39;onglet « Applications enregistrées », cliquez sur le bouton « Ajouter une nouvelle application ».

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-new-app-channel-level.png)

Comme illustré ci-dessous, les champs à remplir sont les suivants :

* **Nom de l’application** - nom de l’application

* **Attribué au canal** - Comme illustré ci-dessous, ce qui est légèrement différent ici, par rapport à la même action effectuée au niveau du programmeur, est le menu déroulant « Canaux attribués » qui n’est pas activé, de sorte qu’il n’y a pas d’option pour lier l’application enregistrée à d’autres canaux que le canal actuel.

* **Version de l’application** - par défaut, cette valeur est définie sur « 1.0.0 », mais nous vous encourageons vivement à la modifier avec votre propre version de l’application. Si vous décidez de modifier la version de votre application, il est recommandé de la refléter en créant une nouvelle application enregistrée pour celle-ci.

* **Plateformes d’application** - Plateformes avec lesquelles l’application doit être liée. Vous avez la possibilité de les sélectionner toutes ou plusieurs valeurs.

* **Noms de domaine** - les domaines auxquels l’application doit être liée. Les domaines de la liste déroulante sont une sélection unifiée de tous les domaines de tous vos canaux. Vous avez la possibilité de sélectionner plusieurs domaines dans la liste. La signification des domaines est URL de redirection [RFC6749](https://tools.ietf.org/html/rfc6749). Dans le processus d’enregistrement du client, l’application cliente peut demander l’autorisation d’utiliser une URL de redirection pour la finalisation du flux d’authentification. Lorsqu’une application cliente demande une URL de redirection spécifique, elle est validée par rapport aux domaines whitelistés dans cette application enregistrée associée à l’instruction du logiciel.

![](../../../assets/tve-dashboard/old-tve-dashboard/new-reg-app-channel.png)

Après avoir rempli les champs avec les valeurs appropriées, vous devez cliquer sur « Terminé » pour que l&#39;application soit enregistrée dans la configuration.

N’oubliez pas qu’il n’existe **aucune option pour modifier une application déjà créée**. Si vous découvrez qu’un élément créé ne répond plus aux exigences, une nouvelle application enregistrée doit être créée et utilisée avec l’application cliente dont elle répond aux exigences.

##### Téléchargement d’un relevé de logiciel {#download-software-statement-channel-level}

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-list.png)

Cliquer sur le bouton « Télécharger » dans l’entrée de liste pour laquelle une instruction logicielle est nécessaire génère un fichier texte. Ce fichier contiendra un élément similaire à l’exemple de sortie ci-dessous.

![](../../../assets/download-software-statement.png)

Le nom du fichier est identifié de manière unique en lui ajoutant le préfixe « software_statement » et l’horodatage actuel.

Veuillez noter que, pour la même application enregistrée, différentes déclarations de logiciel seront reçues chaque fois que le bouton de téléchargement sera cliqué, mais cela n&#39;invalide pas les déclarations de logiciel obtenues précédemment pour cette application. Cela se produit, car elles sont générées sur place, par demande d’action.

Il existe une **limitation** concernant l’action de téléchargement. Si une instruction logicielle est demandée en cliquant sur le bouton « Télécharger » peu de temps après la création de l’application enregistrée et que celle-ci n’a pas encore été enregistrée et que le fichier json de configuration n’a pas été synchronisé, le message d’erreur suivant s’affiche au bas de la page.

![](../../../assets/tve-dashboard/old-tve-dashboard/error-sw-statement-notready.png)

Cela inclut un code d’erreur HTTP 404 Introuvable reçu du noyau, car l’identifiant de l’application enregistrée n’a pas encore été propagé et le noyau n’en a pas connaissance.

La solution consiste, après la création de l’application enregistrée, à attendre au plus 2 minutes pour que la configuration soit synchronisée. Le message d&#39;erreur ne sera alors plus reçu et le fichier texte contenant l&#39;instruction logicielle pourra être téléchargé.

### Programmeurs {#tve-db-programmers-section}

Cette section permet d’afficher et de modifier les paramètres pour les programmeurs disponibles ou d’en créer un nouveau. Cliquez sur l’un des programmeurs disponibles pour renvoyer un écran avec les onglets suivants :

* **Données du programmeur**
   * **ID du programmeur** - ID unique du programmeur utilisé dans notre système.
   * **Nom complet** - Nom commercial du programmeur.
   * **URL du logo** - localisateur de ressources uniformes (URL) du logo commercial du programmeur.
   * **Aperçu du logo** - L’aperçu du logo commercial du programmeur en le téléchargeant à partir de l’URL (Uniform Resource Locator) ci-dessus.

* **Certificats**

  Contient la liste des certificats utilisés dans le flux d’authentification avec leur organisation d’émission, leur date d’émission et leur date d’expiration. Ces certificats servent de clés privées/publiques et sont utilisés à des fins de validation.

* **Canaux**

  Contient la liste des canaux appartenant à ce programmeur spécifique. Pour accéder à la section Canaux , cliquez sur une entrée spécifique.

* **Applications enregistrées**

  Contient la liste des enregistrements d’applications. Pour plus d’informations, voir [Dynamic Client Registration Management](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

* **Schémas personnalisés**

  Contient la liste des schémas personnalisés. Pour plus d’informations, voir Enregistrement de l’application [iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

#### Créer une application enregistrée au niveau du programmeur {#create-registered-application-programmer-level}

Accédez à l’onglet **Programmeurs** > **Applications enregistrées**.

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-progr-level.png)

Dans l&#39;onglet Applications enregistrées, cliquez sur **Ajouter une nouvelle application**. Renseignez les champs obligatoires dans la nouvelle fenêtre.

Comme illustré ci-dessous, les champs à remplir sont les suivants :

* **Nom de l’application** - nom de l’application

* **Affecté au canal** : nom de votre canal auquel </span> cette application est liée. Le paramètre par défaut du masque de liste déroulante est **Tous les canaux.** L’interface vous permet de sélectionner un canal ou tous les canaux.

* **Version de l’application** - par défaut, cette valeur est définie sur « 1.0.0 », mais nous vous encourageons vivement à la modifier avec votre propre version de l’application. Si vous décidez de modifier la version de votre application, il est recommandé de la refléter en créant une nouvelle application enregistrée pour celle-ci.

* **Plateformes d’application** - Plateformes avec lesquelles l’application doit être liée. Vous avez la possibilité de les sélectionner toutes ou plusieurs valeurs.

* **Noms de domaine** - les domaines auxquels l’application doit être liée. Les domaines de la liste déroulante sont une sélection unifiée de tous les domaines de tous vos canaux. Vous avez la possibilité de sélectionner plusieurs domaines dans la liste. La signification des domaines est URL de redirection [RFC6749](https://tools.ietf.org/html/rfc6749). Dans le processus d’enregistrement du client, l’application cliente peut demander l’autorisation d’utiliser une URL de redirection pour la finalisation du flux d’authentification. Lorsqu’une application cliente demande une URL de redirection spécifique, elle est validée par rapport aux domaines whitelistés dans cette application enregistrée associée à l’instruction du logiciel.

![](../../../assets/tve-dashboard/old-tve-dashboard/new-reg-app.png)

Après avoir rempli les champs avec les valeurs appropriées, vous devez cliquer sur « Terminé » pour que l&#39;application soit enregistrée dans la configuration.

N’oubliez pas qu’il n’existe **aucune option pour modifier une application déjà créée**. Si vous découvrez qu’un élément créé ne répond plus aux exigences, une nouvelle application enregistrée doit être créée et utilisée avec l’application cliente dont elle répond aux exigences.

##### Téléchargement d’un relevé de logiciel {#download-software-statement-programmer-level}

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-list.png)

Cliquer sur le bouton « Télécharger » dans l’entrée de liste pour laquelle une instruction logicielle est nécessaire génère un fichier texte. Ce fichier contiendra un élément similaire à l’exemple de sortie ci-dessous.

![](../../../assets/download-software-statement.png)

Le nom du fichier est identifié de manière unique en lui ajoutant le préfixe « software_statement » et l’horodatage actuel.

Veuillez noter que, pour la même application enregistrée, différentes déclarations de logiciel seront reçues chaque fois que le bouton de téléchargement sera cliqué, mais cela n&#39;invalide pas les déclarations de logiciel obtenues précédemment pour cette application. Cela se produit, car elles sont générées sur place, par demande d’action.

Il existe une **limitation** concernant l’action de téléchargement. Si une instruction logicielle est demandée en cliquant sur le bouton « Télécharger » peu de temps après la création de l’application enregistrée et que celle-ci n’a pas encore été enregistrée et que le fichier json de configuration n’a pas été synchronisé , le message d’erreur suivant s’affiche au bas de la page.

![](../../../assets/tve-dashboard/old-tve-dashboard/error-sw-statement-notready.png)

Cela inclut un code d’erreur HTTP 404 Introuvable reçu du noyau, car l’identifiant de l’application enregistrée n’a pas encore été propagé et le noyau n’en a pas connaissance.

La solution consiste, après la création de l’application enregistrée, à attendre au plus 2 minutes pour que la configuration soit synchronisée. Le message d&#39;erreur ne sera alors plus reçu et le fichier texte contenant l&#39;instruction logicielle pourra être téléchargé.

### Intégrations {#tve-db-integrations-sec}

Cette section permet d’afficher et de modifier les paramètres des intégrations entre les canaux et les MVPD disponibles ou d’en créer un. Cliquez sur l’une des intégrations disponibles pour renvoyer une seule page lors de l’utilisation du Workspace de base ou un écran avec les onglets suivants lors de l’utilisation du Workspace avancé :

* **Données d’intégration**
   * **ID d’intégration** : résultat de l’ajout de l’ID unique des MVPD à l’ID unique du canal séparé par le caractère « _ ».
   * **Nom d’affichage du canal** - Nom commercial du canal.
   * **Identifiant du canal** - L’identifiant unique du canal utilisé dans notre système, également appelé « identifiant du demandeur ».
   * **Nom d&#39;affichage MVPD** - Nom commercial MVPD.
   * **MVPD Id** - Identifiant unique de MVPD utilisé dans notre système.
* **Paramètres généraux**
   * **Clés de métadonnées de l’utilisateur** - Configurez les clés de métadonnées disponibles pour une intégration spécifique.
   * **Paramètres spécifiques à Platform** - Configurez différents paramètres pour une plateforme spécifique (par exemple, les TTL, la connexion unique et les IFrames).

* **Paramètres d’authentification**
   * Contient les paramètres liés à la fonction d’authentification d’Adobe Pass.
* **Paramètres d’autorisation**
   * Contient les paramètres liés à la fonctionnalité d’autorisation de l’authentification Adobe Pass.
* **Paramètres de déconnexion**
   * Contient les paramètres liés à la fonction de déconnexion de l’authentification Adobe Pass.

#### Créer une intégration {#create-integration}

Pour créer une intégration, procédez comme suit :

* cliquez sur le bouton « Ajouter une nouvelle intégration »
* rechercher et sélectionner un canal
* rechercher et sélectionner un MVPD
* attendez que le tableau de bord TVE calcule « Integration Id » et affiche les points d’entrée MVPD disponibles
* sélectionnez authentification, autorisation et points d’entrée de déconnexion ou utilisez les valeurs par défaut
* cliquez sur le bouton « Créer une intégration ».
* selon les paramètres de MVPD, une fenêtre contextuelle peut s’afficher et demander des propriétés supplémentaires, qui auraient dû être fournies par le MVPD au préalable. dans le cas contraire, une redirection vers la page d’intégration nouvellement créée a lieu

![](../../../assets/tve-dashboard/old-tve-dashboard/new-integration-window.png)



*Figure 5. La fenêtre Nouvelle intégration du tableau de bord Adobe Primetime TVE*


#### Mettre à jour l’intégration {#update-integration}

Pour mettre à jour une intégration existante, cliquez sur l’entrée de la table pour cette intégration spécifique à partir de la section Intégrations ou de la section Canaux, qui contient un onglet Intégrations .

Lors de l’utilisation du mode Workspace de base, cette section permet d’afficher et de modifier les paramètres les plus couramment mis à jour, tels que les TTL d’authentification et de jeton d’autorisation (durée de vie), ainsi que les paramètres iFrame. Gardez à l’esprit que les paramètres de durée de vie peuvent être manquants pour les intégrations aux MVPD qui prennent en charge la durée de vie de persistance des jetons définie dynamiquement (voir l’entrée 1.19 de la section [Exigences d’intégration de MVPD](/help/authentication/integration-guide-mvpds/mvpd-integr-features.md)).



En mode Workspace avancé, cette section permet d’afficher et de modifier des paramètres moins courants.



Dans le cas des modes de Workspace de base et avancé, ces paramètres peuvent être modifiés au niveau de la plateforme (par exemple, sélectionnez une valeur personnalisée pour le jeton de TTL d’autorisation sur Android, par défaut sur toutes les autres plateformes).



>[!IMPORTANT]
>Il est important de comprendre la chaîne d’héritage des paramètres : MVPD -> Point d’entrée MVPD -> Intégration -> Platform, où Platform a la valeur la plus spécifique et MVPD la valeur par défaut la plus générique.

![](../../../assets/tve-dashboard/old-tve-dashboard/inheritance-chain-component.png)


*Figure 6. Composant de chaîne d’héritage des propriétés du tableau de bord TVE Adobe Primetime*


#### Paramètres spécifiques à Platform {#platform-sp-settings}

Cette sous-section peut être utilisée pour remplacer les paramètres de plateformes spécifiques. Les plateformes disponibles sont les suivantes :

* **Toutes les plateformes** - Définissez les valeurs qui seront appliquées à toutes les plateformes, quelles que soient les implémentations du programmeur, au cas où aucune autre valeur n’est définie pour une plateforme spécifique.
* **Android** - Définissez les valeurs qui seront appliquées aux implémentations du programmeur via l’authentification Adobe Pass Android SDK.
* **API REST sans client** - Définissez les valeurs qui seront appliquées aux implémentations du programmeur sur l’API REST d’authentification Adobe Pass.
* **Fire TV** - Définissez les valeurs qui seront appliquées aux implémentations du programmeur sur Adobe Pass Authentication FireTV SDK.
* **Flash SDK** - Cette plateforme est obsolète. **obsolète**
* **JavaScript SDK** - Définissez les valeurs qui seront appliquées aux implémentations du programmeur sur Adobe Pass Authentication JavaScript SDK.
* **Roku** - Définissez les valeurs qui seront appliquées aux implémentations du programmeur sur l’API REST d’authentification Adobe Pass et qui envoient « Roku » en tant que type d’appareil. Cela est prioritaire par rapport aux valeurs définies pour la plateforme d’API REST sans client dans le cas des appareils Roku.
* **Xbox native SDK** - Cette plateforme est obsolète. **obsolète**
* **API REST Xbox 360** - Définissez les valeurs qui seront appliquées aux implémentations du programmeur via l’API REST d’authentification Adobe Pass et qui envoient « xbox » en tant que type d’appareil. Ceci est prioritaire sur les valeurs définies pour la plateforme API REST sans client dans le cas des appareils Xbox 360.
* **API REST Xbox One** - Définissez les valeurs qui seront appliquées aux implémentations du programmeur sur l’API REST d’authentification Adobe Pass et qui envoient « xboxOne » en tant que type d’appareil. Cette valeur est prioritaire sur les valeurs définies pour la plateforme de l’API REST sans client dans le cas des appareils XboxOne.
* **iOS** - Définissez les valeurs qui seront appliquées aux implémentations du programmeur via l’authentification Adobe Pass iOS SDK.
* **tvOS** - Définissez les valeurs qui seront appliquées aux implémentations du programmeur sur Adobe Pass Authentification tvOS SDK.


![](../../../assets/tve-dashboard/old-tve-dashboard/platform-sp-settings.png)

*Figure 7. Paramètres spécifiques à la plateforme du tableau de bord Adobe Primetime TVE*


#### Activer l’authentification SSO de Platform {#enable-platform-sso}

Suivez les étapes ci-dessous pour activer/désactiver l’authentification SSO pour une intégration et une plateforme spécifiques :

* Assurez-vous d’utiliser le mode Workspace avancé
* accédez à l’intégration souhaitée
* Accédez à l’onglet **Paramètres généraux**
* sélectionner la plateforme sur laquelle activer ou désactiver l&#39;authentification SSO
* Basculez l’indicateur **Activer l’authentification unique** sur la valeur souhaitée (Oui/Non).

  >[!IMPORTANT]
  >Il est important de noter que l’indicateur **Activer l’authentification unique** n’est disponible que pour les plateformes iOS, tvOS, Roku et FireTV et uniquement pour les intégrations aux MVPD qui prennent en charge l’authentification unique pour ces plateformes.

* Définissez l’indicateur **Exiger l’autorisation de plateforme** sur la valeur souhaitée (Oui/Non).

  >[!IMPORTANT]
  >Il est important de noter que l’indicateur **Appliquer l’autorisation de plateforme** contrôle si la décision de l’utilisateur d’autoriser ou de refuser l’accès de la plateforme à son abonnement de fournisseur de télévision sera appliquée ou non. En prenant en compte le scénario où **Activer l’authentification unique** l’indicateur est défini sur « Oui », **Imposer l’autorisation de plateforme** l’indicateur est également défini sur « Oui » et l’utilisateur choisit de Refuser l’accès de la plateforme à son abonnement de fournisseur de télévision, l’application correspondante (canal) ne pourra pas utiliser le jeton d’authentification Adobe Pass obtenu par une autre application (canal).


#### Activer l’authentification basée sur la page d’accueil {#enable-hba}

Suivez les étapes ci-dessous pour activer/désactiver l’authentification de base à domicile pour les fichiers MVPD basés sur **OAuth2** :

* Assurez-vous d’utiliser le mode Workspace avancé
* accédez à l’intégration souhaitée
* accédez à l’onglet **Paramètres d’authentification**
* accédez au sous-onglet **Règles dynamiques AuthN**.
* Basculez l&#39;indicateur **Tentative d&#39;adaptateur HBA** sur la valeur souhaitée (Oui/Non)


>[!IMPORTANT]
>Gardez à l’esprit que la valeur « HBA AuthN TTL » ne doit jamais être remplacée, sinon le flux d’autorisation pourrait échouer de manière inattendue.

Contactez **tve-support@adobe.com** pour plus d’informations sur l’activation de l’authentification de base interne pour les fichiers MVPD SAML.

### MVPD {#tve-db-mvpds-sec}

Cette section permet d’afficher les paramètres des fichiers MVPD disponibles. Cliquez sur l’un des fichiers MVPD disponibles pour afficher un écran avec les onglets suivants :

* **Données MVPD**
   * **MVPD Id** - Identifiant unique de MVPD utilisé dans notre système.
   * **Nom d’affichage** - Nom commercial MVPD pouvant être utilisé dans le sélecteur de l’utilisateur.
   * **URL du logo** - Localisateur de ressource uniforme (URL) du logo commercial MVPD.
   * **Aperçu du logo** - L’aperçu du logo commercial MVPD en le téléchargeant à partir du localisateur de ressources uniformes (URL) ci-dessus.
* **Paramètres généraux**
   * **Clés de métadonnées utilisateur**
      * Clés de métadonnées disponibles pour le MVPD spécifique.
   * **Propriétés des données client**
      * **Authentification/Agrégateur** - Si la valeur est définie sur « Oui », un nouveau jeton d’authentification est nécessaire pour chaque nouveau canal auquel l’utilisateur tente d’accéder.
      * **Authentification passive activée** - Si l’indicateur Auth/Agrégateur est défini sur « Oui » et si l’Authentification passive activée est définie sur « Oui », le processus d’authentification avec un autre canal se produit en arrière-plan sans qu’il soit nécessaire d’effectuer une redirection complète du navigateur et d’afficher le sélecteur.
      * **Session d’authentification/de navigateur** - Si la valeur est définie sur « Oui », la déconnexion de l’utilisateur a lieu après la fermeture du navigateur. Si la valeur est définie sur « Non », l’utilisateur peut redémarrer le navigateur et rester connecté.
      * **IFrame Obligatoire** - Si cette valeur est définie sur « Oui », cela signifie que la fenêtre de connexion de MVPD nécessite un iFrame. Les champs « Largeur iFrame » et « Hauteur iFrame » représentent la taille nécessaire pour que l’iFrame charge la page de connexion de MVPD.
* **Paramètres d’authentification**
   * **Sélectionner le point d’entrée**
      * Ce champ indique le ou les points d’entrée d’authentification exposés par le MVPD. Le point d’entrée peut différer en fonction du protocole d’authentification utilisé.
   * **Paramètres généraux AuthN**
      * Ce sous-onglet affiche le protocole d&#39;authentification utilisé par le MVPD et les informations relatives au protocole.
   * **Certificats AuthN**
      * Ce sous-onglet affiche les certificats utilisés par MVPD dans le flux d’authentification ainsi que l’organisation de l’émetteur, la date d’émission et la date d’expiration. Ces certificats servent de clés privées/publiques et sont utilisés à des fins de validation.
   * **Règles dynamiques AuthN**
      * Ce sous-onglet affiche les règles qui s&#39;appliquent au processus d&#39;authentification. En appuyant sur la touche Requête/Réponse/Jeton du diagramme, vous pouvez voir comme mis en surbrillance les paramètres appliqués à cette partie du flux d’authentification.
* **Paramètres d’autorisation**
   * **Sélectionner le point d’entrée**
      * Ce champ indique le point d’entrée d’autorisation exposé par le MVPD. Le point d’entrée peut varier en fonction du protocole d’autorisation utilisé. Les protocoles d’autorisation disponibles sont SOAP, REST (pour les appareils sans client), SAML, XACML et OAUTH.
   * **Paramètres généraux d’AuthZ**
      * Ce sous-onglet affiche le protocole d&#39;autorisation utilisé par le MVPD et les informations relatives au protocole.
      * **Configuration du contrôle en amont**
         * Il décrit le nombre de ressources qui peuvent être préautorisées par un MVPD en un seul appel, le modèle de pré-vol utilisé, ainsi que le seuil de temporisation. Parfois, le nombre de ressources peut être différent pour une intégration donnée. Pour ce faire, modifiez la propriété « **Nombre maximal de ressources de contrôle en amont** » disponible sous l’onglet Paramètres généraux. Cette propriété est disponible uniquement pour une intégration donnée et, si elle est définie, elle sera utilisée au lieu de la valeur définie dans Paramètres d’autorisation -> Configuration de pré-vol -> Ressources pré-vol max.
      * **Protection DOS**
         * Il décrit la protection contre le déni de service sur le point d’entrée d’autorisation de MVPD. Pour obtenir une description exacte de chaque champ, affichez les info-bulles en pointant la souris sur les champs Protection DOS .
      * Si le MVPD est un **TempPass**, les **Paramètres généraux d’AuthZ** contiennent également des informations concernant la durée du TempPass.
      * Si le MVPD est un **FlexibleTempPass**, alors les **Paramètres généraux d&#39;AuthZ** contiennent également des informations concernant la durée du TempPass, le nombre maximal de ressources et le champ d&#39;identification (voir l&#39;image ci-dessous).
   * **Certificats AuthZ**
      * Ce sous-onglet affiche les certificats utilisés par MVPD dans le flux d’autorisation avec l’organisation de l’émetteur, la date d’émission et la date d’expiration. Ces certificats servent de clés privées/publiques et sont utilisés à des fins de validation.
   * **Règles dynamiques AuthZ**
      * Ce sous-onglet affiche les règles qui s&#39;appliquent au processus d&#39;autorisation. En appuyant sur la touche **Requête/Réponse/Jeton** du diagramme, vous pouvez voir comme mis en surbrillance les paramètres appliqués à cette partie du flux d’autorisation.
* **Paramètres de déconnexion**
   * **Sélectionner le point d’entrée**
      * Ce champ indique le point d’entrée de déconnexion exposé par le MVPD. Les protocoles fournis peuvent être SAML ou OAuth2.
      * **Paramètres généraux de la déconnexion**
         * Ce sous-onglet affiche le protocole de déconnexion utilisé par le MVPD ainsi que des informations relatives au protocole.
         * **Exiger une réponse de déconnexion signée** - Si cette valeur est définie sur « Oui », la réponse doit être signée par un certificat de confiance.
      * **Certificats de déconnexion**
         * Ce sous-onglet affiche les certificats utilisés par MVPD dans le flux de déconnexion avec l’organisation de l’émetteur, la date d’émission et la date d’expiration. Ces certificats servent de clés privées/publiques et sont utilisés à des fins de validation.
      * **Déconnexion des règles dynamiques**
         * Ce sous-onglet affiche les règles qui s&#39;appliquent au processus de déconnexion. En appuyant sur la touche **Requête/Réponse/Jeton** du diagramme, vous pouvez voir comme mis en surbrillance les paramètres appliqués à cette partie du flux de déconnexion.

### Rapports {#tve-db-reports-sec}

Pour accéder à cette section, cliquez sur « Rapports » dans le menu « [Sections du tableau de bord »](#sections). Vous accédez alors à un écran avec 3 onglets, qui sont présentés en détail dans les sous-sections suivantes : [Rapports de durée de vie AuthN](#authn-ttl-reports), [Rapports de durée de vie AuthZ](#authz-ttl-reports), [Rapports de connexion unique](#sso-reports).

Cette section permet d’afficher et d’exporter des données agrégées pour plusieurs types de rapports pour votre ou vos intégration(s) de canaux avec différents MVPD sur toutes les plateformes.

#### Plateformes {#report-platforms}

Tous les rapports agrégent les valeurs sur les plateformes suivantes :

**NAVIGATEURS**
Affiche les valeurs qui seront appliquées aux implémentations du programmeur sur Adobe Pass Authentication JavaScript SDK.

**MOBILE : IOS**
Affiche les valeurs qui seront appliquées aux implémentations du programmeur sur Adobe Pass Authentication iOS SDK.

**MOBILE : ANDROID**
Affiche les valeurs qui seront appliquées aux implémentations du programmeur sur Adobe Pass Authentication Android SDK.

**MOBILE : AUTRES**
Affiche les valeurs qui seront appliquées aux implémentations du programmeur sur l’API REST d’authentification Adobe Pass développée pour les appareils mobiles.

**TVCD : ROKU**
Affiche les valeurs qui seront appliquées aux implémentations du programmeur sur l’API REST d’authentification Adobe Pass et qui envoient « Roku » en tant que type d’appareil.

**TVCD : FIRETV**
Affiche les valeurs qui seront appliquées aux implémentations du programmeur sur Adobe Pass Authentication FireTV SDK.

**TVCD : APPLETV**
Affiche les valeurs qui seront appliquées aux implémentations du programmeur sur Adobe Pass Authentication tvOS SDK.

**TVCD : AUTRES**
Affiche les valeurs qui seront appliquées aux implémentations du programmeur sur l’API REST d’authentification Adobe Pass développée pour les appareils connectés à la télévision.

**PLATEFORME : INCONNUE**
Affiche les valeurs qui seront appliquées aux implémentations du programmeur pour lesquelles les services d’authentification d’Adobe Pass détectent un type d’appareil inconnu.

Consultez le mécanisme de [transmission des informations du client](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md) aux API REST d’authentification Adobe Pass ou aux SDK pour plus d’informations sur la manière d’envoyer le type d’appareil souhaité (par exemple, « Roku »).

Tous les rapports agrégent les valeurs calculées en fonction de la configuration spécifique à chaque environnement Adobe Pass Authentication. Par conséquent, vous pouvez vous attendre à des données de rapports différentes lors du changement entre différents environnements de tableaux de bord TVE.

Consultez la section [Environnements](#authn-environments) pour plus d’informations sur les environnements disponibles pour l’authentification Adobe Pass.


##### Sélection de canaux/fichiers MVPD spécifiques {#selecting-specific-channels-mvpds}

Tous les rapports permettent d’utiliser des filtres en sélectionnant des canaux spécifiques ou des fichiers MVPD spécifiques à inclure dans les rapports résultants.

Pour sélectionner un ou plusieurs canaux, veuillez utiliser la **liste déroulante** placée après le libellé « Canaux sélectionnés pour le rapport ». Voir Figure 8./9./10. images ci-dessous.

Pour sélectionner un ou plusieurs MVPD(s), veuillez utiliser la **liste déroulante** placée après le libellé « MVPD sélectionnées pour le rapport ». Voir Figure 8./9./10. images ci-dessous.

Par défaut, les données sont agrégées sur tous les canaux de votre entreprise (« Tous les canaux ») et les MVPD auxquels elles sont intégrées (« Tous les MVPD »).

Si vous choisissez de désélectionner « Tous les canaux » ou « Tous les MVPD » sans choisir d’options spécifiques, l’interface utilisateur affiche un espace réservé « Aucune donnée disponible ».


##### Rapport d’exportation {#export-report}

Tous les rapports permettent d’exporter des données dans un fichier au format CSV (valeurs séparées par des virgules).

Pour exporter des données, veuillez utiliser le bouton « Exporter le rapport » placé dans le coin supérieur droit de la fenêtre. Voir Figure 8./9./10. images ci-dessous.

Un fichier nommé **Report.csv** sera automatiquement téléchargé sur votre ordinateur. Par conséquent, assurez-vous que les paramètres de votre navigateur autorisent le téléchargement des fichiers.

L’icône de chargement « Exportation des données » apparaît à l’écran pendant le calcul du fichier Report.csv, ce qui peut prendre jusqu’**minutes** selon la taille des données que vous souhaitez exporter.

#### Rapports TTL AuthN (#authn-ttl-reports)

Ce rapport affiche la durée de vie (TTL) du jeton d’authentification configuré pour votre ou vos intégration(s) de canal(aux) avec différents MVPD sur toutes les plateformes.

La durée de vie du jeton d’authentification, également appelée **TTL AuthN**, s’affiche en valeurs lisibles par l’utilisateur, telles que : **jours, heures, minutes, secondes**.

En termes d’expérience utilisateur, les rapports de durée de vie d’AuthN vous permettent d’inspecter visuellement la durée pendant laquelle un utilisateur sera authentifié, en prenant en compte un MVPD et une plateforme spécifiques.

Pour accéder à ce type de rapport, cliquez sur l’onglet « Rapports de durée de vie d’authentification » de la section « Rapports ».

![ Rapports de durée de vie AuthN ](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-authn-ttl-export-button.png)

*Figure 8 : onglet Rapport de durée de vie AuthN du tableau de bord Adobe Primetime TVE*

Le tableau Rapports de durée de vie AuthN contient des pages et peut défiler horizontalement et verticalement en fonction de la taille de votre écran.

Si vous envisagez de modifier une valeur de TTL AuthN, consultez la section [ Intégrations ](#tve-db-integrations-sec).

>[!IMPORTANT]
>L’espace réservé « **Défini par MVPD** » est utilisé dans les cas où le MVPD sera celui qui applique la valeur de TTL AuthN et non la configuration d’authentification Adobe Pass.


#### Rapports TTL AuthZ {#authz-ttl-reports}

Ce rapport affiche la durée de vie (TTL) du jeton d’autorisation configuré pour votre ou vos intégration(s) de canal(aux) avec différents MVPD sur toutes les plateformes.

La durée de vie du jeton d’autorisation, également appelée **TTL AuthZ**, s’affiche en valeurs lisibles par l’utilisateur, telles que : **jours, heures, minutes, secondes**.

En termes d’expérience utilisateur, les rapports de durée de vie AuthZ vous permettent d’examiner visuellement la durée pendant laquelle un utilisateur sera autorisé, en fonction d’un MVPD et d’une plateforme spécifiques.

Pour accéder à ce type de rapport, cliquez sur l’onglet « Rapports de durée de vie d’authentification » de la section « Rapports ».

![ Rapports de durée de vie AuthZ ](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-authz-ttl-export-button.png)

*Figure 9. L’onglet Rapport de durée de vie d’authentification TTL du tableau de bord Adobe Primetime TVE*

Le tableau Rapports de durée de vie AuthZ contient des pages et peut défiler horizontalement et verticalement en fonction de la taille de votre écran.

Si vous envisagez d’apporter une modification à une valeur de TTL AuthZ, consultez la section [ Intégrations ](#tve-db-integrations-sec).

>[!IMPORTANT]
>L’espace réservé « **Défini par MVPD** » est utilisé dans les cas où c’est le MVPD qui applique la valeur de TTL AuthZ et non la configuration de l’authentification Adobe Pass.


#### Rapports SSO {#sso-reports}

Ce rapport affiche le statut d’authentification unique (SSO) configuré pour votre ou vos intégration(s) de canal(aux) avec divers MVPD sur toutes les plateformes.

Le statut d’authentification unique, également appelé **statut de l’authentification unique**, s’affiche sous la forme d’un tri-état avec les valeurs possibles suivantes : **SSO désactivée, SSO activée, SSO incertaine**.

En termes d’expérience utilisateur, les rapports SSO vous permettent d’inspecter visuellement l’expérience SSO d’authentification de l’utilisateur attendue à partir d’un MVPD et d’une plateforme spécifiques.

Pour accéder à ce type de rapport, cliquez sur l’onglet « **Rapports SSO** » de la section « **Rapports** ».


![Onglet Rapports SSO du tableau de bord TVE](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-sso-export-button.png)


*Figure 10 : onglet Rapports SSO du tableau de bord Adobe Primetime TVE*

Le tableau Rapports SSO contient des pages et peut défiler horizontalement et verticalement en fonction de la taille de l’écran.

Si vous envisagez de modifier un statut d’authentification unique, consultez la section [ Intégrations ](#tve-db-integrations-sec).

>[!IMPORTANT]
>L’espace réservé « **SSO incertaine** » est utilisé dans les cas où la connexion unique est activée et possible, mais les paramètres de la plateforme de l’utilisateur/les décisions de l’utilisateur (par exemple, l’option de navigateur de l’utilisateur permettant de bloquer les cookies tiers, le choix de l’utilisateur de refuser l’accès de la plateforme à son abonnement de fournisseur de télévision) ou les paramètres de MVPD (par exemple, MVPD demandant l’authentification pour chaque canal) peuvent empêcher la connexion unique.

### Log des modifications {#tve-db-changelog-sec}

Cette section affiche une liste de toutes les modifications transmises par le biais du tableau de bord TVE à l’environnement d’authentification et à la configuration d’Adobe Pass.

Certaines colonnes indiquent la date de notification push, l’utilisateur qui a opéré la modification et le statut de la notification push.

Cette section permet également de comparer deux entrées de tableau afin de limiter les modifications spécifiques que vous souhaitez examiner et même partager la comparaison sous la forme d’un élément de courrier.

### Feedback {#tve-db-feedback-sec}

Cette section permet aux utilisateurs d’envoyer des commentaires. Suivez les étapes pour fournir un retour d’informations à l’équipe du produit Authentification Adobe Pass :

* cliquez sur le bouton « Commentaires » sur le côté droit de l’écran
* saisir l’objet ;
* saisir le message
* Si nécessaire, chargez une capture d’écran dans le message en cliquant sur le bouton « Charger la capture d’écran »
* cliquez sur le bouton « Envoyer ».

![formulaire de commentaires sur le tableau de bord tve](../../../assets/tve-dashboard/old-tve-dashboard/tve-dashboard-feedback.png)

*Figure 11 : section Commentaires sur le tableau de bord Adobe Primetime TVE*

Pour obtenir des instructions sur la capture de captures d’écran, consultez les liens ci-dessous :

* [Comment capturer des captures d’écran sous Windows ](https://support.microsoft.com/en-us/windows/use-snipping-tool-to-capture-screenshots-00246869-1843-655f-f220-97299b865f6b#1TC=windows-7)

* [Comment capturer des captures d’écran sur Mac ](https://support.apple.com/en-us/HT201361)

## Dépannage {#tve-db-troubleshoot}

### Mode de maintenance {#maintenance-mode}

![Application TVE en mode de maintenance](../../../assets/tve-dashboard/old-tve-dashboard/tveapp-maintenance-mode.png)


*Image : application TVE en mode de maintenance*


Si le tableau de bord TVE est en « mode de maintenance », les utilisateurs ne pourront pas afficher ni apporter de nouvelles modifications.

Si cela se produit, vous devrez attendre que l’équipe d’ingénieurs en charge de l’authentification Adobe Pass termine le travail de maintenance sur le tableau de bord TVE.

### État dégradé {#degraded-state}

![Application TVE en état dégradé](../../../assets/tve-dashboard/old-tve-dashboard/tve-degraded-state.png)


*Image : application TVE à l’état dégradé*

Si le tableau de bord TVE est à l’état « dégradé », les utilisateurs ne disposent pas des fonctionnalités de recherche et de tri, mais ils peuvent afficher ou apporter de nouvelles modifications.

Si cela se produit, vous devrez attendre que l’équipe d’ingénieurs en charge de l’authentification Adobe Pass termine le travail de maintenance sur le tableau de bord TVE.
