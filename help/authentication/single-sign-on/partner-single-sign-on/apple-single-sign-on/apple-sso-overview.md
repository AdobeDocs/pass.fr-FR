---
title: Présentation d’Apple SSO
description: Présentation d’Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1256'
ht-degree: 0%

---

# Présentation d’Apple SSO {#apple-sso-overview}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Apple offre aux utilisateurs la possibilité de se connecter à leur compte de fournisseur de télévision au niveau du système de l’appareil, éliminant ainsi la nécessité de s’authentifier application par application.

Adobe Pass Authentication s’est associé à Apple pour créer l’expérience utilisateur de connexion unique (SSO) du partenaire dans l’écosystème TV Everywhere pour les propriétaires iPhone, iPad et Apple TV.

Afin de bénéficier de l’expérience utilisateur de connexion unique (SSO) sur un appareil Apple, une liste de conditions préalables documentées ci-dessous doit être remplie.

Le résultat final doit créer une expérience conforme aux flux d’utilisateurs suivants, que nous vous recommandons de consulter avant de commencer à développer votre application :

* Connexion unique (SSO) [ flux d’utilisateurs pour les appareils iPhone et iPad](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf).
* Authentification unique (SSO) [flux utilisateur pour les appareils Apple TV](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf).

## Conditions préalables {#apple-sso-prerequisites}

Les conditions d’intégration peuvent s’appliquer à une ou plusieurs entités impliquées dans l’activité TVE, telles que les programmeurs, les MVPD, l’authentification Adobe Pass ou Apple.

### Programmeur {#apple-sso-prerequisites-programmer}

Pour bénéficier de l’expérience utilisateur de connexion unique (SSO), un programmeur doit :

* Contactez Apple pour activer la [structure de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) dans le cadre de votre identifiant d’équipe Apple et configurer le [droit de connexion unique de l’abonné vidéo](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) dans le cadre de votre compte de développeur Apple.

   * Utilisez Xcode version 8 ou ultérieure et iOS/tvOS version 10 ou ultérieure.

* Activez l’authentification unique (SSO) pour chaque intégration et plateforme souhaitée (iOS/tvOS) par le biais du [ tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) en définissant la propriété `Enable Single Sign On` sur `Yes`.

| Adobe Activez l’authentification unique | Apple **MVPD intégrés (pris en charge)** | Apple **Sélecteur** MVPD | Apple **Non intégré (non pris en charge)** MVPD |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Oui (activé) | Les flux d’authentification et de déconnexion impliquent à la fois les solutions d’authentification Apple et Adobe Pass, tandis que tous les autres flux (autorisation, préautorisation, métadonnées, etc.) sera uniquement servi par l’authentification Adobe Pass. | Les flux d’authentification et de déconnexion reviendront aux flux réguliers fournis uniquement par l’authentification Adobe Pass. | Les flux d’authentification et de déconnexion reviendront aux flux réguliers fournis uniquement par l’authentification Adobe Pass. |
| Non (désactivé) | Les flux d’authentification et de déconnexion reviendront aux flux réguliers fournis uniquement par l’authentification Adobe Pass. | Les flux d’authentification et de déconnexion reviendront aux flux réguliers fournis uniquement par l’authentification Adobe Pass. | Les flux d’authentification et de déconnexion reviendront aux flux réguliers fournis uniquement par l’authentification Adobe Pass. |

* Intégrez les flux d’utilisateurs de l’authentification unique (SSO) à l’aide de l’une des solutions suivantes proposées par l’authentification Adobe Pass pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS.

   * L’API REST d’authentification Adobe Pass V2 prend en charge l’authentification unique du partenaire (SSO).

     Reportez-vous à la documentation [Apple SSO Cookbook (API REST V2)](./apple-sso-cookbook-rest-api-v2.md).

   * L’API REST d’authentification Adobe Pass V1 prend en charge l’authentification unique du partenaire (SSO).

     Reportez-vous à la documentation [Apple SSO Cookbook (API REST V1)](./apple-sso-cookbook-rest-api-v1.md).

   * Le SDK Adobe Pass Authentication AccessEnabler iOS/tvOS prend en charge l’authentification unique du partenaire (SSO).

     Reportez-vous à la documentation [Guide pas à pas Apple SSO (SDK iOS/tvOS)](./apple-sso-cookbook-iostvos-sdk.md).

### MVPD {#apple-sso-prerequisites-mvpd}

Pour bénéficier de l’expérience utilisateur de l’authentification unique (SSO), un MVPD doit :

* Contactez Apple pour lancer le processus d’intégration côté Apple.

   * Demandez la documentation technique sur l’intégration et le développement d’une application TVML JavaScript capable de gérer le formulaire de connexion de l’utilisateur.

* Contactez l’ authentification Adobe Pass pour lancer le processus d’intégration côté Adobe.

   * Indiquez la valeur string représentant l’identifiant du fournisseur de télévision attribué par Apple pendant le processus d’intégration.

## FAQ {#FAQ}

* En cas de problème avec le workflow SSO d’Apple, l’application utilisant le SDK Adobe Pass Authentication AccessEnabler iOS/tvOS peut-elle revenir au flux d’authentification régulier ?

  Cela est possible, mais nécessite une modification de configuration effectuée via le [tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) pour définir l’ **activation de l’authentification unique** sur **NO** pour l’intégration et la plateforme souhaitées (iOS/tvOS). Gardez à l’esprit que l’application cliente n’acquittera la modification de configuration qu’après avoir appelé l’API [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3).


* L’application sait-elle quand une authentification s’est produite suite à une connexion via l’authentification unique Apple ?

  Ces informations sont disponibles dans le cadre de la clé de métadonnées utilisateur : *tokenSource*, qui doit renvoyer la valeur de chaîne : &quot;Apple&quot; dans ce cas.


* L’application sait-elle quand une authentification s’est produite suite à une connexion via l’authentification unique Apple sur une autre application ?

  Ces informations ne sont pas disponibles.


* Que se passe-t-il si un utilisateur se connecte en accédant à *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur la section tvOS à l’aide d’un MVPD qui n’est pas intégré à l’application ?

  Lorsque l’utilisateur lance l’application, il ne sera pas authentifié via le workflow SSO Apple. Par conséquent, l’application doit revenir à un flux d’authentification régulier et présenter son propre sélecteur MVPD.


* Que se passe-t-il si un utilisateur se connecte en se rendant à l’instance *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur la section tvOS à l’aide d’un MVPD dont le paramètre **Activer l’authentification unique** est défini sur **NO** via le [tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) pour la plateforme iOS/tvOS ?

  Lorsque l’utilisateur lance l’application, il ne sera pas authentifié via le workflow SSO Apple. Par conséquent, l’application doit revenir à un flux d’authentification régulier et présenter son propre sélecteur MVPD.


* Que se passe-t-il si un utilisateur dispose d’un MVPD qui n’est pas intégré (non pris en charge) par Apple, mais qu’il est présent dans le sélecteur Apple ?

  Lorsque l’utilisateur lance l’application, il sélectionne uniquement le MVPD via le workflow SSO Apple sans terminer le flux d’authentification. Par conséquent, l’application doit retomber dans un flux d’authentification normal, mais peut utiliser le MVPD déjà sélectionné.


* Que se passe-t-il si un utilisateur dispose d’un MVPD qui n’est pas intégré (non pris en charge) par Apple ?

  Lorsque l’utilisateur lance l’application, il sélectionne &quot;Autres fournisseurs de télévision&quot; à l’aide du workflow SSO Apple. Par conséquent, l’application doit revenir à un flux d’authentification régulier et présenter son propre sélecteur MVPD.


* Que se passe-t-il si un utilisateur possède un MVPD qui est dégradé au moyen du [tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) ?

  Lorsque l’utilisateur lance l’application, il est authentifié via le mécanisme de dégradation, et non via le workflow SSO Apple. L’expérience doit être transparente pour l’utilisateur, tandis que l’application sera informée par le biais du code d’avertissement *N010* si elle utilise le SDK Adobe Pass Authentication AccessEnabler iOS/tvOS.


* L’identifiant utilisateur MVPD va-t-il changer entre les flux d’authentification SSO Apple et les flux d’authentification SSO non Apple ?

  On s’attend à ce que l’ID utilisateur ne change pas, mais il doit être vérifié pour chaque fournisseur sélectionné.


* Y aura-t-il une modification des TTL d’authentification ?

  L’authentification Adobe Pass continuera à respecter les TTL requis par les programmeurs pour leur intégration à chaque MVPD. Lorsque vous naviguez d’une application de programmeur à une autre application de programmeur via Apple SSO, la deuxième application aura le TTL de son intégration programmeur x MVPD correspondante (elle ne partage pas le TTL de la première application qui s’authentifie).

|                                      | Délai d’expiration de l’authentification Adobe Pass | Adobe Pass Authentication TTL valid |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Jeton de périphérique Apple ayant expiré** | L’utilisateur n’est PAS authentifié (le sélecteur MVPD doit apparaître) | l’utilisateur est authentifié et le délai d’activation est le temps restant de son jeton/profil d’authentification Adobe Pass. |
| **Jeton de périphérique Apple TTL valide** | L’utilisateur est authentifié en mode silencieux et obtient un autre jeton/profil d’authentification Adobe Pass avec le délai d’activation spécifié dans le tableau de bord TVE. | l’utilisateur est authentifié et le délai d’activation est le temps restant de son jeton/profil d’authentification Adobe Pass. |
