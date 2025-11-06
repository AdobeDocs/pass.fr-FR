---
title: Authentification utilisant le protocole OAuth 2.0
description: Authentification utilisant le protocole OAuth 2.0
exl-id: 0c1f04fe-51dc-4b4d-88e7-66e8f4609e02
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 0%

---

# Authentification utilisant le protocole OAuth 2.0

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

Bien que SAML reste le principal protocole utilisé pour l’authentification par les MVPD des États-Unis et les entreprises en général, il existe une nette tendance à passer à OAuth 2.0 comme protocole d’authentification principal. Le protocole OAuth 2.0 (https://tools.ietf.org/html/rfc6749) a été principalement développé pour les sites grand public et a été rapidement adopté par les géants d&#39;Internet comme Facebook, Google et Twitter.

OAuth 2.0 connaît un énorme succès, ce qui a incité les entreprises à mettre lentement à niveau leur infrastructure pour la prendre en charge.



## Avantages de la migration vers OAuth 2.0 {#adv-oauth2}

À un niveau élevé, le protocole OAuth 2.0 offre la même fonctionnalité que le protocole SAML, mais il existe quelques différences importantes.

L’un d’eux est le fait que le flux de jeton d’actualisation peut être utilisé comme un moyen d’actualiser l’authentification en arrière-plan. Cela permet à l’IdP (les MVPD dans ce cas) de garder le contrôle tout en offrant une bonne expérience utilisateur, car l’utilisateur n’est plus tenu de se connecter souvent en raison de problèmes de sécurité.

Le protocole offre également plus de flexibilité en termes de données exposées, car un fournisseur de services peut désormais utiliser les jetons pour accéder à d’autres API afin d’obtenir des informations supplémentaires. Il en résulte un protocole de conversation pour les cas d’utilisation TVE, mais il offre la flexibilité nécessaire pour les workflows complexes.





## Conditions requises pour le passage à OAuth 2.0 {#oauth-req}

Pour prendre en charge l’authentification avec OAuth 2.0, un MVPD doit remplir les conditions préalables suivantes :

Tout d’abord, le MVPD doit s’assurer qu’il prend en charge le flux [Octroi du code d’autorisation](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/authcode.html).

Après avoir confirmé qu’il prend en charge le flux , le MVPD doit nous fournir les informations suivantes :

* le point d’entrée d’authentification
   * le point d’entrée fournit le code d’autorisation qui sera utilisé ultérieurement en échange du jeton d’actualisation et d’accès
* le point d’entrée /token
   * ceci fournira le jeton d’actualisation et le jeton d’accès
   * le jeton d’actualisation doit être stable (il ne doit pas changer à chaque fois que nous demandons un nouveau jeton d’accès)
   * le MVPD doit autoriser plusieurs jetons d’accès actifs pour chaque jeton d’actualisation
   * ce point d’entrée échange également un jeton d’actualisation contre un jeton d’accès
* nous avons besoin d’un **point d’entrée pour le profil utilisateur**
   * ce point d’entrée fournit l’identifiant utilisateur, qui doit être unique pour un compte et ne doit contenir aucune information d’identification personnelle
* le point d’entrée **/logout** (facultatif)
   * L’authentification Adobe Pass redirige vers ce point d’entrée et fournit au MVPD un URI de redirection. Sur ce point d’entrée, le MVPD peut effacer les cookies de l’ordinateur client ou appliquer la logique de déconnexion souhaitée
* il est vivement recommandé de disposer d’une prise en charge pour les clients autorisés (applications clientes qui ne déclenchent pas de page d’autorisation utilisateur)
* nous aurons également besoin des éléments suivants :
   * **clientID** et **client secret** pour les configurations d’intégration
   * **durée de vie** (TTL) du jeton d’actualisation et du jeton d’accès
   * Nous pouvons fournir au MVPD un URI de rappel d’autorisation et de rappel de déconnexion. En outre, si nécessaire, nous pouvons fournir aux MVPD une liste d&#39;adresses IP à whitelister dans les paramètres de votre pare-feu.


## Flux d’authentification {#authn-flow}

Dans le flux d’authentification, l’authentification Adobe Pass communique avec le MVPD selon le protocole sélectionné dans la configuration. Le flux OAuth 2.0 est illustré dans l’image ci-dessous :



![Diagramme pour afficher le flux d’authentification dans l’authentification Adobe qui communique avec le MVPD selon le protocole sélectionné dans la configuration.](../assets/authn-flow.png)

**Figure 1 : flux d’authentification OAuth 2.0**



## Demande d&#39;authentification et réponse {#authn-req-response}

En résumé, le flux d’authentification pour les MVPD prenant en charge le protocole OAuth 2.0 suit les étapes suivantes :

1. L’utilisateur final accède au site du programmeur et choisit de se connecter avec ses informations d’identification MVPD
1. AccessEnabler installé côté programmeur avec envoie une requête d’authentification sous la forme d’une requête HTTP au point d’entrée d’authentification Adobe Pass, que le point d’entrée d’authentification Adobe Pass redirige vers le point d’entrée d’autorisation MVPD.
1. Le point d’entrée d’autorisation MVPD envoie un code d’autorisation au point d’entrée d’authentification Adobe Pass
1. L’authentification Adobe Pass utilise le code d’autorisation reçu pour demander un jeton d’actualisation et un jeton d’accès au point d’entrée du jeton MVPD
1. Un appel de récupération des informations et des métadonnées de l’utilisateur peut être envoyé au point d’entrée du profil utilisateur au cas où les informations de l’utilisateur ne seraient pas incluses dans le jeton
1. Le jeton d’authentification est transmis à l’utilisateur final qui peut désormais parcourir le site du programmeur

   >[!NOTE]
   >
   >Le jeton d’actualisation est utilisé pour obtenir un nouveau jeton d’accès une fois que le jeton d’accès actuel n’est plus valide ou expire.


>[!IMPORTANT]
>
>Le jeton d’actualisation ne doit pas changer lorsqu’il est échangé contre un jeton d’accès.

Cette limitation provient des flux client qui ne permettent pas au serveur de mettre à jour AuthNToken qui, pour le protocole OAuth 2.0, contient également le jeton d’actualisation.

Un flux d’autorisation type effectue un échange du jeton d’actualisation enregistré dans l’AuthNToken contre un jeton d’accès qui est ensuite utilisé pour effectuer l’appel d’autorisation au nom de l’utilisateur qui a été authentifié en premier lieu. Si le serveur d’autorisation (le MVPD) devait modifier le jeton d’actualisation et invalider l’ancien, nous ne pourrions pas mettre à jour le jeton AuthNToken valide. Pour cette raison, les MVPD doivent prendre en charge les jetons d’actualisation stables afin de pouvoir configurer les intégrations OAuth 2.0 pour eux.


## Migration de SAML vers OAuth 2.0 {#saml-auth2-migr}

La migration des intégrations de SAML vers OAuth 2.0 sera effectuée par Adobe et MVPD. Aucune modification technique n’est nécessaire du côté du programmeur, bien que celui-ci puisse souhaiter vérifier/tester le co-branding sur la page de connexion de MVPD. Du point de vue du MVPD, les points d’entrée et les autres informations requises dans les exigences Oauth 2.0 sont requis.

Afin de **préserver la connexion unique**, les utilisateurs qui disposent déjà d’un jeton d’authentification obtenu via SAML seront toujours considérés comme authentifiés et leurs requêtes seront acheminées via l’ancienne intégration SAML.

D’un point de vue technique :

1. Adobe activera une intégration OAuth 2.0 entre le programmeur et MVPD, SANS supprimer l’intégration SAML.
1. Après l’activation, tous les nouveaux utilisateurs utiliseront les flux OAuth 2.0.
1. Les utilisateurs déjà authentifiés, qui disposent déjà d’un jeton AuthN local contenant l’ID d’objet SAML, seront automatiquement acheminés par Adobe via l’intégration SAML.
1. Pour les utilisateurs de l’étape numéro 3, une fois que leur jeton AuthN généré par SAML expire, Adobe les traitera comme de nouveaux utilisateurs et se comportera comme les utilisateurs de l’étape numéro 2.
1. Adobe examinera les modèles d’utilisation afin de déterminer le moment où l’intégration SAML peut être désactivée en toute sécurité.
