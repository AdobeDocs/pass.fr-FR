---
title: Présentation d’Apple SSO
description: Présentation d’Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1452'
ht-degree: 0%

---

# Présentation d’Apple SSO {#apple-sso-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#Introduction}

Apple fournit une API qui permet aux utilisateurs de se connecter à leur compte de fournisseur de télévision au niveau du système de l’appareil, éliminant ainsi la nécessité de s’authentifier application par application.

Par conséquent, Apple et Adobe Pass Authentication se sont associées pour créer l’expérience utilisateur SSO (Single Sign-On) de la plateforme dans l’écosystème TV Everywhere pour les propriétaires iPhone, iPad et Apple TV.

Pour bénéficier de l’expérience utilisateur de connexion unique (SSO) sur un appareil Apple, une liste de conditions préalables doit être remplie.

</br>

## Conditions préalables {#Prerequisites}

La condition préalable peut s’appliquer à une ou plusieurs entités impliquées dans l’activité TVE, telles que les programmeurs, les MVPD, l’authentification Adobe Pass ou Apple.

</br>

### Programmeur {#Programmer}

Pour bénéficier de l’expérience utilisateur de connexion unique (SSO), un programmeur doit :

1. Utilisez au moins Xcode version 8 et iOS/tvOS version 10.

1. Si la variable [Droit de connexion unique de l’abonné vidéo](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) configuré sur leur compte de développeur Apple. Veuillez contacter Apple pour activer [Structure du compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) pour votre identifiant d’équipe Apple.

1. Activez l’authentification unique (YES) pour chaque intégration souhaitée (Canal x MVPD) et la plateforme souhaitée (iOS/tvOS) via le [Tableau de bord Adobe Primetime TVE](https://console.auth.adobe.com/).

1. Intégrez les workflows SSO Apple à l’aide de l’une des deux solutions proposées par l’équipe d’authentification Adobe Pass :

   - L’API REST d’authentification Adobe Pass peut prendre en charge l’authentification par authentification unique (SSO) de la plateforme pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS. Voir aussi [Guide pas à pas Apple SSO (API REST)](/help/authentication/apple-sso-cookbook-rest-api.md).

   - Le SDK Adobe Pass Authentication AccessEnabler iOS/tvOS peut prendre en charge l’authentification SSO (Single Sign-On) par plateforme pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS. Voir aussi [Guide pas à pas Apple SSO (SDK iOS/tvOS)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md).

   - **<u>Conseil Pro :</u>** Pour pouvoir accéder aux informations d’abonnement de l’utilisateur, celui-ci doit donner à l’application l’autorisation de continuer, comme s’il avait accès à la caméra ou au microphone de l’appareil. Cette autorisation doit être demandée par application et l’appareil enregistre la sélection de l’utilisateur. Gardez à l’esprit que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès aux autorisations du fournisseur de télévision) ou à la section depuis *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS.

   - **<u>Conseil Pro :</u>** Nous vous recommandons de demander l’autorisation de l’utilisateur lorsque l’application entre en premier plan, mais il s’agit uniquement d’une suggestion, car l’application peut vérifier [autorisation d’accès](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) les informations d’abonnement de l’utilisateur à tout moment avant de nécessiter une authentification de l’utilisateur. En outre, les API du SDK iOS/tvOS AccessEnabler demandent automatiquement l’autorisation de l’utilisateur lorsqu’il en a besoin.

   - **<u>Conseil Pro :</u>** Nous vous recommandons d’inciter les utilisateurs qui refusent de donner l’autorisation d’accéder aux informations d’abonnement en expliquant les avantages de l’expérience utilisateur de connexion unique (SSO). Gardez à l’esprit que l’utilisateur peut modifier sa décision en accédant aux paramètres de l’application (accès aux autorisations du fournisseur de télévision) ou à la section depuis *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS.

Le résultat doit créer une expérience conforme aux flux d’utilisateurs suivants, que nous vous suggérons de consulter avant de commencer à développer votre ou vos applications :

- [iPhone/iPad](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) flux d’utilisateurs
- [APPLE TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) flux d’utilisateurs


>[!IMPORTANT]
>
> Lorsque la fonction de connexion unique est **enabled** pour iOS/tvOS **et** dans le cas d’Apple **Intégration (prise en charge) ou sélecteur** Les MVPD que les flux d’authentification/déconnexion des workflows SSO Apple impliquent à la fois les solutions d’authentification Apple et Adobe Pass, tandis que tous les autres flux (autorisation, préautorisation, métadonnées, etc.) sera uniquement servi par l’authentification Adobe Pass.


>[!IMPORTANT]
>
> Lorsque la fonction de connexion unique est **disabled** pour iOS/tvOS **ou** dans le cas d’Apple **non intégré (non pris en charge)** Les MVPD que les flux d’authentification/de déconnexion reviennent des workflows SSO Apple vers les workflows standard fournis uniquement par l’authentification Adobe Pass.


>[!IMPORTANT]
>
> L’un des principaux avantages du workflow SSO d’Apple est représenté par le flux utilisateur d’authentification à un écran, qui peut également être diffusé sur les téléviseurs Apple lorsque la fonctionnalité de connexion unique est **enabled** pour tvOS **et** dans le cas d’Apple **intégré (pris en charge)** MVPD.


### MVPD {#MVPD}

Pour bénéficier de l’expérience utilisateur de l’authentification unique (SSO), un MVPD doit :



1. Être intégré dans le workflow SSO Apple côté Apple. Veuillez contacter Apple pour faciliter le processus d’intégration.
1. Fournissez une application JavaScript TVML capable de gérer le formulaire de connexion de l’utilisateur. Veuillez contacter Apple pour recevoir la documentation appropriée.
1. Fournissez une valeur string qui représente l’identifiant du fournisseur attribué par Apple pendant le processus d’intégration. Contactez l’ authentification Adobe Pass pour effectuer les modifications de configuration.

</br>

## FAQ {#FAQ}

1. En cas de problème avec le workflow SSO d’Apple, l’application utilisant le SDK AccessEnabler iOS/tvOS peut-elle revenir à un flux d’authentification standard ?
   - Cela est possible, mais nécessite qu’une modification de configuration soit effectuée sur la variable [Tableau de bord Adobe Primetime TVE](https://console.auth.adobe.com/). La variable *Activation de l’authentification unique* doit être défini sur *NON* pour l’intégration souhaitée (Canal x MVPD) et la plateforme souhaitée (iOS/tvOS).
   - L’application reconnaîtra le changement de configuration uniquement après avoir appelé [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) API si elle utilise le SDK AccessEnabler iOS/tvOS.
1. L’application sait-elle quand une authentification a eu lieu suite à une connexion via la connexion unique à la plate-forme sur un autre appareil ou une autre application ?
   - Ces informations ne seront pas disponibles.
1. L’application sait-elle quand une authentification s’est produite suite à une connexion via la connexion unique à la plateforme sur le même appareil ?
   - Ces informations sont disponibles dans le cadre de la clé de métadonnées utilisateur : *tokenSource*, qui doit renvoyer la valeur de chaîne : &quot;Apple&quot; dans ce cas.
1. Que se passe-t-il si un utilisateur se connecte en accédant à la variable *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur la section tvOS à l’aide d’un MVPD qui n’est pas intégré à l’application ?
   - Lorsque l’utilisateur lance l’application, il ne sera pas authentifié via le workflow SSO Apple. Par conséquent, l’application doit revenir au flux d’authentification standard et présenter son propre sélecteur MVPD.
1. Que se passe-t-il si un utilisateur se connecte en accédant à la variable *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur la section tvOS à l’aide d’un MVPD doté de la propriété *Activation de l’authentification unique* défini sur *NON* sur le [Tableau de bord Adobe Primetime TVE](https://console.auth.adobe.com/) pour la plateforme iOS/tvOS ?
   - Lorsque l’utilisateur lance l’application, il ne sera pas authentifié via le workflow SSO Apple. Par conséquent, l’application doit revenir au flux d’authentification standard et présenter son propre sélecteur MVPD.
1. Que se passe-t-il si un utilisateur dispose d’un MVPD qui n’est pas intégré (non pris en charge) par Apple, mais qu’il est présent dans le sélecteur Apple ?
   - Lorsque l’utilisateur lance l’application, il sélectionne uniquement le MVPD via le workflow SSO Apple sans terminer le flux d’authentification. Par conséquent, l’application doit revenir au flux d’authentification standard, mais peut utiliser le MVPD déjà sélectionné.
1. Que se passe-t-il si un utilisateur dispose d’un MVPD qui n’est pas intégré (non pris en charge) par Apple ?
   - Lorsque l’utilisateur lance l’application, il sélectionne &quot;Autres fournisseurs de télévision&quot; à l’aide du workflow SSO Apple. Par conséquent, l’application doit revenir au flux d’authentification standard et présenter son propre sélecteur MVPD.
1. Que se passe-t-il si un utilisateur possède un MVPD qui est dégradé au moyen de la fonction [Tableau de bord Adobe Primetime TVE](https://console.auth.adobe.com/)?
   - Lorsque l’utilisateur lance l’application, il est authentifié via le mécanisme de dégradation, et non via le workflow SSO Apple.
   - L’expérience doit être transparente pour l’utilisateur, tandis que l’application sera informée via la variable *N010* code d’avertissement s’il utilise le SDK AccessEnabler iOS/tvOS.
1. L’identifiant utilisateur MVPD va-t-il changer entre le flux d’authentification SSO Apple et le flux d’authentification SSO non Apple ?
   - On s’attend à ce que l’ID utilisateur ne change pas, mais il doit être vérifié pour chaque fournisseur sélectionné.
1. Y aura-t-il une modification des TTL d’authentification ?
   - L’authentification Adobe Pass continuera à respecter les TTL requis par les programmeurs pour leur intégration à chaque MVPD.
   - Lorsque vous naviguez d’une application de programmeur à une autre application de programmeur via Apple SSO, la deuxième application aura le TTL de son intégration programmeur x MVPD correspondante (elle ne partage pas le TTL de la première application qui s’authentifie).

|                                      | Délai d’expiration de l’authentification Adobe Pass | Adobe Pass Authentication TTL valid |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Jeton d’appareil Apple ayant expiré** | L’utilisateur n’est PAS authentifié (le sélecteur MVPD doit apparaître) | l’utilisateur est authentifié et le délai d’activation est le temps restant de son jeton d’authentification Adobe Pass. |
| **Jeton d’appareil Apple valide** | L’utilisateur est authentifié en mode silencieux et obtient un autre jeton d’authentification Adobe Pass avec le TTL spécifié dans le tableau de bord TVE. | l’utilisateur est authentifié et le délai d’activation est le temps restant de son jeton d’authentification Adobe Pass. |

<!--

## Resources {#Resources}

- [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)
- [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)
- [Sign in with your TV provider on your iPhone, iPad, or iPod touch](https://support.apple.com/en-us/HT207035)
- [Use your pay TV or cable provider with Apple TV](https://support.apple.com/en-us/HT207035)
- [TV providers that let you sign in on your iPhone, iPad, or Apple TV](https://support.apple.com/en-us/HT208084)
- [TV Provider Authentication](https://developer.apple.com/design/human-interface-guidelines/tvos/system-capabilities/tv-provider-authentication/)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
