---
title: Authentification basée sur la page d’accueil pour TV Everywhere
description: Authentification basée sur la page d’accueil pour TV Everywhere
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '1638'
ht-degree: 0%

---

# Authentification basée sur la page d’accueil pour TV Everywhere

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Qu’est-ce que l’authentification à domicile ? {#whatis-home-based-authn}

L’authentification basée sur la page d’accueil (HBA) est une fonctionnalité TV Everywhere qui permet aux abonnés de la télévision payante de regarder du contenu TV en ligne sans saisir d’informations d’identification MVPD lorsqu’ils sont à la maison, améliorant ainsi considérablement l’expérience utilisateur du flux d’authentification.

Définition de l’authentification à domicile par l’Open Authentication Technology Committee (OATC) : « L’authentification automatique à domicile est le processus par lequel un MVPD/OVD utilise les caractéristiques du réseau domestique (ou des identifiants automatiquement accessibles entre les appareils du réseau domestique) pour authentifier le compte d’abonné associé à ce réseau domestique, de sorte que les utilisateurs n’aient pas besoin de saisir manuellement les informations d’identification lors de l’établissement d’une session TVE pour accéder au contenu protégé par TVE. »



Pour plus d’informations sur les adaptateurs HBA et les normes du secteur, consultez la documentation [Cas d’utilisation et exigences OATC](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank} et les **Recommandations relatives à l’expérience utilisateur OATC pour les adaptateurs HBA**.

>[!NOTE]
>
>Certains flux HBA font partie du package Premium Workflow. Contactez votre représentant Adobe Pass si vous souhaitez utiliser cette fonctionnalité.

## Pourquoi le HBA est important pour vous {#why-hba}

L&#39;adaptateur HBA est important, car il supprime pratiquement la barrière de connexion pour vos téléspectateurs qui sont à la maison et qui disposent déjà d&#39;un abonnement par câble. En outre, l’authentification à domicile peut augmenter considérablement l’engagement de vos téléspectateurs et offrir une meilleure expérience utilisateur pour votre contenu TV Everywhere.

Actuellement, près de la moitié des tentatives de connexion échouent.

Une fois que l&#39;adaptateur HBA a été activé par l&#39;un des 5 principaux MVPD, son taux de conversion d&#39;authentification **augmenté de 40 %** (de 45 % à 63 %)

![](../../../assets/authn-conv-pre-post.png)

En outre, vous pouvez voir ci-dessous le taux de conversion de connexion pour un canal intégré à différents MVPD : ceux qui ont activé HBA pour lui et ceux qui n&#39;ont pas HBA. Le taux de conversion est nettement plus élevé pour les personnes équipées d&#39;un adaptateur HBA que pour celles qui n&#39;en sont pas équipées.

![](../../../assets/hba-vs-non-hba.png)

Six mois après l’activation de l’adaptateur HBA pour la plupart des canaux intégrés avec ce MVPD, nous avons constaté une augmentation de 82 % du nombre d’utilisateurs uniques (le nombre d’utilisateurs accédant aux canaux TV Everywhere via ce MVPD a presque doublé).

2w3En revanche, comme vous pouvez le voir dans le graphique ci-dessous, les autres MVPD qui n&#39;avaient pas activé HBA n&#39;ont connu qu&#39;une augmentation de 26 % des utilisateurs uniques au cours des 6 derniers mois.

![](../../../assets/unique-visitors-incr.png)

D&#39;après nos données, collectées 6 mois avant et 6 mois après l&#39;activation de l&#39;adaptateur HBA, nous avons constaté une augmentation importante de l&#39;engagement des téléspectateurs pour les canaux qui étaient activés pour l&#39;adaptateur HBA. En pratique, les utilisateurs des MVPD qui ont activé HBA ont tendance à regarder en moyenne 30 % plus de contenu que les utilisateurs des MVPD qui n&#39;ont pas activé HBA.

![](../../../assets/user-engagement-increase.png)

## Prise en charge de l’adaptateur HBA pour l’authentification Adobe Pass {#auth-hba-support}

Cette section décrit la prise en charge des adaptateurs HBA fournie par l&#39;authentification Adobe Pass, le comportement des plateformes d&#39;authentification Adobe Pass dans les flux d&#39;adaptateurs HBA et offre également des détails techniques utiles à la mise en œuvre des adaptateurs HBA.

Fonctionnalités d’authentification d’Adobe Pass prenant en charge HBA

* Possibilité de définir des durées de vie d’authentification différentes pour les authentifications HBA et non HBA (nécessite également la prise en charge de MVPD)
* Possibilité de sélectionner automatiquement un MVPD (ignorer le sélecteur MVPD) si l’authentification a expiré. Cela s’avère particulièrement utile lorsque les TTL des adaptateurs HBA sont faibles.
* Possibilité d&#39;exposer aux programmeurs si l&#39;authentification était HBA ou non (nécessite également la prise en charge de MVPD)

### Expérience utilisateur du HBA sur les plateformes d’authentification Adobe Pass {#hba-user-exp}

Les tableaux suivants fournissent des informations sur l&#39;expérience utilisateur pour les plateformes prises en charge lorsque l&#39;adaptateur HBA est activé et lorsqu&#39;il n&#39;est pas activé :

| Flux d’utilisateurs - Type de plateforme | swf, iOS, Android |
|---|---|
| Avec adaptateur HBA activé | Lorsque les utilisateurs sont à la maison, ils sont automatiquement authentifiés. Une fois le jeton AuthN du HBA expiré, les utilisateurs sont automatiquement réauthentifiés. |
| Sans adaptateur HBA | Les utilisateurs sont invités à sélectionner leur MVPD et à saisir leurs informations d’identification, même s’ils sont chez eux. Une fois le jeton d’authentification expiré, ils doivent saisir à nouveau leurs informations d’identification. |

| Flux d’utilisateurs - Type de plateforme | js, Windows (natif) |
|---|---|
| Avec adaptateur HBA activé | Lorsque les utilisateurs sont à la maison, ils sont automatiquement authentifiés. Après l’expiration du jeton AuthN de l’adaptateur HBA, les utilisateurs doivent sélectionner à nouveau leur MVPD dans le sélecteur et il sera automatiquement authentifié. |
| Sans adaptateur HBA | Les utilisateurs sont invités à sélectionner leur MVPD et à saisir leurs informations d’identification, même s’ils sont à la maison. Une fois le jeton AuthN expiré, les utilisateurs doivent saisir à nouveau leurs informations d’identification. |

| Flux d’utilisateurs - Type de plateforme | API REST sans client (deuxième authentification d’écran) |
|---|---|
| Avec adaptateur HBA activé | Lorsque les utilisateurs sont à la maison et qu’ils utilisent une application API REST sans client, ils sont automatiquement authentifiés sur le deuxième appareil d’écran après avoir saisi le code d’enregistrement et sélectionné leur MVPD. Après l&#39;expiration du jeton AuthN de l&#39;adaptateur HBA, les utilisateurs sont automatiquement réauthentifiés (sur le second périphérique d&#39;écran). |
| Sans adaptateur HBA | Les utilisateurs sont invités à sélectionner leur MVPD et à saisir leurs informations d’identification, même s’ils sont à la maison. Une fois le jeton AuthN expiré, les utilisateurs doivent saisir à nouveau leurs informations d’identification. |

### Détails techniques de la mise en œuvre du HBA {#tech-details-hba}

#### Protocole OAuth 2.0 {#oauth-2-protocol}

Dans le flux HBA pour les MVPD intégrés au protocole d’authentification OAuth 2.0, le MVPD émet un jeton d’actualisation et l’Adobe émet un jeton d’authentification HBA :

* Le jeton d’actualisation a une durée de vie déterminée par les besoins de l’entreprise du MVPD.
* Le TTL du jeton d&#39;authentification HBA **doit être inférieur ou égal à** le TTL du jeton d&#39;actualisation.


*Description du flux d’authentification de l’adaptateur HBA pour le protocole OAuth 2.0*


| Actions de l’utilisateur | Actions du système |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| L’utilisateur accède au site du programmeur. Lorsque vous tentez de lire une vidéo, le sélecteur MVPD s’affiche. L’utilisateur sélectionne son MVPD et clique sur se connecter. | Une vérification des antécédents est effectuée. Le MVPD applique son ensemble de règles pour la détection des utilisateurs (par exemple, mapper l’adresse IP de l’utilisateur avec l’adresse MAC des modems fournis par le distributeur ou des décodeurs connectés à large bande). |
| Un écran, qui persiste pendant environ 3 secondes, s’affiche. Une page interstitielle peut s’afficher pour informer l’utilisateur qu’il est automatiquement connecté à l’aide de son compte MVPD. | <ol><li>AccessEnabler, qui est installé du côté du programmeur, envoie une requête d’authentification (sous la forme d’une requête HTTP) au point d’entrée d’authentification Adobe Pass.</li><li>Le point d’entrée de l’authentification Adobe Pass redirige la requête vers le point d’entrée de l’authentification MVPD. <br />**Remarque :** la requête contient le paramètre `hba_flag` (tentative HBA = true) qui indique que le MVPD doit tenter l&#39;authentification HBA.</li><li>Le point d’entrée d’authentification MVPD envoie un code d’autorisation au point d’entrée d’authentification Adobe Pass.</li><li>L’authentification Adobe Pass utilise le code d’autorisation pour demander un jeton d’actualisation et un jeton d’accès au point d’entrée du jeton MVPD.</li><li>Le MVPD envoie une décision d’authentification et le paramètre `hba_status` (true/false) dans le `id_token`.</li><li>Un appel au point d’entrée du profil utilisateur MVPD est envoyé pour exposer la clé [hba_status dans les métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).</li><li>Le MVPD définit la TTL du jeton d’actualisation sur une valeur approuvée par MVPD et l’Adobe définit la TTL du jeton AuthN sur une valeur inférieure ou égale à la valeur du jeton d’actualisation.</li></ol> |
| L’utilisateur est authentifié et peut désormais parcourir le contenu TV Everywhere autorisé. | Le jeton d’authentification est transmis à l’utilisateur qui peut désormais naviguer correctement sur le site du programmeur. |

#### Protocole SAML {#saml-protocol}

Description du flux d&#39;authentification HBA pour le protocole d&#39;authentification SAML

| Actions de l’utilisateur | Actions du système |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| L’utilisateur accède au site du programmeur. Lorsque vous tentez de lire une vidéo, le sélecteur MVPD s’affiche. L’utilisateur sélectionne son MVPD et clique sur se connecter. | Une vérification des antécédents est effectuée. Le MVPD applique son ensemble de règles pour la détection des utilisateurs (par exemple, mapper l’adresse IP de l’utilisateur avec l’adresse MAC des modems fournis par le distributeur ou des décodeurs connectés à large bande). |
| Un écran, qui persiste pendant environ 3 secondes, s’affiche. Une page interstitielle peut s’afficher pour informer l’utilisateur qu’il est automatiquement connecté à l’aide de son compte MVPD. | <ol><li>AccessEnabler, qui est installé du côté du programmeur, envoie une requête d’authentification (sous la forme d’une requête HTTP) au point d’entrée d’authentification Adobe Pass.</li><li>Le point d’entrée de l’authentification Adobe Pass redirige la requête vers le point d’entrée de l’authentification MVPD.</li><li>Le MVPD doit envoyer une décision d&#39;authentification sous la forme d&#39;une réponse SAML contenant l&#39;indicateur HBA : hba_status (true/false).</li><li>Un appel au point d’entrée du profil utilisateur MVPD est envoyé pour exposer la clé [hba_status dans les métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).</li></ol> |
| L’utilisateur est authentifié et peut désormais parcourir le contenu TV Everywhere autorisé. | Le jeton d’authentification est transmis à l’utilisateur qui peut désormais naviguer correctement sur le site du programmeur. |


## Activation de l’adaptateur HBA {#how-to-activate-hba}

* Protocole **OAuth :**
   * Pour activer le HBA, consultez le [Guide d&#39;utilisation du tableau de bord Adobe Pass TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
* **Protocole SAML :** l’authentification basée sur l’accueil est activée côté MVPD. Aucune action n’est requise de la part du programmeur ou de l’Adobe.
Pour plus d&#39;informations sur les MVPD qui prennent en charge l&#39;authentification à domicile, voir [Statut du HBA pour les MVPD](/help/authentication/integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md).

## FAQ {#faqs}


**Question :** Pourquoi la séparation entre l’authentification basée sur l’accueil avec les protocoles SAML et OAuth2 ?

**Réponse :** le flux de l’adaptateur HBA est différent pour les deux protocoles. Du point de vue du programmeur, il n’est pas nécessaire d’agir pour s’assurer que l’adaptateur HBA est activé pour les MVPD SAML, tandis que pour les MVPD OAuth2, l’adaptateur HBA peut être activé ou désactivé dans le tableau de bord Adobe Pass TVE.



**Question :** Les utilisateurs doivent-ils indiquer un nom d&#39;utilisateur et un mot de passe la première fois qu&#39;ils s&#39;authentifient lorsque l&#39;adaptateur HBA est activé ?

**Réponse :** non, le nom d’utilisateur et le mot de passe ne sont pas requis.



**Question :** comment appliquer le contrôle parental ?

**Réponse 1 :** Adobe peut désactiver l’adaptateur HBA pour les intégrations aux canaux nécessitant l’approbation du contrôle parental.

**Réponse 2 :** Adobe travaille avec OATC sur un document UX qui recommande comment configurer l’expérience de l’adaptateur HBA avec le contrôle parental.



**Question :** Les fournisseurs qui prennent en charge les adaptateurs HBA ont-ils des fenêtres de durée de vie plus courtes pour les adaptateurs HBA qu&#39;ils ne le font pour l&#39;authentification normale ?

**Réponse :** paramètre de durée de vie est configurable. Nous vous recommandons de définir une durée de vie plus courte pour les jetons d’authentification HBA afin d’éviter toute mauvaise gestion.


## Informations utiles {#useful-info}

* [Recommendations d’accès instantané (HBA)](http://www.ctamtve.com/instantaccess){target=_blank} par CTAM
* [Exemple d’implémentation d’un adaptateur HBA sur l’application du programmeur](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/HBA_Flow_Sample.pdf?dc=201604222139-1346){target=_blank} par Adobe
  <!--* [Home Based Authentication User Experience Guidelines for TV Everywhere](http://oatc.us/Standards/DownloadRecommendedPractices.aspx){target=_blank} - by OATC-->
* [Cas d’utilisation et exigences de l’authentification à domicile](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank} par OATC
* [Infographie sur l’authentification à domicile](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640){target=_blank} par Adobe
* [Authentification utilisant le protocole OAuth 2.0](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
* [Authentification avec les MVPD SAML](/help/authentication/integration-guide-mvpds/authn-usecase.md)
* [Guide de l’utilisateur du tableau de bord Adobe Pass TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
* [hba_status Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)
