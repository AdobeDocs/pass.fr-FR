---
title: Authentification à domicile (HBA)
description: Authentification à domicile (HBA)
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: ffedb5db269644c8d9c81480d27dff43bd4eb5d6
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 2%

---

# Authentification à domicile (HBA) {#home-based-authentication}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

L’authentification à domicile (HBA) est une fonctionnalité TV Everywhere qui permet aux abonnés d’une télévision payante d’accéder au contenu TV en ligne sans saisir les informations d’identification MVPD lorsqu’ils sont connectés à leur réseau domestique, ce qui améliore considérablement l’expérience d’authentification.

Selon l&#39;Open Authentication Technology Committee (OATC) :

> « L’authentification automatique à domicile est le processus par lequel un MVPD/OVD utilise des caractéristiques du réseau domestique (ou des identifiants automatiquement accessibles entre les appareils du réseau domestique) pour authentifier le compte d’abonné associé à ce réseau domestique, ce qui élimine la nécessité pour les utilisateurs de saisir manuellement les informations d’identification lors du lancement d’une session TVE pour accéder au contenu protégé. »

Pour plus d’informations sur l’authentification à domicile (HBA) et les normes du secteur en la matière, consultez les ressources suivantes :

>[!MORELIKETHIS]
>
> * [Accès instantané (HBA) par CTAM](http://www.ctamtve.com/instantaccess)
> * [Cas d’utilisation et exigences de l’authentification à domicile par OATC](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf)
> * [Infographie sur l’authentification à domicile par Adobe &#x200B;](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640)
> * [Authentification avec MVPD activés OAuth2](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
> * [Authentification avec MVPD SAML](/help/authentication/integration-guide-mvpds/authn-usecase.md)
> * [Métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

## Avantages des adaptateurs HBA {#hba-benefits}

L’authentification à domicile (HBA) est une fonctionnalité clé qui supprime l’obstacle de connexion pour les utilisateurs à domicile disposant d’un abonnement par câble actif. Cet obstacle représente un défi important pour les services TV Everywhere, près de la moitié de toutes les tentatives de connexion ayant abouti à un échec.

Le HBA peut améliorer considérablement l&#39;engagement des téléspectateurs, offrant une expérience utilisateur transparente et supérieure pour accéder au contenu TV Everywhere.

## Prise en charge des adaptateurs HBA {#hba-support}

>[!IMPORTANT]
>
> Si vous souhaitez tirer parti des fonctionnalités des adaptateurs HBA, contactez votre représentant commercial Adobe Pass Authentication pour obtenir plus d’informations, car certains flux d’adaptateurs HBA sont inclus dans le package Premium Workflow.

L&#39;adaptateur HBA est pris en charge par un certain nombre de MVPD qui sont intégrés à l&#39;authentification Adobe Pass, mais pour bénéficier de l&#39;adaptateur HBA, vous devrez peut-être prendre des mesures supplémentaires.

**MVPD SAML**

Pour les MVPD SAML, l’adaptateur HBA n’est activé que du côté MVPD.

**MVPD OAuth2**

Pour les MVPD OAuth2, l’adaptateur HBA peut être activé ou désactivé via le tableau de bord Adobe Pass TVE [&#128279;](https://experience.adobe.com/#/pass/authentication) en suivant les étapes du Guide de l’utilisateur [Intégrations du tableau de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

### MVPD {#hba-support-mvpds}

**MVPD SAML**

Le tableau suivant présente une vue d&#39;ensemble des MVPD SAML qui prennent en charge les adaptateurs HBA :

| MVPD | Fonctionnalités de base disponibles ? | Envoie l&#39;indicateur sur la réponse d&#39;authentification ? |
|--------------|--------------------------------|----------------------------------------|
| DirectTV | Oui | Non |
| Plat | Oui | Non |
| Spectre | Oui | Oui |
| Cox | Oui | Non |
| AT&amp;T | Oui | Non |
| Version | Oui | Oui |
| Télévision Par Câble | Oui | Non |
| Mediacom | Oui | Non |
| Midcontinent | Oui | Non |
| Masse | Oui | Non |
| AlticeOne | Oui | Oui |

**MVPD OAuth2**

Le tableau suivant présente une vue d&#39;ensemble des MVPD compatibles OAuth2 qui prennent en charge les adaptateurs HBA :

| MVPD | Fonctionnalités de base disponibles ? | Envoie l&#39;indicateur sur la réponse d&#39;authentification ? |
|---------|--------------------------------|----------------------------------------|
| Comcast | Oui | Oui |

### Authentification Adobe Pass {#hba-support-adobe-pass-authentication}

Cette section décrit l’expérience compatible avec les adaptateurs HBA et détaille la prise en charge fournie par l’authentification Adobe Pass, en mettant en évidence les fonctionnalités clés suivantes :

* **Identification de l’adaptateur HBA :** possibilité d’indiquer aux programmeurs si l’authentification était un adaptateur HBA ou non (prise en charge de MVPD requise).
* **TTL d’authentification configurables :** possibilité de définir des valeurs de durée de vie (TTL) d’authentification différentes pour les authentifications HBA et non HBA (prise en charge de MVPD requise).

Le tableau suivant donne un aperçu général de l’expérience utilisateur dans un flux d’authentification HBA et standard (hors HBA) :

| Type d’authentification | Expérience utilisateur |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HBA | Les utilisateurs sélectionnent leur MVPD et sont automatiquement authentifiés lors de la connexion à leur réseau domestique. Une fois l’authentification expirée, les utilisateurs doivent sélectionner à nouveau leur MVPD, après quoi ils seront automatiquement réauthentifiés. |
| Standard (hors HBA) | Les utilisateurs sélectionnent leur MVPD et sont invités à saisir leurs informations d’identification, quelle que soit leur connexion à un réseau domestique. Une fois l’authentification expirée, les utilisateurs doivent sélectionner à nouveau leur MVPD et saisir à nouveau leurs informations d’identification. |

**MVPD SAML**

Le tableau suivant présente une vue d&#39;ensemble de l&#39;implémentation de l&#39;adaptateur HBA dans le cas de MVPD SAML :

| Actions de l’utilisateur | Actions du système |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| L’utilisateur tente de lire une vidéo.<br/><br/>Le sélecteur MVPD s’affiche.<br/><br/>L’utilisateur sélectionne son MVPD et continue de se connecter. | Une vérification des antécédents est effectuée lorsque le MVPD applique ses règles d’identification des utilisateurs. Par exemple, cela peut impliquer de mapper l’adresse IP de l’utilisateur à l’adresse MAC des modems fournis par le distributeur ou des décodeurs connectés à large bande. |
| Un écran qui persiste pendant environ 3 secondes s’affiche.<br/><br/>Une page interstitielle peut informer l’utilisateur qu’il est automatiquement connecté à l’aide de son compte MVPD. | L’application ouvre l’URL d’authentification dans un agent utilisateur, lançant une requête HTTP vers le point d’entrée d’authentification Adobe Pass.<br/><br/>Le point d’entrée de l’authentification Adobe Pass transfère la requête au point d’entrée de l’authentification MVPD via une redirection de l’agent utilisateur.<br/><br/>Le MVPD doit envoyer une décision d’authentification sous la forme d’une réponse SAML qui inclut l’indicateur HBA (`hba_status`) avec une valeur définie sur « true » ou « false ».<br/><br/>Le serveur principal de l’authentification Adobe Pass effectue une requête au point d’entrée du profil utilisateur MVPD pour exposer l’indicateur `hba_status` dans le cadre des [métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md). |
| L’utilisateur est authentifié et peut désormais parcourir le contenu TV Everywhere autorisé. | L’application récupère le profil utilisateur et peut poursuivre le flux de décisions pour lire le contenu. |

**MVPD OAuth2**

Le tableau suivant présente une vue d&#39;ensemble de l&#39;implémentation de l&#39;adaptateur HBA dans le cas de MVPD activés OAuth2 :

| Actions de l’utilisateur | Actions du système |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| L’utilisateur tente de lire une vidéo.<br/><br/>Le sélecteur MVPD s’affiche.<br/><br/>L’utilisateur sélectionne son MVPD et continue de se connecter. | Une vérification des antécédents est effectuée lorsque le MVPD applique ses règles d’identification des utilisateurs. Par exemple, cela peut impliquer de mapper l’adresse IP de l’utilisateur à l’adresse MAC des modems fournis par le distributeur ou des décodeurs connectés à large bande. |
| Un écran qui persiste pendant environ 3 secondes s’affiche.<br/><br/>Une page interstitielle peut informer l’utilisateur qu’il est automatiquement connecté à l’aide de son compte MVPD. | L’application ouvre l’URL d’authentification dans un agent utilisateur, lançant une requête HTTP vers le point d’entrée d’authentification Adobe Pass.<br/><br/>Le point d’entrée de l’authentification Adobe Pass transfère la requête au point d’entrée de l’authentification MVPD via une redirection de l’agent utilisateur.<br/><br/>Le point d’entrée de l’authentification MVPD envoie un code d’autorisation au point d’entrée de l’authentification Adobe Pass.<br/><br/>L’authentification Adobe Pass utilise le code d’autorisation pour demander un jeton d’actualisation et un jeton d’accès au point d’entrée du jeton de MVPD.<br/><br/>Le MVPD doit envoyer une décision d&#39;authentification qui inclut l&#39;indicateur de l&#39;adaptateur HBA (`hba_status`) avec une valeur « true » ou « false » dans le cadre du `id_token`.<br/><br/>Le serveur principal de l’authentification Adobe Pass effectue une requête au point d’entrée du profil utilisateur MVPD pour exposer l’indicateur `hba_status` dans le cadre des [métadonnées utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).<br/><br/>Le MVPD définit la TTL du jeton d’actualisation sur une valeur approuvée par MVPD et l’Adobe définit la TTL d’authentification sur une valeur inférieure ou égale à la valeur du jeton d’actualisation. |
| L’utilisateur est authentifié et peut désormais parcourir le contenu TV Everywhere autorisé. | L’application récupère le profil utilisateur et peut poursuivre le flux de décisions pour lire le contenu. |

>[!IMPORTANT]
>
> Dans le flux de l’adaptateur HBA pour les MVPD utilisant le protocole d’authentification OAuth 2.0, le MVPD émet un jeton d’actualisation avec une durée de vie définie par ses besoins professionnels, tandis que Adobe émet un jeton d’authentification de l’adaptateur HBA avec une durée de vie qui ne doit pas dépasser la durée de vie du jeton d’actualisation.

## Questions fréquentes {#faqs}

1. Pourquoi y a-t-il une distinction entre l’implémentation de l’adaptateur HBA pour les protocoles SAML et OAuth2 ?

   La séparation entre l’authentification à domicile (HBA) pour les protocoles SAML et OAuth2 existe car ces protocoles fonctionnent différemment en termes de mécanismes d’authentification, de configuration et de flexibilité d’implémentation. Pour les MVPD SAML, aucune action n’est requise de la part du programmeur pour activer le HBA, tandis que pour les MVPD OAuth2, le HBA peut être activé ou désactivé via le tableau de bord [Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

1. Lorsque l&#39;adaptateur HBA est activé, les utilisateurs doivent-ils toujours saisir leur nom d&#39;utilisateur et leur mot de passe lors de leur authentification initiale ?

   Non, le nom d’utilisateur et le mot de passe ne sont pas requis.

1. Comment appliquer le contrôle parental ?

   L’authentification Adobe Pass peut désactiver l’adaptateur HBA pour les intégrations aux canaux nécessitant l’approbation du contrôle parental. En outre, nous travaillons avec OATC sur un document UX qui recommande comment configurer l&#39;expérience HBA avec le contrôle parental.

1. Les fournisseurs prenant en charge les adaptateurs HBA utilisent-ils des fenêtres de durée de vie plus courtes pour les adaptateurs HBA que pour une authentification standard ?

   Le paramètre TTL est configurable. Nous vous recommandons de définir une durée de vie plus courte pour les intégrations MVPD avec les adaptateurs HBA afin d’éviter toute mauvaise gestion.
