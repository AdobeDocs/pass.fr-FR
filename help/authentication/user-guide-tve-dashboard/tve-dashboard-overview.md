---
title: Présentation du tableau de bord TVE
description: Connaître le tableau de bord TVE et les ressources.
exl-id: 91baeb34-a32a-4dc3-94d8-f6cfca59dc4e
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# Présentation du tableau de bord TVE {#tve-db-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Le tableau de bord [[!DNL Adobe] Pass TVE Dashboard](https://experience.adobe.com/pass/authentication) est un outil permettant aux clients (programmeurs) d’Adobe Pass Authentication de gérer leur configuration et leurs données. Ce tableau de bord en libre-service permet de bénéficier de nombreuses fonctionnalités, notamment :

* **Gestion de l’intégration** : ajoutez de nouvelles intégrations entre les marques (canaux) respectives d’un programmeur et des distributeurs de programmation vidéo multicanaux (MVPD) dans l’écosystème d’authentification Adobe Pass.

* **Configuration des propriétés** : configurez plusieurs propriétés pour chaque intégration afin d’implémenter des règles métier granulaires personnalisées en fonction des besoins spécifiques de la plateforme.

* **Génération de rapports** : accédez aux rapports détaillés sur la configuration de la configuration et exportez-les dans les MVPD. Ces rapports incluent les éléments suivants :
   * Catégories de plateforme telles que *Ordinateur de bureau, mobile et appareils connectés à la télévision*
   * Plateformes telles que *iOS, Android™, tvOS, Roku et FireTV*

  Les rapports fournissent des informations sur la prise en charge de l’authentification unique (SSO) et la durée de session d’authentification ou d’autorisation des abonnés au niveau de MVPD et de la plateforme.

* **Visualisation du trafic** : visualisez les données de trafic d’authentification et d’autorisation de haut niveau des propriétés du programmeur.

Contactez votre gestionnaire de compte technique (TAM) pour accéder au tableau de bord TVE. Cet accès nécessite la configuration de deux nouveaux groupes d’utilisateurs au sein de votre organisation Adobe Marketing Cloud :

* **TVE Dashboard Read-Write** : les membres de ce groupe ont des droits de modification complets sur toutes les sections du tableau de bord.
* **TVE Dashboard Read-Only** : les membres de ce groupe ont un accès en lecture seule à l’ensemble du tableau de bord.

L’authentification Adobe Pass fournit les sections suivantes dans le tableau de bord TVE :

* [Environnements](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
* [Vérifier et transmettre les modifications](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
* [Tableau de bord](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
* [Canaux](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
* [Programmeurs](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
* [MVPD](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
* [Intégrations](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
* [Rapports](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
* [Log des modifications](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)

## Ressources {#resources-tve-dashboard}

Adobe suggère d’utiliser les ressources suivantes pour bien comprendre les flux et les fonctionnalités, ce qui vous aidera à vous familiariser avec la terminologie utilisée dans ce guide :

* [Document Technique TVE](/help/authentication/kickstart/technical-paper.md)
* [Guide De Démarrage Rapide Du Programmeur](/help/authentication/kickstart/programmer-kickstart-guide.md)
* [Présentation du guide d’intégration du programmeur](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)
* [Glossaire de Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)
* [Glossaire de l’API REST v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
