---
title: Manuel de l’API REST (client à serveur)
description: Client de guide pas à pas de l’API REST au serveur.
exl-id: f54a1eda-47d5-4f02-b343-8cdbc99a73c0
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 0%

---

# Manuel de l’API REST (hérité) (client à serveur) {#rest-api-cookbook-client-to-server}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Vue d’ensemble {#overview}

Ce document fournit des instructions détaillées à l’équipe d’ingénierie d’un programmeur pour intégrer un « appareil intelligent » (console de jeu, application de télévision intelligente, décodeur, etc.) à l’authentification Adobe Pass à l’aide des services d’API REST. Cette approche client à serveur, qui utilise des API REST plutôt qu’un SDK client, permet une prise en charge plus large de différentes plateformes pour lesquelles le développement d’un nombre important de SDK uniques ne serait pas possible. Pour une présentation technique générale du fonctionnement de la solution découplée, reportez-vous à la [Présentation technique du découplage](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md).


Cette approche nécessite deux composants (application de streaming et application AuthN) pour terminer les flux requis : les flux de démarrage, d’enregistrement, d’autorisation et de média d’affichage dans l’application de streaming, ainsi que le flux d’authentification dans votre application AuthN.

### Mécanisme de limitation

L’API REST d’authentification Adobe Pass est régie par un [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Composants {#components}

Dans une solution client à serveur opérationnelle, les composants suivants sont impliqués :



| Type | Composant | Description |
| --- | --- | --- |
| Appareil De Diffusion En Continu | Application de streaming | L’application Programmeur qui réside sur l’appareil de diffusion en continu de l’utilisateur et lit une vidéo authentifiée. |
| | \[Facultatif\] Module AuthN | Si l’appareil de diffusion en continu dispose d’un agent utilisateur (c’est-à-dire un navigateur web), le module AuthN est chargé d’authentifier l’utilisateur sur l’IdP MVPD. |
| \[Facultatif\] Périphérique AuthN | Application AuthN | Si l’appareil de diffusion en continu ne dispose pas d’un agent utilisateur (c’est-à-dire un navigateur web), l’application AuthN est une application web de programmation accessible à partir de l’appareil d’un utilisateur distinct à l’aide d’un navigateur web. |
| Infrastructure d&#39;Adobe | Service Adobe Pass | Un service qui s’intègre aux services MVPD IdP et AuthZ et fournit des décisions d’authentification et d’autorisation. |
| Infrastructure MVPD | IdP MVPD | Point d’entrée MVPD qui fournit un service d’authentification basé sur les informations d’identification pour valider l’identité de l’utilisateur. |
| | Service MVPD AuthZ | Point d’entrée MVPD qui fournit des décisions d’autorisation basées sur les abonnements des utilisateurs, le contrôle parental, etc. |

## Flux{#flows}

### Enregistrement de client dynamique (DCR)

Adobe Pass utilise le DCR pour sécuriser les communications client entre une application ou un serveur de programmation et les services Adobe Pass. Le flux DCR est distinct et est décrit dans la documentation [ Présentation de l’enregistrement du client dynamique ](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


### Flux D’Applications De Diffusion En Continu (Smart Device)

![](../../../../assets/smart-device-app-flow.png)

#### Flux de démarrage

1. Votre application démarre et charge son interface utilisateur initiale.

2. Obtenir/générer un identifiant d’appareil.

3. Émettez un appel Check-authentication pour vérifier si l’appareil est déjà authentifié.  Par exemple : [`<SP_FQDN>/api/v1/checkauthn [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

4. Si l’appel `checkauthn` réussit, passez au flux d’autorisation à partir de l’étape 2.  En cas d’échec, démarrez le flux d’enregistrement.



#### Flux d’enregistrement

1. Obtenez un code d’enregistrement et une URL que votre utilisateur peut utiliser pour accéder à l’application de connexion du deuxième écran, puis présentez-les à l’utilisateur :

   a. Envoyez une demande de POST au service de code d’enregistrement d’Adobe, en transmettant un ID d’appareil haché et une « URL d’enregistrement ».  Par exemple : [`<REGGIE_FQDN>/reggie/v1/[requestorId]/regcode [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)

   b. Présentez le code d’enregistrement et l’URL renvoyés à l’utilisateur.

   c. Demandez à l’utilisateur de passer à un appareil compatible avec le Web, accédez à l’URL, puis saisissez le code d’enregistrement.



#### Flux d’autorisation

1. L’utilisateur revient de l’application du deuxième écran et appuie sur le bouton « Continuer » sur votre appareil. Vous pouvez également implémenter un mécanisme d’interrogation pour vérifier le statut de l’authentification, mais l’Authentification Adobe Pass recommande la méthode du bouton Continuer plutôt que l’interrogation. <!--(For information on employing a "Continue" button versus polling the Adobe Pass Authentication backend server, see the Clientless Technical Overview: Managing 2nd-Screen Workflow Transition.)--> Par exemple : [\&lt;SP\_FQDN\>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)

2. Envoyez une demande de GET au service d’autorisation Authentification Adobe Pass pour lancer l’autorisation. Par exemple : `<SP_FQDN>/api/v1/authorize [device ID, Requestor ID, Resource ID]`

<!-- end list -->

* Si la réponse indique la réussite : l’utilisateur possède un jeton AuthN valide ET il est autorisé à regarder le média demandé (il existe un jeton AuthZ valide pour cet utilisateur).

* Si la réponse indique un échec : examinez l’exception renvoyée pour déterminer son type (AuthN, AuthZ ou autre chose) :

   * S’il s’agissait d’une erreur d’authentification, redémarrez le flux d’enregistrement.

   * S’il s’agissait d’une erreur AuthZ, l’utilisateur n’est pas autorisé à regarder le média demandé et un message d’erreur doit s’afficher à l’intention de l’utilisateur.

   * Si une autre erreur s’est produite (erreur de connexion, erreur réseau, etc.), affichez un message d’erreur approprié à l’intention de l’utilisateur.



#### Afficher le flux multimédia

1. Présenter les choix de médias. L’utilisateur sélectionne le média à afficher.

2. Les médias sont-ils protégés ?

   a. Votre application vérifie si le média est protégé.

   b. Si le média est protégé, votre application démarre l’autorisation.
(AuthZ) Flux ci-dessus.

   c. Si le média n’est pas protégé, lisez le média pour le
utilisateur.

3. Lire le média.


### Flux d’application AuthN (deuxième écran)

![](../../../../assets/secnd-screen-authn-flow.png)

1. Obtenez une liste des fichiers MVPD pour cet utilisateur. Par exemple : [`<SP_FQDN>/api/v1/config/[requestorID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)

1. Lancez le flux d’authentification.  Par exemple : [`<SP_FQDN>/api/v1/authenticate [requestorID, MVPD ID, Redirect URL, Domain name, Registration Code, "noflash=true"]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)

1. Vérifiez si l’authentification a réussi. Par exemple :[`<SP_FQDN>/api/v1/checkauthn/[registration code][requestor ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

1. Renvoyez l’utilisateur à votre application Smart Device pour terminer le flux d’autorisation.

## Authentification SSO Du Partenaire {#partner-sso}

Certains appareils fournissent une prise en charge dédiée à l’authentification unique (SSO) du partenaire :

* [SSO APPLE](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)

## Authentification Unique Platform {#platform-sso}

Certains appareils fournissent une prise en charge dédiée à l’authentification unique (SSO) de Platform :

* [SSO AMAZON](../../sso-access/amazon-sso-cookbook-rest-api-v1.md)
* [SSO Roku](../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)

## TempPass et TempPass promotionnel pour l’API REST {#temppass}

Pour les implémentations TempPass et Promotionnel TempPass où l’utilisateur n’est pas tenu de saisir ses informations d’identification, l’authentification peut être mise en œuvre directement dans l’application de diffusion en continu.

**Pour utiliser cette API, l’application de diffusion en continu doit s’assurer de l’unicité de l’identifiant de l’appareil, car il est utilisé pour identifier le jeton, avec les données supplémentaires facultatives.**


![](../../../../assets/temp-pass-promo-temppass.png)
