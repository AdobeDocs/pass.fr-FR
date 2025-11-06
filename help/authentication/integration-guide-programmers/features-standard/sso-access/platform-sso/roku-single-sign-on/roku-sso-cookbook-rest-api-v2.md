---
title: Guide pas à pas de Roku SSO (API REST V2)
description: Guide pas à pas de Roku SSO (API REST V2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Guide pas à pas de Roku SSO (API REST V2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

L’API REST d’authentification Adobe Pass V2 prend en charge l’authentification unique (SSO) de Platform pour les utilisateurs finaux des applications clientes s’exécutant sur RokuOS.

Ce document agit comme une extension de l’aperçu [API REST V2)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) existant qui fournit une vue d’ensemble et le document qui décrit comment implémenter [authentification unique à l’aide des flux d’identité de plateforme](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Authentification unique Roku à l’aide des flux d’identité de plateforme {#cookbook}

L’authentification Adobe Pass collabore avec Roku pour améliorer l’expérience de connexion des utilisateurs et faciliter l’authentification unique (SSO) dans les applications TV Everywhere pour les abonnés aux chaînes de télévision.

### Conditions préalables {#prerequisites}

Avant de poursuivre l’authentification unique Roku à l’aide des flux d’identité de plateforme, assurez-vous que l’authentification unique Roku est activée. L’authentification unique Roku est activée par défaut, sauf si le programmeur ou MVPD demande que l’authentification unique soit désactivée.

Chaque programmeur peut activer ou désactiver l’authentification unique (SSO) sur la plateforme Roku pour des intégrations spécifiques via le tableau de bord [Adobe Pass TVE](https://experience.adobe.com/pass/authentication).

### Workflow {#workflow}

**Client à serveur**

Pour les applications de programmation utilisant une architecture client à serveur pour intégrer l&#39;API REST V2, Roku SSO fonctionne de manière transparente sans aucune modification.

RokuOS ajoute automatiquement deux en-têtes HTTP à toutes les requêtes envoyées aux points d’entrée de l’authentification Adobe Pass.

**Serveur à serveur**

Pour les applications de programmation utilisant une architecture de serveur à serveur pour intégrer l’API REST V2, le programmeur doit se coordonner avec l’équipe Roku pour configurer ces en-têtes afin de les inclure dans tous les flux d’API dirigés vers leur domaine.

Pour activer la connexion unique entre applications et entre appareils, l’ID d’abonné fourni par Roku doit être utilisé à la place de l’ID d’appareil lorsqu’il est transmis par l’application.

Pour plus d’informations, consultez la documentation suivante :

* [En-Tête : X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [En-tête - Identifiant-appareil-AP](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

Pour plus d’informations sur le format des en-têtes nécessaires, contactez votre représentant ou représentante Adobe.

### Questions fréquentes {#faqs}

* **Comment le SSO fonctionnera-t-il ?**

  La connexion unique fonctionne sur toutes les applications de programmation optimisées par l’authentification Adobe Pass sur tous les appareils Roku associés au même utilisateur Roku. Tous les MVPD n&#39;autorisent pas Roku SSO.


* **Les TTL d’authentification seront-elles modifiées ?**

  Le premier jeton d’authentification valide sera utilisé pour effectuer la connexion unique et, dans ce cas, toutes les autres applications qui seront authentifiées par le biais de la connexion unique utiliseront le même TTL jusqu’à son expiration. Ainsi, lorsque vous naviguez d’une application à une autre, la deuxième application partage la durée de vie de la première application qui s’authentifie.


* **D’autres fonctionnalités d’Adobe fonctionneront-elles comme auparavant ?**

  Toutes les fonctionnalités d’authentification d’Adobe Pass fonctionneront comme auparavant.


* **Existe-t-il un processus d’opt-in/opt-out du programmeur bénéficiant de l’authentification unique sur la plateforme Roku**

  Il s’agira d’une modification de configuration dans le tableau de bord TVE d’Adobe. Chaque programmeur peut activer ou désactiver la connexion unique sur la plateforme Roku pour des intégrations spécifiques.


* **Quels sont les problèmes courants ?**

  Les programmeurs doivent vérifier que leurs implémentations actuelles basées sur l’API REST Adobe n’entravent pas l’authentification unique de la plateforme Roku.

  Vous trouverez ci-dessous une liste des problèmes possibles et la manière dont ils doivent être résolus.

| Problème | Cause possible | Solutions possibles |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Aucun en-tête SSO Roku envoyé à Adobe | Utilisation de HTTP au lieu de HTTPS pour les appels aux domaines d’authentification Adobe Pass | Utiliser HTTPS |
| Logo MVPD non affiché/non mis à jour pour les jetons SSO | L’interface utilisateur repose sur le stockage local. | Les applications doivent mettre à jour l’interface utilisateur (et le stockage local, si nécessaire) après avoir vérifié l’authentification |
| Déconnexion déclenchée sur aucune authentification | Conception de l&#39;application | L’application doit être mise à jour pour ne jamais se déconnecter en arrière-plan |
