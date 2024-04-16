---
title: Exporter des informations pour les comptes avec un score de partage élevé
description: Exportez des informations pour les comptes ayant un score de partage élevé.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# Exporter des informations pour les comptes avec un score de partage élevé {#export-account-info-high-score}

[!UICONTROL Account IQ] vous permet d’exporter les détails du partage de compte pour les 1 000 premiers comptes abonnés en fonction de leurs [probabilités de partage](/help/accountiq/product-concepts.md#account-sharing-probability-def). Vous pouvez exporter les informations de partage de compte pour la [segment](/help/accountiq/product-concepts.md#segment-def) et [intervalle de temps spécifié](/help/accountiq/product-concepts.md#time-interval-def) sur le [Rapports sur les comptes partagés](/help/accountiq/shared-acc-reports.md) page.

Suivez les étapes pour exporter les informations de partage de compte des comptes abonnés pour un segment spécifique.

1. Connectez-vous à l’aide de vos informations d’identification.
1. Accédez au **Comptes partagés** sous **Rapports** .
1. Sélectionnez le segment et l’intervalle dans le panneau Segment et intervalle de temps. Formation [comment sélectionner un segment et un intervalle de temps](segments-timeinterval.md).

   Si nécessaire, reportez-vous aux instructions pour [création d’un segment](work-with-segments.md#create-new-segment) ou [modification d’un segment](work-with-segments.md#edit-segment).

1. Sélectionner **[!UICONTROL Export top 1000 accounts]** situé dans le coin supérieur droit du panneau segment et intervalle de temps.

   ![Exporter les 1 000 premiers comptes](assets/export-top-1000-accounts.png)

   *Sélectionnez l’option Exporter les 1 000 premiers comptes .*

Le fichier sera automatiquement téléchargé sur votre ordinateur local au format .csv.

Ce fichier contient les données des 1000 premiers comptes en fonction des probabilités de partage des comptes abonnés du segment actuel dans un ordre décroissant.

Voici un exemple du fichier .csv exporté.

![données exportées dans le fichier .csv](assets/exported-csv.png)

*Données exportées dans le fichier .csv*

## Colonnes du rapport exporté {#columns-in-export}

**Semaine/Mois**

La semaine ou le mois sélectionné dans la variable **[!UICONTROL Granularity and Time Interval]** dans le sélecteur de segments.

**MVPD**

Si vous êtes programmeur, la colonne indique le distributeur avec lequel le compte est abonné.

>[!NOTE]
>
> La variable **MVPD** n’est disponible que pour les versions TV partout.

**ID d’abonné**

Identifiant unique du compte spécifique.

**Nombre minimal d’appareils**

Nombre minimum d’appareils sur lesquels les utilisateurs diffusent activement du contenu.

>[!NOTE]
>
>Le nombre réel d’appareils qui diffusent du contenu en continu est supérieur au nombre minimum d’appareils spécifié pour un compte particulier.

**Nombre minimum de personnes**

Le nombre minimum d’individus qui diffusent activement du contenu à l’aide de ces appareils.

>[!NOTE]
>
>Le nombre réel d’individus qui diffusent du contenu est supérieur au nombre minimum de personnes affectées à un compte particulier.

**[!UICONTROL # IPs]**

Nombre d’adresses IP à partir desquelles le contenu est diffusé en continu.

**[!UICONTROL # Locations]**

Nombre d’emplacements (en fonction du code postal) à partir desquels le contenu est diffusé en continu.

**[!UICONTROL # Cities]**

Nombre de villes où l’activité de diffusion en continu a eu lieu.

**[!UICONTROL # States]**

Nombre d’états où l’activité de diffusion en continu a eu lieu.

**[!UICONTROL # Clusters]**

Le nombre de variables distinctes [grappes](/help/accountiq/product-concepts.md#cluster-def) où la diffusion en continu a eu lieu.

**[!UICONTROL Geographic span (miles)]**

Distance maximale entre les emplacements de diffusion en continu associés au compte.

**[!UICONTROL # AuthN OK]**

Nombre d’identifications effectuées par les utilisateurs au cours de la période spécifiée à l’aide de ce compte.

>[!NOTE]
>
> Certains services D2C peuvent ne pas voir **[!UICONTROL # AuthN OK]** données, car elles peuvent ne pas être incluses dans les données de leur entreprise.

**[!UICONTROL # AuthZ OK]**

Nombre de fois où un MVPD a autorisé un flux ou accordé l’accès au contenu pour ce compte.

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** n’est pas disponible pour les services D2C.

>[!NOTE]
>
>Pour TV partout, **[!UICONTROL # AuthZ OK]** est corrélé avec le nombre de **[# Lecture de requêtes](/help/accountiq/product-concepts.md##play-requests-def)**. Elle sera toujours inférieure à **[!UICONTROL # Play Requests]** car Adobe met généralement les autorisations des MVPD en cache pendant environ 24 heures.


**[!UICONTROL # Play Requests]**

Le nombre réel de diffusions s’est produit au cours d’une période donnée.

>[!NOTE]
>
>La variable [# Lecture de requêtes](/help/accountiq/product-concepts.md##play-requests-def) n’est pas disponible dans la version MVPD de TV Everywhere.

**[!UICONTROL # Channels]**

Nombre total de canaux que le compte a visionnés au cours d’une période donnée.

>[!NOTE]
>
> Pour les services D2C **[!UICONTROL # Channels]** équivaut au nombre de **[!UICONTROL # Video categories]**.

>[!NOTE]
>
>Pour TV partout, ils incluent les chaînes qui peuvent ne pas appartenir au programmeur connecté. Ce numéro du compte inclut votre canal et d’autres canaux accessibles au cours de la période spécifiée.


**Modèle d’utilisation**

Les valeurs de ces colonnes servent d’identifiants correspondant à l’un des 14 modèles que nous utilisons pour classer tous les comptes d’utilisateurs.

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">Modèles d’utilisation</th>
      </tr>
      <tr>
        <td>1</td>
        <td>Utilisateur régulier</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Voyageur ou navetteur</td>
      </tr>
      <tr>
        <td>3</td>
        <td>Grande famille</td>
      </tr>
      <tr>
        <td>4</td>
        <td>Famille proche et amis</td>
      </tr>
      </tr>
         <td>5 et 8</td>
         <td>Partage de groupes sociaux</td>
      </tr>
      </tr>
         <td>6</td>
         <td>Grand groupe d'amis</td>
      </tr>
      </tr>
         <td>7</td>
         <td>Diffusion en continu simultanée</td>
      </tr>
      </tr>
         <td>9</td>
         <td>Partage de communautés</td>
      </tr>
      </tr>
         <td>10 et 11</td>
         <td>Comportement incertain</td>
      </tr>
      </tr>
         <td>12</td>
         <td>Petite famille</td>
      </tr>
      </tr>
         <td>13</td>
         <td>Second home </td>
      </tr>
      </tr>
         <td>14</td>
         <td>Utilisation anormale</td>
      </tr>
    </tbody>
  </table>

*Identifiants de modèle d’utilisation dans le mappage .csv exporté avec des modèles d’utilisation*

**Probabilité de partage**

La probabilité qu’un compte spécifique partage ses informations d’identification.

>[!NOTE]
>
> La moyenne de la probabilité de partage de tous les comptes dans le segment sélectionné est utilisée pour calculer la valeur [niveau de partage](/help/accountiq/data-panels.md#sharing-level) de [note de partage moyenne](/help/accountiq/data-panels.md#aggregated-sharing).
