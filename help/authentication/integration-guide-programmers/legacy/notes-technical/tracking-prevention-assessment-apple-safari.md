---
title: Évaluation de la prévention du suivi dans Apple Safari
description: Évaluation de la prévention du suivi dans Apple Safari
exl-id: a3362020-92ff-4232-b923-e462868730d5
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '1849'
ht-degree: 0%

---

# Évaluation de la prévention du suivi (hérité) - Apple Safari {#tracking-prevention-assessment-apple-safari}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Safari 10 {#safari10}

**Détails**

À partir de Safari 10, les paramètres de confidentialité par défaut du navigateur entraînent l’arrêt des fonctionnalités d’authentification unique (SSO), de déconnexion unique (SLO) et d’authentification passive. L’authentification unique (SSO) et l’authentification passive ne fonctionneront pas, même dans
la même session entre plusieurs onglets ou fenêtres de navigateur.

Ces modifications affectent et ont un impact sur les processus d’authentification Adobe Pass
pour les versions suivantes du SDK JavaScript AccessEnabler : v2 (versions 2.x), v3 (versions 3.x), v4 (versions 4.x).

### Atténuation {#mitigation-safari10}

Pour atténuer ces limitations, vous pouvez demander à l’utilisateur de modifier les paramètres de confidentialité du navigateur Safari 10 et d’utiliser l’option « **Toujours autoriser** » pour l’entrée « **Cookies et données de site web** » dans l’onglet Confidentialité du navigateur à partir de Préférences, comme illustré dans l’image ci-dessous.

![](../../../assets/always-allow-safari10.png)


## Safari 11 {#safari11}

**Détails**

>[!IMPORTANT]
>
>Tous les détails ci-dessus de la section Safari 10 s’appliquent toujours dans le cas de Safari 11.

À partir de Safari 11, le navigateur introduit le mécanisme [ITP (Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/)), une technologie qui utilise l’heuristique pour empêcher le suivi sur plusieurs sites. Ces heuristiques affectent la manière dont les cookies tiers sont stockés et relus sur les appels réseau, ce qui signifie que selon l’activation du mécanisme ITP, le navigateur Safari bloquera les cookies tiers dans la communication client - modèle de serveur.

Le service d’authentification d’Adobe Pass utilise des cookies dans le cadre du processus d’authentification **pour fonctionner**. Dans les cas où le processus d’authentification se produit automatiquement (par exemple, une passe temporaire) ou dans les implémentations qui utilisent des fonctionnalités iFrames ou « sans actualisation », les cookies Adobe sont considérés comme des cookies tiers et bloqués par défaut. Dans tous les autres cas, Safari utilise un algorithme de machine learning qui peut marquer tous les cookies du service d’authentification Pass d’Adobe comme cookies de suivi, étant ainsi soumis au blocage d’ITP.

En conclusion, un utilisateur du navigateur Safari 11 peut ne pas être en mesure de s’authentifier sur un site web activé pour l’authentification Adobe Pass après l’activation du mécanisme ITP (Intelligent Tracking Prevention), en particulier lorsqu’il utilise plusieurs sites web activés pour l’authentification Adobe Pass. Par conséquent, l’expérience d’authentification de l’utilisateur peut être inattendue et non définie, allant de l’impossibilité de se connecter à une durée d’authentification plus courte que prévue.

Ces modifications affectent et ont un impact sur les processus d’authentification Adobe Pass pour les versions suivantes du SDK JavaScript AccessEnabler : v2 (versions 2.x), v3 (versions 3.x).

### Atténuation {#mitigation-safari11}

Pour AccessEnabler JavaScript SDK v3 (versions 3.x) et AccessEnabler JavaScript SDK v4 (versions 4.x), la bibliothèque contient un mécanisme capable d’identifier les situations où l’authentification de l’utilisateur a été bloquée en raison de l’absence des cookies requis. Dans ces situations, la bibliothèque déclenche un rappel d’erreur spécifique [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference), qui est renvoyé au site web activé pour l’authentification Adobe Pass afin d’être utilisé comme signal pour demander à l’utilisateur de prendre des actions qui peuvent atténuer le problème. Pour bénéficier de ce mécanisme, le site web doit mettre en œuvre la spécification [Rapport d’erreur](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md).

Pour AccessEnabler JavaScript SDK v2 (versions 2.x), la bibliothèque ne propose pas le mécanisme décrit ci-dessus. Par conséquent, le site web activé pour l’authentification Adobe Pass ne peut pas être signalé lorsqu’il faut demander à l’utilisateur de prendre des mesures pour atténuer le problème.

La liste des actions qui peuvent atténuer les problèmes susmentionnés **s’applique aux trois versions** d’AccessEnabler JavaScript SDK.

Lorsque le site web de l’implémentateur reçoit un rappel d’erreur [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference), l’utilisateur doit être invité à désactiver la prévention intelligente du suivi (ITP) et à activer les cookies tiers en :

* Dans le cas de Mac OS X High Sierra et versions ultérieures : désélection de l’option « **Empêcher le suivi sur plusieurs sites** » pour l’entrée « **Suivi de sites Web** » dans l’onglet Confidentialité du navigateur à partir des Préférences, comme illustré dans l’image ci-dessous.

  ![](../../../assets/uncheck-prvnt-cr-st-tr-safari11.png)


* Dans le cas de Mac OS X Sierra et versions précédentes : Vérification de l’option « **Toujours autoriser** » pour l’entrée « **Cookies et données de site web** » dans l’onglet Confidentialité du navigateur à partir des Préférences, comme illustré dans l’image ci-dessous.

  ![](../../../assets/always-allow-safari11.png)

## Safari 12 {#safari12}

**Détails**

>[!IMPORTANT]
>
>Tous les détails ci-dessus de la section Safari 10 et de la section Safari 11 s’appliquent toujours dans le cas de Safari 12.

Cette section décrit les problèmes de compatibilité de **AccessEnabler JavaScript SDK versions 4.x** sur Safari 12.

>[!NOTE]
>
>Gardez à l’esprit que dans le cas des versions 2.x et 3.x d’AccessEnabler JavaScript SDK JavaScript SDK, les deux utilisent des cookies tiers pour les processus d’authentification. En raison des politiques ITP et de cookies tiers commençant par Safari 11, l’expérience d’authentification de l’utilisateur peut être inattendue et non définie, allant de l’impossibilité de se connecter à une durée d’authentification plus courte que prévu.


### Fonctionnalité certifiée d’AccessEnabler JavaScript SDK v4 (versions 4.x) sur Safari 12 {#certified-functionality-of-accessenabler-javacscript=sdk-v4}

**Authentification** les flux qui font appel à l’interaction utilisateur fonctionneront toujours, même si les cookies tiers sont désactivés dans le navigateur de l’utilisateur, car à partir de la version 4.0, le SDK JavaScript AccessEnabler n’utilise plus de cookies tiers pour les processus d’authentification.

>[!NOTE]
>
>L’utilisateur DOIT interagir avec le site pour ouvrir des fenêtres de connexion et/ou interagir avec la page de connexion de MVPD.

Les opérations **Autorisation/Contrôle en amont/Métadonnées utilisateur** sont entièrement fonctionnelles, à condition que l’utilisateur soit déjà authentifié.

### Problèmes connus d’AccessEnabler JavaScript SDK v4 (versions 4.x) sur Safari 12 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO et SLO

   * En raison de la manière dont le localStorage est implémenté dans Safari à partir de Safari 10, le SDK JS ne peut plus partager l’état de connexion via un iFrame de domaine commun. Cela signifie que l’utilisateur doit se connecter à chaque site qui utilise AccessEnabler JavaScript SDK. La déconnexion ne supprime pas non plus les jetons d’authentification sur plusieurs sites. L’utilisateur doit donc se déconnecter de chaque site web auquel l’authentification Adobe Pass est activée.

* Temp Pass

   * Pour les passes temporaires, le SDK JavaScript AccessEnabler utilise un mécanisme d’individualisation afin de verrouiller un jeton d’authentification sur un appareil spécifique (instance de navigateur). En raison des nouveaux mécanismes de Safari 12 conçus pour empêcher le suivi, l’empreinte que nous calculons et utilisons dans le mécanisme d’individualisation **sera la même pour tous les utilisateurs qui ont la même adresse IP**. Nous prenons l’adresse IP du client en considération à des fins d’individualisation, mais même ainsi, l’impact se fait sentir sur les utilisateurs qui partagent la même adresse IP publique. Pour ces utilisateurs, nous allons calculer le même identifiant d’individualisation, et le laissez-passer temporaire sera lié à celui-ci. Cela signifie qu’une fois qu’un tel utilisateur utilise un pass temporaire, personne d’autre n’y aura accès \! Cela a un impact en particulier sur les utilisateurs d’entreprise, les établissements d’enseignement ou toute autre organisation dont plusieurs utilisateurs utilisent la technique NAT ou un proxy commun pour accéder à Internet.

>[!NOTE]
>
>Ce problème n’affecte les utilisateurs que si l’implémentateur ou l’implémentatrice utilise une passe temporaire suite à une interaction de l’utilisateur ou de l’utilisatrice, sinon l’authentification de la passe temporaire est soumise aux **flux automatiques** ci-dessous.

* Flux automatiques

   * Les flux d’authentification tentés en mode automatisé, sans interaction de l’utilisateur, ne réussiront pas dans Safari 12 lors de l’utilisation de JS SDK 4.0. Notez que la version 4.1 de JS SDK à venir corrige tous les problèmes liés aux flux automatisés.

Cas d’utilisation concernés par ce problème :

* Authentification automatique TempPass (Prévisualisation gratuite) : pour de tels flux, le SDK renvoie une erreur N130.

* Authentification passive (échoue silencieusement) : l’utilisateur est invité à sélectionner ce MVPD et à saisir ses informations d’identification

### Atténuation {#mitigation-safari12}

**SSO et SLO**

Aucune atténuation connue n’est disponible ou possible au moment de la rédaction de ces lignes. Apple a introduit une « API d’accès au stockage » dans Safari 12 (`https://webkit.org/blog/8124/introducing-storage-access-api`), mais l’implémentation actuelle ne s’applique pas à localStorage, mais uniquement aux cookies. De plus, l’API nécessite une interaction utilisateur pour être utilisée, et une fois que vous l’utilisez, l’utilisateur est également invité à ouvrir une boîte de dialogue d’autorisation similaire à celle ci-dessous.

![](../../../assets/permission-dialog-apple.png)


À ce stade, ces exigences/invites Safari ne sont pas conformes à nos exigences d’expérience utilisateur et nous n’avons pas un comportement cohérent par rapport aux autres navigateurs, où la connexion unique « fonctionne » une fois que nous avons enregistré un jeton dans un domaine commun localStorage.

**passe temp**

Afin de limiter les problèmes d’individualisation et d’obtenir une interaction client, nous vous recommandons d’utiliser **[Promotional Temp Pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)** de manière interactive et de fournir au moins une information supplémentaire sur l’utilisateur (par exemple, son adresse e-mail).

## Safari 13 {#safari13}

**Détails**

>[!IMPORTANT]
>
>Tous les détails ci-dessus de la section Safari 10 à la section Safari 12 s’appliquent toujours dans le cas de Safari 13.


À partir de Safari 13, le navigateur introduit de nouvelles modifications dans l’[ITP (Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/)), rendant l’heuristique derrière le mécanisme plus stricte dans le processus de marquage des cookies tiers en tant que cookies de suivi, afin d’empêcher le suivi intersite.

Comme décrit dans les sections précédentes, le service d’authentification d’Adobe Pass utilise des cookies tiers dans le cadre des processus d’authentification lorsque les personnes en charge de l’implémentation utilisent AccessEnabler JavaScript SDK v2 (versions 2.x) et AccessEnabler JavaScript SDK v3 (versions 3.x). Par rapport aux versions précédentes du navigateur Safari lorsque l’ITP s’est lancé après avoir passé un certain temps à « apprendre » à propos de l’interaction entre l’utilisateur et les parties impliquées (sites web du programmeur et Adobe), le navigateur Safari 13 bloque dès le départ les cookies tiers qui sont considérés comme des cookies de suivi dans la communication client - modèle de serveur.

En conclusion, un utilisateur du navigateur Safari 13 ne pourra probablement pas initier de nouvelles authentifications sur un site web activé pour l’authentification Adobe Pass qui utilise une ancienne version de AccessEnabler JavaScript SDK, v2 (versions 2.x) ou v3 (versions 3.x). Cela est dû au fait que tous les cookies du service Adobe Primetime Authentication requis sont bloqués par ITP, ce qui rend le service incapable de répondre à la demande d’authentification.

La bibliothèque AccessEnabler JavaScript SDK v4 (versions 4.x) n’utilise pas de cookies tiers pour le processus d’authentification. Par conséquent, ses opérations ne sont en aucune manière affectées par les modifications de Safari 13.

### Atténuation {#mitigation-safari13}

Tout d’abord, nous recommandons vivement **la migration vers les versions 4.x** d’AccessEnabler JavaScript SDK pour un comportement stable et prévisible sur le navigateur Safari.

Ensuite, pour AccessEnabler JavaScript SDK v3 (versions 3.x), la bibliothèque contient un mécanisme capable d’identifier les situations où l’authentification des utilisateurs a été bloquée en raison de l’absence des cookies requis. Dans ces situations, la bibliothèque déclenche un rappel d’erreur spécifique ([N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference)) qui est renvoyé au site web activé pour l’authentification Adobe Pass afin d’être utilisé comme signal pour demander à l’utilisateur de prendre des actions qui peuvent atténuer le problème. Pour bénéficier de ce mécanisme, le site web doit mettre en œuvre la spécification [Rapport d’erreur](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md).

Pour AccessEnabler JavaScript SDK v2 (versions 2.x), la bibliothèque ne propose pas le mécanisme décrit ci-dessus. Par conséquent, le site web activé pour l’authentification Adobe Pass ne peut pas être signalé lorsqu’il faut demander à l’utilisateur de prendre des mesures pour atténuer le problème.

Lorsque le site web de l’implémentateur reçoit un rappel d’erreur [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference), l’utilisateur doit être invité à désactiver la prévention intelligente du suivi (ITP) et à activer les cookies tiers en :

* Dans le cas de Mac OS X High Sierra et versions ultérieures : désélection de l’option « **Empêcher le suivi sur plusieurs sites** » pour l’entrée « **Suivi de sites Web** » dans l’onglet Confidentialité du navigateur à partir des Préférences, comme illustré dans l’image ci-dessous.

  ![](../../../assets/prvnt-cross-site-tr-safari13.png)

* Dans le cas de Mac OS X Sierra et versions précédentes : Vérification de l</span>option « **Toujours autoriser** » pour l’entrée « **Cookies et données de site web** » dans l’onglet Confidentialité du navigateur à partir des Préférences, comme illustré dans l’image ci-dessous.

  ![](../../../assets/always-allow-safari13.png)
