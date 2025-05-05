---
title: Manuel de JavaScript SDK
description: Manuel de JavaScript SDK
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '957'
ht-degree: 0%

---

# Manuel JavaScript SDK (hérité) {#javascript-sdk-cookbook}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Introduction {#intro}

Ce document décrit les workflows de droits que l’application de niveau supérieur d’un programmeur implémente pour une intégration de JavaScript avec le service d’authentification Adobe Pass. Des liens vers la référence de l’API JavaScript sont inclus dans tout.

Notez également que la section [Informations connexes](#related) comprend une
lien vers un ensemble d’exemples de code JavaScript.

## Flux de droits {#entitlement}

1. [Conditions préalables](#prereq)
2. [Flux de démarrage](#startup)
3. [Flux d’authentification](#authn)
4. [Flux d’autorisation](#authz)
5. [Afficher le flux multimédia](#logout)

</br>

![](../../../../assets/javascript-flows.png)


## Conditions préalables {#prereq}

**Dépendances :**

- Bibliothèque d’authentification Adobe Pass (AccessEnabler). Contactez votre gestionnaire de compte d’authentification Adobe Pass pour organiser cette opération.
- ID du demandeur d’authentification Adobe Pass valide. Contactez votre gestionnaire de compte d’authentification Adobe Pass pour organiser l’authentification.

Créez vos fonctions de rappel :

- `entitlementLoaded`
</br>

**Trigger :** l’AccessEnabler est chargé et l’initialisation est terminée.

- `displayProviderDialog(mvpds)`

  **Déclencheur :** `getAuthentication(),` uniquement si l’utilisateur n’a pas sélectionné de fournisseur (un MVPD) et n’est pas encore authentifié
Le paramètre mvpds est un tableau de fournisseurs disponibles pour l’utilisateur.

- `setAuthenticationStatus(status, errorcode)`

  **Déclencheur:**
   - `checkAuthentication()`à chaque fois.
   - `getAuthentication()` uniquement si l’utilisateur est déjà authentifié et a sélectionné un fournisseur.

  Le statut renvoyé est succès ou échec ; le code d’erreur décrit le type de l’échec.

- `createIFrame(width, height)`

  **Trigger :** `setSelectedProvider(providerID)`, uniquement si le fournisseur sélectionné est configuré pour s’afficher dans un IFrame.

  >[!NOTE]
  >
  >Un fournisseur est configuré pour effectuer le rendu de son écran d’authentification sous la forme d’une redirection ou d’un iFrame, et le programmeur doit tenir compte des deux.

- `sendTrackingData(event, data)`

  **Triggers:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  Le paramètre `event` indique quel événement de droit s’est produit ; le paramètre `data` est une liste de valeurs relatives à l’événement.
- `setToken(token, resource)`
  **Déclencheur :** `checkAuthorization()`et `getAuthorization()` après une autorisation réussie d’affichage d’une ressource.   Le paramètre `token` est le jeton de média de courte durée ; le paramètre `resource` est le contenu que l’utilisateur est autorisé à afficher.

- `tokenRequestFailed(resource, code, description)`
  **Déclencheur :**`checkAuthorization()` and`getAuthorization()` après une autorisation infructueuse.\
  Le paramètre `resource` correspond au contenu que l’utilisateur tentait d’afficher ; le paramètre `code` correspond au code d’erreur indiquant le type d’échec ; le paramètre `description` décrit l’erreur associée au code d’erreur.

- `selectedProvider(mvpd)`

  **Trigger:** [`getSelectedProvider()`] (#$getSelProv Le paramètre `mvpd` fournit des informations sur le fournisseur sélectionné par
l’utilisateur .

- `setMetadataStatus(metadata, key, arguments)`

  **Déclencheur:** `getMetadata().`\
  Le paramètre `metadata` fournit les données spécifiques que vous avez demandées ; le paramètre key est la clé utilisée dans la `getMetadata()`requête ; et le paramètre `arguments` est le même dictionnaire que celui transmis à `getMetadata()`.


## 2. Flux de démarrage

**I. Chargez le JavaScript AccessEnabler :**

**Pour le profil intermédiaire**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

ou...

**Pour Profil De Production**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Triggers :** une fois l’initialisation terminée, Adobe Pass
L’authentification appelle votre fonction de rappel `entitlementLoaded()`. Il s’agit du point d’entrée pour la communication de votre application avec AccessEnabler.


**II.** l’appel `setRequestor()`pour établir
l’identité du programmeur ; transmettre le `requestorID` du programmeur ; et
(facultatif) tableau de points d’entrée de l’authentification Adobe Pass.

**Déclencheurs :** aucun, mais permet d’appeler les `displayProviderDialog()` en cas de besoin.


**III.** l’appel `checkAuthentication()` pour vérifier une authentification existante sans lancer le [flux d’authentification] complet.  Si cet appel réussit, vous pouvez passer directement à l’`authorization flow` .  Dans le cas contraire, passez à l’`authentication flow` .

**Dépendance :** un appel réussi à `setRequestor()` (cette dépendance s’applique également à tous les appels suivants).

**Triggers:** rappel `setAuthenticationStatus()`

</br>

## 3. Flux d’authentification </span>


**Dépendance :** un appel réussi à `setRequestor()` (cette dépendance s’applique également à tous les appels suivants).


Appelez `getAuthentication()` pour obtenir le statut d’authentification OU pour déclencher le flux d’authentification du fournisseur.

**Déclencheurs:**

- `displayProviderDialog()`si l’utilisateur n’a pas encore été authentifié
- `setAuthenticationStatus()` si l’authentification a déjà eu lieu

La fin du flux d’authentification est atteinte lorsque l’AccessEnabler appelle `setAuthenticationStatus()`avec `isAuthenticated == 1`.

## 4. Flux d’autorisation {#authz}

**Dépendances :**

- Un appel réussi à `setRequestor()` (cette dépendance s’applique également à tous les appels suivants).
- Identifiant(s) de ressource valide(s) convenu(s) avec le ou les MVPD. Notez que les ID de ressource doivent être identiques à ceux utilisés sur d’autres appareils ou plateformes et qu’ils seront identiques sur tous les MVPD.

Appelez `getAuthorization()` et transmettez le ResourceID pour le média demandé. Un appel réussi renvoie un jeton de média court, qui confirme que l’utilisateur est autorisé à afficher le média demandé.

- Si l’appel réussit : l’utilisateur dispose d’un jeton AuthN valide et il est autorisé à regarder le média demandé.
- Si l’appel échoue : examinez l’exception renvoyée pour déterminer son type (AuthN, AuthZ ou autre chose) :
- Si l’appel était une erreur AuthN, redémarrez le flux AuthN.
- Si l’appel était une erreur AuthZ, l’utilisateur n’est pas autorisé à regarder le média demandé et un message d’erreur doit s’afficher à l’intention de l’utilisateur.
- Si une autre erreur s’est produite (erreur de connexion, erreur réseau, etc.), affichez un message d’erreur approprié à l’intention de l’utilisateur.

Utilisez le vérificateur de jeton de média pour valider le shortMediaToken renvoyé à partir d’un appel `getAuthorization()` réussi.


**Dépendance :** le vérificateur de jeton de média court (inclus avec
Bibliothèque AccessEnabler)

- Si la validation est réussie : affichez/lisez le média demandé pour l’utilisateur.
- En cas d’échec : le jeton AuthZ n’était pas valide, la requête de média doit être refusée et un message d’erreur doit s’afficher pour l’utilisateur ou l’utilisatrice.

## 5. Afficher le flux multimédia {#logout}

- L’utilisateur sélectionne le média à afficher.
   - Les médias sont-ils protégés ?
      - Votre application vérifie si le média est protégé :
         - Si le média est protégé, votre application lance le flux d’autorisation (AuthZ) ci-dessus.
         - Si le média n’est pas protégé, poursuivez avec le flux Afficher le média .
         - Média de lecture

## Configuration de l’identifiant visiteur {#visitorID}

La configuration d&#39;une valeur [visitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html) Experience Cloud est très importante du point de vue analytique. Une fois qu’une valeur d’identifiant visiteur EC est définie, le SDK envoie ces informations avec chaque appel réseau et le service d’authentification Adobe Pass collecte ces informations. Vous pourrez ainsi mettre en corrélation les données d’analyse du service d’authentification Adobe Pass avec tout autre rapport d’analyse que vous pouvez avoir à partir d’autres applications ou sites web. Vous trouverez des informations sur la configuration de l’identifiant visiteur EC [ici](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>Notez que cette prise en charge des fonctionnalités est disponible à partir de la version 3.1.0 de JS SDK.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
