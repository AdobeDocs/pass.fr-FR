---
title: Opérations sur le compte IQ
description: Les opérations dans le compte IQ impliquent des actions pour effectuer des automatisations et des opérations en bloc sur les comptes d’abonnés et suivre leurs effets.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Opérations {#operations-tab-next-steps}

Une fois que vous avez analysé les schémas d’utilisation de votre abonné et identifié les instances de partage de mot de passe pour un segment sélectionné à l’aide de [!DNL Account IQ] analytics, vous pouvez effectuer des actions ciblées par le biais de procédures ciblées appelées opérations dans [!DNL Account IQ].

**Opérations** vous permettent de suivre et de gérer efficacement le partage des informations d’identification vers un groupe de comptes afin d’atténuer le partage des mots de passe et d’améliorer l’expérience des abonnés estimés.

Vous pouvez appliquer des actions à un [segment](/help/accountiq/product-concepts.md#segment-def) pour gérer le partage de mot de passe dans une [intervalle de temps](/help/accountiq/product-concepts.md#time-interval-def) et planifiez l’exécution de l’opération à une date ultérieure. Ces actions comprennent des restrictions visant à minimiser le partage de mot de passe ou à alléger les contraintes sur les comptes de non-partage.

En utilisant les opérations, vous spécifiez non seulement les actions et leur portée, mais vous évaluez également leurs résultats.

En évaluant les résultats, vous pouvez affiner votre stratégie pour optimiser les effets, que ce soit en convertissant les emprunteurs, en atténuant le partage des informations d’identification ou en réduisant le taux de perte de clientèle.

Vous pouvez exécuter différentes fonctions avec des opérations :

* [Affichage des rapports d’opération](#operation-reports)
* [Création d’une opération](#create-new-operation)
* [Opération Stop](#stop-operation)

## Affichage des rapports d’opération {#operation-reports}

Vous pouvez examiner les effets d’une opération à l’aide de rapports d’opération. Pour afficher le rapport d’opération, sélectionnez **Opérations** sous **Actions** dans le panneau de gauche de l’application Account IQ. Une liste des opérations disponibles dans le système s’affiche. Vous pouvez accéder aux détails clés de chaque opération dans un format tabulaire. Les détails incluent :

* Nom de l’opération
* État actuel (planifié, en cours d’exécution, terminé, erreur ou arrêté)
* Pourcentage d&#39;achèvement de la progression
* Public cible ou segment sur lequel l’opération est appliquée
* Type d’action sélectionnée pour l’opération
* Date de début de l’opération
* Date de fin de l’opération
* Date de création de l’opération
* Date de la dernière modification de l’opération

![](assets/operations-page.png)

*Liste et détails des opérations existantes dans le compte IQ*

Sélectionnez la **Nom de l’opération** dans la liste des opérations. Les rapports suivants sont affichés :

### Performances des opérations {#operation-performance}

Les performances de l’opération fournissent une vue d’ensemble résumant le nombre de comptes concernés, l’état d’avancement de l’opération et le score de partage global des comptes dans le segment au cours de l’opération. [période d’évaluation](/help/accountiq/product-concepts.md#evaluation-period-def).

![Rapport Performance des opérations](assets/operation-performance.png)

*Rapport Performance des opérations*

**A.** Comptes affectés **B.** En cours d&#39;opération **C.** Score de partage global

#### Comptes affectés {#impacted-accounts}

Ce nombre indique le nombre de comptes abonnés affectés par l&#39;action entreprise au cours de la période d&#39;évaluation de l&#39;opération.

#### En cours d&#39;opération {#operation-progress}

Cette jauge indique le nombre de jours et le pourcentage de l’opération effectuée en dehors du planning planifié.

#### Score de partage global {#overall-sharing-score}

Ce graphique linéaire représente la variable [score de partage global](/help/accountiq/data-panels.md#overall-sharing-score), qui inclut le niveau de partage et l’utilisation des comptes partagés de chaque semaine durant la période d’évaluation de l’opération.

### Impact de l’opération : comptes dans le segment {#impact-accounts}

Ce rapport s’affiche sous la forme d’un graphique en colonnes empilé qui illustre l’impact d’une opération au fil du temps.

![Incidence sur les opérations des comptes dans le graphique de segments](assets/accounts-in-segment.png)

*Incidence sur les opérations des comptes dans le graphique de segments*

L’axe X représente le [période d’évaluation](/help/accountiq/product-concepts.md#evaluation-period-def), tandis que l’axe des ordonnées indique l’état des comptes dans le segment de l’opération. Chaque barre du graphique est divisée en trois couleurs :

* Pink représente le nombre de comptes répondant aux conditions du segment utilisées dans cette opération.

* Bleu représente le nombre de comptes actifs qui se trouvaient à l’origine dans le segment mais qui ne remplissaient pas les conditions du segment durant chaque semaine ou mois dans le [période d’évaluation](/help/accountiq/product-concepts.md#evaluation-period-def).

* Gris représente les comptes qui étaient inactifs pendant la période d’évaluation.

>[!NOTE]
>
>La première barre rose représente le nombre de comptes répondant aux conditions du segment d&#39;opération au début de la période d&#39;évaluation.

Au fil du temps, le graphique illustre les changements de comportement du compte par rapport aux critères d’origine (par exemple, la probabilité de partage de plus de 90 appareils et l’utilisation de plus de 5 appareils sont devenus inactifs).

### Impact de l’opération : mesures des comptes partagés {#impact-shared-accounts}

Les mesures des comptes partagés fournissent un aperçu du niveau de partage et des requêtes de lecture des comptes des abonnés dans le segment de l’opération pendant le [période d’évaluation](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Niveau de partage {#share-level}

Ce graphique linéaire représente la variable [niveau de partage](/help/accountiq/data-panels.md#sharing-level) chaque semaine pendant la période d&#39;évaluation de l&#39;opération.

![Graphique linéaire du niveau de partage](assets/share-level.png){width="550" align="left"}

*Graphique linéaire du niveau de partage*

#### Nombre de requêtes de lecture {#play-requests}

Ce graphique linéaire représente la variable [requêtes de lecture](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) chaque semaine de la période d&#39;évaluation de l&#39;opération.

![Graphique linéaire du nombre de requêtes de lecture](assets/number-play-requests.png){width="550" align="left"}

*Graphique linéaire du nombre de requêtes de lecture*

### Impact de l’opération : mesures d’utilisation générales {#impact-general-usage}

Les mesures d’utilisation générales fournissent un aperçu du nombre moyen de périphériques, d’adresses IP et d’emplacements dans le segment de l’opération au cours de la [période d’évaluation](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Nombre d’appareils {#devices}

Ce graphique linéaire représente la moyenne [nombre d’appareils](/help/accountiq/general-usage-reports.md#devices-week-account) chaque semaine de la période d&#39;évaluation de l&#39;opération.

![Graphique linéaire du nombre d’appareils](assets/number-devices.png){width="550" align="left"}

*Graphique linéaire du nombre d’appareils*

#### Nombre d’adresses IP et d’emplacements {#IPs-locations}

Ce graphique linéaire représente la moyenne [nombre d’adresses IP](/help/accountiq/general-usage-reports.md#ip-week-account) et [emplacements](/help/accountiq/general-usage-reports.md#locations-week-account) chaque semaine de la période d&#39;évaluation de l&#39;opération.

![Graphique linéaire du nombre d’adresses IP et d’emplacements](assets/number-ips-locations.png){width="550" align="left"}

*Graphique linéaire du nombre d’adresses IP et d’emplacements*

Pour fermer le rapport et revenir à la page principale **Opérations** page, sélectionnez **Opérations** sous **Actions** dans le panneau de gauche.

## Créer une opération {#create-new-operation}

Lorsque vous accédez au **Opérations** sous **Actions** dans le panneau de gauche, sélectionnez **Créer une opération** en haut de la page **Opérations** page.

Pour créer une opération, suivez les instructions des sections suivantes :

* [Détails de l&#39;opération](#operation-details)
* [Segment](#segment)
* [Action](#action)
* [Planning](#schedule)

### Détails de l&#39;opération {#operation-details}

Dans cette section, saisissez le nom de l’opération dans **Nom de l’opération**.

>[!TIP]
>
>Décrire l’objectif de l’opération ou la nature de l’action dans **nom de l’opération** pour une identification rapide. L’option pour **Ajout d’une description et de balises** seront disponibles dans les prochaines versions.

![Ajouter le nom de l’opération dans les détails de l’opération](assets/operation-details.png)

*Ajouter le nom de l’opération*

### Segment {#segment}

Dans cette section, cliquez sur **Sélectionner un segment** et choisissez un segment auquel vous souhaitez utiliser cette opération. Formation [sélection d’un segment](/help/accountiq/segments-timeinterval.md#segment-selection).

Une fois que vous avez sélectionné un segment, utilisez <img alt= "développer le résumé du segment" src="./assets/expand-segment-summary.svg" width="25"> pour afficher le résumé détaillé du segment. En savoir plus sur [résumé du segment](segments-timeinterval.md#segment-summary).

![Sélection d’un segment et d’un intervalle de temps](assets/select-segment-timeinterval.png)

*Sélection d’un segment et d’un intervalle de temps*

>[!NOTE]
>
>La variable [catégories vidéo](product-concepts.md#video-category-def) affiché dans l’image précédente, comme **MVPD**, **Programmeurs**, et **Canaux** représentent les étiquettes utilisées dans la version TV Everywhere de Account IQ. Si vous êtes connecté en tant que service D2C, ces étiquettes affichent les catégories vidéo spécifiques de votre entreprise.

Si nécessaire, utilisez <img alt= "modifier le segment" src="./assets/edit-segment.svg" width="25"> pour modifier le segment sélectionné ou  <img alt= "créer un segment" src="./assets/create-new-segment.svg" width="25"> pour créer un segment. Pour plus d’informations, reportez-vous aux instructions de la section [création d’un segment](work-with-segments.md#create-new-segment) ou [modification d’un segment](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>**Type de segment** named **[!UICONTROL Fixed number of accounts]** est actuellement sélectionné par défaut. L’option à sélectionner **[!UICONTROL Variable number of accounts]** seront disponibles dans les prochaines versions.

Sélectionner **Granularité et intervalle de temps** pour surveiller l’opération pendant une période spécifique. En savoir plus sur [sélection de la granularité et de l’intervalle de temps](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### Action {#action}

Dans cette section, choisissez une **Action** vous souhaitez exécuter sur le segment sélectionné dans le menu déroulant.

![Sélectionner le type d’action](assets/apply-actions.png)

*Sélectionner le type d’action*

Deux options sont disponibles :

* Sélectionner **CM Policy** pour le système de surveillance de la simultanéité intégré au compte IQ.

* Sélectionner **Actions externes** pour créer et traiter des processus externes à Account IQ et non intégrés au système de compte IQ.

>[!NOTE]
>
>Les actions externes peuvent ne pas toujours être directement liées au partage de mot de passe, mais peuvent encore avoir un impact, comme le lancement d’une nouvelle saison.

### Planning {#schedule}

Dans cette section, sélectionnez la variable **Date de début** et **Date de fin** à partir du sélecteur de date pour définir l’activation de l’opération.

>[!IMPORTANT]
>
>Actuellement, l’activation par défaut **Date de début** et **Date de fin** sont définis sur **À date**. L’option à sélectionner **Lorsqu’une condition est remplie** et **Manuellement** seront disponibles dans les prochaines versions.

>[!NOTE]
>
>Assurez-vous que la date de début et la date de fin correspondent à la granularité sélectionnée pour l’évaluation dans **Étape 4**.

* Si vous avez choisi d’agréger la granularité par semaines, sélectionnez les dates de début et de fin dans les semaines (Semaine 10, par exemple).
* Si vous avez choisi d’agréger la granularité par mois, sélectionnez les dates de début et de fin par mois.

![Sélectionnez les dates de début et de fin dans le sélecteur de date.](assets/add-schedule.png)

*Sélectionnez Date de début et Date de fin dans le sélecteur de date.*

**A.** Démarrage du sélecteur de date **B.** Sélecteur de date de fin

>[!NOTE]
>
>La variable **Date de début** doit être postérieur à la période d’évaluation et à la date actuelle, tandis que la variable **Date de fin** doit être postérieur à la date de début et à la date courante pour planifier et exécuter des opérations dans la période ultérieure.

Sélectionner **Opération Save** en haut de la page **Opérations** pour traiter une nouvelle opération.

## Opération Stop {#stop-operation}

Vous pouvez uniquement arrêter les opérations qui se trouvent actuellement dans **En cours** statut. Pour arrêter une opération existante, procédez comme suit :

1. Accédez au **Opérations** sous **Actions** dans le volet de navigation de gauche de l’application Account IQ.
1. Sélectionner **Options** du menu de l’opération que vous souhaitez arrêter.

   ![Menu Sélectionner les options pour arrêter l’opération](assets/stop-operation.png)

   *Sélectionnez le menu Options pour arrêter l’opération.*

1. Sélectionner **Arrêter**.



