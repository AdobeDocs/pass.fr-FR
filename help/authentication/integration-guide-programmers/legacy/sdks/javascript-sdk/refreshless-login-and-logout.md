---
title: Connexion et déconnexion sans actualisation
description: Connexion et déconnexion sans actualisation
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1762'
ht-degree: 0%

---

# (Hérité) Connexion et déconnexion sans actualisation {#tefresh-less-login-and-logout}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

Pour les applications web, vous devez tenir compte de différents scénarios possibles pour l’authentification et la déconnexion des utilisateurs.  Les fichiers MVPD exigent que les utilisateurs se connectent à la page web MVPD pour s’authentifier, avec les facteurs supplémentaires suivants en jeu :

- Certaines MVPD nécessitent une redirection complète de votre site vers leur page de connexion
- Certains MVPD nécessitent que vous ouvriez un iFrame sur votre site pour afficher la page de connexion de MVPD
- Certains navigateurs ne gèrent pas bien le scénario iFrame. Pour ces navigateurs, une meilleure alternative consiste donc à utiliser une fenêtre contextuelle au lieu de l’iFrame

Avant l’authentification Adobe Pass 2.7, tous ces scénarios d’authentification d’un utilisateur impliquaient une actualisation complète de la page du programmeur. Pour la version 2.7 et les versions ultérieures, l’équipe Authentification Adobe Pass a amélioré ces flux afin que l’utilisateur n’ait pas à effectuer une actualisation de la page de votre application lors de la connexion et de la déconnexion.


## Description détaillée {#detailed_description}

Commençons par un résumé des flux d’authentification et de déconnexion d’origine, puis suivons avec les flux d’authentification et de déconnexion améliorés. Notez que les quatre premières sections traitent des fichiers MVPD normaux (non-TempPass), tandis que la dernière section décrit l’implémentation spéciale qui doit être appliquée à TempPass :

- [Flux d’authentification d’origine](#orig_authn)
- [Flux de déconnexion d’origine](#orig_logout)
- [Flux d’authentification amélioré](#improved_authn)
- [Flux de déconnexion amélioré](#improved_logout)
- [Flux TempPass](#improved_temppass)

</br>

## Flux d’authentification/de déconnexion d’origine {#orig_authn}

**Authentification**

Les clients web Adobe Pass Authentication disposent de deux méthodes d’authentification, selon les exigences des fichiers MVPD :

1. **Redirection de page entière -** après la sélection d’un fournisseur par l’utilisateur ou l’utilisatrice    (configurée avec la redirection de page complète) à partir du sélecteur MVPD sur le    Site web du programmeur, `setSelectedProvider(<mvpd>)` est appelé sur l’AccessEnabler et l’utilisateur est redirigé vers la page de connexion de MVPD. Une fois que l’utilisateur a fourni des informations d’identification valides, il est redirigé vers le site web du programmeur. L’AccessEnabler est initialisé et le jeton d’authentification est récupéré à partir de l’authentification Adobe Pass lors de la `setRequestor`.
1. **iFrame / Fenêtre contextuelle -** Une fois que l’utilisateur a sélectionné un fournisseur (configuré avec iFrame), `setSelectedProvider(<mvpd>)` est appelé sur l’AccessEnabler. Cette action déclenche le rappel `createIFrame(width, height)`, en informant le programmeur de la création d’un iFrame (ou d’une fenêtre contextuelle, selon le navigateur/les préférences) avec le nom `"mvpdframe"` et les dimensions fournies. Une fois l’iFrame/la fenêtre contextuelle créée, AccessEnabler charge la page de connexion MVPD dans l’iFrame/la fenêtre contextuelle. L’utilisateur fournit des informations d’identification valides et l’iFrame/popup est redirigé vers l’authentification Adobe Pass, qui renvoie un extrait de code JS qui ferme l’iFrame/popup et recharge la page parente (site web du programmeur). De même que pour le flux 1, le jeton d’authentification est récupéré pendant l’`setRequestor`.

Le rappel `displayProviderDialog` (déclenché par `getAuthentication`/`getAuthorization`) renvoie une liste de fichiers MVPD et leurs paramètres appropriés. La propriété `iFrameRequired` d’un MVPD permet au programmeur de savoir s’il doit activer le flux 1 ou le flux 2. Notez que le programmeur doit effectuer une action supplémentaire (créer un iFrame/une fenêtre contextuelle) uniquement pour le flux 2.

**Annuler l’authentification**

Il existe également une situation dans laquelle l’utilisateur ou l’utilisatrice annule explicitement le flux d’authentification en fermant la page de connexion. Voici les scénarios et la solution proposée pour les programmeurs :

1. **Redirection de page entière -** Lorsque la page de connexion est fermée, l’utilisateur doit de nouveau accéder au site web du programmeur et lancer l’ensemble du flux depuis le début. Aucune action explicite n’est requise de la part du programmeur dans ce scénario.
1. **iFrame -** Il est recommandé au programmeur d’héberger l’iFrame dans un `div` (ou un composant d’IU similaire) auquel est associé un bouton Fermer. Lorsque l’utilisateur appuie sur le bouton Fermer, le programmeur détruit l’iFrame ainsi que l’interface utilisateur associée et effectue des `setSelectedProvider(null)`. Cet appel permet à AccessEnabler d’effacer son état interne et permet à l’utilisateur de lancer un flux d’authentification suivant. `setAuthenticationStatus` et `sendTrackingData(AUTHENTICATION_DETECTION...)` seront déclenchés pour signaler un flux d’authentification ayant échoué (à la fois sur `getAuthentication` et `getAuthorization`).
1. **Fenêtre contextuelle -** Certains navigateurs ne peuvent pas détecter avec précision l’événement de fermeture de fenêtre. Une approche différente doit donc être adoptée ici (contrairement au flux iFrame ci-dessus). Adobe recommande au programmeur d’initialiser un minuteur qui vérifie périodiquement l’existence de la fenêtre contextuelle de connexion. Si la fenêtre n’existe pas, le programmeur peut s’assurer que l’utilisateur a annulé manuellement le flux de connexion et qu’il peut procéder à l’appel de `setSelectedProvider(null)`. Les rappels déclenchés sont les mêmes que dans le flux 2 ci-dessus.

</br>

## Flux de déconnexion d’origine {#orig_logout}

L’API de déconnexion d’AccessEnabler efface le statut local de la bibliothèque et charge l’URL de déconnexion MVPD dans l’onglet/la fenêtre active. Le navigateur accède au point d’entrée de déconnexion MVPD. Une fois le processus terminé, l’utilisateur est redirigé vers le site web du programmeur. La seule action requise de la part de l’utilisateur est d’appuyer sur le bouton/lien de déconnexion et de lancer le flux ; aucune interaction utilisateur n’est requise sur le point d’entrée de déconnexion MVPD.

**Authentification D’Origine/Flux De Déconnexion Avec Actualisation De Page**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## Amélioration de l’authentification (sans actualisation) {#improved_authn}

>[!NOTE]
>
>L’amélioration des flux de connexion et de déconnexion sans actualisation nécessite que le navigateur prenne en charge les technologies modernes d’HTML 5, y compris la messagerie web.

Les flux d’authentification (connexion) et de déconnexion décrits ci-dessus offrent à l’utilisateur une expérience similaire en rechargeant la page principale une fois chaque flux terminé.  La fonctionnalité actuelle vise à améliorer l’expérience utilisateur en fournissant une connexion et une déconnexion sans actualisation (en arrière-plan). Le programmeur peut activer/désactiver la connexion et la déconnexion en arrière-plan en transmettant deux indicateurs booléens (`backgroundLogin` et `backgroundLogout`) au paramètre `configInfo` de l’API `setRequestor`. Par défaut, la connexion/déconnexion en arrière-plan est désactivée (ce qui permet d’assurer la compatibilité avec la mise en œuvre précédente).

**Exemple:**

```JSON
    var configInfo = {
        callSetConfig: true,
        backgroundLogin: true,
        backgroundLogout: true
    };
    accessEnabler.setRequestor(REQUESTOR_ID, null, configInfo);
```

**Authentification**

Les points suivants décrivent la transition entre les flux d’authentification d’origine et les flux améliorés :

1. La redirection de pleine page est remplacée par un nouvel onglet du navigateur dans lequel la connexion à MVPD est effectuée. Le programmeur doit créer un nouvel onglet (via `window.open`) nommé `mvpdwindow` lorsque l’utilisateur sélectionne un MVPD (avec `iFrameRequired = false`). Le programmeur exécute ensuite `setSelectedProvider(<mvpd>)`, ce qui permet à AccessEnabler de charger l’URL de connexion MVPD dans le nouvel onglet. Une fois que l’utilisateur a fourni des informations d’identification valides, l’authentification Adobe Pass ferme l’onglet et envoie un window.postMessage au site web du programmeur pour signaler à AccessEnabler que le flux d’authentification est terminé. Les rappels suivants sont déclenchés :

   - Si le flux a été initié par `getAuthentication` : `setAuthenticationStatus` et `sendTrackingData(AUTHENTICATION_DETECTION...)` seront déclenchés pour signaler une authentification réussie/non réussie.

   - Si le flux a été initié par `getAuthorization` : `setToken/tokenRequestFailed` et `sendTrackingData(AUTHORIZATION_DETECTION...)` seront déclenchés pour signaler une autorisation réussie/non réussie.

1. Le flux iFrame/fenêtre contextuelle reste pratiquement inchangé, à la différence qu’une fois que l’utilisateur ou l’utilisatrice a fourni des informations d’identification valides, la page parente n’est pas rechargée. L’iFrame/la fenêtre contextuelle se ferme automatiquement après la connexion et un `window.postMessage` est envoyé à la page parente, informant l’AccessEnabler que le flux est terminé. Les mêmes rappels sont déclenchés que dans le flux précédent, **plus le nouveau rappel suivant :`destroyIFrame`**. Le rappel `destroyIFrame` permet au programmeur de supprimer tous les composants auxiliaires/associés à l’iFrame, tels que les décorations de l’interface utilisateur. L’existence de ce rappel n’était pas requise dans l’ancien flux d’authentification, car une fois la connexion terminée, l’authentification Adobe Pass rechargeait la page du programmeur, détruisant ainsi tous les composants de l’interface utilisateur.

</br>

>[!IMPORTANT]
> 
>Vous devez charger l’iFrame de connexion MVPD ou la fenêtre contextuelle en tant qu’enfant direct de la page qui contient l’instance AccessEnabler. Si l’iFrame de connexion ou la fenêtre contextuelle de MVPD est imbriqué à deux niveaux ou plus en dessous de la page contenant l’instance AccessEnabler, le flux peut se bloquer. Par exemple, si un iFrame se trouve entre la page principale et l’iFrame MVPD (Page =\> iFrame =\> iFrame MVPD), le flux de connexion peut échouer.

</br>

**Annuler l’authentification**

Voici les flux permettant d’annuler l’authentification :

1. **Onglet Navigateur -** L’onglet étant essentiellement une nouvelle fenêtre, la capture de son événement de fermeture présente les mêmes limitations que celles décrites dans le scénario 3 à partir des anciens flux d’authentification. De plus, l’approche par minuteur n’est pas possible ici, car il n’existe aucun moyen de distinguer un onglet qui a été fermé manuellement par l’utilisateur ou l’utilisatrice d’un onglet qui a été fermé automatiquement à la fin du flux de connexion. La solution ici est que AccessEnabler reste « silencieux » (aucun rappel n’est déclenché) lorsque l’utilisateur ou l’utilisatrice annule le flux. En outre, le programmeur n’est pas tenu de prendre des mesures spécifiques. L’utilisateur pourra lancer un autre flux d’authentification sans recevoir l’erreur « Plusieurs demandes d’authentification » (cette erreur a été désactivée dans AccessEnabler pour la connexion en arrière-plan).

1. **iFrame -** Le programmeur peut adopter l’approche décrite dans le scénario 2 à partir des anciens flux d’authentification (création d’une interface utilisateur wrapper à partir de l’iFrame et du bouton Fermer associé qui déclenche l’`setSelectedProvider(null)`). Bien que cette approche ne soit plus une exigence stricte (plusieurs flux d’authentification sont autorisés pour la connexion en arrière-plan, comme décrit dans le scénario 1 ci-dessus), elle est toujours recommandée par Adobe.

1. **Fenêtre contextuelle -** Identique au flux de l’onglet Navigateur ci-dessus.

</br>

## Flux de déconnexion amélioré {#improved_logout}

Le nouveau flux de déconnexion sera exécuté dans un iFrame masqué, ce qui élimine la redirection de la page entière.  Cela est possible, car l’utilisateur n’a pas besoin d’effectuer d’action spécifique sur la page de déconnexion de MVPD.

Une fois le flux de déconnexion terminé, il redirige l’iFrame vers un point d’entrée d’authentification Adobe Pass personnalisé. Cela servira un extrait de code JS qui effectue un `window.postMessage` au parent, informant l&#39;AccessEnabler que la déconnexion est terminée. Les rappels suivants sont déclenchés : `setAuthenticationStatus()` et `sendTrackingData(AUTHENTICATION_DETECTION ...)`, signalant que l’utilisateur n’est plus authentifié.

L’illustration ci-dessous montre le flux sans actualisation qui permet à un utilisateur de se connecter à son MVPD sans actualiser la page principale de votre application :

**Amélioration du flux d’authentification/de déconnexion (sans actualisation)**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## Flux TempPass {#improved_temppas}

La connexion sans actualisation suit une approche différente pour les fichiers MVPD de type TempPass.

Comme le flux TempPass exige qu&#39;une fenêtre soit créée automatiquement et fermée sans interaction explicite de l&#39;utilisateur, cela peut poser un problème pour certains navigateurs (bloqueurs de fenêtres contextuelles). Par conséquent, AccessEnabler implémente la phase de connexion en arrière-plan, sans avoir besoin d’un conteneur web créé par le programmeur.

Voici les aspects que le programmeur doit prendre en compte lors de l’implémentation de TempPass pour la connexion et la déconnexion sans actualisation :

- Avant de démarrer l’authentification, la fenêtre iFrame ou la fenêtre contextuelle doit être créée uniquement pour les fichiers MVPD non-TempPass. Le programmeur peut détecter si un MVPD est TempPass ou non en lisant la propriété `tempPass` de l&#39;objet MVPD (renvoyée par `setConfig()` / `displayProviderDialog()`).

- Le rappel `createIFrame()` doit contenir une vérification de TempPass et n’exécuter sa logique que lorsque le MVPD n’est PAS un TempPass.

- Le rappel `destroyIFrame()` doit contenir une vérification de TempPass et n’exécuter sa logique que lorsque le MVPD n’est PAS un TempPass.

- Les rappels `setAuthenticationStatus()` et `sendTrackingData()` sont appelés une fois l’authentification terminée (exactement comme dans le flux sans actualisation pour les MVPD normales).

>[!NOTE]
>
>Ce flux est disponible uniquement pour un TempPass sans actualisation. Pour le flux d’actualisation, TempPass doit être géré explicitement (lorsque TempPass nécessite iFrame / popup)

</br>

L’exemple de code suivant montre comment gérer une fenêtre MVPD sur le site Web d’un programmeur (pour les MVPD standard et pour TempPass) :

```javascript
    var aeHostname = "https://entitlement.auth.adobe.com";
    var mvpdWindow = null;
    var mvpd = <mvpd_object_from_displayProviderDialog>;
    var useIframeLogin = <boolean_depending_on_browser_or_Programmer_preferences>;
    var backgroundLogin = <boolean_depending_on_Programmer_preferences>;
     
    // Do not create any windows for refreshless and temp pass
    if (!(backgroundLogin && mvpd.tempPass)) {
        if (backgroundLogin && !mvpd.popup) {
            mvpdWindow = window.open(aeHostname, "mvpdwindow");
        } else if (mvpd.popup && !useIframeLogin) {
            var width = mvpd.width;
            var height = mvpd.height;
            // Center on screen
            var top = (document.all) ? window.screenTop : window.screenY + 100;
            var left = (document.all) ? window.screenLeft : window.screenX + window.innerWidth / 2 - width / 2;
        
            mvpdWindow = window.open(aeHostname, "mvpdframe",
                           "width=" + width + ",height=" + height + ",top=" + top + ",left=" + left);
            // Monitor the mvpd popup for close
            if (!backgroundLogin) {
                clearInterval(cancelTimer);
                cancelTimer = setInterval(function () {
                    if (mvpdWindow && mvpdWindow.closed) {
                        clearInterval(cancelTimer);
                        $('#mvpddiv').hide();
                        accessEnablerAPI.setSelectedProvider(null);
                    }
                }, 200);
            }
        }
    }
```
