---
title: Affichage des rapports en mode d’isolation
description: Affichez les rapports en mode d’isolation pour Xfinity.
exl-id: e7cf24c5-9bfa-48f6-b5c8-20443a976891
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Affichage des rapports de partage en mode d’isolation {#report-isolation-mode}

En mode Isolation, les distributeurs multicanaux de programmes audiovisuels (Xfinity, par exemple) identifient de manière cohérente les abonnés sur tous les appareils, mais identifient leurs abonnés différemment, en fonction des programmeurs avec lesquels ils interagissent. En revanche, en mode standard, les distributeurs multicanaux de programmes identifient systématiquement les abonnés sur tous les appareils, indépendamment des programmeurs.

Par exemple, dans l’image suivante, si un abonné B d’un MVPD de mode d’isolation (tel que Xfinity) accède au contenu proposé par deux programmeurs différents utilisant le même appareil, le MVPD associera différents identifiants aux deux tentatives d’accès différentes. Ainsi, pour ces programmeurs (L et M dans la figure) et pour Account IQ, il semble qu&#39;il y ait deux abonnés différents qui accèdent au contenu. Toutefois, pour le MVPD standard, si l’abonné B accède au contenu proposé par deux programmeurs différents, le MVPD associe un identifiant d’accès unique pour les deux tentatives d’accès. Les MVPD (tels que Xfinity) en mode Isolation n’identifient pas de manière cohérente un abonné, même si l’abonné utilise le même appareil sur différents programmeurs.

![](assets/isolation-diff-new.png)

*Figure : Le mode Isolation MVPD identifie quatre abonnés différents au lieu de deux.*

Pour gérer la distorsion des données (en raison de l’identification du même abonné que différent selon l’accès à différents programmeurs), le mode Isolation limite l’activité signalée sur un programmeur à l’activité uniquement sur les applications de ce programmeur. Par exemple, pour le mode Isolation dans l’image ci-dessus, le programmeur L voit les données basées uniquement sur l’activité des identités W et Y, ignorant les identités X et Z.

>[!IMPORTANT]
>
> L&#39;inconvénient est que le programmeur L est privé de partager les informations rassemblées sur les Abonnés A et B en raison d&#39;une activité avec un programmeur autre que L.

En mode Isolation, tous les calculs effectués pour obtenir les scores de partage et toutes les mesures associées sont effectués à l’aide de l’activité des périphériques qui diffusent à partir des applications appartenant au programmeur et aux canaux sélectionnés.
Les scores de partage et les probabilités sont calculés uniquement à l’aide du flux qui commence à partir des canaux actuellement sélectionnés.

Pour afficher les mesures en mode d’isolation :

1. Sélectionner **[!UICONTROL isolation mode]** de la **[!UICONTROL MVPDs in segment]** , puis sélectionnez **[!UICONTROL Apply Selection]**.

   ![](assets/xfinity-in-segment.gif)

   *Figure : Sélection du MVPD en mode Isolation*

1. Sélectionnez les canaux de votre choix dans la **[!UICONTROL Channels in segment]** , puis sélectionnez **[!UICONTROL Apply Selection]**.

   Sélectionnez également un [période](/help/accountiq/product-concepts.md#granularity-def).

   >[!IMPORTANT]
   >
   >Comme le partage de compte est plus pertinent lorsqu’il est mesuré pour la diffusion en continu dans toutes les applications des programmeurs, vous constaterez des scores de partage plus faibles et une certaine variation dans les mesures en mode Isolation.

   ![](assets/aggregate-sharing-isolation.png)

   *Figure : Jauges de probabilité du partage en mode Isolation*

   Notez que les jauges ci-dessus montrent que seulement 6 % de tous les comptes sont partagés ; et que seulement 8 % du contenu est consommé par ces 8 %. Ainsi, les canaux peuvent comparer leurs scores en mode Isolation avec ceux des autres MVPD. Par conséquent, les informations obtenues en utilisant le mode Isolation doivent être interprétées différemment des autres données.
