---
title: Limites
description: Découvrez les limites et le mode d’isolation MVPD pour les programmeurs dans le compte IQ.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Limites {#limitations}

Les versions D2C et TV Everywhere de Account IQ fournissent des analyses d’utilisation et de partage d’abonnements aux fournisseurs de diffusion en continu. Toutefois, la version actuelle 1.3 présente certaines limites qui seront corrigées dans les prochaines versions.

* La variable [Score de partage global](/help/accountiq/data-panels.md#overall-sharing-score) inclut actuellement [Niveau de partage](/help/accountiq/data-panels.md#sharing-level) et [Utilisation de comptes partagés](/help/accountiq/data-panels.md#usage-from-shared-accounts). D’autres mesures seront incluses dans les prochaines versions.

* Lors de la définition de segments sur le tableau de bord ou de schémas d’utilisation, la variable [Catégories de vidéos dans un segment](/help/accountiq/data-panels.md#video-categories-segment), [Score de partage par canaux et MVPD](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds), et [Distribution du modèle d’utilisation pour les catégories vidéo](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) les rapports ne peuvent afficher que les données d’au plus 20 [catégories vidéo](product-concepts.md#video-category-def). Les segments contenant plus de 20 catégories vidéo n’affichent pas de données dans ces rapports.

* Actuellement, l&#39;option d&#39;export des statistiques de compte est limitée à l&#39;export de 1000 comptes.

* Lors de la définition des opérations, l’option à sélectionner [type de segment](/help/accountiq/operations.md#segment) est limité à **Nombre fixe de comptes**. La variable **Nombre variable de comptes** sera disponible dans les prochaines versions.

* La variable **Évaluation**, **Modèles de détection**, **Actions**, et **Paramètres** les sections dans le volet de navigation de gauche sont actuellement désactivées et seront disponibles dans une version ultérieure.

* Lors de la création d’opérations, vous ne pouvez appliquer que deux types de [actions](/help/accountiq/operations.md#action) — Règles de surveillance simultanées et actions externes.

* Actuellement, vous pouvez uniquement [create](/help/accountiq/operations.md#create-new-operation) et [planning](/help/accountiq/operations.md#schedule) Les opérations. Les prochaines versions vous permettront de mettre en pause, de reprendre et de les gérer complètement.

* Vous ne pouvez analyser les données que pour une seule semaine ou un seul mois à la fois lorsque vous sélectionnez Granularité et Intervalle temporel.

* Vous ne pouvez pas ajouter de MVPD Mode d’isolation à la définition de segment avec d’autres MVPD. Certains MVPD n’identifient pas de manière unique les abonnés sur plusieurs canaux de programmation. Par conséquent, ces MVPD sont traités séparément pour les programmeurs TV Everywhere.



