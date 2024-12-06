---
title: Canaux
description: Découvrez les canaux et leurs différentes configurations dans le tableau de bord TVE.
exl-id: bbddeccb-6b6f-4a8f-87ab-d4af538eee1d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 0%

---

# Canaux {#channels}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

La section **Canaux** du tableau de bord TVE vous permet d’afficher et de gérer les paramètres des canaux associés à un programmeur spécifique. Vous pouvez également [ajouter un nouveau canal](#add-new-channel) selon vos besoins.

L’onglet **Canaux** du panneau de gauche affiche une liste des canaux liés avec les détails suivants :

* **Nom d’affichage** : nom de marque du canal utilisé à des fins commerciales.
* **Identifiant de canal** : un identifiant unique, également appelé identifiant de demandeur.
* **Intégrations** : nombre de connexions établies avec [MVPDs](/help/authentication/kickstart/glossary.md#mvpd).

![Liste des canaux existants](../assets/tve-dashboard/new-tve-dashboard/channels/channels-list-view.png)

*Liste des canaux existants*

Saisissez le nom du canal dans la barre **Rechercher** située au-dessus de la liste pour en savoir plus sur le canal.

## Gestion des configurations de canal {#manage-channel-conf}

Suivez les étapes de gestion des différents paramètres d’un canal spécifique.

1. Sélectionnez l’onglet **Channels** (Canaux) dans le panneau de gauche.

1. Sélectionnez le canal dans la liste disponible.

1. Sélectionnez l’un des onglets suivants pour afficher et modifier les paramètres correspondants du canal sélectionné :

   * [Paramètres généraux](#general-settings)
   * [Intégrations](#integrations)
   * [Certificats](#certificates)
   * [Domaines](#domains)
   * [Applications enregistrées](#registered-applications)
   * [Schémas personnalisés](#custom-schemes)

   ![Paramètres du canal](../assets/tve-dashboard/new-tve-dashboard/channels/channel-tabs-view.png)

   *Paramètres du canal*

>[!IMPORTANT]
>
> Afficher [Réviser et pousser les modifications](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) pour plus d’informations sur l’activation des modifications de configuration.

### Paramètres généraux {#general-settings}

Cet onglet présente les **informations sur le canal** et la **configuration Analytics**.

#### Informations sur les canaux {#channel-information}

Dans cette section, vous pouvez modifier les détails suivants :

* **Nom d’affichage** : nom de marque du canal utilisé à des fins commerciales.

* **URL de redirection par défaut** : URL de redirection de sauvegarde pour l’authentification et la déconnexion.

* **Rapport d’erreurs** : lorsque vous sélectionnez **Oui**, les SDK Adobe Pass envoient des rapports d’erreur au serveur principal Adobe Pass pour les analyses.

![Modifier les informations sur le canal](../assets/tve-dashboard/new-tve-dashboard/channels/channel-general-settings-tab-view.png)

*Modifier les informations sur le canal*

#### Configuration Analytics {#analytics-configuration}

Cette section vous permet de configurer le transfert des événements d’authentification Adobe Pass vers Adobe Analytics.

Pour activer la **configuration Analytics**, contactez votre gestionnaire de compte technique (TAM) pour plus d’informations sur la configuration de l’identifiant de suite de rapports (RSID).

![Activer les configurations Analytics](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-analytics-configuration-button.png)

*Activer les configurations Analytics*

Sélectionnez **Ajouter une nouvelle configuration d’analyse** pour ajouter plusieurs configurations.

Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Pour utiliser la nouvelle configuration d’analyse de la section **Configuration d’Analytics**, passez au flux [ de  modifications de révision et de notification push](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

### Intégrations {#integrations}

Cet onglet affiche une liste des intégrations disponibles entre le canal actuellement sélectionné et les MVPD. La liste présente chaque intégration avec son état, indiquant si elle est activée ou non. Sélectionnez une intégration spécifique dans cette liste pour accéder à des informations détaillées dans la section [Intégrations](tve-dashboard-integrations.md) .

![Liste des intégrations disponibles](../assets/tve-dashboard/new-tve-dashboard/channels/channel-integrations-tab-view.png)

*Liste des intégrations disponibles*

### Certificats {#certificates}

Cet onglet affiche la liste des [certificats disponibles](#available-certificates) et des [ certificats disponibles hérités](#inherited-avail-certificates) utilisés dans les flux de chiffrement des métadonnées utilisateur. Il affiche des détails sur chaque certificat qui comprend :

* État (activé ou non pour l’utilisation du **chiffrement des métadonnées utilisateur**)
* Numéro de série
* Nom de l’organisation émettrice
* Nom de l’organisation concernée
* Date d’émission
* Date d’expiration
* Un menu déroulant pour chiffrer les métadonnées utilisateur (si vous sélectionnez **Oui**, le certificat chiffre les informations utilisateur sensibles, telles que les valeurs de code postal).

#### Certificats disponibles {#available-certificates}

Ces certificats servent de clés privées ou publiques et sont utilisés pour le chiffrement des métadonnées utilisateur.
Vous pouvez apporter les modifications suivantes dans la section certificats disponibles :

* [Ajouter un nouveau certificat](#add-new-certificate)
* [Suppression de certificat](#delete-certificate)

##### Ajouter un nouveau certificat {#add-new-certificate}

Pour ajouter un nouveau certificat, procédez comme suit :

1. Sélectionnez **Ajouter un nouveau certificat** en haut de la section **Certificats disponibles** .

   ![Ajouter un nouveau certificat](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-certificate-button.png)

   *Ajouter un nouveau certificat*

1. Collez la clé publique de votre certificat dans la boîte de dialogue **Nouveau certificat**.

1. Sélectionnez **Ajouter un certificat**.

1. Recherchez le nouveau certificat dans la liste de **certificats disponibles**.

   >[!IMPORTANT]
   >
   > Assurez-vous que vos systèmes sont à jour et prêts à utiliser le nouveau certificat.

1. Sélectionnez **Oui** dans le menu déroulant **Utilisé pour les métadonnées utilisateur chiffrées** pour activer un nouveau certificat.

Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Pour utiliser le nouveau certificat répertorié dans la section **Certificats disponibles**, passez au flux [ de  modifications de révision et de notification push](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

##### Suppression de certificat {#delete-certificate}

Pour supprimer un certificat, procédez comme suit.

1. Pointez sur le certificat que vous souhaitez supprimer de la liste des **certificats disponibles**.

1. Sélectionnez **Supprimer**.

   ![Supprimer le certificat sélectionné](../assets/tve-dashboard/new-tve-dashboard/channels/channel-delete-certificate-button.png)

   *Supprimer le certificat sélectionné*

1. Sélectionnez **Supprimer** dans la boîte de dialogue **Supprimer le certificat actif**.

Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Le certificat sera supprimé de la section **Certificats disponibles** uniquement après [la révision et la notification push des modifications](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Certificats disponibles hérités {#inherited-avail-certificates}

Les sociétés de médias définissent ces certificats à leur propre niveau. Tous les canaux associés à la même société de médias peuvent utiliser ces certificats.

![Certificats disponibles hérités](../assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-available-certificates-panel-view.png)

*Certificats disponibles hérités*

### Domaines {#domains}

Cet onglet affiche une liste des domaines disponibles par le biais desquels le canal respectif communique avec l’authentification Adobe Pass.

Vous pouvez apporter les modifications suivantes aux domaines :

* [Ajouter un nouveau domaine](#add-domains)
* [Supprimer le domaine](#delete-domain)

>[!TIP]
>
> Évitez d’ajouter un nouveau sous-domaine s’il existe un domaine plus général dans la liste.

#### Ajouter un nouveau domaine {#add-domains}

Pour ajouter un domaine, procédez comme suit.

1. Sélectionnez **Ajouter un nouveau domaine** dans le coin supérieur droit de la section **Domaines disponibles** .

   ![Ajouter un nouveau domaine](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-domain-button.png)

   *Ajouter un nouveau domaine*

1. Saisissez le nom de votre domaine dans la boîte de dialogue **Nouveau domaine**.

1. Sélectionnez **Ajouter un domaine** pour ajouter un nouveau domaine pour le canal sélectionné.

Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Pour utiliser le nouveau domaine répertorié dans la section **Domaines disponibles**, passez au flux [ de  modifications de révision et de notification push](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Supprimer le domaine {#delete-domain}

Pour supprimer un domaine, procédez comme suit.

1. Pointez sur le domaine que vous souhaitez supprimer de la liste de **domaines disponibles**.

1. Sélectionnez **Supprimer**.

   ![Supprimer le domaine sélectionné](../assets/tve-dashboard/new-tve-dashboard/channels/channel-remove-domain-button.png)

   *Supprimer le domaine sélectionné*

1. Sélectionnez **Supprimer** dans la boîte de dialogue **Supprimer le domaine**.

Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Le domaine sera supprimé de la section **Domaines disponibles** uniquement après [la révision et la notification push des modifications](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

Le domaine sélectionné n’est plus disponible. Par conséquent, l’application associée à ce domaine perd l’accès aux services d’authentification Adobe Pass.

### Applications enregistrées {#registered-applications}

Cet onglet affiche la liste des applications enregistrées. Pour plus d’informations sur l’utilisation des applications enregistrées, consultez la documentation [présentation de l’enregistrement dynamique du client](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) .

Vous pouvez effectuer les actions suivantes avec les applications enregistrées :

* [Ajouter une nouvelle application enregistrée](#add-registered-applications)
* [Téléchargement d’une instruction logicielle](#download-software-statement)

#### Ajouter une nouvelle application enregistrée {#add-registered-applications}

Pour ajouter une nouvelle application enregistrée, procédez comme suit.

1. Sélectionnez **Ajouter une nouvelle application** dans le coin supérieur droit de la section **Applications enregistrées** .

   ![Ajouter une nouvelle application](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-application-button.png)

   *Ajouter une nouvelle application*

1. Sélectionnez **Plateformes** dans le menu déroulant de la boîte de dialogue **Nouvelle application**.

   >[!IMPORTANT]
   >
   > Il est recommandé de créer des applications enregistrées avec des autorisations plus spécifiques et limitées afin d’améliorer la sécurité et de prévenir les accès non autorisés. Par conséquent, lors de la création d’applications enregistrées, envisagez d’utiliser des options plus étroites pour le `platforms` affecté.

1. Sélectionnez **Domaines** dans le menu déroulant.

   >[!IMPORTANT]
   >
   > Dans le processus d’enregistrement du client, l’application cliente peut demander à être autorisée à utiliser une URL de redirection pour finaliser le flux d’authentification. Lorsqu’une application cliente utilise une URL de redirection spécifique, elle est validée par rapport au `domains` sélectionné dans cette sélection.

1. Saisissez le **nom** de l’application.

1. Saisissez la **version** de l’application.

   >[!IMPORTANT]
   >
   > Il est recommandé de créer une nouvelle application enregistrée pour chaque mise à jour majeure de votre application cliente afin de gérer son cycle de vie et son utilisation. Si nécessaire, créez un ticket via notre [Zendesk](https://adobeprimetime.zendesk.com) et demandez à votre gestionnaire de compte technique (TAM) de révoquer une application enregistrée afin de bloquer les fonctionnalités d’une version spécifique de l’application cliente.

1. Sélectionnez la valeur **Type** &quot;DIRECT&quot; dans le menu déroulant.

1. Sélectionnez **Ajouter une application**.

Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Pour utiliser la nouvelle application enregistrée répertoriée dans la section **Applications enregistrées**, passez au flux [ review and push changes](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) .

#### Téléchargement de l’instruction logicielle {#download-software-statement}

Pour télécharger une instruction logicielle, procédez comme suit.

1. Pointez sur l’application enregistrée que vous souhaitez télécharger l’instruction logicielle de la liste des **Applications enregistrées**.

1. Sélectionnez **Télécharger**.

   ![Télécharger une instruction logicielle](../assets/tve-dashboard/new-tve-dashboard/channels/channel-download-software-statement-button.png)

   *Télécharger une instruction logicielle*

### Schémas personnalisés {#custom-schemes}

Cet onglet affiche une liste des schémas personnalisés. Pour plus d’informations sur l’utilisation des schémas personnalisés, reportez-vous à l’ [enregistrement de l’application iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

Vous pouvez apporter les modifications suivantes aux schémas personnalisés :

* [Génération d’un nouveau modèle personnalisé](#generate-custom-schemes)

#### Générer un nouveau modèle personnalisé {#generate-custom-schemes}

Pour générer un nouveau schéma personnalisé, procédez comme suit.

1. Sélectionnez **Générer un nouveau schéma personnalisé**.

   ![Générer un nouveau schéma personnalisé](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-custom-scheme-button.png)

   *Générer un nouveau schéma personnalisé*

Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Pour utiliser le nouveau schéma personnalisé répertorié dans la section **Schémas personnalisés**, passez au flux [ de  révision et notification push modifications](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Schémas personnalisés hérités {#inherited-custom-schemes}

Les sociétés de médias définissent ces schémas personnalisés à leur propre niveau. Tous les canaux associés à la même société de médias peuvent utiliser ces schémas personnalisés.

![Schémas personnalisés hérités](../assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-custom-schemes-panel-view.png)

*Schémas personnalisés hérités*

## Ajouter un nouveau canal {#add-new-channel}

Pour ajouter un nouveau canal, procédez comme suit.

1. Sélectionnez l’onglet **Channels** (Canaux) dans le panneau de gauche.

1. Sélectionnez **Ajouter un nouveau canal** dans le coin supérieur droit de la section **Canaux** .

   ![Ajouter un nouveau canal](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-channel-button.png)

   *Ajouter un nouveau canal*

1. Sélectionnez **ID de programmeur** dans le menu déroulant de la boîte de dialogue **Nouveau canal**.

1. Saisissez un identifiant unique dans le **identifiant de canal**.

1. Saisissez le nom de marque du canal utilisé à des fins commerciales dans le **Nom d’affichage**.

1. Sélectionnez **Ajouter un canal**.

Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Pour utiliser le nouveau canal répertorié dans la section **Channels** (Canaux), passez au flux [review and push changes](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) (revoir et pousser les modifications).
