---
title: Présentation des API REST API V2
description: Présentation des API REST API V2
exl-id: 62b48bf0-d200-4949-b268-8f8ea2daabfa
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 297578d3b311fbc2a5702f1bcc947e94463fb884
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%
---
# Présentation des API REST API V2 {#rest-api-v2-apis-overview}

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

Pour commencer, consultez la documentation publique des API qui vous intéressent dans la liste ci-dessous.

## URL de base {#rest-api-v2-apis-base-urls}

Utilisez l’URL de base spécifique à un environnement ci-dessous pour les requêtes API REST V2. Ajoutez les `path` relatives documentées pour chaque API à cette URL de base.

Certaines réponses d’API renvoient des URL interactives que l’appareil de diffusion en continu doit ouvrir dans un navigateur ou un agent utilisateur. Par exemple, l’attribut `url` renvoyé par [Lancer la déconnexion pour un fichier mvpd spécifique](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) et la redirection `Location` renvoyée par [Effectuer l’authentification dans l’agent utilisateur](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

Ces URL interactives utilisent le même hôte de fournisseur de services (SP) Adobe Pass. Ouvrez une URL interactive absolue telle qu’elle est renvoyée. Si Adobe Pass renvoie une URL interactive relative commençant par `/adobe-services/`, résolvez-la sur l’hôte SP de l’environnement cible :

| Environnement | API REST V2 et URL de base interactive |
| --- |--------------------------------------|
| Production | `https://sp.auth.adobe.com` |
| Évaluation | `https://sp.auth-staging.adobe.com` |

## Configuration {#rest-api-v2-apis-configuration}

* [Récupération de la configuration pour un fournisseur de services spécifique](configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

## Sessions {#rest-api-v2-apis-sessions}

* [Créer une session d’authentification](sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Reprendre la session d’authentification](sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Récupérer la session d’authentification](sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Authentification dans l’agent utilisateur](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)

## Profils {#rest-api-v2-apis-profiles}

* [Récupération des profils](profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Récupération du profil pour un fichier mvpd spécifique](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Récupération du profil pour un code spécifique](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

## Décisions {#rest-api-v2-apis-decisions}

* [Récupérer des décisions d’autorisation à l’aide de mvpd spécifiques](decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Récupérer des décisions de préautorisation à l’aide de mvpd spécifiques](decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

## Déconnexion {#rest-api-v2-apis-logout}

* [Lancer la déconnexion pour un fichier mvpd spécifique](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

## Authentification SSO Du Partenaire {#rest-api-v2-apis-partner-single-sign-on}

* [Récupérer la demande d’authentification du partenaire](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [Créer et récupérer un profil à l’aide de la réponse d’authentification du partenaire](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

## Vous souhaitez essayer l’API REST V2 ?

Vous pouvez désormais explorer l’API REST V2 via notre page dédiée aux produits sur le site web [](https://developer.adobe.com/adobe-pass/).
