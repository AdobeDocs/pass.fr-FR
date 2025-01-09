---
title: Guide pas à pas API REST V2 (serveur à serveur)
description: Guide pas à pas API REST V2 (serveur à serveur)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1578'
ht-degree: 0%

---

# Guide pas à pas API REST V2 (serveur à serveur) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> L’implémentation de l’API REST V2 est limitée par la documentation [Mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

L’objectif de ce guide pas à pas est de détailler les bonnes pratiques pour l’implémentation de l’authentification Adobe Pass à l’aide de l’API REST V2 dans une architecture de serveur à serveur. Il fournit des exigences de base, une implémentation étape par étape et des considérations générales pour les environnements de production et l’exploitation.

## Composants {#components}

Dans une solution de serveur à serveur opérationnelle, les composants suivants sont impliqués :

| Type | Composant | Description |
|---------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Appareil De Diffusion En Continu | Application de streaming | L’application Programmeur qui réside sur l’appareil de diffusion en continu de l’utilisateur et lit une vidéo authentifiée. |
|                           | \[Facultatif\] Module AuthN | Si l’appareil de diffusion en continu dispose d’un agent utilisateur (c’est-à-dire un navigateur web), le module AuthN est chargé d’authentifier l’utilisateur sur l’IdP MVPD. |
| \[Facultatif\] Périphérique AuthN | Application AuthN | Si l’appareil de diffusion en continu ne dispose pas d’un agent utilisateur (c’est-à-dire un navigateur web), l’application AuthN est une application web de programmation accessible à partir de l’appareil d’un utilisateur distinct à l’aide d’un navigateur web. |
| Infrastructure du programmeur | Service de programmation | Service qui relie l’appareil de diffusion en continu au service Adobe Pass afin d’obtenir des décisions d’authentification et d’autorisation. |
| Infrastructure d&#39;Adobe | Service Adobe Pass | Un service qui s’intègre aux services MVPD IdP et AuthZ et fournit des décisions d’authentification et d’autorisation. |
| Infrastructure MVPD | IdP MVPD | Point d’entrée MVPD qui fournit un service d’authentification basé sur les informations d’identification pour valider l’identité de l’utilisateur. |
|                           | Service MVPD AuthZ | Point d’entrée MVPD qui fournit des décisions d’autorisation basées sur les abonnements des utilisateurs, le contrôle parental, etc. |

D’autres termes utilisés dans le flux sont définis dans le [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md).

Le diagramme suivant illustre l’ensemble du flux :

![Guide pas à pas API REST V2 (serveur à serveur)](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

### Procédure d’implémentation de l’API REST V2 dans une architecture serveur à serveur {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Pour mettre en œuvre l’API REST Adobe Pass V2, vous devez suivre les étapes ci-dessous, regroupées en phases.

## 0. Prérequis {#prerequisites}

Dans les implémentations de serveur à serveur, le service de programmation et d’application de diffusion en continu doit établir un protocole pour que le service de programmation puisse :

* Identifiez de manière unique l’application de diffusion en continu sur l’appareil.
* Agissez au nom de l’application de diffusion en continu et communiquez avec le service Adobe Pass.
* Collectez et stockez des informations sur l’application en flux continu et l’appareil, telles que l’adresse IP, le port source et les informations sur l’appareil, afin de les transmettre à Adobe Pass.
* Renvoyer les décisions et les instructions à l’application de diffusion en continu.

Les paramètres tels que l’ID d’appareil, l’ID client, le secret client (défini ci-dessous) peuvent être stockés sur l’application de diffusion en continu ou le service de programmation.

## A. Phase d&#39;enregistrement {#registration-phase}

### Étape 1 : enregistrer votre application {#step-1-register-your-application}

Pour que l’application puisse appeler l’API REST Adobe Pass V2, elle a besoin d’un jeton d’accès requis par la couche de sécurité de l’API.

Pour les implémentations serveur à serveur, le service de programmation peut s’enregistrer au nom d’une instance d’application, mais les valeurs des informations d’identification du client (identifiant client et secret client) doivent être obtenues pour chaque appareil de diffusion en continu.

Pour obtenir le jeton d’accès, le service de programmation peut agir au nom d’une application en flux continu et doit suivre les étapes décrites dans la documentation [Enregistrement dynamique de client](../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

## B. Phase d’authentification {#authentication-phase}

### Étape 2 : vérifier les profils authentifiés existants {#step-2-check-for-existing-authenticated-profiles}

Le service de programmation recherche les profils authentifiés existants `/api/v2/{serviceProvider}/profiles` pour le compte de l’application de diffusion en continu ([Récupérer les profils authentifiés](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)).

* Si aucun profil n’a été trouvé et que l’application de diffusion en continu implémente un flux TempPass
   * Consultez la documentation sur la mise en œuvre des [flux d’accès temporaires](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Si aucun profil trouvé, l’application de diffusion en continu implémente un flux d’authentification
   * <b>Étape 2.a :</b> le service de programmation récupère la liste des MVPD disponibles pour serviceProvider : <b>/api/v2/{serviceProvider}/configuration</b><br>
([Récupérer la liste des fichiers MVPD disponibles](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Le service de programmation peut mettre en œuvre un filtrage sur la liste des MVPD et afficher uniquement les MVPD destinés à en masquer d&#39;autres (TempPass, MVPD de test, MVPD en cours de développement, etc.)
   * Le service de programmation doit renvoyer une liste MVPD filtrée pour que l’application de diffusion en continu affiche le sélecteur . L’utilisateur sélectionne le MVPD.
   * Une fois le MVPD sélectionné dans l’application de diffusion en continu, le service de programmation crée une session : <b>/api/v2/{serviceProvider}/sessions</b><br>
([Créer une session d’authentification](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * Un CODE et une URL à utiliser pour l’authentification sont renvoyés
      * Si un profil est trouvé, le service de programmation peut passer à l’étape <a href="#preauthorization-phase">C. Phase de préautorisation </a>
   * Le service de programmation doit renvoyer le CODE et l’URL à l’application de diffusion en continu

### Étape 3 : Authentifier l’utilisateur {#step-3-authenticate-the-user}

Utilisation d’un navigateur ou d’une application web de deuxième écran :

* Option 1. L’application de diffusion en continu peut ouvrir un navigateur ou une vue web, charger l’URL pour l’authentification et l’utilisateur accède à la page de connexion de MVPD où les informations d’identification doivent être envoyées
   * L’utilisateur saisit le nom d’utilisateur/mot de passe, la redirection finale affiche une page de réussite
* Option 2. L’application de streaming ne peut pas ouvrir un navigateur et affiche simplement le CODE. <b>Une application web distincte, AuthN_APP, doit être développée</b> pour demander à l’utilisateur de saisir du CODE, de créer et d’ouvrir l’URL : <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * L’utilisateur saisit le nom d’utilisateur/mot de passe, la redirection finale affiche une page de réussite

### Étape 4 : rechercher des profils authentifiés {#step-4-check-for-authenticated-profiles}

Le service de programmation vérifie que l’authentification auprès de MVPD doit se terminer dans le navigateur ou le deuxième écran

* Une interrogation toutes les 15 secondes est recommandée le <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Récupérer des profils authentifiés pour des MVPD spécifiques](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Si la sélection MVPD n’est pas effectuée dans l’application de diffusion en continu, car le sélecteur MVPD est présenté dans l’application du deuxième écran, l’interrogation doit se produire avec CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Récupérer des profils authentifiés pour un CODE spécifique](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* L’interrogation ne doit pas dépasser 30 minutes. Si 30 minutes sont atteintes et que l’application de diffusion en continu est toujours active, une nouvelle session doit être lancée et un nouveau CODE et une nouvelle URL seront renvoyés
* Une fois l’authentification terminée, la valeur renvoyée est 200 avec le profil authentifié
* Le service de programmation peut passer à l&#39;étape <a href="#preauthorization-phase">C. Phase de préautorisation </a>

## C. Phase de préautorisation {#preauthorization-phase}

### Étape 5 : rechercher des ressources préautorisées {#step-5-check-for-preauthorized-resources}

Avec un profil d’authentification valide pour un utilisateur, le service de programmation a la possibilité de vérifier l’accès aux vidéos disponibles et de transmettre la liste à l’application de diffusion en continu pour affichage.

* L’étape est facultative et exécutée si l’application souhaite filtrer les ressources non disponibles dans le package d’utilisateurs authentifiés
* Appel à <b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br>
([Récupérer la décision de préautorisation à l’aide d’un MVPD spécifique](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. Phase d’autorisation {#authorization-phase}

### Étape 6 : vérifier les ressources autorisées {#step-6-check-for-authorized-resources}

L’application de diffusion en continu se prépare à lire une vidéo/ressource/ressource sélectionnée par l’utilisateur.

* L’étape est nécessaire pour chaque début de lecture
* L’application de diffusion en continu transmet ces informations au service de programmation
* Le service de programmation, au nom de l’application de diffusion en continu, appelle <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
([Récupérer une décision d’autorisation à l’aide d’un MVPD spécifique](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * Décision = « Autoriser ». Le service de programmation demande à l’application de diffusion de démarrer la diffusion en continu.
   * Décision = « Refuser », le service de programmation demande à l’application de diffusion en continu d’informer l’utilisateur qu’elle n’a pas accès à cette vidéo
   * dans le processus, le service de programmation peut évaluer d’autres règles métier et renvoyer la décision appropriée à l’application de diffusion en continu

## E. Phase de déconnexion {#logout-phase}

### Étape 7 : Déconnexion {#step-7-logout}

Application en flux continu : l’utilisateur souhaite se déconnecter du MVPD

* L’application en flux continu informe le service de programmation qu’il doit se déconnecter du MVPD pour cette application spécifique.
* Le service de programmation peut nettoyer les informations stockées sur l’utilisateur authentifié
* Appel du service de programmation <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Déconnexion d’un MVPD spécifique](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Si la réponse actionType=&#39;interactive&#39; et l’URL sont présentes, le service de programmation renvoie l’URL à l’application de diffusion en continu
* Selon les fonctionnalités existantes, l’application de diffusion en continu peut ouvrir l’URL dans le navigateur (généralement la même que celle utilisée pour l’authentification)
* Si l’application de diffusion en continu n’a pas de navigateur ou s’il s’agit d’une instance différente de celle de l’authentification, le flux peut être arrêté, car la session MVPD n’a pas été conservée dans le cache du navigateur.

## Environnements et exigences fonctionnelles{#environments}

Un programmeur doit créer au moins deux environnements : un pour la production et un ou plusieurs pour l’évaluation.

### Production

L’environnement de production doit être hautement disponible et dimensionné de manière appropriée en cas de pics importants ou inattendus (par exemple, sports en direct, actualités).

Le service Adobe Pass s’exécute sur plusieurs centres de données géographiquement dispersés dans l’ensemble des États-Unis. Pour obtenir le meilleur temps de réponse (c’est-à-dire la latence la plus faible) à partir du service Adobe Pass, le programmeur doit également créer une infrastructure de service géographiquement dispersée similaire.

Le service de programmation doit limiter le cache DNS à un maximum de 30 s au cas où l’Adobe devrait réacheminer le trafic. Cela peut se produire si un centre de données n’est plus disponible.

Le programmeur doit fournir la plage d’adresses IP publique de l’environnement de production. Ils seront entrés dans une liste autorisée d’adresses IP dans l’infrastructure Adobe Pass pour l’accès et gérés par les politiques d’utilisation équitable des API d’Adobe.

### Évaluation

L’environnement d’évaluation peut être minimal, mais il doit inclure tous les composants système et la logique commerciale. Il doit fonctionner de la même manière que la production et permettre de tester les versions en dehors de la production. Idéalement, l’environnement d’évaluation peut être connecté aux environnements de test Adobe Pass pour être utilisé par le programmeur et par Adobe si nécessaire, afin que nous puissions vous aider à effectuer les tests et à résoudre les problèmes.

### Exigences fonctionnelles

Le service de programmation doit transmettre des informations d’identification d’appareil précises pour l’appareil pour lequel il exécute les flux. En outre, le service Programmer doit transmettre l’adresse IP de l’appareil pour lequel il exécute les flux (dans un en-tête x-forwarded-for) avec le port source de connexion (dans le champ informations sur l’appareil ) :

Le service de programmation doit envoyer les données et le formatage requis par les MVPD individuels ou les applications intégrées (par exemple, l’adresse IP de l’appareil, le port source, les informations sur l’appareil, le MRSS, les données facultatives telles que l’ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.

Le service de programmation doit respecter le profil d’authentification et la validité des décisions lors de la mise en cache et invalider les authentifications ou les décisions lorsqu’elles sont notifiées.

Le service de programmation doit gérer les certificats partagés avec l’Adobe (pour les métadonnées utilisateur chiffrées).

## Informations connexes {#related}

* [Référence de l’API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
