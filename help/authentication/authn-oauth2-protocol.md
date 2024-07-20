---
title: Authentification à l’aide du protocole OAuth 2.0
description: Authentification à l’aide du protocole OAuth 2.0
exl-id: 0c1f04fe-51dc-4b4d-88e7-66e8f4609e02
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 0%

---

# Authentification à l’aide du protocole OAuth 2.0

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

Bien que SAML reste le protocole principal utilisé pour l’authentification par les distributeurs multicanaux de programmes audiovisuels américains et par les entreprises en général, il existe une tendance claire à passer à OAuth 2.0 comme protocole d’authentification principal. Le protocole OAuth 2.0 (https://tools.ietf.org/html/rfc6749) a été principalement développé pour les sites de consommation et a été rapidement adopté par des géants Internet comme Facebook, Google &amp; Twitter.

OAuth 2.0 est un énorme succès, ce qui a conduit les entreprises à mettre à niveau lentement leurs infrastructures pour les soutenir.



## Avantages de la transition vers OAuth 2.0 {#adv-oauth2}

À un niveau élevé, le protocole OAuth 2.0 offre les mêmes fonctionnalités que le protocole SAML, mais il existe quelques différences importantes.

L’une d’elles est que le flux de jeton d’actualisation peut être utilisé comme un moyen d’actualiser l’authentification en arrière-plan. Cela permet au IdP (les MVPD dans ce cas) de maintenir le contrôle tout en permettant une bonne expérience utilisateur, car l’utilisateur n’est plus tenu de se connecter souvent en raison de problèmes de sécurité.

Le protocole offre également plus de flexibilité en termes de données exposées en tant que fournisseur de services peut désormais utiliser les jetons pour accéder à d’autres API afin d’obtenir des informations supplémentaires. Cela se traduit par un protocole plus &quot;bavard&quot; pour les cas d’utilisation de TVE, mais permet la flexibilité nécessaire pour les workflows complexes.





## Conditions requises pour passer à OAuth 2.0 {#oauth-req}

Pour prendre en charge l’authentification avec OAuth 2.0, un MVPD doit respecter les conditions préalables suivantes :

Tout d’abord, le MVPD doit s’assurer qu’il prend en charge le flux [Authorization Code Grant](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/authcode.html) .

Après avoir confirmé qu’il prend en charge le flux, le MVPD doit nous fournir les informations suivantes :

* le point de terminaison de l’authentification
   * le point de fin fournit le code d’autorisation qui sera ensuite utilisé en exchange pour l’actualisation et le jeton d’accès.
* le point de terminaison /token
   * cela fournira le jeton d’actualisation et le jeton d’accès.
   * le jeton d’actualisation doit être stable (il ne doit pas changer chaque fois que nous demandons un nouveau jeton d’accès).
   * le MVPD doit autoriser plusieurs jetons d’accès actifs pour chaque jeton d’actualisation.
   * ce point de fin exchange également un jeton d’actualisation pour un jeton d’accès
* nous avons besoin d’un point de terminaison **pour user-profile**
   * ce point de fin fournit l’ID utilisateur, qui doit être unique pour un compte et ne doit pas contenir d’informations d’identification personnelle.
* le point de terminaison **/logout** (facultatif)
   * L’authentification Adobe Pass redirige vers ce point de terminaison, fournit au MVPD un URI de redirection. Sur ce point de terminaison, le MVPD peut effacer les cookies sur l’ordinateur client ou appliquer la logique souhaitée pour se déconnecter.
* il est vivement recommandé d’avoir une prise en charge des clients autorisés (applications clientes qui ne déclenchent pas de page d’autorisation de l’utilisateur).
* nous aurons également besoin des éléments suivants :
   * **clientID** et **client secret** pour les configurations d’intégration
   * Valeurs **time to live** (TTL) pour le jeton d’actualisation et le jeton d’accès
   * Nous pouvons fournir au MVPD un URI de rappel d’autorisation et de rappel de déconnexion. En outre, si nécessaire, nous pouvons fournir aux distributeurs multicanaux une liste d’adresses IP à whitelister dans les paramètres de votre pare-feu.


## Flux d’authentification {#authn-flow}

Dans le flux d’authentification, l’authentification Adobe Pass communiquera avec le MVPD sur le protocole sélectionné dans la configuration. Le flux OAuth 2.0 est représenté sur la photo ci-dessous :



![Diagramme pour afficher le flux d&#39;authentification dans l&#39;authentification Adobe qui communique avec le MVPD sur le protocole sélectionné dans la configuration.](assets/authn-flow.png)

**Figure 1 : Flux d’authentification OAuth 2.0**



## Requête d’authentification et réponse {#authn-req-response}

En résumé, le flux d’authentification pour les MVPD prenant en charge le protocole OAuth 2.0 suit les étapes suivantes :

1. L’utilisateur final accède au site du programmeur et choisit de se connecter à l’aide de ses informations d’identification MVPD.
1. AccessEnabler installé côté programmeur avec envoi d’une demande d’authentification sous la forme d’une requête HTTP au point de terminaison d’authentification Adobe Pass, que le point de terminaison d’authentification Adobe Pass redirige vers le point de terminaison d’autorisation MVPD.
1. Le point d’entrée d’autorisation MVPD envoie un code d’autorisation au point d’entrée d’authentification Adobe Pass.
1. L’authentification Adobe Pass utilise le code d’autorisation reçu pour demander un jeton d’actualisation et un jeton d’accès à partir du point de terminaison de jeton du MVPD.
1. Un appel pour récupérer les informations et métadonnées utilisateur peut être envoyé au point de terminaison du profil utilisateur au cas où les informations utilisateur ne seraient pas incluses dans le jeton.
1. Le jeton d’authentification est transmis à l’utilisateur final qui peut désormais parcourir le site du programmeur.

   >[!NOTE]
   >
   >Le jeton d’actualisation est utilisé pour obtenir un nouveau jeton d’accès une fois que le jeton d’accès actuel n’est plus valide ou expire.


>[!IMPORTANT]
>
>Le jeton d’actualisation ne doit pas changer lorsqu’il est échangé contre un jeton d’accès.

Cette limitation provient des flux de clients qui ne permettent pas au serveur de mettre à jour AuthNToken qui, pour le protocole OAuth 2.0, contient également le jeton d’actualisation.

Un flux d’autorisation type effectue un exchange du jeton d’actualisation enregistré dans AuthNToken pour un jeton d’accès qui est ensuite utilisé pour effectuer l’appel d’autorisation au nom de l’utilisateur authentifié en premier lieu. Si le serveur d’autorisation (le MVPD) devait modifier le jeton d’actualisation et invalider l’ancien, nous ne pourrons pas mettre à jour le jeton AuthNToken valide. Pour cette raison, les MVPD doivent prendre en charge les jetons d’actualisation stables afin de pouvoir configurer les intégrations OAuth 2.0 pour eux.


## Migration de SAML vers OAuth 2.0 {#saml-auth2-migr}

La migration des intégrations de SAML vers OAuth 2.0 sera effectuée par Adobe et le MVPD. Aucun changement technique n’est nécessaire du côté programmeur, bien que le programmeur puisse vouloir vérifier/tester l’alliance de marques sur la page de connexion MVPD. Du point de vue du MVPD, les points de terminaison et d’autres informations demandés dans les exigences de Oauth 2.0 sont requis.

Pour **préserver SSO**, les utilisateurs qui disposent déjà d’un jeton d’authentification obtenu via SAML seront toujours considérés comme authentifiés et leurs demandes seront acheminées via l’ancienne intégration SAML.

D’un point de vue technique :

1. Adobe activera une intégration OAuth 2.0 entre le programmeur et le MVPD, SANS supprimer l’intégration SAML.
1. Après l’activation, tous les nouveaux utilisateurs utiliseront les flux OAuth 2.0.
1. Les utilisateurs déjà authentifiés, qui disposent déjà d’un jeton AuthN local contenant l’objet-id SAML, seront automatiquement acheminés par Adobe via l’intégration SAML.
1. Pour les utilisateurs de l’étape 3, une fois que leur jeton AuthN généré par SAML expire, l’Adobe les traite comme de nouveaux utilisateurs et se comporte comme les utilisateurs de l’étape 2.
1. Adobe examine les schémas d’utilisation afin de déterminer le moment où l’intégration SAML peut être désactivée en toute sécurité.
