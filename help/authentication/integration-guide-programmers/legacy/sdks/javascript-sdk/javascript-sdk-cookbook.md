---
title: Guide pas à pas du SDK JavaScript
description: Guide pas à pas du SDK JavaScript
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# Guide pas à pas du SDK JavaScript {#javascript-sdk-cookbook}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Introduction {#intro}

Ce document décrit les processus de droits que l’application de niveau supérieur d’un programmeur implémente pour une intégration JavaScript avec le service d’authentification Adobe Pass. Des liens vers la référence API JavaScript sont inclus dans tout le contenu.

Notez également que la section [Informations connexes](#related) comprend une
lien vers un ensemble d’exemples de code JavaScript.

## Flux de droits {#entitlement}

1. [Conditions préalables](#prereq)
2. [Flux de démarrage](#startup)
3. [Flux d’authentification](#authn)
4. [Flux d’autorisation](#authz)
5. [Afficher le flux du média](#logout)

</br>

![](../../../../assets/javascript-flows.png)


## Conditions préalables {#prereq}

**Dépendances :**

- Bibliothèque d’authentification Adobe Pass (AccessEnabler), adressez-vous à votre gestionnaire de compte d’authentification Adobe Pass pour organiser cela.
- Identifiant de demandeur d’authentification Adobe Pass valide, demandez-le à votre gestionnaire de compte d’authentification Adobe Pass.

Créez vos fonctions de rappel :

- `entitlementLoaded`
</br>

**Déclencheur :** L&#39;AccessEnabler a été chargé et l&#39;initialisation est terminée.

- `displayProviderDialog(mvpds)`

  **Déclencheur :** `getAuthentication(),` uniquement si l’utilisateur n’a pas sélectionné de fournisseur (MVPD) et n’est pas encore authentifié
Le paramètre mvpds est un tableau de fournisseurs mis à la disposition de l’utilisateur.

- `setAuthenticationStatus(status, errorcode)`

  **Déclencheur :**
   - `checkAuthentication()` à chaque fois.
   - `getAuthentication()` uniquement si l’utilisateur est déjà authentifié et a sélectionné un fournisseur.

  L’état renvoyé est une réussite ou un échec ; le code d’erreur décrit le type de l’échec.

- `createIFrame(width, height)`

  **Déclencheur :** `setSelectedProvider(providerID)`, uniquement si le fournisseur sélectionné est configuré pour s’afficher dans un IFrame.

  >[!NOTE]
  >
  >Un fournisseur est configuré pour afficher son écran d’authentification sous la forme d’une redirection ou dans un iFrame, et le programmeur doit tenir compte des deux.

- `sendTrackingData(event, data)`

  **Triggers :** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  Le paramètre `event` indique quel événement de droit s’est produit ; le paramètre `data` est une liste de valeurs relatives à l’événement.
- `setToken(token, resource)`
  **Déclencheur :** `checkAuthorization()` et `getAuthorization()` après une autorisation réussie d’affichage d’une ressource.   Le paramètre `token` est le jeton multimédia de courte durée ; le paramètre `resource` est le contenu que l’utilisateur est autorisé à afficher.

- `tokenRequestFailed(resource, code, description)`
  **Déclencheur :**`checkAuthorization()` et`getAuthorization()` après une autorisation manquée.\
  Le paramètre `resource` est le contenu que l’utilisateur tentait de consulter ; le paramètre `code` est le code d’erreur indiquant le type d’échec survenu ; le paramètre `description` décrit l’erreur associée au code d’erreur.

- `selectedProvider(mvpd)`

  **Déclencheur :** [`getSelectedProvider()`](#$getSelProv Le paramètre `mvpd` fournit des informations sur le fournisseur sélectionné par
l’utilisateur.

- `setMetadataStatus(metadata, key, arguments)`

  **Déclencheur :** `getMetadata().`\
  Le paramètre `metadata` fournit les données spécifiques que vous avez demandées ; le paramètre key est la clé utilisée dans la requête `getMetadata()` et le paramètre `arguments` est le même dictionnaire qui a été transmis à `getMetadata()`.


## 2. Flux de démarrage

**I. Chargez le JavaScript AccessEnabler :**

**Pour Le Profil D’Évaluation**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

ou ...

**Pour Profil de production**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Triggers :** Une fois l’initialisation terminée, Adobe Pass
authentication appelle votre fonction de rappel `entitlementLoaded()` . Il s’agit du point d’entrée de la communication de votre application avec AccessEnabler.


**II.** Appelez `setRequestor()` pour établir la variable
identité du programmeur ; transmettez le `requestorID` du programmeur et
(facultatif) un tableau de points de fin d’authentification Adobe Pass.

**Triggers :** Aucun, mais permet d’appeler `displayProviderDialog()` si nécessaire.


**III.** Appelez `checkAuthentication()` pour rechercher une authentification existante sans initialiser le [flux d&#39;authentification] complet.  Si cet appel réussit, vous pouvez passer directement à `authorization flow`.  Si ce n&#39;est pas le cas, passez à l&#39;élément `authentication flow`.

**Dépendance :** Appel réussi à `setRequestor()` (cette dépendance s’applique également à tous les appels suivants).

**Triggers:** `setAuthenticationStatus()` callback

</br>

## 3. Flux d’authentification</span>


**Dépendance :** Appel réussi à `setRequestor()` (cette dépendance s’applique également à tous les appels suivants).


Appelez `getAuthentication()` pour obtenir l’état d’authentification OU pour déclencher le flux d’authentification du fournisseur.

**Trigers:**

- `displayProviderDialog()` si l’utilisateur n’a pas encore été authentifié
- `setAuthenticationStatus()` si l’authentification a déjà eu lieu

La fin du flux d’authentification est atteinte lorsque AccessEnabler appelle `setAuthenticationStatus()` avec `isAuthenticated == 1`.

## 4. Flux d’autorisation {#authz}

**Dépendances :**

- Un appel réussi à `setRequestor()` (cette dépendance s’applique également à tous les appels suivants).
- ID de ressource valide accepté par le ou les MVPD. Notez que les ResourceID doivent être identiques à ceux utilisés sur d’autres appareils ou plateformes et seront identiques sur plusieurs MVPD.

Appelez `getAuthorization()` et transmettez l’ID de ressource pour le média demandé. Un appel réussi renvoie un jeton de média court, qui confirme que l’utilisateur est autorisé à afficher le média demandé.

- Si l’appel est transmis : l’utilisateur dispose d’un jeton AuthN valide et est autorisé à regarder le média demandé.
- Si l’appel échoue : examinez l’exception générée pour déterminer son type (AuthN, AuthZ, etc.) :
- Si l’appel était une erreur AuthN, redémarrez le flux AuthN.
- Si l’appel était une erreur AuthZ, l’utilisateur n’est pas autorisé à regarder le média demandé et un message d’erreur de ce type doit s’afficher à l’utilisateur.
- Si une autre erreur s&#39;est produite (erreur de connexion, erreur réseau, etc.), affichez un message d&#39;erreur approprié à l&#39;utilisateur.

Utilisez le vérificateur de jeton multimédia pour valider le shortMediaToken renvoyé par un appel `getAuthorization()` réussi.


**Dépendance :** Vérification du jeton de média court (inclus avec
Bibliothèque AccessEnabler)

- Si la validation réussit : affichez/relayez le média demandé à l’utilisateur.
- En cas d’échec : le jeton AuthZ n’était pas valide, la demande de média doit être refusée et un message d’erreur doit s’afficher à l’utilisateur.

## 5. Afficher le flux multimédia {#logout}

- L’utilisateur sélectionne le média à afficher.
   - Les médias sont-ils protégés ?
      - Votre application vérifie si le média est protégé :
         - Si le média est protégé, votre application lance le flux d’autorisation (AuthZ) ci-dessus.
         - Si le média n’est pas protégé, passez à l’étape Afficher le flux multimédia .
         - Média de lecture

## Configuration de l’identifiant visiteur {#visitorID}

La configuration d’une valeur [visitorID Experience Cloud](https://experienceleague.adobe.com/docs/id-service/using/home.html) est très importante du point de vue des analyses. Une fois la valeur visitorID d’un EC définie, le SDK envoie ces informations avec chaque appel réseau et le service d’authentification Adobe Pass collecte ces informations. De cette manière, vous pourrez mettre en corrélation les données d’analyse du service d’authentification Adobe Pass avec tout autre rapport d’analyse que vous pourriez avoir d’autres applications ou sites web. Vous trouverez des informations sur la configuration de l’identifiant visiteur EC [ici](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>Notez que cette prise en charge de cette fonctionnalité est disponible à partir de la version 3.1.0 du SDK JS.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
