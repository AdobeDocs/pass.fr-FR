---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Authentification Adobe Pass
user-guide-description: L’authentification Adobe Pass est une solution de droits pour TV Everywhere, qui fournit une structure modulaire afin de déterminer si une personne qui demande l’accès à une ressource y a droit.
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '1250'
ht-degree: 2%

---


# Aide sur l’authentification Adobe Pass {#authentication}

+ [Authentification Adobe Pass](home.md)
+ [Annonces de produit](product-announcements.md)
+ Versions du produit {#product-releases}
   + {#2024} 2024
      + [Notes de mise à jour de l’authentification Adobe Pass version 3.0.3](notes-releases/auth-rn-303.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 3.0](notes-releases/auth-rn-300.md)
      + [Notes de mise à jour de l’authentification Adobe Pass version 2.70](notes-releases/auth-rn-270.md)
      + [Notes de mise à jour de l’authentification Adobe Pass version 2.69](notes-releases/auth-rn-269.md)
      + [Notes de mise à jour de la version 4.7.0 d’Adobe Pass Authentication JavaScript](notes-releases/authn-rn-javascript-470.md)
      + [Authentification Adobe Pass Notes de mise à jour d’iOS / tvOS 3.9.2](notes-releases/authn-rn-ios-tvos-392.md)
      + [Authentification Adobe Pass Notes de mise à jour d’iOS / tvOS 3.8.4](notes-releases/authn-rn-ios-tvos-384.md)
   + {#2023} 2023
      + [Notes de mise à jour de l’authentification Adobe Pass version 2.68](notes-releases/auth-rn-268.md)
      + [Notes de mise à jour de l’authentification Adobe Pass version 2.67](notes-releases/auth-rn-267.md)
      + [Notes de mise à jour de l’authentification Adobe Pass version 2.66](notes-releases/auth-rn-266.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 2.65.1](notes-releases/auth-rn-2651.md)
      + [Notes de mise à jour de l’authentification Adobe Pass version 2.65](notes-releases/auth-rn-265.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 2.64.1](notes-releases/auth-rn-2641.md)
      + [Authentification Adobe Pass Notes de mise à jour d’iOS / tvOS 3.8.3](notes-releases/authn-rn-ios-tvos-383.md)
      + [Authentification Adobe Pass Notes de mise à jour d’iOS / tvOS 3.8.2](notes-releases/authn-rn-ios-tvos-382.md)
      + [Authentification Adobe Pass Notes de mise à jour d’iOS / tvOS 3.8.1](notes-releases/authn-rn-ios-tvos-381.md)
      + [Notes de mise à jour de l’authentification Adobe Pass Android 3.7.3](notes-releases/authn-rn-android-373.md)
   + {#2022} 2022
      + [Notes de mise à jour de l’authentification Adobe Pass version 2.64](notes-releases/auth-rn-264.md)
      + [Notes de mise à jour de l’authentification Adobe Pass version 2.63](notes-releases/auth-rn-263.md)
      + [Notes de mise à jour de l’authentification Adobe Pass 2.62.1](notes-releases/auth-rn-2621.md)
      + [Notes de mise à jour de la version 4.6.0 d’Adobe Pass Authentication JavaScript](notes-releases/authn-rn-javascript-460.md)
   + {#2021} 2021
      + [Notes de mise à jour de la version 4.4.0 d’Adobe Pass Authentication JavaScript](notes-releases/authn-rn-javascript-440.md)
      + [Authentification Adobe Pass Notes de mise à jour d’iOS / tvOS 3.7.0](notes-releases/authn-rn-ios-tvos-370.md)
+ {#kickstart} Kickstart
   + [Document technique](kickstart/technical-paper.md)
   + [Présentation du programmeur](kickstart/programmer-overview.md)
   + [Présentation de MVPD](kickstart/mvpd-overview.md)
   + [Guide de démarrage rapide du programmeur](kickstart/programmer-kickstart-guide.md)
   + [Guide de démarrage rapide de MVPD](kickstart/mvpd-kickstart-guide.md)
   + [Procédures d’escalade](kickstart/escalation-procedures.md)
+ Guide D’Intégration Pour Les Programmeurs {#integration-guide-programmers}
   + {#rest-apis} des API REST
      + {#rest-api-dcr} du DCR de l’API REST
         + [Présentation de l’enregistrement client dynamique](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + [Glossaire d’enregistrement client dynamique](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)
         + [FAQ sur l’enregistrement de clients dynamiques](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)
         + {#rest-api-dcr-apis} des API
            + [Récupérer les informations d’identification du client](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [Récupérer le jeton d’accès](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + Flux {#rest-api-dcr-flows}
            + [Flux d’enregistrement client dynamique](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + API REST V2 {#rest-api-v2}
         + [Présentation de l’API REST V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [Glossaire de l’API REST V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [FAQ sur l’API REST V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + {#rest-api-v2-apis} des API
            + [Présentation des API REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + {#rest-api-v2-configuration-apis} de configuration
               + [Récupération de la configuration pour un fournisseur de services spécifique](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + Sessions {#rest-api-v2-sessions-apis}
               + [Créer une session d’authentification](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [Reprendre la session d’authentification](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [Récupérer la session d’authentification](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [Authentification dans l’agent utilisateur](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + {#rest-api-v2-profiles-apis} de profils
               + [Récupération des profils](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [Récupération du profil pour un fichier mvpd spécifique](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [Récupération du profil pour un code spécifique](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + {#rest-api-v2-decisions-apis} des décisions
               + [Récupérer des décisions d’autorisation à l’aide de mvpd spécifiques](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [Récupérer des décisions de préautorisation à l’aide de mvpd spécifiques](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + {#rest-api-v2-logout-apis} de déconnexion
               + [Lancer la déconnexion pour un fichier mvpd spécifique](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + {#rest-api-v2-partner-single-sign-on-apis} d&#39;authentification SSO du partenaire
               + [Récupérer la demande d’authentification du partenaire](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [Récupérer le profil à l’aide de la réponse d’authentification du partenaire](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + Flux {#rest-api-v2-flows}
            + [Présentation des flux de l’API REST V2](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + Flux d’accès de base {#rest-api-v2-basic-access-flows}
               + [Flux de profils de base exécuté dans l’application principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [Flux de profils de base exécuté dans l’application secondaire](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [Flux d’authentification de base effectué dans l’application principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [Flux d’authentification de base effectué dans l’application secondaire](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [Flux d’autorisation de base exécuté dans l’application principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [Flux de préautorisation de base exécuté dans l’application principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [Flux de déconnexion de base effectué dans l’application principale](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + Flux d’accès dégradés {#rest-api-v2-degraded-access-flows}
               + [Flux d’accès dégradés](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + Flux d’accès temporaires {#rest-api-v2-temporary-access-flows}
               + [Flux d’accès temporaires](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + Flux d’accès SSO {#rest-api-v2-single-sign-on-access-flows}
               + [Authentification unique à l’aide des flux de partenaires](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [Authentification unique à l’aide des flux d’identité de plateforme](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [Authentification unique à l’aide des flux de jeton de service](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [Flux de déconnexion unique](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + {#rest-api-v2-cookbooks} des livres de cookie
            + [Guide pas à pas API REST V2 (client à serveur)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [Guide pas à pas API REST V2 (serveur à serveur)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + Annexe {#rest-api-v2-appendix}
            + En-têtes {#rest-api-v2-appendix-headers}
               + [En-tête - Autorisation](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [En-tête - Identifiant-appareil-AP](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [En-Tête - X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [En-tête - Jeton de service AD](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [En-Tête - Adobe-Subject-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [En-tête - AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [En-tête - AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + Fonctionnalités standard {#standard-features}
      + Droits {#entitlements}
         + [Autorisation de contrôle en amont](integration-guide-programmers/features-standard/entitlements/preflight-authz.md)
         + [Ressource protégée](integration-guide-programmers/features-standard/entitlements/protected-resources.md)
         + [Jetons de média](integration-guide-programmers/features-standard/entitlements/media-tokens.md)
         + [Métadonnées utilisateur](integration-guide-programmers/features-standard/entitlements/user-metadata.md)
      + {#error-reporting} de rapport d’erreur
         + [Codes d’erreur améliorés](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
      + {#sso-access} d&#39;accès SSO
         + {#partner-sso} d&#39;authentification SSO du partenaire
            + {#apple-sso} d’authentification SSO Apple
               + [Présentation de l’authentification unique (SSO) Apple](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Manuel de l’authentification unique Apple (API REST V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
         + {#platform-sso} de l’authentification SSO de Platform
            + {#amazon-sso} d’authentification SSO Amazon
               + [Manuel de l’authentification unique Amazon (API REST V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
            + {#roku-sso} d’authentification SSO Roku
               + [Présentation de l’authentification unique (SSO) Roku](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
      + {#hba-access} d’accès à l’authentification basée sur l’accueil
         + [Authentification à domicile pour TV Everywhere](integration-guide-programmers/features-standard/hba-access/home-based-authn-tve.md)
         + [Statut de l’adaptateur HBA pour les MVPD](integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)
      + {#privacy-support} d’assistance en matière de confidentialité
         + [Présentation de l’assistance en matière de confidentialité](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [Comment effectuer une demande d’accès à des informations personnelles](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + Fonctionnalités Premium {#features-premium}
      + {#temporary-access} d’accès temporaire
         + [Fonction TempPass](integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
      + {#degraded-access} d’accès dégradés
         + [Présentation de l’API de dégradation](integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md)
      + {#esm} ESM
         + [Présentation de la surveillance du service de droit](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [API de surveillance du service de droit](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [Mesures côté serveur](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + {#analytics} Analytics
         + [Intégration des données côté serveur de l’authentification Adobe Pass dans Adobe Analytics](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [Utilisation de l’ID Experience Cloud dans l’authentification Adobe Pass](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + {#legacy} hérité
      + {#rest-api-v1} de l’API REST V1 (héritée)
         + [Présentation de l’API REST V1 (héritée)](integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
         + [Référence API REST V1 (héritée)](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + {#rest-api-v1-apis} des API (héritées)
            + [Demande de code d’enregistrement (hérité)](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [Enregistrement d’enregistrement de retour (hérité)](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [(Hérité) Supprimer l’enregistrement d’enregistrement](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [(Hérité) Fournir Une Liste MVPD](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [(Hérité) Lancer l’authentification](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [(Hérité) Vérifier le jeton d’authentification](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [(Hérité) Récupérer Le Jeton D’Authentification](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [(Hérité) Lancer l’autorisation](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [(Hérité) Récupérer Le Jeton D’Autorisation](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [(Hérité) Obtention D’Un Jeton Multimédia Court](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [(Hérité) Vérifier le flux d’authentification par application web du deuxième écran](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [(Hérité) Récupération de la liste des ressources préautorisées](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [(Hérité) Récupérer la liste des ressources préautorisées par l’application web du deuxième écran](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [(Hérité) Lancer la déconnexion](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [Métadonnées utilisateur (héritées)](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [(Hérité) Récupérer la requête de profil](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [exchange du jeton (hérité)](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [(Hérité) Aperçu gratuit pour Temp Pass et Promotionnel Temp Pass](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + {#rest-api-v1-cookbooks} des livres de cookie (hérités)
            + [Cookbook de l’API REST V1 (hérité) (client à serveur)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [Guide pas à pas API REST V1 (hérité) (serveur à serveur)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + {#sdks} des SDK (hérités)
         + {#javascript-sdk} JavaScript SDK (hérité)
            + [Présentation de JavaScript SDK (hérité)](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [Manuel JavaScript SDK (hérité)](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [Référence de l’API SDK JavaScript (héritée)](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [Préautorisation de l’API JavaScript SDK (héritée)](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + Instructions (héritées) {#javascript-sdk-guidelines}
               + [(Hérité) Connexion et déconnexion sans actualisation](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + {#ios-tvos-sdk} SDK iOS/tvOS (hérité)
            + [Présentation d’iOS/tvOS SDK (hérité)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [Manuel SDK d’iOS/tvOS (hérité)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [Référence de l’API SDK iOS/tvOS (héritée)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [Préautorisation de l’API SDK iOS/tvOS (héritée)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + Instructions (héritées) {#ios-tvos-sdk-guidelines}
               + [Enregistrement de l’application iOS/tvOS (hérité)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [Guide de migration d’iOS/tvOS v3.x (hérité)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [Contrôles d’intégrité du stockage iOS/tvOS (hérités)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + {#android-sdk} Android SDK (hérité)
            + [Présentation d’Android SDK (hérité)](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [Manuel Android SDK (hérité)](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [Référence de l’API SDK Android (héritée)](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [Préautorisation de l’API Android SDK (héritée)](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + Instructions (héritées) {#android-sdk-guidelines}
               + [Enregistrement de l’application Android (hérité)](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [SDK Android avec enregistrement client dynamique (hérité)](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + {#fireos-sdk} FireOS SDK (hérité)
            + [Présentation technique d’Amazon FireOS (hérité)](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [Manuel d’intégration d’Amazon FireOS (hérité)](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [Référence de l’API Amazon FireOS (héritée)](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [Enregistrement de l’application Amazon FireOS (héritée)](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [SDK FireOS (hérité) avec enregistrement dynamique de client](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [SSO FireOS d’Amazon (hérité) - Guide de lancement du programmeur](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
      + {#client-information} d’informations sur le client (hérité)
         + [(Hérité) Transmission des informations client (appareil, connexion et application)](integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)
      + {#error-reporting} de rapport d’erreurs (hérité)
         + [Rapport d’erreurs (hérité)](integration-guide-programmers/legacy/error-reporting/error-reporting.md)
      + {#sso-access} d&#39;accès SSO (hérité)
         + [(Hérité) Prise en charge de l’authentification unique](integration-guide-programmers/legacy/sso-access/sso-support.md)
         + [Authentification unique (héritée) via l’authentification passive](integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)
         + [Manuel de l’authentification unique Amazon (hérité) (API REST V1)](integration-guide-programmers/legacy/sso-access/amazon-sso-cookbook-rest-api-v1.md)
         + [Manuel de l’authentification unique Apple (hérité) (API REST V1)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)
         + [Manuel de l’authentification unique Apple (hérité) (iOS/tvOS SDK)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md)
      + {#tve-dashboard} du tableau de bord TVE (hérité)
         + [Guide d’utilisation du tableau de bord TVE (hérité)](integration-guide-programmers/legacy/tve-dashboard/tve-dashboard-user-guide.md)
      + Notes techniques (héritées) {#tech-notes}
         + {#rest-api-v1} de l’API REST V1 (héritée)
            + [Implémentation de l’API sans client (héritée) - Codes d’erreur/messages avec raison/cause probable](integration-guide-programmers/legacy/notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
            + [Flux d’API sans client (hérité) en l’absence d’identifiant d’appareil](integration-guide-programmers/legacy/notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
            + [(Hérité) Sans client : évitez d’utiliser le code ’&amp;’reg_code dans la requête /authenticate](integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
            + [(Hérité) Activation des services de droits Adobe Pass pour un programmeur sur Xbox 360 et XboxOne sans client](integration-guide-programmers/legacy/notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
            + [(Hérité) Type d’appareil et mesures sans client](integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
         + {#sdks} des SDK (hérités)
            + [Questions/réponses sur les certificats (hérités)](integration-guide-programmers/legacy/notes-technical/certificates-qa.md)
            + [(Hérité) Comprendre les ID d’utilisateur](integration-guide-programmers/legacy/notes-technical/understanding-user-ids.md)
            + {#javascript-sdk} JavaScript SDK (hérité)
               + [Évaluation de la prévention du suivi (hérité) - Apple Safari](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-apple-safari.md)
               + [Évaluation de la prévention du suivi (hérité) - Google Chrome](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-google-chrome.md)
               + [Mises à jour des cookies (hérités) - Indicateurs SameSite et Secure](integration-guide-programmers/legacy/notes-technical/cookies-updates-samesite-and-secure-flags.md)
               + [Conseils de débogage (hérités)](integration-guide-programmers/legacy/notes-technical/appendix-b-debugging-tips.md)
            + {#android-sdk} Android SDK (hérité)
               + [(Hérité) Accéder à l’authentification unique (SSO) Android SDK Enabler sur les applications Android 10](integration-guide-programmers/legacy/notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
               + [(Hérité) Authentification Adobe Pass et nouveau modèle d’autorisations Android 6 « Marshmallow »](integration-guide-programmers/legacy/notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
            + {#ios-tvos-sdk} SDK iOS/tvOS (hérité)
               + [Prise en charge de WKWebView sur iOS SDK 3.1+ (héritée)](integration-guide-programmers/legacy/notes-technical/wkwebview-support-on-ios-sdk-31.md)
               + [Prise en charge de SFSafariViewController sur iOS SDK 3.2+ (héritée)](integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
               + [Authentification unique (héritée) sur iOS lors de l’utilisation d’Adobe Pass Authentication Access Enabler](integration-guide-programmers/legacy/notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
               + [Erreur d’authentification iOS (héritée) - adobepass.ios.app introuvable](integration-guide-programmers/legacy/notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
               + [(Hérité) Réinitialiser la passe temporaire sur iOS](integration-guide-programmers/legacy/notes-technical/reset-temp-pass-on-ios.md)
               + [(Hérité) Déboguer l’AccessEnabler iOS/tvOS SDK à l’aide des journaux d’application de la console](integration-guide-programmers/legacy/notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
               + [Chemin de mise à niveau AccessEnabler iOS/tvOS 3.7.0 (hérité)](integration-guide-programmers/legacy/notes-technical/accessenabler-iostvos-370-upgrade-path.md)
         + {#user-experience} de l’expérience utilisateur (héritée)
            + [(Hérité) Comment migrer la page de connexion MVPD d’iFrame vers une fenêtre contextuelle](integration-guide-programmers/legacy/notes-technical/migr-mvpd-login-iframe-popup.md)
            + [Fonction de contrôle en amont (héritée) : comment activer, résoudre les problèmes ou déterminer le problème](integration-guide-programmers/legacy/notes-technical/preflight-feature.md)
            + [(Hérité) Autoriser les fichiers MVPD dans la boîte de dialogue de sélection](integration-guide-programmers/legacy/notes-technical/allow-mvpd-selectn-dialog.md)
            + [(Hérité) Empêcher l’affichage des fichiers MVPD dans la boîte de dialogue de sélection](integration-guide-programmers/legacy/notes-technical/prevent-mvpd-selectn-dialog.md)
         + {#troubleshooting} de dépannage (hérité)
            + [(Hérité) Utilisation du proxy Charles](integration-guide-programmers/legacy/notes-technical/using-charles-proxy.md)
            + [(Hérité) Surveillance du Pass PayTV Adobe Pass Adobe](integration-guide-programmers/legacy/notes-technical/monitoring-adobe-pay-tv-pass.md)
            + [(Hérité) Comment tester les flux d’authentification et d’autorisation à l’aide du site de test de l’API Adobe](integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + [Présentation du guide d’intégration du programmeur](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [Flux de droits du programmeur](integration-guide-programmers/entitlement-flow.md)
   + [Cas d’utilisation du programmeur](integration-guide-programmers/programmer-use-cases.md)
   + [Mécanisme de limitation](integration-guide-programmers/throttling-mechanism.md)
   + [Configuration minimale requise](integration-guide-programmers/minimum-system-requirements.md)
+ Guide d’intégration pour MVPDs {#integration-guide-mvpds}
   + [Fonctionnalités d’intégration](integration-guide-mvpds/mvpd-integr-features.md)
   + [Authentification](integration-guide-mvpds/authn-usecase.md)
   + [Authentification utilisant le protocole OAuth 2.0](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [Autorisation](integration-guide-mvpds/authz-usecase.md)
   + [Autorisation de contrôle en amont](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [Déconnexion de MVPD](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [Exchange des métadonnées de contenu](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [Exchange des métadonnées de l’utilisateur](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [Service web de MVPD du proxy](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [Intégration de MVPD SAML par proxy](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [Définition de la portée du fournisseur de services](integration-guide-mvpds/serv-provider-scoping.md)
   + [Adresses IP autorisées MVPD](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ Guide de l&#39;utilisateur pour TVE Dashboard {#user-guide-tve-dashboard}
   + [Présentation du tableau de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [Environnements](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [Vérifier et transmettre les modifications](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Tableau de bord](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [Canaux](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [Programmeurs](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [Intégrations](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [Rapports](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [Log des modifications](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
+ Notes techniques {#tech-notes}
   + {#environments} Environnements
      + [Présentation des environnements d’Adobe](notes-technical/environments/understanding-the-adobe-environments.md)
      + [Configuration de votre environnement et tests dans Pre-Qual](notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)
