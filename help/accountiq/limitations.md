---
title: Limites et problèmes connus
description: Problèmes connus dans le produit.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 2ced89dc1f77d2c090b599c40e778f3054f1a8dd
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Problèmes connus et limites {#known-issues}

Adobe s’efforce d’offrir des fonctionnalités robustes et des expériences utilisateur fluides grâce à ses offres. La version actuelle (version 1.0) de Account IQ fournit des analyses d’utilisation et de partage d’abonnements aux fournisseurs de diffusion en continu avec un degré de confiance élevé. Toutefois, les restrictions suivantes seront corrigées dans les versions à venir.

* Lors de la définition de cohortes dans le tableau de bord ou les pages de rapports, il n’existe actuellement aucune option pour ajouter des mesures telles que **nombre d’appareils** pour affiner le segment. Cette fonctionnalité sera disponible dans une version ultérieure.

* Lors de l’estimation des scores de partage pour les comptes individuels, Account IQ adopte une approche conservatrice qui permet aux entreprises d’agir sur le partage avec un degré de confiance élevé. Cependant, cette approche tend à sous-estimer le montant total du partage lorsqu’il est agrégé sur de nombreux comptes.

* La variable [Score de partage global](/help/accountiq/dashboard.md#overall-sharing-score) facteurs figurant actuellement uniquement dans [Niveau de partage](/help/accountiq/dashboard.md#sharing-level) et [Utilisation des comptes partagés](/help/accountiq/dashboard.md#usage-from-shared-accounts). Les futures versions prendront en compte les mesures supplémentaires.

* Lors de la définition de cohortes dans le tableau de bord ou les pages de rapports, les sélecteurs des distributeurs multicanaux de programmes audiovisuels et des canaux ne disposent pas du mécanisme de recherche, pour l’instant.

* Lors de la définition de cohortes dans le tableau de bord ou les pages de rapports, il est limité de sélectionner uniquement 10 MVPD et programmeurs (ou canaux individuels).

* L&#39;option d&#39;export des statistiques de compte est limitée à l&#39;export de 1000 comptes, pour l&#39;instant.

* L’option à sélectionner [Type de segment](#segment-type) lorsque la définition des opérations est limitée à **Nombre fixe de comptes**. La variable **Nombre variable de comptes** sera disponible dans une version ultérieure.

* Les sections Évaluation, Modèles de détection, Segments, Instantanés et Règles du volet de navigation de gauche sont actuellement désactivées et seront disponibles dans une version à venir.

* Lors de la création [Opérations](/help/accountiq/operation-affecting-user-segment.md), vous ne pouvez identifier que deux types de [Actions](/help/accountiq/operation-affecting-user-segment.md) à compter de maintenant — Règles de contrôle de simultanéité et actions externes.

* Actuellement, les opérations ne peuvent être créées que et [scheduled](/help/accountiq/operation-affecting-user-segment.md#action). Les prochaines versions vous permettront de mettre en pause, de reprendre et de les gérer entièrement.

* En raison de l’ensemble de données plus limité utilisé, le mode Isolation ne reflète pas vraiment la quantité de partage. Par conséquent, le MVPD en mode Isolation ne peut pas être comparé à un autre MVPD. <!--do we need to separate out this limitation, which is from a different persona i.e. only for Programmer persona?-->

* Lorsque vous définissez une nouvelle [segment](/help/accountiq/segments-timeframe.md) pour une opération, vous pouvez ajouter des mesures. Mais si vous sélectionnez un segment enregistré, vous ne pouvez pas ajouter d’autres mesures pour affiner le segment.

* La granularité et le sélecteur de période sont limités à une semaine ou un mois, ce qui signifie que les données peuvent être évaluées sur une seule semaine ou un seul mois.

* Les intervalles prédéfinis sont actuellement désactivés dans la granularité et le sélecteur de périodes et seront disponibles dans une version ultérieure.
