---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Authentification Adobe Pass
user-guide-description: L’authentification Adobe Pass est une solution de droits pour TV Everywhere, qui fournit une structure modulaire afin de déterminer si une personne qui demande l’accès à une ressource y a droit.
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '915'
ht-degree: 3%

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
   + API REST {#restapi}
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
         + [Échange de jetons](token-exchange.md)
         + [Aperçu gratuit pour la transmission temporaire et la transmission temporaire promotionnelle](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + SDK AccessEnabler {#accessenabler-sdk}
      + SDK JavaScript {#javascriptsdk}
         + [Présentation du SDK JavaScript](javascript-sdk-overview.md)
         + [Guide pas à pas du SDK JavaScript](javascript-sdk-cookbook.md)
         + [Référence de l’API du SDK JavaScript](javascript-sdk-api-reference.md)
         + Instructions {#js-sdk-guidelines}
            + [Connexion et déconnexion sans actualisation](refreshless-login-and-logout.md)
         + API JavaScript {#js-api}
            + [Préautoriser](js-preauthorize.md)
      + SDK iOS/tvOS {#ios-sdk}
         + [Présentation du SDK iOS/tvOS](iostvos-sdk-overview.md)
         + [Guide pas à pas du SDK iOS/tvOS](iostvos-sdk-cookbook.md)
         + [Référence de l’API du SDK iOS/tvOS](iostvos-sdk-api-reference.md)
         + Instructions {#ios-tvos-sdk-guidelines}
            + [Enregistrement de l’application iOS/tvOS](iostvos-application-registration.md)
            + Instructions de migration {#migration-guidelines}
               + [Guide de migration d’iOS/tvOS v3.x](iostvos-v3x-migration-guide.md)
            + [Vérifications de l’intégrité du stockage iOS/tvOS](iostvos-sdk-storage-integrity-checks.md)
         + API iOS/tvOS {#ios-tvos-api}
            + [Préautoriser](preauthorize.md)
      + SDK Android {#androidsdk}
         + [Présentation du SDK Android](android-sdk-overview.md)
         + [Guide pas à pas du SDK Android](android-sdk-cookbook.md)
         + [Référence de l’API du SDK Android](android-sdk-api-reference.md)
         + Instructions {#androidguidelines}
            + [Enregistrement d’applications Android](android-application-registration.md)
            + [SDK Android avec enregistrement du client dynamique](android-sdk-with-dynamic-client-registration.md)
         + API Android{#androidapi}
            + [Préautoriser](preauthorize-android.md)
      + SDK Amazon FireOS {#fireossdk}
         + [SSO Amazon FireOS - Guide de démarrage du programmeur](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [Amazon FireOS SSO à l’aide du guide pas à pas API client](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Présentation technique d’Amazon FireOS](amazon-fireos-technical-overview.md)
         + [Guide d’intégration Amazon FireOS](amazon-fireos-integration-cookbook.md)
         + [Référence de l’API Amazon FireOS](amazon-fireos-native-client-api-reference.md)
         + [Enregistrement de l’application Amazon FireOS](amazon-fireos-application-registration.md)
         + [SDK FireOS avec enregistrement de client dynamique](fireos-sdk-with-dynamic-client-registration.md)
   + SSO de la plateforme {#platform-sso}
      + APPLE SSO {#apple-sso}
         + [Présentation d’Apple SSO](apple-sso-overview.md)
         + [Guide pas à pas Apple SSO (API REST)](apple-sso-cookbook-rest-api.md)
         + [Guide pas à pas Apple SSO (SDK iOS/tvOS)](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
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
   + [Échange de métadonnées de contenu](mvpd-content-metadata-exchange.md)
   + [Échange de métadonnées utilisateur](mvpd-user-metadata-exchng.md)
   + [Service Web MVPD de proxy](proxy-mvpd-webserv.md)
   + [Intégration SAML du proxy MVPD](proxy-mvpd-saml-int.md)
   + [Portée du fournisseur de services](serv-provider-scoping.md)
   + [Adresses IP autorisées MVPD](mvpd-listing-ip-addres.md)
+ Fonctionnalités d’authentification Adobe Pass {#auth-features}
   + Intégration d’Adobe Analytics {#analytics-int}
      + [Intégration des données côté serveur d’authentification Adobe Pass dans Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [Utilisation de l’ID Experience Cloud dans l’authentification Adobe Pass](exp-cloud-id-authn.md)
   + Surveillance du service de droits {#entitlement-service-monitoring}
      + [Présentation de la surveillance du service de droits](entitlement-service-monitoring-overview.md)
      + [API de surveillance du service de droits](entitlement-service-monitoring-api.md)
      + [Mesures côté serveur](understanding-serverside-metrics.md)
   + Temp pass {#temp-pass}
      + [Temp pass](temp-pass.md)
      + [Passe temporaire promotionnelle](promotional-temp-pass.md)
      + [Réinitialiser le transfert temporaire](reset-temp-pass.md)
   + Authentification unique {#sso}
      + [Prise en charge de la connexion unique](sso-support.md)
      + [SSO via authentification passive](sso-passive-authn.md)
   + Authentification par domicile {#home-based-auth}
      + [Authentification basée sur la maison pour TV partout](home-based-authn-tve.md)
      + [État de l’adaptateur de bus hôte pour les MVPD](hba-status-mvpds.md)
   + Métadonnées utilisateur {#user-metadat}
      + [Métadonnées utilisateur](user-metadata-feature.md)
   + [Autorisation de contrôle en amont](preflight-authz.md)
   + Rapport d’erreurs {#error-reportn}
      + [Rapport d’erreurs](error-reporting.md)
      + [Amélioration des codes d’erreur](enhanced-error-codes.md)
   + Enregistrement du client {#client-regn}
      + [Enregistrement du client dynamique](dynamic-client-registration.md)
      + [API d’enregistrement de client dynamique](dynamic-client-registration-api.md)
      + [Gestion de l’enregistrement du client dynamique](dynamic-client-registration-management.md)
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
   + Notes de mise à jour du SDK JavaScript  {#release-notes-javascript}
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.7.0](authn-rn-javascript-470.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.6.0](authn-rn-javascript-460.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.4.0](authn-rn-javascript-440.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.2.0](authn-rn-javascript-420.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.1.1](authn-rn-javascript-411.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.1.0](authn-rn-javascript-410.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 4.0.0](authn-rn-javascript-400.md)
      + [Notes de mise à jour d’Adobe Pass Authentication JavaScript 3.5.0](authn-rn-javascript-350.md)
   + Notes de mise à jour du SDK iOS/tvOS  {#release-notes-ios}
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
         + [Limites du SDK JS pour le navigateur Safari](js-sdk-limitations-for-safari-browser.md)
         + [Mises à jour des cookies : indicateurs samesite et sécurisé](cookies-updates-samesite-and-secure-flags.md)
      + SDK Android {#android}
         + [Accéder à l’authentification unique (SSO) du SDK Android sur les applications Android 10](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Authentification Adobe Pass et nouveau modèle d’autorisations Android 6 &quot;Marshmallow&quot;](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + SDK iOS/tvOS {#iostvos}
         + [Prise en charge de WKWebView sur le SDK iOS 3.1+](wkwebview-support-on-ios-sdk-31.md)
         + [Prise en charge de SFSafariViewController sur le SDK iOS 3.2+](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO sur iOS lors de l’utilisation de l’activateur d’accès à l’authentification Adobe Pass](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [Erreur d’authentification iOS - Impossible de trouver adobepass.ios.app](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Réinitialiser le transfert temporaire sur iOS](reset-temp-pass-on-ios.md)
         + [Débogage du SDK AccessEnabler iOS/tvOS à l’aide des journaux d’application de la console](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [Chemin de mise à niveau d’AccessEnabler iOS/tvOS 3.7.0](accessenabler-iostvos-370-upgrade-path.md)
   + Transmission d’environnements d’authentification {#primetime-authentication-environments}
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
+ [Guide d’utilisation du tableau de bord TVE](tve-dashboard-user-guide.md)
+ [Glossaire](glossary.md)