---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Authentification Adobe Pass
user-guide-description: L’authentification Adobe Pass est une solution de droits pour TV Everywhere, qui fournit une structure modulaire afin de déterminer si une personne qui demande l’accès à une ressource y a droit.
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1199'
ht-degree: 2%

---


# Aide sur l’authentification Adobe Pass {#authentication}

+ [Présentation de l’authentification Adobe Pass](home.md)
+ Concepts d’authentification Adobe Pass {#authentication-concepts}
   + [Document technique](technical-paper.md)
   + [Présentation pour les programmeurs](programmer-overview.md)
   + [Présentation MVPD](mvpd-overview.md)
+ Guides de démarrage rapide {#kickstart-guides}
   + [Guide de démarrage rapide du programmeur](programmer-kickstart-guide.md)
   + [Guide de démarrage rapide MVPD](mvpd-kickstart-guide.md)
+ Guide d’intégration des programmeurs {#programmer-integration-guide}
   + [Présentation du guide d’intégration des programmeurs](programmer-integration-guide-overview.md)
   + [Flux de droits du programmeur](entitlement-flow.md)
   + [Cas d’utilisation des programmeurs](programmer-use-cases.md)
   + [Transmission des informations client (appareil, connexion et application)](passing-client-information-device-connection-and-application.md)
   + [Mécanisme de ralentissement](throttling-mechanism.md)
   + API REST V1 {#rest-api-v1}
      + [Présentation de l’API REST](rest-api-overview.md)
      + [Guide pas à pas de l’API REST (serveur à serveur)](rest-api-cookbook-servertoserver.md)
      + [Guide pas à pas de l&#39;API REST (client à serveur)](rest-api-cookbook-clienttoserver.md)
      + Référence de l’API REST {#rest-api-reference}
         + [Référence de l’API REST](rest-api-reference.md)
         + [Demande de code d’enregistrement](registration-code-request.md)
         + [Enregistrement des retours](return-registration-record.md)
         + [Supprimer un enregistrement d’enregistrement](delete-registration-record.md)
         + [Fournir la liste MVPD](provide-mvpd-list.md)
         + [Lancer l’authentification](initiate-authentication.md)
         + [Vérifier le jeton d’authentification](check-authentication-token.md)
         + [Récupération du jeton d’authentification](retrieve-authentication-token.md)
         + [Lancer l’autorisation](initiate-authorization.md)
         + [Récupération du jeton d’autorisation](retrieve-authorization-token.md)
         + [Obtention d’un jeton de média court](obtain-short-media-token.md)
         + [Vérification du flux d’authentification par application web de deuxième écran](check-authentication-flow-by-second-screen-web-app.md)
         + [Récupérer la liste des ressources préautorisées](retrieve-list-of-preauthorized-resources.md)
         + [Récupération de la liste des ressources préautorisées par une application web de deuxième écran](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [Lancement de la déconnexion](initiate-logout.md)
         + [Métadonnées utilisateur](user-metadata.md)
         + [Récupération de la requête de profil](retrieve-profilerequest.md)
         + [Exchange du jeton](token-exchange.md)
         + [Aperçu gratuit pour la transmission temporaire et la transmission temporaire promotionnelle](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + API REST V2 {#rest-api-v2}
      + [Présentation de l’API REST V2](./rest-api-v2/rest-api-v2-overview.md)
      + [Glossaire de l’API REST V2](./rest-api-v2/rest-api-v2-glossary.md)
      + API {#rest-api-v2-apis}
         + [Présentation des API REST V2](rest-api-v2/apis/rest-api-v2-apis-overview.md)
         + Configuration {#rest-api-v2-configuration-apis}
            + [Récupérer la configuration pour un prestataire spécifique](rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
         + Sessions {#rest-api-v2-sessions-apis}
            + [Créer une session d’authentification](rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
            + [Reprendre la session d’authentification](rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
            + [Récupération d’une session d’authentification](rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
            + [Authentification dans l’agent utilisateur](rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
         + Profils {#rest-api-v2-profiles-apis}
            + [Récupération des profils](rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
            + [Récupération du profil pour mvpd spécifique](rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
            + [Récupération du profil pour un code spécifique](rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
         + Décisions {#rest-api-v2-decisions-apis}
            + [Récupération des décisions d’autorisation à l’aide de mvpd spécifique](rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
            + [Récupération des décisions de préautorisation à l’aide de mvpd spécifique](rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
         + Déconnexion {#rest-api-v2-logout-apis}
            + [Lancement de la déconnexion pour mvpd spécifique](rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
         + Connexion unique du partenaire {#rest-api-v2-partner-single-sign-on-apis}
            + [Récupération de la demande d’authentification du partenaire](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
            + [Récupération du profil à l’aide de la réponse d’authentification du partenaire](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
      + Flux {#rest-api-v2-flows}
         + [Présentation des flux de l’API REST V2](rest-api-v2/flows/rest-api-v2-flows-overview.md)
         + Flux d’accès de base {#rest-api-v2-basic-access-flows}
            + [Flux de profils de base exécuté dans une application principale](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
            + [Flux de profils de base exécuté dans une application secondaire](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
            + [Flux d’authentification de base exécuté sur une application principale](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
            + [Flux d’authentification de base exécuté sur une application secondaire](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
            + [Flux d’autorisation de base exécuté sur une application principale](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
            + [Flux de préautorisation de base effectué dans une application principale](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
            + [Flux de connexion de base exécuté dans l’application principale](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
         + Flux d’accès dégradé {#rest-api-v2-degraded-access-flows}
            + [Flux d&#39;accès dégradés](rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
         + Flux d’accès temporaire {#rest-api-v2-temporary-access-flows}
            + [Flux d’accès temporaires](rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
         + Flux d’accès de connexion unique {#rest-api-v2-single-sign-on-access-flows}
            + [Authentification unique à l’aide de flux de partenaires](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
            + [Authentification unique à l’aide des flux d’identités de plateforme](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
            + [Authentification unique à l’aide de flux de jetons de service](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
            + [Flux de déconnexion unique](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
      + Cookbooks {#rest-api-v2-cookbooks}
         + [Guide pas à pas de l’API REST V2 (client à serveur)](rest-api-v2/cookbooks/rest-api-v2-cookbooks-client-server.md)
      + Annexe {#rest-api-v2-appendix}
         + En-têtes {#rest-api-v2-appendix-headers}
            + [En-tête - Autorisation](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
            + [En-tête - AP-Device-Identifier](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
            + [En-tête - X-Device-Info](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
            + [En-tête - AD-Service-Token](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
            + [En-tête - Adobe-Objet-Jeton](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
            + [En-tête - AP-Partner-Framework-Status](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
            + [En-tête - AP-TempPass-Identity](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + SDK AccessEnabler {#accessenabler-sdk}
      + SDK JavaScript {#javascriptsdk}
         + [Présentation du SDK JavaScript](javascript-sdk-overview.md)
         + [Guide pas à pas du SDK JavaScript](javascript-sdk-cookbook.md)
         + [Référence de l’API du SDK JavaScript](javascript-sdk-api-reference.md)
         + Instructions {#js-sdk-guidelines}
            + [Connexion et déconnexion sans actualisation](refreshless-login-and-logout.md)
         + API JavaScript {#javascript-sdk-api}
            + [Préautoriser](preauthorize-api-javascript-sdk.md)
      + SDK iOS/tvOS {#ios-sdk}
         + [Présentation du SDK iOS/tvOS](iostvos-sdk-overview.md)
         + [Guide pas à pas du SDK iOS/tvOS](iostvos-sdk-cookbook.md)
         + [Référence de l’API du SDK iOS/tvOS](iostvos-sdk-api-reference.md)
         + Instructions {#ios-tvos-sdk-guidelines}
            + [Enregistrement de l’application iOS/tvOS](iostvos-application-registration.md)
            + Directives de migration {#migration-guidelines}
               + [Guide de migration d’iOS/tvOS v3.x](iostvos-v3x-migration-guide.md)
            + [Vérifications de l’intégrité du stockage iOS/tvOS](iostvos-sdk-storage-integrity-checks.md)
         + API iOS/tvOS {#ios-tvos-sdk-api}
            + [Préautoriser](preauthorize-api-ios-tvos-sdk.md)
      + SDK Android {#androidsdk}
         + [Présentation du SDK Android](android-sdk-overview.md)
         + [Guide pas à pas du SDK Android](android-sdk-cookbook.md)
         + [Référence de l’API du SDK Android](android-sdk-api-reference.md)
         + Instructions {#androidguidelines}
            + [Enregistrement d’une application Android](android-application-registration.md)
            + [SDK Android avec enregistrement de client dynamique](android-sdk-with-dynamic-client-registration.md)
         + API Android{#android-sdk-api}
            + [Préautoriser](preauthorize-api-android-sdk.md)
      + SDK Amazon FireOS {#fireossdk}
         + [Présentation technique d’Amazon FireOS](amazon-fireos-technical-overview.md)
         + [Guide d’intégration Amazon FireOS](amazon-fireos-integration-cookbook.md)
         + [Référence de l’API Amazon FireOS](amazon-fireos-native-client-api-reference.md)
         + [Enregistrement de l’application Amazon FireOS](amazon-fireos-application-registration.md)
         + [SDK FireOS avec enregistrement de client dynamique](fireos-sdk-with-dynamic-client-registration.md)
         + [SSO Amazon FireOS - Guide de démarrage du programmeur](amazon-firetv-sso-programmer-kickoff-guide.md)
   + Connexion unique {#sso}
      + Connexion unique du partenaire {#partner-sso}
         + Connexion unique à Apple {#apple-sso}
            + [Présentation d’Apple SSO](single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-overview.md)
            + [Guide pas à pas Apple SSO (API REST V2)](single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-cookbook-rest-api-v2.md)
            + [Guide pas à pas Apple SSO (API REST V1)](single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-cookbook-rest-api-v1.md)
            + [Guide pas à pas Apple SSO (SDK iOS/tvOS)](single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-cookbook-iostvos-sdk.md)
      + Connexion unique à la plateforme {#platform-sso}
         + Connexion unique à Amazon {#amazon-sso}
            + [Guide pas à pas Amazon SSO (API REST V1)](single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)
         + Connexion unique Roku {#roku-sso}
            + [Présentation de Roku SSO](single-sign-on/platform-single-sign-on/roku-single-sign-on/roku-sso-overview.md)
   + Métadonnées de contenu {#content-metadata}
      + [Identification de la ressource protégée](identify-protected-resources.md)
   + Intégration du serveur de contenu {#content-serv-int}
      + [Comment intégrer le vérificateur de jeton multimédia](media-token-verifier-int.md)
   + Annexes {#appendices}
      + [Conseils de débogage](appendix-b-debugging-tips.md)
+ Guide d’intégration MVPD {#mvpd-int-guide}
   + [Fonctionnalités d’intégration](mvpd-integr-features.md)
   + [Authentification](authn-usecase.md)
   + [Authentification à l’aide du protocole OAuth 2.0](authn-oauth2-protocol.md)
   + [Autorisation](authz-usecase.md)
   + [Autorisation de contrôle en amont](mvpd-preflight-authz.md)
   + [Déconnexion MVPD](usecase-mvpd-logout.md)
   + [Exchange de métadonnées de contenu](mvpd-content-metadata-exchange.md)
   + [Exchange de métadonnées utilisateur](mvpd-user-metadata-exchng.md)
   + [Service Web MVPD de proxy](proxy-mvpd-webserv.md)
   + [Intégration SAML du proxy MVPD](proxy-mvpd-saml-int.md)
   + [Portée du fournisseur de services](serv-provider-scoping.md)
   + [Adresses IP autorisées MVPD](mvpd-listing-ip-addres.md)
+ Fonctionnalités d’authentification Adobe Pass {#auth-features}
   + Intégration Adobe Analytics {#analytics-int}
      + [Intégration des données côté serveur d’authentification Adobe Pass dans Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [Utilisation de l’ID Experience Cloud dans l’authentification Adobe Pass](exp-cloud-id-authn.md)
   + Surveillance du service de droits {#entitlement-service-monitoring}
      + [Présentation de la surveillance du service de droits](entitlement-service-monitoring-overview.md)
      + [API de surveillance du service de droits](entitlement-service-monitoring-api.md)
      + [Mesures côté serveur](understanding-serverside-metrics.md)
   + Passe temporaire {#temp-pass}
      + [Temp pass](temp-pass.md)
      + [Passe temporaire promotionnelle](promotional-temp-pass.md)
      + [Réinitialiser le transfert temporaire](reset-temp-pass.md)
   + Authentification unique {#sso}
      + [Prise en charge de la connexion unique](sso-support.md)
      + [SSO via authentification passive](sso-passive-authn.md)
   + Authentification basée sur la maison {#home-based-auth}
      + [Authentification basée sur la maison pour TV partout](home-based-authn-tve.md)
      + [État de l’adaptateur de bus hôte pour les MVPD](hba-status-mvpds.md)
   + Métadonnées utilisateur {#user-metadat}
      + [Métadonnées utilisateur](user-metadata-feature.md)
      + [Certificat de métadonnées utilisateur pour le chiffrement](user-metadata-certificate.md)
   + [Autorisation de contrôle en amont](preflight-authz.md)
   + Rapport d’erreurs {#error-reportn}
      + [Rapport d’erreurs](error-reporting.md)
      + [Amélioration des codes d’erreur](enhanced-error-codes.md)
   + Enregistrement du client {#dcr-api}
      + [Présentation de l’enregistrement des clients dynamiques](./dcr-api/dynamic-client-registration-overview.md)
      + API {#dcr-api-apis}
         + [Récupération des informations d’identification client](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
         + [Récupération du jeton d’accès](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
      + Flux {#dcr-api-flows}
         + [Flux d’enregistrement de client dynamique](./dcr-api/flows/dynamic-client-registration-flow.md)
   + Service de dégradation {#degrn-service}
      + [Présentation de l’API de dégradation](degradation-api-overview.md)
   + Préparation à la confidentialité {#privacy-readiness}
      + [Présentation de la prise en charge de la confidentialité](privacy-supp-overview.md)
      + [Comment effectuer une demande d’accès à des informations personnelles](make-privacy-req.md)
+ Conseils et dépannage {#tips-troubleshoot}
   + [Autorisation des MVPD dans la boîte de dialogue de sélection](allow-mvpd-selectn-dialog.md)
   + [Empêcher les MVPD d’apparaître la boîte de dialogue de sélection](prevent-mvpd-selectn-dialog.md)
+ Assistance {#support}
   + [Procédures de réaffectation](escalation-procedures.md)
   + [Surveillance de la transmission PayTV de l’Adobe Adobe Pass](monitoring-adobe-pay-tv-pass.md)
   + [Configuration système minimale](minimum-system-requirements.md)
+ Notes de mise à jour {#release-notes}
   + [Notes de mise à jour d’Adobe Pass Authentication 3.0.3](auth-rn-303.md)
   + [Notes de mise à jour de l’authentification Adobe Pass 3.0](auth-rn-300.md)
   + [Notes de mise à jour de l’authentification Adobe Pass 2.70](auth-rn-270.md)
   + [Notes de mise à jour de l’authentification Adobe Pass 2.69](auth-rn-269.md)
   + [Notes de mise à jour de l’authentification Adobe Pass 2.68](auth-rn-268.md)
   + [Notes de mise à jour de l’authentification Adobe Pass 2.67](auth-rn-267.md)
   + [Notes de mise à jour de l’authentification Adobe Pass 2.66](auth-rn-266.md)
   + [Notes de mise à jour d’Adobe Pass Authentication 2.65.1](auth-rn-2651.md)
   + [Notes de mise à jour de l’authentification Adobe Pass 2.65](auth-rn-265.md)
   + [Notes de mise à jour d’Adobe Pass Authentication 2.64.1](auth-rn-2641.md)
   + [Notes de mise à jour de l’authentification Adobe Pass 2.64](auth-rn-264.md)
   + [Notes de mise à jour de l’authentification Adobe Pass 2.63](auth-rn-263.md)
   + [Notes de mise à jour d’Adobe Pass Authentication 2.62.1](auth-rn-2621.md)
   + Notes de mise à jour du SDK JavaScript {#release-notes-javascript}
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.7.0](authn-rn-javascript-470.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.6.0](authn-rn-javascript-460.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.4.0](authn-rn-javascript-440.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.2.0](authn-rn-javascript-420.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.1.1](authn-rn-javascript-411.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.1.0](authn-rn-javascript-410.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.0.0](authn-rn-javascript-400.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 3.5.0](authn-rn-javascript-350.md)
   + Notes de mise à jour du SDK iOS/tvOS {#release-notes-ios}
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.9.2](authn-rn-ios-tvos-392.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.8.4](authn-rn-ios-tvos-384.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.8.3](authn-rn-ios-tvos-383.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.8.2](authn-rn-ios-tvos-382.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.8.1](authn-rn-ios-tvos-381.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.7.0](authn-rn-ios-tvos-370.md)
   + Notes de mise à jour du SDK Android {#release-notes-android}
      + [Notes de mise à jour d’Adobe Pass Authentication Android 3.7.3](authn-rn-android-373.md)
+ Notes techniques {#tech-notes}
   + SDK d’authentification Adobe Pass {#primetime-authentication-sdks}
      + [Questions et réponses sur les certificats](certificates-qa.md)
      + SDK JavaScript {#javascript}
         + [Évaluation de la prévention du suivi - Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [Évaluation de la prévention du suivi - Chrome Google](tracking-prevention-assessment-google-chrome.md)
         + [Mises à jour des cookies : indicateurs samesite et sécurisé](cookies-updates-samesite-and-secure-flags.md)
      + SDK Android {#android}
         + [Accès à l’authentification unique (SSO) du SDK Android sur les applications Android 10](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Authentification Adobe Pass et nouveau modèle d’autorisations &quot;Marshmallow&quot; d’Android 6](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + SDK iOS/tvOS {#iostvos}
         + [Prise en charge de WKWebView sur le SDK iOS 3.1+](wkwebview-support-on-ios-sdk-31.md)
         + [Prise en charge de SFSafariViewController sur le SDK iOS 3.2+](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO sur iOS lors de l’utilisation de l’activateur d’accès à l’authentification Adobe Pass](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [Erreur d’authentification iOS - Impossible de trouver adobepass.ios.app](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Réinitialiser le transfert temporaire sur iOS](reset-temp-pass-on-ios.md)
         + [Débogage du SDK AccessEnabler iOS/tvOS à l’aide des journaux d’application de la console](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [Chemin de mise à niveau d’AccessEnabler iOS/tvOS 3.7.0](accessenabler-iostvos-370-upgrade-path.md)
   + Transmettre des environnements d’authentification {#primetime-authentication-environments}
      + [Présentation des environnements Adobe](understanding-the-adobe-environments.md)
      + [Configuration de votre environnement et test dans un environnement de préqualification](setting-up-your-environment-and-testing-in-prequal.md)
      + [Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API Adobe](test-authn-authz-flows-using-adobes-api-test-site.md)
   + API sans client {#clientless-api}
      + [Mise en oeuvre de l’API sans client - Codes d’erreur/messages avec une raison/cause probable](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Flux d’API sans client en l’absence d’identifiant d’appareil](clientless-api-flow-in-the-absence-of-device-id.md)
      + [Sans client : évitez d’utiliser &#39;&amp;&#39;reg_code dans /authenticRequest](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Activation des services de droits Adobe Pass pour un programmeur sur Xbox 360 et XboxOne sans client](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Type d’appareil sans client et mesures](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + Expérience utilisateur {#user-exp}
      + [Comment migrer la page de connexion MVPD d’iFrame vers la fenêtre contextuelle](migr-mvpd-login-iframe-popup.md)
      + [Fonctionnalité de contrôle en amont : comment activer, résoudre ou déterminer le problème](preflight-feature.md)
   + Outils et utilitaires {#tools-and-utilities}
      + [Utilisation du proxy Charles](using-charles-proxy.md)
   + Concepts {#concepts}
      + [Présentation des ID utilisateur](understanding-user-ids.md)
+ [Guide d’utilisation du tableau de bord TVE](tve-dashboard/old-tve-dashboard/tve-dashboard-user-guide.md)
+ Nouveau guide d’utilisation du tableau de bord TVE {#user-guide}
   + [Présentation du tableau de bord TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md)
   + [Environnements](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-environments.md)
   + [Révision et notification push des modifications](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Tableau de bord](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-home.md)
   + [Canaux](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md)
   + [Programmeurs](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-mvpds.md)
   + [Intégrations](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md)
   + [Rapports](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-reports.md)
   + [Journal des modifications](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-changes-log.md)
+ [Glossaire](glossary.md)
