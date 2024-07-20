---
title: Évaluation de la prévention du suivi dans Apple Safari
description: Évaluation de la prévention du suivi dans Apple Safari
exl-id: a3362020-92ff-4232-b923-e462868730d5
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '1826'
ht-degree: 0%

---

# Évaluation de la prévention du suivi - Apple Safari {#tracking-prevention-assessment-apple-safari}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Safari 10 {#safari10}

**Détails**

À compter de Safari 10, les paramètres de confidentialité par défaut du navigateur provoqueront l’arrêt du fonctionnement des fonctionnalités d’authentification unique (SSO), de connexion unique (SLO) et d’authentification passive. L’authentification unique (SSO) et l’authentification passive ne fonctionneront pas même dans
session identique entre plusieurs onglets ou fenêtres de navigateur.

Ces modifications ont un impact sur les processus d’authentification Adobe Pass.
pour les versions suivantes du SDK JavaScript AccessEnabler : v2 (versions 2.x), v3 (versions 3.x), v4 (versions 4.x).

### Atténuation {#mitigation-safari10}

Afin d’atténuer ces restrictions, vous pouvez demander à l’utilisateur de modifier les paramètres de confidentialité du navigateur Safari 10, et d’utiliser l’option &quot;**Toujours autoriser**&quot; pour l’entrée &quot;**Cookies et données du site Web**&quot; dans l’onglet Confidentialité du navigateur à partir des Préférences, comme illustré ci-dessous.

![](assets/always-allow-safari10.png)


## Safari 11 {#safari11}

**Détails**

>[!IMPORTANT]
>
>Tous les détails ci-dessus de la section Safari 10 s’appliquent toujours dans le cas de Safari 11.

À partir de Safari 11, le navigateur introduit le mécanisme [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) (ITP), une technologie qui utilise la méthode heuristique pour empêcher le suivi intersite. Ces heuristiques ont un impact sur la manière dont les cookies tiers sont stockés et relus sur les appels réseau, ce qui signifie que selon l’activation du mécanisme ITP, le navigateur Safari bloquera les cookies tiers dans la communication client-modèle de serveur.

Le service d’authentification Adobe Pass utilise et utilise des cookies dans le cadre du processus d’authentification **pour fonctionner**. Dans les cas où le processus d’authentification a lieu automatiquement (par exemple, Temp Pass) ou dans les mises en oeuvre qui utilisent des iFrames ou une fonctionnalité &quot;refressless&quot;, les cookies d’Adobe sont considérés comme des cookies tiers et bloqués par défaut. Dans tous les autres cas, Safari utilise un algorithme d’apprentissage automatique qui peut signaler tous les cookies du service d’authentification par mot de passe d’Adobe comme des cookies de suivi, ce qui peut entraîner le blocage d’ITP.

En conclusion, un utilisateur du navigateur Safari 11 peut ne pas pouvoir s’authentifier sur un site web compatible avec l’authentification Adobe Pass après l’activation du mécanisme ITP (Intelligent Tracking Prevention), en particulier lorsque l’utilisateur utilise plusieurs sites web compatibles avec l’authentification Adobe Pass. Par conséquent, l’expérience d’authentification de l’utilisateur peut être inattendue et indéfinie, allant de l’impossibilité de se connecter à une durée d’authentification plus courte que prévu.

Ces modifications ont un impact sur les processus d’authentification Adobe Pass pour les versions suivantes du SDK AccessEnabler JavaScript : v2 (versions 2.x), v3 (versions 3.x).

### Atténuation {#mitigation-safari11}

Pour le SDK JavaScript AccessEnabler v3 (versions 3.x) et le SDK JavaScript AccessEnabler v4 (versions 4.x), la bibliothèque contient un mécanisme capable d’identifier les situations où l’authentification de l’utilisateur a été bloquée en raison de l’absence de cookies requis. Dans ces cas, la bibliothèque déclenche un rappel d’erreur spécifique [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference), qui est transmis au site web activé par l’authentification Adobe Pass afin d’être utilisé comme signal pour demander à l’utilisateur d’agir pour atténuer le problème. Pour bénéficier de ce mécanisme, le site web doit mettre en oeuvre la spécification [Error Reporting](/help/authentication/error-reporting.md).

Pour AccessEnabler JavaScript SDK v2 (versions 2.x), la bibliothèque n’offre pas le mécanisme décrit ci-dessus. Par conséquent, le site web activé pour l’authentification Adobe Pass ne peut pas être signalé quand demander à l’utilisateur d’agir pour atténuer le problème.

La liste des actions pouvant atténuer les problèmes mentionnés ci-dessus **s’applique aux trois versions** du SDK JavaScript AccessEnabler.

Lorsque le rappel d’erreur [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference) est reçu par le site web de l’implémentateur, l’utilisateur doit être invité à désactiver l’ITP (Intelligent Tracking Prevention) et à activer les cookies tiers en :

* Dans le cas de Mac OS X High Sierra et versions ultérieures : désactivation de l’option &quot;**Empêcher le suivi intersite**&quot; pour l’entrée &quot;**Suivi de site Web**&quot; dans l’onglet Confidentialité du navigateur à partir des préférences, comme illustré ci-dessous.

  ![](assets/uncheck-prvnt-cr-st-tr-safari11.png)


* En cas de Mac OS X Sierra et de version précédente : en cochant l’option &quot;**Toujours autoriser**&quot; pour l’entrée &quot;**Cookies et données de site Web**&quot; dans l’onglet Confidentialité du navigateur à partir des Préférences, comme illustré ci-dessous.

  ![](assets/always-allow-safari11.png)

## Safari 12 {#safari12}

**Détails**

>[!IMPORTANT]
>
>Tous les détails ci-dessus de la section Safari 10 et de la section Safari 11 s’appliquent toujours dans le cas de Safari 12.

Cette section décrit les problèmes de compatibilité du **SDK JavaScript AccessEnabler versions 4.x** sur Safari 12.

>[!NOTE]
>
>Gardez à l’esprit qu’en cas de SDK JavaScript AccessEnabler versions 2.x et du SDK JavaScript AccessEnabler versions 3.x, ils utilisent tous deux des cookies tiers pour les processus d’authentification. En raison des stratégies de cookies ITP et tiers commençant par Safari 11, l’expérience d’authentification de l’utilisateur peut être inattendue et indéfinie, allant de l’impossibilité de se connecter à une durée d’authentification plus courte que prévu.


### Fonctionnalités certifiées du SDK JavaScript AccessEnabler v4 (versions 4.x) sur Safari 12 {#certified-functionality-of-accessenabler-javacscript=sdk-v4}

Les flux **d’authentification** qui font appel à l’interaction de l’utilisateur fonctionneront toujours, même si les cookies tiers sont désactivés dans le navigateur de l’utilisateur, car à partir de la version 4.0, le SDK JavaScript AccessEnabler n’utilise plus de cookies tiers pour les processus d’authentification.

>[!NOTE]
>
>L’utilisateur DOIT interagir avec le site pour ouvrir des fenêtres de connexion et/ou interagir avec la page de connexion MVPD.

Les opérations **Authorization/Preflight/User Metadata** sont entièrement fonctionnelles, à condition que l’utilisateur soit déjà authentifié.

### Problèmes connus du SDK JavaScript AccessEnabler v4 (versions 4.x) sur Safari 12 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO et SLO

   * En raison de la manière dont localStorage est implémenté dans Safari à partir de Safari 10, le SDK JS ne peut plus partager l’état de connexion via un iFrame de domaine commun. Cela signifie que l’utilisateur doit se connecter à chaque site qui utilise le SDK JavaScript AccessEnabler. La déconnexion ne supprime pas non plus les jetons d’authentification sur les sites. Par conséquent, l’utilisateur doit se déconnecter de chaque site web d’authentification Adobe Pass activé.

* Temp Pass

   * Pour les passes temporaires, le SDK JavaScript AccessEnabler utilise un mécanisme d’individualisation, afin de verrouiller un jeton d’authentification sur un appareil spécifique (instance de navigateur). En raison des nouveaux mécanismes dans Safari 12 conçus pour empêcher le suivi, l’empreinte digitale que nous calculons et utilisons dans le mécanisme d’individualisation **sera la même pour tous les utilisateurs qui ont la même adresse IP**. Nous prenons l’IP du client en considération à des fins d’individualisation, mais même ainsi, l’impact est sur les utilisateurs qui partagent la même adresse IP publique. Pour ces utilisateurs, nous allons calculer le même identifiant d’individualisation, et le laissez-passer temporaire lui sera associé. Cela signifie qu’une fois qu’un utilisateur de ce type utilise un laissez-passer temporaire, personne d’autre n’y aura accès \! Cela a un impact en particulier sur les utilisateurs d’entreprise, les établissements d’enseignement ou toute autre organisation qui ont plusieurs utilisateurs utilisant NAT ou un proxy commun pour accéder à Internet.

>[!NOTE]
>
>Ce problème affecte les utilisateurs uniquement si l’implémentateur utilise Temp Pass comme résultat d’une interaction de l’utilisateur. Dans le cas contraire, l’authentification Temp Pass est soumise aux **flux automatiques** ci-dessous.

* Flux automatiques

   * Les flux d’authentification tentés en mode automatisé, sans aucune interaction de l’utilisateur, ne fonctionneront pas dans Safari 12 lors de l’utilisation du SDK JS 4.0. Notez que le prochain SDK JS 4.1 corrige tous les problèmes liés aux flux automatisés.

Cas d’utilisation concernés par ce problème :

* Authentification TempPass automatique (aperçu libre) : pour ces flux, le SDK renvoie une erreur N130.

* Authentification passive (échec silencieux) : l’utilisateur est invité à sélectionner ce MVPD et à saisir des informations d’identification.

### Atténuation {#mitigation-safari12}

**SSO et SLO**

Aucune atténuation connue n&#39;est disponible ou possible au moment de l&#39;écriture de cet article. Apple a introduit une &quot;API d’accès au stockage&quot; dans Safari 12 (`https://webkit.org/blog/8124/introducing-storage-access-api`), mais l’implémentation actuelle ne s’applique pas à localStorage, mais uniquement aux cookies. De plus, l’API requiert une interaction de l’utilisateur pour être utilisée. Une fois que vous l’utilisez, l’utilisateur est également invité à saisir une boîte de dialogue d’autorisation similaire à celle ci-dessous.

![](assets/permission-dialog-apple.png)


À ce stade, ces exigences/invites Safari ne correspondent pas à nos exigences UX et nous n’avons pas de comportement cohérent comme sur les autres navigateurs, où SSO &quot;fonctionne simplement&quot; une fois que nous avons enregistré un jeton dans un domaine commun localStorage.

**Temp Pass**

Afin d’atténuer les problèmes d’individualisation et d’avoir une interaction avec l’utilisateur, nous vous recommandons d’utiliser de manière interactive la **[Passe temporaire promotionnelle](/help/authentication/promotional-temp-pass.md)** et de fournir au moins une information supplémentaire sur l’utilisateur (par exemple, l’adresse électronique).

## Safari 13 {#safari13}

**Détails**

>[!IMPORTANT]
>
>Tous les détails ci-dessus, de la section Safari 10 à la section Safari 12, s’appliquent toujours dans le cas de Safari 13.


À partir de Safari 13, le navigateur introduit de nouvelles modifications à l’ [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) (ITP), rendant la heuristique derrière le mécanisme plus stricte dans le processus de marquage des cookies tiers comme cookies de suivi, afin d’empêcher le suivi intersite.

Comme décrit dans les sections précédentes, le service d’authentification Adobe Pass utilise et repose sur des cookies tiers dans le cadre des processus d’authentification lorsque les implémentateurs utilisent le SDK AccessEnabler JavaScript v2 (versions 2.x) et le SDK JavaScript AccessEnabler v3 (versions 3.x). Par rapport aux versions précédentes du navigateur Safari lorsque ITP était lancé après avoir passé un certain temps à &quot;découvrir&quot; l’interaction entre l’utilisateur et les parties concernées (sites web et Adobe du programmeur), le navigateur Safari 13 bloque depuis le début les cookies tiers, qui sont considérés comme des cookies de suivi dans la communication entre le client et le modèle de serveur.

En conclusion, un utilisateur du navigateur Safari 13 ne pourra probablement pas lancer de nouvelles authentifications sur un site web compatible avec l’authentification Adobe Pass qui utilise une ancienne version du SDK JavaScript AccessEnabler, v2 (versions 2.x) ou v3 (versions 3.x). Cela est dû au fait que tous les cookies du service d’authentification Primetime de l’Adobe requis sont bloqués par ITP, ce qui rend le service incapable de remplir la demande d’authentification.

La bibliothèque SDK JavaScript AccessEnabler v4 (versions 4.x) n’utilise pas de cookies tiers pour le processus d’authentification. Par conséquent, ses opérations ne sont pas affectées par les modifications de Safari 13.

### Atténuation {#mitigation-safari13}

D’abord et avant tout, nous vous recommandons vivement d’effectuer une **migration vers le SDK JavaScript AccessEnabler versions 4.x** afin d’avoir un comportement stable et prévisible sur le navigateur Safari.

Deuxièmement, pour AccessEnabler JavaScript SDK v3 (versions 3.x), la bibliothèque contient un mécanisme capable d’identifier les situations dans lesquelles l’authentification des utilisateurs a été bloquée en raison de cookies requis manquants. Dans ces situations, la bibliothèque déclenche un rappel d’erreur spécifique ([N130](/help/authentication/error-reporting.md#advanced-error-codes-reference)) qui est transmis au site web activé pour l’authentification Adobe Pass afin d’être utilisé comme signal pour demander à l’utilisateur d’agir pour atténuer le problème. Pour bénéficier de ce mécanisme, le site web doit mettre en oeuvre la spécification [Error Reporting](/help/authentication/error-reporting.md).

Pour AccessEnabler JavaScript SDK v2 (versions 2.x), la bibliothèque n’offre pas le mécanisme décrit ci-dessus. Par conséquent, le site web activé pour l’authentification Adobe Pass ne peut pas être signalé quand demander à l’utilisateur d’agir pour atténuer le problème.

Lorsque le rappel d’erreur [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference) est reçu par le site web de l’implémentateur, l’utilisateur doit être invité à désactiver l’ITP (Intelligent Tracking Prevention) et à activer les cookies tiers en :

* Dans le cas de Mac OS X High Sierra et versions ultérieures : désactivation de l’option &quot;**Empêcher le suivi intersite**&quot; pour l’entrée &quot;**Suivi de site Web**&quot; dans l’onglet Confidentialité du navigateur à partir des préférences, comme illustré ci-dessous.

  ![](assets/prvnt-cross-site-tr-safari13.png)

* En cas de Mac OS X Sierra et précédent : en cochant t</span>l’option &quot;**Toujours autoriser**&quot; pour l’entrée &quot;**Cookies et données de site Web**&quot; dans l’onglet Confidentialité du navigateur à partir des Préférences, comme illustré ci-dessous.

  ![](assets/always-allow-safari13.png)
