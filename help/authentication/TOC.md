---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Authentification Adobe Pass
user-guide-description: L’authentification Adobe Pass est une solution de droits pour TV Everywhere, qui fournit une structure modulaire afin de déterminer si une personne qui demande l’accès à une ressource y a droit.
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 3%

---


# Aide sur l’authentification Adobe Pass {#authentication}

+ [Authentification Adobe Pass](home.md)
+ Démarrage rapide {#kickstart}
   + [Document technique](kickstart/technical-paper.md)
   + [Présentation du programmeur](kickstart/programmer-overview.md)
   + [Présentation MVPD](kickstart/mvpd-overview.md)
   + [Guide de démarrage rapide du programmeur](kickstart/programmer-kickstart-guide.md)
   + [Guide de démarrage rapide MVPD](kickstart/mvpd-kickstart-guide.md)
   + [Procédures de réaffectation](notes-technical/escalation-procedures.md)
   + [Glossaire](kickstart/glossary.md)
+ Guide D’Intégration Pour Les Programmeurs {#integration-guide-programmers}
   + API REST {#rest-apis}
      + API REST DCR {#rest-api-dcr}
         + [Présentation de l’enregistrement des clients dynamiques](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + API {#rest-api-dcr-apis}
            + [Récupération des informations d’identification client](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [Récupération du jeton d’accès](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + Flux {#rest-api-dcr-flows}
            + [Flux d’enregistrement de client dynamique](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + API REST V2 {#rest-api-v2}
         + [Présentation de l’API REST V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [Glossaire de l’API REST V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [FAQ sur l’API REST V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + API {#rest-api-v2-apis}
            + [Présentation des API REST V2](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + Configuration {#rest-api-v2-configuration-apis}
               + [Récupérer la configuration pour un prestataire spécifique](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + Sessions {#rest-api-v2-sessions-apis}
               + [Créer une session d’authentification](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [Reprendre la session d’authentification](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [Récupération d’une session d’authentification](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [Authentification dans l’agent utilisateur](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + Profils {#rest-api-v2-profiles-apis}
               + [Récupération des profils](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [Récupération du profil pour mvpd spécifique](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [Récupération du profil pour un code spécifique](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + Décisions {#rest-api-v2-decisions-apis}
               + [Récupération des décisions d’autorisation à l’aide de mvpd spécifique](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [Récupération des décisions de préautorisation à l’aide de mvpd spécifique](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + Déconnexion {#rest-api-v2-logout-apis}
               + [Lancement de la déconnexion pour mvpd spécifique](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + Connexion unique du partenaire {#rest-api-v2-partner-single-sign-on-apis}
               + [Récupération de la demande d’authentification du partenaire](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [Récupération du profil à l’aide de la réponse d’authentification du partenaire](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + Flux {#rest-api-v2-flows}
            + [Présentation des flux de l’API REST V2](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + Flux d’accès de base {#rest-api-v2-basic-access-flows}
               + [Flux de profils de base exécuté dans une application principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [Flux de profils de base exécuté dans une application secondaire](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [Flux d’authentification de base exécuté sur une application principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [Flux d’authentification de base exécuté sur une application secondaire](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [Flux d’autorisation de base exécuté sur une application principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [Flux de préautorisation de base effectué dans une application principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [Flux de connexion de base exécuté dans l’application principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + Flux d’accès dégradé {#rest-api-v2-degraded-access-flows}
               + [Flux d&#39;accès dégradés](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + Flux d’accès temporaire {#rest-api-v2-temporary-access-flows}
               + [Flux d’accès temporaires](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + Flux d’accès de connexion unique {#rest-api-v2-single-sign-on-access-flows}
               + [Authentification unique à l’aide de flux de partenaires](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [Authentification unique à l’aide des flux d’identités de plateforme](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [Authentification unique à l’aide de flux de jetons de service](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [Flux de déconnexion unique](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + Cookbooks {#rest-api-v2-cookbooks}
            + [Guide pas à pas de l’API REST V2 (client à serveur)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [Guide pas à pas de l’API REST V2 (serveur à serveur)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + Annexe {#rest-api-v2-appendix}
            + En-têtes {#rest-api-v2-appendix-headers}
               + [En-tête - Autorisation](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [En-tête - AP-Device-Identifier](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [En-tête - X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [En-tête - AD-Service-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [En-tête - Adobe-Objet-Jeton](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [En-tête - AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [En-tête - AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + Fonctionnalités standard {#standard-features}
      + Droits {#entitlements}
         + [Identification de la ressource protégée](integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)
         + [Autorisation de contrôle en amont](integration-guide-programmers/features-standard/entitlements/preflight-authz.md)
         + [Comment intégrer le vérificateur de jeton multimédia](integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md)
         + [Métadonnées utilisateur](integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md)
         + [Certificat de métadonnées utilisateur pour le chiffrement](integration-guide-programmers/features-standard/entitlements/user-metadata-certificate.md)
      + Rapport d’erreurs {#error-reporting}
         + [Amélioration des codes d’erreur](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
         + [Rapport d’erreurs](integration-guide-programmers/features-standard/error-reporting/error-reporting.md)
      + Accès à authentification unique {#sso-access}
         + Connexion unique du partenaire {#partner-sso}
            + Connexion unique à Apple {#apple-sso}
               + [Présentation d’Apple SSO](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Guide pas à pas Apple SSO (API REST V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
               + [Guide pas à pas Apple SSO (API REST V1)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v1.md)
               + [Guide pas à pas Apple SSO (SDK iOS/tvOS)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-iostvos-sdk.md)
         + Connexion unique à la plateforme {#platform-sso}
            + Connexion unique à Amazon {#amazon-sso}
               + [Guide pas à pas Amazon SSO (API REST V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
               + [Guide pas à pas Amazon SSO (API REST V1)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)
            + Connexion unique Roku {#roku-sso}
               + [Présentation de Roku SSO](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
         + [Prise en charge de la connexion unique](integration-guide-programmers/features-standard/sso-access/sso-support.md)
         + [SSO via authentification passive](integration-guide-programmers/features-standard/sso-access/sso-passive-authn.md)
      + Accès à l’authentification par domicile {#hba-access}
         + [Authentification basée sur la maison pour TV partout](integration-guide-programmers/features-standard/hba-access/home-based-authn-tve.md)
         + [État de l’adaptateur de bus hôte pour les MVPD](integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)
      + Prise en charge de la confidentialité {#privacy-support}
         + [Présentation de la prise en charge de la confidentialité](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [Comment effectuer une demande d’accès à des informations personnelles](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + Fonctionnalités Premium {#features-premium}
      + Accès temporaire {#temporary-access}
         + [Temp pass](integration-guide-programmers/features-premium/temporary-access/temp-pass.md)
         + [Passe temporaire promotionnelle](integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)
         + [Réinitialiser le transfert temporaire](integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md)
      + Accès dégradé {#degraded-access}
         + [Présentation de l’API de dégradation](integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md)
      + ESM {#esm}
         + [Présentation de la surveillance du service de droits](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [API de surveillance du service de droits](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [Mesures côté serveur](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + Analytics {#analytics}
         + [Intégration des données côté serveur d’authentification Adobe Pass dans Adobe Analytics](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [Utilisation de l’ID Experience Cloud dans l’authentification Adobe Pass](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + Hérité {#legacy}
      + (Hérité) API REST V1 {#rest-api-v1}
         + [Présentation de l’API REST V1](integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md)
         + [Référence de l’API REST V1](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + API (héritées) {#rest-api-v1-apis}
            + [Demande de code d’enregistrement](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [Enregistrement des retours](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [Supprimer un enregistrement d’enregistrement](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [Fournir la liste MVPD](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [Lancer l’authentification](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [Vérifier le jeton d’authentification](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [Récupération du jeton d’authentification](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [Lancer l’autorisation](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [Récupération du jeton d’autorisation](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [Obtention d’un jeton de média court](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [Vérification du flux d’authentification par application web de deuxième écran](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [Récupérer la liste des ressources préautorisées](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [Récupération de la liste des ressources préautorisées par une application web de deuxième écran](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [Lancement de la déconnexion](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [Métadonnées utilisateur](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [Récupération de la requête de profil](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [Exchange du jeton](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [Aperçu gratuit pour la transmission temporaire et la transmission temporaire promotionnelle](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + Cookbooks (hérités) {#rest-api-v1-cookbooks}
            + [Guide pas à pas de l’API REST V1 (client à serveur)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [Guide pas à pas de l’API REST V1 (serveur à serveur)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + SDK (hérités) {#sdks}
         + (Hérité) SDK JavaScript {#javascript-sdk}
            + [Présentation du SDK JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [Guide pas à pas du SDK JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [Référence de l’API du SDK JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [Préautorisation de l’API SDK JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + Instructions (héritées) {#javascript-sdk-guidelines}
               + [Connexion et déconnexion sans actualisation](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + (Hérité) SDK iOS/tvOS {#ios-tvos-sdk}
            + [Présentation du SDK iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [Guide pas à pas du SDK iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [Référence de l’API du SDK iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [Préautorisation de l’API du SDK iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + Instructions (héritées) {#ios-tvos-sdk-guidelines}
               + [Enregistrement de l’application iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [Guide de migration d’iOS/tvOS v3.x](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [Vérifications de l’intégrité du stockage iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + (Hérité) SDK Android {#android-sdk}
            + [Présentation du SDK Android](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [Guide pas à pas du SDK Android](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [Référence de l’API du SDK Android](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [Préautorisation de l’API SDK Android](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + Instructions (héritées) {#android-sdk-guidelines}
               + [Enregistrement d’une application Android](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [SDK Android avec enregistrement de client dynamique](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + (Hérité) SDK FireOS {#fireos-sdk}
            + [Présentation technique d’Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [Guide d’intégration Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [Référence de l’API Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [Enregistrement de l’application Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [SDK FireOS avec enregistrement de client dynamique](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [SSO Amazon FireOS - Guide de démarrage du programmeur](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
   + [Présentation du guide d’intégration des programmeurs](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [Mécanisme de ralentissement](integration-guide-programmers/throttling-mechanism.md)
   + [Configuration minimale requise](integration-guide-programmers/minimum-system-requirements.md)
   + [Flux de droits du programmeur](integration-guide-programmers/entitlement-flow.md)
   + [Cas d’utilisation des programmeurs](integration-guide-programmers/programmer-use-cases.md)
   + [Transmission des informations client (appareil, connexion et application)](integration-guide-programmers/passing-client-information-device-connection-and-application.md)
+ Guide d’intégration pour les MVPD {#integration-guide-mvpds}
   + [Fonctionnalités d’intégration](integration-guide-mvpds/mvpd-integr-features.md)
   + [Authentification](integration-guide-mvpds/authn-usecase.md)
   + [Authentification à l’aide du protocole OAuth 2.0](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [Autorisation](integration-guide-mvpds/authz-usecase.md)
   + [Autorisation de contrôle en amont](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [Déconnexion MVPD](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [Exchange de métadonnées de contenu](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [Exchange de métadonnées utilisateur](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [Service Web MVPD de proxy](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [Intégration SAML du proxy MVPD](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [Portée du fournisseur de services](integration-guide-mvpds/serv-provider-scoping.md)
   + [Adresses IP autorisées MVPD](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ Guide De L’Utilisateur Pour Le Tableau De Bord TVE {#user-guide-tve-dashboard}
   + [Présentation du tableau de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [Environnements](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [Révision et notification push des modifications](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Tableau de bord](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [Canaux](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [Programmeurs](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [Intégrations](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [Rapports](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [Journal des modifications](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
   + [Guide d’utilisation du tableau de bord TVE](user-guide-tve-dashboard/tve-dashboard-user-guide.md)
+ Notes de mise à jour {#release-notes}
   + 2024 {#release-notes-2024}
      + [Notes de mise à jour d’Adobe Pass Authentication 3.0.3](notes-releases/auth-rn-303.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 3.0](notes-releases/auth-rn-300.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 2.70](notes-releases/auth-rn-270.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 2.69](notes-releases/auth-rn-269.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.7.0](notes-releases/authn-rn-javascript-470.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.9.2](notes-releases/authn-rn-ios-tvos-392.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.8.4](notes-releases/authn-rn-ios-tvos-384.md)
   + 2023 {#release-notes-2023}
      + [Notes de mise à jour de l’authentification Adobe Pass 2.68](notes-releases/auth-rn-268.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 2.67](notes-releases/auth-rn-267.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 2.66](notes-releases/auth-rn-266.md)
      + [Notes de mise à jour d’Adobe Pass Authentication 2.65.1](notes-releases/auth-rn-2651.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 2.65](notes-releases/auth-rn-265.md)
      + [Notes de mise à jour d’Adobe Pass Authentication 2.64.1](notes-releases/auth-rn-2641.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.8.3](notes-releases/authn-rn-ios-tvos-383.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.8.2](notes-releases/authn-rn-ios-tvos-382.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.8.1](notes-releases/authn-rn-ios-tvos-381.md)
      + [Notes de mise à jour d’Adobe Pass Authentication Android 3.7.3](notes-releases/authn-rn-android-373.md)
   + 2022 {#release-notes-2022}
      + [Notes de mise à jour de l’authentification Adobe Pass 2.64](notes-releases/auth-rn-264.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 2.63](notes-releases/auth-rn-263.md)
      + [Notes de mise à jour d’Adobe Pass Authentication 2.62.1](notes-releases/auth-rn-2621.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.6.0](notes-releases/authn-rn-javascript-460.md)
   + 2021 {#release-notes-2021}
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.4.0](notes-releases/authn-rn-javascript-440.md)
      + [Notes de mise à jour d’Adobe Pass Authentication iOS / tvOS 3.7.0](notes-releases/authn-rn-ios-tvos-370.md)
+ Notes techniques {#tech-notes}
   + Environnements {#tech-notes-environments}
      + [Présentation des environnements Adobe](notes-technical/understanding-the-adobe-environments.md)
      + [Configuration de votre environnement et test dans un environnement de préqualification](notes-technical/setting-up-your-environment-and-testing-in-prequal.md)
      + [Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API Adobe](notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + Expérience utilisateur {#tech-notes-user-experience}
      + [Comment migrer la page de connexion MVPD d’iFrame vers la fenêtre contextuelle](notes-technical/migr-mvpd-login-iframe-popup.md)
      + [Fonctionnalité de contrôle en amont : comment activer, résoudre ou déterminer le problème](notes-technical/preflight-feature.md)
      + [Autorisation des MVPD dans la boîte de dialogue de sélection](notes-technical/allow-mvpd-selectn-dialog.md)
      + [Empêcher les MVPD d’apparaître la boîte de dialogue de sélection](notes-technical/prevent-mvpd-selectn-dialog.md)
   + API REST V1 {#tech-notes-rest-api-v1}
      + [Mise en oeuvre de l’API sans client - Codes d’erreur/messages avec une raison/cause probable](notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Flux d’API sans client en l’absence d’identifiant d’appareil](notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
      + [Sans client : évitez d’utiliser &#39;&amp;&#39;reg_code dans /authenticRequest](notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Activation des services de droits Adobe Pass pour un programmeur sur Xbox 360 et XboxOne sans client](notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Type d’appareil sans client et mesures](notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + SDK {#tech-notes-sdks}
      + [Questions et réponses sur les certificats](notes-technical/certificates-qa.md)
      + [Présentation des ID utilisateur](notes-technical/understanding-user-ids.md)
      + SDK JavaScript {#tech-notes-javascript-sdk}
         + [Évaluation de la prévention du suivi - Apple Safari](notes-technical/tracking-prevention-assessment-apple-safari.md)
         + [Évaluation de la prévention du suivi - Chrome Google](notes-technical/tracking-prevention-assessment-google-chrome.md)
         + [Mises à jour des cookies : indicateurs samesite et sécurisé](notes-technical/cookies-updates-samesite-and-secure-flags.md)
         + [Conseils de débogage](notes-technical/appendix-b-debugging-tips.md)
      + SDK Android {#tech-notes-android-sdk}
         + [Accès à l’authentification unique (SSO) du SDK Android sur les applications Android 10](notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Authentification Adobe Pass et nouveau modèle d’autorisations &quot;Marshmallow&quot; d’Android 6](notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + SDK iOS/tvOS {#tech-notes-ios-tvos-sdk}
         + [Prise en charge de WKWebView sur le SDK iOS 3.1+](notes-technical/wkwebview-support-on-ios-sdk-31.md)
         + [Prise en charge de SFSafariViewController sur le SDK iOS 3.2+](notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO sur iOS lors de l’utilisation de l’activateur d’accès à l’authentification Adobe Pass](notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [Erreur d’authentification iOS - Impossible de trouver adobepass.ios.app](notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Réinitialiser le transfert temporaire sur iOS](notes-technical/reset-temp-pass-on-ios.md)
         + [Débogage du SDK AccessEnabler iOS/tvOS à l’aide des journaux d’application de la console](notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [Chemin de mise à niveau d’AccessEnabler iOS/tvOS 3.7.0](notes-technical/accessenabler-iostvos-370-upgrade-path.md)
   + Dépannage {#tech-notes-troubleshooting}
      + [Utilisation du proxy Charles](notes-technical/using-charles-proxy.md)
      + [Surveillance de la transmission PayTV de l’Adobe Adobe Pass](notes-technical/monitoring-adobe-pay-tv-pass.md)
