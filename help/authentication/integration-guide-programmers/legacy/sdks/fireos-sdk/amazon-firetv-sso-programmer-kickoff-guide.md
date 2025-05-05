---
title: Amazon fireTV SSO - Guide de lancement du programmeur
description: Amazon fireTV SSO - Guide de lancement du programmeur
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 0%

---

# (Hérité) Amazon fireTV SSO - Guide de lancement du programmeur {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>

## Introduction {#intro}

Ce document décrit les informations nécessaires pour intégrer le nouveau SDK fireTV d’Adobe Pass Authentication **&#x200B;**&#x200B;dans votre application fireTV. Ce nouveau SDK tire parti de l’intégration au niveau du système d’exploitation sur la plateforme fireTV d’Amazon, offrant ainsi la prise en charge de l’authentification unique **Single Sign On**. Pour bénéficier de l’authentification SSO, vous devez fournir quelques efforts afin de migrer votre application de l’API sans client vers le nouveau SDK fireTV. Certaines modifications apportées aux flux d’authentification sont détaillées ci-dessous.

## Architecture de haut niveau et intégration au niveau du système d’exploitation {#high}

Afin d&#39;obtenir l&#39;authentification unique entre les applications TV Everywhere sur la plateforme Amazon fireTV et d&#39;améliorer l&#39;expérience globale sur cette plateforme, nous avons décidé d&#39;intégrer notre SDK principal au niveau du système d&#39;exploitation fireTV. Les programmeurs devront compiler par rapport à une bibliothèque de stub fournie par Adobe. Les fonctionnalités réelles seront fournies par la bibliothèque d’Adobe présente dans Amazon fireTV OS.

Jusqu’à ce qu’Amazon fournisse un simulateur fireTV qui intègre notre bibliothèque au niveau du système d’exploitation, le développement ne serait possible qu’en utilisant des appareils fireTV réels.

## Avantages {#bene}

* Connexion unique entre toutes les applications TV Everywhere alimentées par l&#39;Adobe sur la plateforme Amazon fireTV avec tous les MVPD intégrés.
* Possibilité de bénéficier d&#39;un adaptateur HBA (avec prise en charge des MVPD).
* Possibilité d&#39;utiliser le dernier SDK fireTV sans avoir à mettre à jour vos applications chaque fois qu&#39;une nouvelle version de SDK est publiée.
* Toutes les applications TVE bénéficient de l&#39;utilisation de la bibliothèque système partagée en supprimant la nécessité d&#39;avoir une copie locale de la bibliothèque AccessEnabler. Cela garantit également que toutes les applications utilisent la même version de SDK.
* Authentification sur un seul écran : pas besoin de code d’enregistrement et de workflows du deuxième écran.

## Migration d’une application basée sur l’API sans client vers une application basée sur FireTV SDK {#migra1}

Pour migrer de l’API sans client vers le SDK fireTV, vous devez supprimer la base de code liée à l’API sans client et intégrer le nouveau SDK fireTV.

Par rapport à l’application basée sur l’API sans client, avec le nouveau SDK fireTV, l’authentification passe au premier écran. Il n’est plus nécessaire d’effectuer une deuxième authentification à l’écran.

Pour ce faire, les programmeurs doivent ajouter un sélecteur MVPD dans leurs applications afin que les utilisateurs puissent sélectionner leur fournisseur de télévision directement sur l’appareil FireTV. Lorsque MVPD est sélectionné, la page de connexion MVPD s’affiche sur l’appareil fireTV.

Les maquettes des flux utilisateur qui décrivent les scénarios standard, HBA et SSO sur fireTV sont disponibles à l&#39;adresse [Amazon Fire TV - Flux utilisateur de connexion MVPD](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Migration d’une application Android SDK vers une application FireTV basée sur SDK {#migra2}

Ce nouveau SDK fireTV est très similaire à notre SDK Android existant et la documentation dont nous disposons actuellement pour **intégrer notre SDK Android** <!--http://tve.helpdocsonline.com/android-technical-overview-->peut être utilisée jusqu’à ce que les documents FireTV SDK soient prêts. Si vous disposez déjà d’applications Android qui utilisent notre SDK Android, l’intégration de fireTV SDK dans votre application fireTV devrait être simple.

Par rapport au SDK Android existant, sur FireTV SDK, le processus d’authentification sera plus simple à développer, car les tâches de gestion/présentation de la page de connexion MVPD et de récupération du jeton AuthN seront effectuées en interne par la bibliothèque AccessEnabler.

## Questions fréquentes {#faq}

1. Comment fonctionnera le **SSO** ?

   * La connexion unique fonctionne sur toutes les applications de programmation optimisées par l’authentification Adobe Pass qui utilisent le nouveau SDK fireTV sur le même appareil Amazon fireTV
   * La connexion unique entre les applications Programmer implémentées sur l’API REST sans client et les applications implémentées sur fireTV SDK **ne sera PAS prise en charge**

1. Quelle est la couverture MVPD de fireTV SSO ?

   * **Toutes les MVPD** intégrées par l’authentification Adobe Pass seront techniquement prises en charge par SSO sur fireTV SDK.

1. Outre l’utilisation du nouveau SDK, de quels autres **changements de workflow** les programmeurs doivent-ils avoir connaissance ?

   * Les programmeurs doivent mettre en œuvre un sélecteur MVPD pour la plateforme fireTV.

1. Les TTL d’authentification **TTL** seront-elles modifiées ?

   * Le comportement des TTL d’authentification reste le même.
   * Le premier jeton d’authentification valide sera utilisé pour effectuer la connexion unique. Dans ce cas, toutes les autres applications qui seront authentifiées par le biais de la connexion unique utiliseront le même TTL jusqu’à son expiration. Ainsi, lorsque vous naviguez d’une application à une autre, la deuxième application partage la durée de vie de la première application qui s’authentifie.

1. Comment fonctionne l’**API de dégradation** ?

   * Aucune modification n’est nécessaire pour l’API de dégradation. L’expérience utilisateur sera la même que sur les appareils Android.

1. Comment les flux **TempPass** sont-ils affectés ?

   * Les flux TempPass sont à écran unique et se comportent comme sur tout autre appareil natif.

1. D’autres fonctionnalités d’Adobe fonctionneront-elles comme auparavant ?

   * Toutes les fonctionnalités d’authentification d’Adobe Pass fonctionnent sur fireTV comme sur les appareils Android.
