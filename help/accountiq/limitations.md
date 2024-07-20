---
title: Limites
description: Découvrez les limites et le mode d’isolation MVPD pour les programmeurs dans Account IQ.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Limites {#limitations}

Les versions D2C et TV Everywhere d’Account IQ fournissent des analyses d’utilisation et de partage d’abonnements aux fournisseurs de diffusion en continu. Toutefois, la version actuelle 1.3 présente certaines limites qui seront corrigées dans les prochaines versions.

* Le [score de partage global](/help/accountiq/data-panels.md#overall-sharing-score) comprend actuellement [niveau de partage](/help/accountiq/data-panels.md#sharing-level) et [Utilisation de comptes partagés](/help/accountiq/data-panels.md#usage-from-shared-accounts). D’autres mesures seront incluses dans les prochaines versions.

* Lors de la définition de segments sur le tableau de bord ou de schémas d’utilisation, les [catégories vidéo dans le segment](/help/accountiq/data-panels.md#video-categories-segment), [score de partage par canaux et MVPD](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds) et [Répartition du modèle d’utilisation pour les catégories vidéo](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) ne peuvent afficher que des données pour 20 [catégories vidéo](product-concepts.md#video-category-def) au maximum. Les segments contenant plus de 20 catégories vidéo n’affichent pas de données dans ces rapports.

* Actuellement, l&#39;option d&#39;export des statistiques de compte est limitée à l&#39;export de 1000 comptes.

* Lors de la définition des opérations, l’option de sélection de [type de segment](/help/accountiq/operations.md#segment) est limitée à **Nombre fixe de comptes**. L’option **Nombre de variables de comptes** sera disponible dans les prochaines versions.

* Les sections **Benchmarking**, **Detection Models**, **Actions** et **Settings** dans le volet de navigation de gauche sont actuellement désactivées et seront disponibles dans une version ultérieure.

* Lors de la création d’opérations, vous ne pouvez appliquer que deux types d’ [actions](/help/accountiq/operations.md#action) : règles de moniteur de simultanéité et actions externes.

* Actuellement, vous ne pouvez effectuer que les opérations [create](/help/accountiq/operations.md#create-new-operation) et [schedule](/help/accountiq/operations.md#schedule). Les prochaines versions vous permettront de mettre en pause, de reprendre et de les gérer complètement.

* Vous ne pouvez analyser les données que pour une seule semaine ou un seul mois à la fois lorsque vous sélectionnez Granularité et Intervalle temporel.

* Vous ne pouvez pas ajouter de MVPD Mode d’isolation à la définition de segment avec d’autres MVPD. Certains MVPD n’identifient pas de manière unique les abonnés sur plusieurs canaux de programmation. Par conséquent, ces MVPD sont traités séparément pour les programmeurs TV Everywhere.



