---
title: Chemin de mise à niveau AccessEnabler iOS/tvOS 3.7.0
description: Chemin de mise à niveau AccessEnabler iOS/tvOS 3.7.0
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Chemin de mise à niveau AccessEnabler iOS/tvOS 3.7.0 (hérité) {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>

Les modifications apportées au stockage de trousseau depuis la [nouvelle version d’AccessEnabler 3.7.0](/help/authentication/notes-releases/authn-rn-ios-tvos-370.md) sont incompatibles avec l’implémentation du stockage de trousseau depuis la version d’AccessEnabler inférieure à 3.7.0.

Le chemin de mise à niveau d’une application adoptant la nouvelle version 3.7.0 d’AccessEnabler migre tous les jetons des versions précédentes du stockage de trousseau. Par conséquent, les utilisateurs finaux **ne doivent pas subir de perte de sessions d&#39;authentification/autorisation** pendant le processus de mise à jour de la structure AccessEnabler.

## Limites connues

Certaines limitations, décrites ci-dessous, peuvent être rencontrées par les personnes chargées de la mise en œuvre.


1. La connexion unique standard (Adobe) ne fonctionne pas entre une application utilisant AccessEnabler version 3.7.0 et une application utilisant AccessEnabler version(s) inférieure(s) à 3.7.0, même pour les applications développées par le même fournisseur.

   >[!IMPORTANT]
   >
   >* L’authentification unique (SSO) au niveau du système (Apple) ne sera pas affectée.
   >
   >* La connexion unique régulière (Adobe) continuera à fonctionner si les deux applications sont développées par le même fournisseur et utilisent la ou les versions d&#39;AccessEnabler inférieures à 3.7.0 !
   >
   >* Une connexion unique standard (Adobe) fonctionne si les deux applications sont développées par le même fournisseur et utilisent la version 3.7.0 d&#39;AccessEnabler !


1. En cas de rétrogradation d’une application à l’aide d’AccessEnabler version 3.7.0 vers une version inférieure d’AccessEnabler, les nouveaux jetons générés ne seront pas migrés. Par conséquent, les utilisateurs finaux peuvent subir la perte de sessions d’authentification/d’autorisation, sans s’y attendre.

   >[!IMPORTANT]
   >
   >* Les utilisateurs finaux authentifiés via l’authentification unique au niveau du système (Apple) ne seront pas affectés.
   >* Les utilisateurs finaux déjà authentifiés avant la mise à jour vers la nouvelle application utilisant AccessEnabler version 3.7.0 ne seront pas affectés !

1. Dans le cas d&#39;une rétrogradation d&#39;une application utilisant AccessEnabler version 3.7.0 à une version inférieure d&#39;AccessEnabler, les jetons supprimés ne seront pas reconnus. Par conséquent, les utilisateurs finaux peuvent rencontrer la présence de sessions d’authentification/autorisation, sans s’y attendre.
