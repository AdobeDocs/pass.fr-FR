---
title: Présentation de l’authentification unique (SSO) Apple
description: Présentation de l’authentification unique (SSO) Apple
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 2afe9ea2a814817757f1ab28484a84466da68d62
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 0%

---

# Présentation de l’authentification unique (SSO) Apple {#apple-sso-overview}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Apple permet aux utilisateurs de se connecter à leur compte de fournisseur de télévision au niveau du système de l’appareil, ce qui élimine la nécessité de s’authentifier application par application.

L’authentification Adobe Pass s’est associée à Apple pour créer l’expérience utilisateur d’authentification unique (SSO) du partenaire dans l’écosystème TV Everywhere pour les propriétaires d’iPhone, d’iPad et d’Apple TV.

Pour bénéficier de l’expérience utilisateur de l’authentification unique (SSO) sur un appareil Apple, vous devez remplir la liste des conditions préalables répertoriées ci-dessous.

Le résultat final doit créer une expérience conforme aux flux d’utilisateurs suivants, que nous vous recommandons de consulter avant de commencer le développement de votre application :

* Authentification unique (SSO) [flux utilisateur pour les appareils iPhone et iPad](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf).
* Authentification unique (SSO) [flux utilisateur pour les appareils Apple TV](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf).

## Conditions préalables {#apple-sso-prerequisites}

Les conditions préalables à l’intégration peuvent s’appliquer à une ou plusieurs entités impliquées dans l’activité TVE, telles que les programmeurs, les MVPD, l’authentification Adobe Pass ou Apple.

### Programmeur {#apple-sso-prerequisites-programmer}

Pour bénéficier de l’expérience utilisateur de l’authentification unique (SSO), un programmeur doit :

* Contactez Apple pour activer le [framework de compte d’abonné vidéo](https://developer.apple.com/documentation/videosubscriberaccount) dans le cadre de votre ID d’équipe Apple et configurez le [droit d’authentification unique de l’abonné vidéo](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) dans le cadre de votre compte de développeur Apple.

   * Utilisez Xcode version 8 ou ultérieure et iOS/tvOS version 10 ou ultérieure.

* Activez l’authentification unique (SSO) pour chaque intégration et plateforme souhaitée (iOS/tvOS) via le [tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) en définissant la propriété `Enable Single Sign On` sur `Yes`.

| Adobe Enable Single Sign On | MVPD Apple **intégrées (prises en charge)** | Apple **Sélecteur** MVPD | Apple **non intégré (non pris en charge)** MVPD |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Oui (Activé) | Les flux d’authentification et de déconnexion impliqueront les solutions d’authentification Apple et Adobe Pass, tandis que tous les autres flux (autorisation, préautorisation, métadonnées, etc.) seront traités uniquement par l’authentification Adobe Pass. | Les flux d’authentification et de déconnexion reviendront aux flux standard gérés uniquement par l’authentification Adobe Pass. | Les flux d’authentification et de déconnexion reviendront aux flux standard gérés uniquement par l’authentification Adobe Pass. |
| Non (désactivé) | Les flux d’authentification et de déconnexion reviendront aux flux standard gérés uniquement par l’authentification Adobe Pass. | Les flux d’authentification et de déconnexion reviendront aux flux standard gérés uniquement par l’authentification Adobe Pass. | Les flux d’authentification et de déconnexion reviendront aux flux standard gérés uniquement par l’authentification Adobe Pass. |

* Intégrez les flux d’utilisateurs de l’authentification unique (SSO) à l’aide de l’une des solutions suivantes proposées par l’Authentification Adobe Pass pour les utilisateurs finaux des applications clientes s’exécutant sur iOS, iPadOS ou tvOS.

   * L’API REST d’authentification Adobe Pass V2 prend en charge l’authentification unique (SSO) du partenaire.

     Reportez-vous à la documentation du [Guide pas à pas Apple SSO (API REST V2)](apple-sso-cookbook-rest-api-v2.md) .

   * L’ancienne version de l’API REST d’authentification Adobe Pass prend en charge l’authentification unique (SSO) du partenaire.

     Reportez-vous à la documentation du [&#x200B; Guide pas à pas Apple (hérité) pour l’authentification unique (API REST V1)](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md) .

   * L’ancien SDK Adobe Pass Authentication AccessEnabler iOS/tvOS prend en charge l’authentification unique (SSO) des partenaires.

     Reportez-vous à la documentation du guide [&#x200B; (hérité) Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md) .

### MVPD {#apple-sso-prerequisites-mvpd}

Pour bénéficier de l’expérience utilisateur de l’authentification unique (SSO), un MVPD doit :

* Contactez Apple pour lancer le processus d’intégration du côté Apple.

   * Demandez la documentation technique sur la façon d’intégrer et de développer une application JavaScript TVML capable de gérer le formulaire de connexion de l’utilisateur.

* Contactez l’Authentification Adobe Pass pour lancer le processus d’intégration du côté Adobe.

   * Fournissez la valeur de chaîne représentant l’identifiant du fournisseur de télévision attribué par Apple lors du processus d’intégration.

## FAQ {#FAQ}

* En cas de problème avec le workflow de l’authentification unique Apple, l’application qui utilise Adobe Pass Authentication AccessEnabler iOS/tvOS SDK peut-elle revenir au flux d’authentification standard ?

  Cela est possible, mais nécessite une modification de configuration effectuée via le [tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) pour définir l’option **Activer l’authentification unique** sur **NON** pour l’intégration et la plateforme souhaitées (iOS/tvOS). N’oubliez pas que l’application cliente accuse réception de la modification de configuration uniquement après avoir appelé l’API [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3).


* L’application saura-t-elle quand une authentification a eu lieu suite à une connexion via l’authentification unique d’Apple ?

  Ces informations sont disponibles dans le cadre de la clé de métadonnées de l’utilisateur : *tokenSource*, qui doit renvoyer la valeur de chaîne : « Apple » dans ce cas.


* L’application saura-t-elle quand une authentification a eu lieu suite à une connexion via l’authentification unique d’Apple sur une autre application ?

  Ces informations ne sont pas disponibles.


* Que se passe-t-il si un utilisateur se connecte en accédant à la section *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS à l’aide d’un MVPD non intégré à l’application ?

  Lorsque l’utilisateur lance l’application, il n’est pas authentifié via le workflow SSO d’Apple. Par conséquent, l’application doit revenir au flux d’authentification normal et présenter son propre sélecteur MVPD.


* Que se passe-t-il si un utilisateur se connecte en accédant à la section *`Settings -> TV Provider`* sur iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* sur tvOS à l’aide d’un MVPD dont le paramètre **Activer l’authentification unique** est défini sur **NON** via le [Tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) pour la plateforme iOS/tvOS ?

  Lorsque l’utilisateur lance l’application, il n’est pas authentifié via le workflow SSO d’Apple. Par conséquent, l’application doit revenir au flux d’authentification normal et présenter son propre sélecteur MVPD.


* Que se passe-t-il si un utilisateur ou une utilisatrice dispose d’un MVPD qui n’est pas intégré (non pris en charge) par Apple, mais qui est présent dans le sélecteur Apple ?

  Lorsque l’utilisateur lance l’application, il sélectionne uniquement le MVPD via le workflow SSO d’Apple sans terminer le flux d’authentification. Par conséquent, l’application doit revenir au flux d’authentification normal, mais peut utiliser le MVPD déjà sélectionné.


* Que se passe-t-il si un utilisateur possède un MVPD qui n’est pas intégré (non pris en charge) par Apple ?

  Lorsque l’utilisateur lance l’application, il sélectionne l’option de sélecteur « Autres fournisseurs de services TV » dans le workflow de connexion unique d’Apple. Par conséquent, l’application doit revenir au flux d’authentification normal et présenter son propre sélecteur MVPD.


* Que se passe-t-il si un utilisateur possède un MVPD dégradé par le biais du tableau de bord [Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) ?

  Lorsque l&#39;utilisateur lance l&#39;application, il est authentifié via le mécanisme de dégradation et non via le workflow de connexion unique Apple. L’expérience doit être transparente pour l’utilisateur, tandis que l’application est informée par le code d’avertissement *N010* au cas où elle utilise le SDK Adobe Pass Authentication AccessEnabler iOS/tvOS.


* L’ID d’utilisateur MVPD changera-t-il entre les flux d’authentification SSO Apple et non Apple ?

  Il est attendu que l’ID utilisateur ne change pas, mais il doit être vérifié pour chaque fournisseur sélectionné.


* Les TTL d’authentification seront-elles modifiées ?

  L’authentification Adobe Pass continuera à respecter les TTL requises par les programmeurs pour leur intégration à chaque MVPD. Lorsque vous naviguez d’une application de programmation à une autre application de programmation via l’authentification unique Apple, la deuxième application dispose de la durée de vie de son intégration Programmer x MVPD correspondante (elle ne partage pas la durée de vie de la première application qui s’authentifie)

|                                      | TTL d’authentification Adobe Pass expiré | TTL d’authentification Adobe Pass valide |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **TTL du jeton d’appareil Apple expiré** | L’utilisateur n’est PAS authentifié (le sélecteur MVPD doit apparaître). | L’utilisateur est authentifié et la TTL est le temps restant de son jeton/profil d’authentification Adobe Pass |
| **TTL de jeton d’appareil Apple valide** | L’utilisateur est authentifié silencieusement et obtient un autre jeton/profil d’authentification Adobe Pass avec la durée de vie spécifiée dans le tableau de bord TVE. | L’utilisateur est authentifié et la TTL est le temps restant de son jeton/profil d’authentification Adobe Pass |
