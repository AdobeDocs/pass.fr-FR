---
title: Guide pas à pas de l'API REST (client à serveur)
description: Client du guide pas à pas de l’API REST sur le serveur.
exl-id: f54a1eda-47d5-4f02-b343-8cdbc99a73c0
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 0%

---

# Guide pas à pas de l&#39;API REST (client à serveur) {#rest-api-cookbook-client-to-server}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


## Vue d’ensemble {#overview}

Ce document fournit des instructions détaillées à l’équipe d’ingénierie d’un programmeur afin qu’elle intègre un &quot;appareil intelligent&quot; (console de jeu, application de télévision dynamique, décodeur, etc.). avec l’authentification Adobe Pass à l’aide des services d’API REST. Cette approche client-serveur, qui utilise des API REST plutôt qu’un SDK client, permet une prise en charge plus large de différentes plateformes pour lesquelles le développement d’un nombre important de SDK uniques ne serait pas possible. Pour une présentation technique générale du fonctionnement de la solution sans client, consultez la [présentation technique sans client](/help/authentication/rest-api-overview.md).


Cette approche nécessite deux composants (application en continu et application AuthN) pour effectuer les flux requis : démarrage, enregistrement, autorisation et flux d’affichage multimédia dans l’application en continu, ainsi que le flux d’authentification dans votre application AuthN.

### Mécanisme de ralentissement

L’API REST d’authentification Adobe Pass est régie par un [mécanisme de limitation](/help/authentication/throttling-mechanism.md).

## Composants {#components}

Dans une solution client à serveur opérationnelle, les composants suivants sont impliqués :



| Type | Composant | Description |
| --- | --- | --- |
| Appareil de diffusion en continu | Application de diffusion en continu | L’application de programmation qui réside sur l’appareil de diffusion en continu de l’utilisateur et lit la vidéo authentifiée. |
| | \[Facultatif\] Module AuthN | Si le périphérique de diffusion dispose d’un agent utilisateur (c’est-à-dire un navigateur web), le module AuthN est responsable de l’authentification de l’utilisateur sur le MVPD IdP. |
| \[Facultatif\] Périphérique AuthN | Application AuthN | Si le périphérique de diffusion en continu ne dispose pas d’un agent utilisateur (c’est-à-dire un navigateur web), l’application AuthN est une application web de programmeur accessible à partir d’un périphérique d’un utilisateur distinct à l’aide d’un navigateur web. |
| Infrastructure d’Adobe | Service Adobe Pass | Un service qui s’intègre au service MVPD IdP et AuthZ et qui fournit des décisions d’authentification et d’autorisation. |
| Infrastructure MVPD | MVPD IdP | Un point de terminaison MVPD qui fournit un service d’authentification basé sur les informations d’identification pour valider l’identité de l’utilisateur. |
| | Service MVPD AuthZ | Un point de terminaison MVPD qui fournit des décisions d’autorisation basées sur les abonnements de l’utilisateur, les contrôles parental, etc. |



Les termes supplémentaires utilisés dans le flux sont définis dans le [Glossaire](/help/authentication/glossary.md).

## Flux{#flows}

### Enregistrement du client dynamique (DCR)

Adobe Pass utilise DCR pour sécuriser les communications client entre une application ou un serveur de programmation et les services Adobe Pass. Le flux DCR est distinct, dépendant et prérequis. Vous pouvez le trouver dans [Enregistrement du client dynamique](/help/authentication/dynamic-client-registration.md)


### Flux d’applications par flux (Smart Device)

![](assets/smart-device-app-flow.png)

#### Flux de démarrage

1. Votre application démarre et charge son interface utilisateur initiale.

2. Obtenez/générez un identifiant d’appareil.

3. Envoyez un appel de vérification d’authentification pour voir si l’appareil est déjà authentifié.  Par exemple : [`<SP_FQDN>/api/v1/checkauthn [device ID]`](/help/authentication/check-authentication-token.md)

4. Si l’appel `checkauthn` réussit, passez au flux d’autorisation à partir de l’étape 2.  En cas d’échec, démarrez le flux d’enregistrement.



#### Flux d’enregistrement

1. Obtenez un code d’enregistrement et une URL à utiliser par votre utilisateur pour accéder à votre application de connexion au 2e écran, puis présentez-les à l’utilisateur :

   a. Envoyez une demande de POST au service de code d’enregistrement Adobe, en transmettant un identifiant d’appareil haché et une &quot;URL d’enregistrement&quot;.  Par exemple : [`<REGGIE_FQDN>/reggie/v1/[requestorId]/regcode [device ID]`](/help/authentication/registration-code-request.md)

   b. Présenter le code d’enregistrement et l’URL renvoyés à l’utilisateur.

   c. Demandez à l’utilisateur de passer à un appareil compatible avec le web, d’accéder à l’URL, puis de saisir le code d’enregistrement.



#### Flux d’autorisation

1. L’utilisateur revient de l’application du deuxième écran et appuie sur le bouton &quot;Continuer&quot; sur votre appareil. Vous pouvez également mettre en oeuvre un mécanisme d’interrogation pour vérifier l’état d’authentification, mais l’authentification Adobe Pass recommande la méthode du bouton Continuer sur l’interrogation. <!--(For information on employing a "Continue" button versus polling the Adobe Pass Authentication backend server, see the Clientless Technical Overview: Managing 2nd-Screen Workflow Transition.)--> Par exemple : [\&lt;SP\_FQDN\>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md)

2. Envoyez une demande de GET au service d’autorisation d’authentification Adobe Pass pour lancer l’autorisation. Par exemple : `<SP_FQDN>/api/v1/authorize [device ID, Requestor ID, Resource ID]`

<!-- end list -->

* Si la réponse indique la réussite : l’utilisateur dispose d’un jeton AuthN valide ET l’utilisateur est autorisé à regarder le média demandé (il existe un jeton AuthZ valide pour cet utilisateur).

* Si la réponse indique un échec : examinez l’exception générée pour déterminer son type (AuthN, AuthZ, etc.) :

   * S’il s’agissait d’une erreur AuthN, redémarrez le flux d’enregistrement.

   * S’il s’agissait d’une erreur AuthZ, l’utilisateur n’est pas autorisé à regarder le média demandé et un message d’erreur doit s’afficher à l’utilisateur.

   * En cas d’erreur (erreur de connexion, erreur réseau, etc.) puis afficher un message d’erreur approprié à l’utilisateur.



#### Afficher le flux du média

1. Présenter les choix de médias. L’utilisateur sélectionne le média à afficher.

2. Les médias sont-ils protégés ?

   a. Votre application vérifie si le média est protégé.

   b. Si le média est protégé, votre application démarre l’autorisation
(AuthZ) Flux ci-dessus.

   c. Si le média n’est pas protégé, lisez-le pour la variable
utilisateur.

3. Lecture du média.


### Flux d’application AuthN (2e écran)

![](assets/secnd-screen-authn-flow.png)

1. Obtenez une liste des MVPD pour cet utilisateur. Par exemple : [`<SP_FQDN>/api/v1/config/[requestorID]`](/help/authentication/provide-mvpd-list.md)

1. Lancez le flux d’authentification.  Par exemple : [`<SP_FQDN>/api/v1/authenticate [requestorID, MVPD ID, Redirect URL, Domain name, Registration Code, "noflash=true"]`](/help/authentication/initiate-authentication.md)

1. Vérifiez si l’authentification a réussi. Par exemple :[`<SP_FQDN>/api/v1/checkauthn/[registration code][requestor ID]`](/help/authentication/check-authentication-token.md)

1. Renvoyez l’utilisateur à votre application Smart Device pour terminer le flux d’autorisation.

## SSO de la plateforme {#platform-sso}

Certaines plateformes prennent en charge la connexion unique (SSO). Les détails de mise en oeuvre se trouvent pour chaque plateforme respective :

* [APPLE SSO](/help/authentication/apple-sso-cookbook-rest-api.md)
* AMAZON SSO

## TempPass et TempPass Promotionnel pour l’API REST {#temppass}

Pour les implémentations TempPass et TempPass de promotion dans le cas où l’utilisateur n’est pas tenu de saisir des informations d’identification, l’authentification peut être mise en oeuvre directement dans l’application de diffusion en continu.

**Pour utiliser cette API, l’application de diffusion en continu doit s’assurer que l’identifiant unique de l’appareil est utilisé pour identifier le jeton, ainsi que les données supplémentaires facultatives.**


![](assets/temp-pass-promo-temppass.png)
