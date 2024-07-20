---
title: Connexion et déconnexion sans actualisation
description: Connexion et déconnexion sans actualisation
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1761'
ht-degree: 0%

---

# Connexion et déconnexion sans actualisation {#tefresh-less-login-and-logout}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

Pour les applications web, vous devez tenir compte de différents scénarios possibles d’authentification et de déconnexion des utilisateurs.  Les MVPD exigent que les utilisateurs se connectent à la page web du MVPD pour s’authentifier, avec les facteurs supplémentaires suivants en jeu :

- Certains MVPD nécessitent une redirection complète de votre site vers leur page de connexion.
- Certains MVPD vous demandent d’ouvrir un iFrame sur votre site pour afficher la page de connexion du MVPD.
- Certains navigateurs ne gèrent pas correctement l’iFrame. Pour ces navigateurs, une meilleure alternative consiste donc à utiliser une fenêtre contextuelle au lieu de l’iFrame.

Avant l’authentification Adobe Pass 2.7, tous ces scénarios d’authentification d’un utilisateur impliquaient une actualisation complète de la page du programmeur. Pour la version 2.7 et les versions ultérieures, l’équipe d’authentification Adobe Pass a amélioré ces flux afin que l’utilisateur n’ait pas à subir d’actualisation de page sur votre application lors de la connexion et de la déconnexion.


## Description détaillée {#detailed_description}

Commençons par un résumé des flux d’authentification et de déconnexion d’origine, puis suivons-le avec les flux d’authentification et de déconnexion améliorés. Notez que les quatre premières sections traitent les MVPD normaux (non TempPass), tandis que la dernière section décrit l’implémentation spéciale qui doit être appliquée pour TempPass :

- [Flux d’authentification initial](#orig_authn)
- [Flux de connexion initial](#orig_logout)
- [Flux d’authentification amélioré](#improved_authn)
- [Flux de connexion amélioré](#improved_logout)
- [Flux TempPass](#improved_temppass)

</br>

## Flux d’authentification/de déconnexion d’origine {#orig_authn}

**Authentification**

Les clients web d’authentification Adobe Pass disposent de deux manières d’authentifier, selon les exigences des MVPD :

1. **Redirection de page entière -** Une fois que l’utilisateur a sélectionné un fournisseur    (configuré avec une redirection de page entière) à partir du sélecteur MVPD sur la page    Le site web du programmeur `setSelectedProvider(<mvpd>)` est appelé sur AccessEnabler et l’utilisateur est redirigé vers la page de connexion du MVPD. Une fois que l’utilisateur a fourni des informations d’identification valides, il est redirigé vers le site web du programmeur. AccessEnabler est initialisé et le jeton d&#39;authentification est récupéré de l&#39;authentification Adobe Pass pendant `setRequestor`.
1. **iFrame / Fenêtre contextuelle -** Une fois que l’utilisateur a sélectionné un fournisseur (configuré avec iFrame), `setSelectedProvider(<mvpd>)` est appelé sur AccessEnabler. Cette action déclenchera le rappel `createIFrame(width, height)`, informant le programmeur de créer un iFrame (ou une fenêtre contextuelle, selon le navigateur/les préférences) avec le nom `"mvpdframe"` et les dimensions fournies. Une fois l’iFrame/la fenêtre contextuelle créée, AccessEnabler charge la page de connexion du MVPD dans l’iFrame/la fenêtre contextuelle. L’utilisateur fournit des informations d’identification valides et l’iFrame/fenêtre contextuelle est redirigé vers l’authentification Adobe Pass, qui renvoie un extrait de code JS qui ferme l’iFrame/la fenêtre contextuelle et recharge la page parente (site web du programmeur). De même que pour le flux 1, le jeton d’authentification est récupéré pendant `setRequestor`.

Le rappel `displayProviderDialog` (déclenché par `getAuthentication`/`getAuthorization`) renvoie une liste de MVPD et leurs paramètres appropriés. La propriété `iFrameRequired` d’un MVPD permet au programmeur de savoir s’il doit activer le flux 1 ou le flux 2. Notez que le programmeur doit effectuer une action supplémentaire (création d’un iFrame/fenêtre contextuelle) uniquement pour le flux 2.

**Annuler l’authentification**

Il existe également une situation dans laquelle l’utilisateur annule explicitement le flux d’authentification en fermant la page de connexion. Voici les scénarios et la solution proposée aux programmeurs :

1. **Redirection pleine page -** Lorsque la page de connexion est fermée, l’utilisateur doit à nouveau accéder au site web du programmeur et lancer le flux entier à partir du début. Aucune action explicite n’est requise du côté du programmeur dans ce scénario.
1. **iFrame -** Le programmeur est recommandé d’héberger l’iFrame dans un `div` (ou un composant d’IU similaire) auquel est associé un bouton Fermer. Lorsque l’utilisateur appuie sur le bouton Fermer, le programmeur détruit l’iFrame avec l’interface utilisateur associée et exécute `setSelectedProvider(null)`. Cet appel permet à AccessEnabler d’effacer son état interne et permet à l’utilisateur de lancer un flux d’authentification ultérieur. `setAuthenticationStatus` et `sendTrackingData(AUTHENTICATION_DETECTION...)` seront déclenchés pour signaler un flux d’authentification ayant échoué (à la fois sur `getAuthentication` et `getAuthorization`).
1. **Fenêtre contextuelle -** Certains navigateurs ne peuvent pas détecter avec précision l’événement de fermeture de fenêtre. Une approche différente doit donc être adoptée ici (par contraste avec le flux iFrame ci-dessus). Adobe recommande que le programmeur initialise un minuteur qui vérifie périodiquement l’existence de la fenêtre contextuelle de connexion. Si la fenêtre n&#39;existe pas, le programmeur peut s&#39;assurer que l&#39;utilisateur a annulé manuellement le flux de connexion et que le programmeur peut poursuivre l&#39;appel `setSelectedProvider(null)`. Les rappels déclenchés sont identiques à ceux du flux 2 ci-dessus.

</br>

## Flux de connexion initial {#orig_logout}

L’API de déconnexion de AccessEnabler efface l’état local de la bibliothèque et charge l’URL de déconnexion du MVPD dans l’onglet/la fenêtre active. Le navigateur accède au point de terminaison de déconnexion du MVPD et, une fois le processus terminé, l’utilisateur est redirigé vers le site web du programmeur. La seule action requise pour le compte de l’utilisateur est d’appuyer sur le bouton/lien Déconnexion et de lancer le flux. Aucune interaction de l’utilisateur n’est requise sur le point de terminaison de déconnexion du MVPD.

**Flux d’authentification/de déconnexion original avec actualisation de page**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## Authentification améliorée (sans actualisation) {#improved_authn}

>[!NOTE]
>
>Les flux de connexion et de déconnexion sans actualisation améliorés exigent que le navigateur prenne en charge les technologies HTML5 modernes, notamment la messagerie web.

Les flux d’authentification (connexion) et de déconnexion décrits ci-dessus offrent une expérience utilisateur similaire en rechargeant la page principale une fois chaque flux terminé.  La fonction actuelle vise à améliorer l’expérience de l’utilisateur en lui fournissant une connexion et une déconnexion sans actualisation (en arrière-plan). Le programmeur peut activer/désactiver la connexion en arrière-plan et la déconnexion en transmettant deux indicateurs booléens (`backgroundLogin` et `backgroundLogout`) au paramètre `configInfo` de l’API `setRequestor`. Par défaut, la connexion/déconnexion en arrière-plan est désactivée (ce qui offre une compatibilité avec la mise en oeuvre précédente).

**Exemple :**

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

1. La redirection de la page entière est remplacée par un nouvel onglet du navigateur dans lequel la connexion MVPD est effectuée. Le programmeur est requis pour créer un onglet (via `window.open`) nommé `mvpdwindow` lorsque l’utilisateur sélectionne un MVPD (avec `iFrameRequired = false`). Le programmeur exécute ensuite `setSelectedProvider(<mvpd>)`, ce qui permet à AccessEnabler de charger l’URL de connexion MVPD dans le nouvel onglet. Une fois que l’utilisateur a fourni des informations d’identification valides, Adobe Pass Authentication ferme l’onglet et envoie un window.postMessage au site web du programmeur qui signale à AccessEnabler que le flux d’authentification est terminé. Les rappels suivants sont déclenchés :

   - Si le flux a été initié par `getAuthentication`: `setAuthenticationStatus` et `sendTrackingData(AUTHENTICATION_DETECTION...)`, il sera déclenché pour signaler une authentification réussie/infructueuse.

   - Si le flux a été initié par `getAuthorization`: `setToken/tokenRequestFailed` et `sendTrackingData(AUTHORIZATION_DETECTION...)`, il sera déclenché pour signaler une autorisation réussie/échouée.

1. Le flux de l’iFrame/fenêtre contextuelle reste pratiquement inchangé, à la différence qu’une fois que l’utilisateur a fourni des informations d’identification valides, la page parente ne sera pas rechargée. L’iFrame/la fenêtre contextuelle se ferme automatiquement après connexion et un `window.postMessage` est envoyé à la page parente, informant AccessEnabler que le flux est terminé. Les mêmes rappels sont déclenchés que dans le flux précédent, **plus le nouveau rappel suivant :`destroyIFrame`**. Le rappel `destroyIFrame` permet au programmeur de supprimer tout composant associé/auxiliaire d’iFrame, tel que les décorations de l’interface utilisateur. L’existence de ce rappel n’était pas nécessaire dans l’ancien flux d’authentification, car une fois la connexion terminée, l’authentification Adobe Pass rechargeait la page du programmeur, ce qui détruisait tous les composants de l’interface utilisateur.

</br>

>[!IMPORTANT]
> 
>Vous devez charger l’iFrame de connexion MVPD ou la fenêtre contextuelle en tant qu’enfant direct de la page qui contient l’instance AccessEnabler. Si l’iFrame de connexion MVPD ou la fenêtre contextuelle est imbriquée à deux niveaux ou plus sous la page contenant l’instance AccessEnabler, le flux peut se bloquer. Par exemple, si un iFrame se trouvait entre la page principale et l’iFrame MVPD (Page =\> iFrame =\> MVPD iFrame), le flux de connexion peut échouer.

</br>

**Annuler l’authentification**

Voici les flux pour annuler l’authentification :

1. **Onglet Navigateur -** Puisque l’onglet est essentiellement une nouvelle fenêtre, la capture de son événement de fermeture présente les mêmes limites que celles décrites dans le scénario 3 des anciens flux d’authentification. De plus, l’approche du minuteur n’est pas possible ici, car il n’existe aucun moyen de distinguer un onglet qui a été fermé manuellement par l’utilisateur d’un onglet qui a été fermé automatiquement à la fin du flux de connexion. La solution ici est que AccessEnabler reste &quot;silencieux&quot; (aucun rappel n’est déclenché) lorsque l’utilisateur annule le flux. En outre, le programmeur n’est pas tenu de prendre des mesures spécifiques. L’utilisateur pourra lancer un autre flux d’authentification sans recevoir l’erreur &quot;Erreur de demandes d’authentification multiples&quot; (cette erreur a été désactivée dans AccessEnabler pour la connexion en arrière-plan).

1. **iFrame -** Le programmeur peut utiliser l’approche décrite dans le scénario 2 à partir des anciens flux d’authentification (créez l’IU wrapper à partir de l’iFrame et du bouton Fermer associé qui déclenche `setSelectedProvider(null)`. Bien que cette approche ne soit plus une exigence stricte (plusieurs flux d’authentification sont autorisés pour la connexion en arrière-plan, comme décrit dans le scénario 1 ci-dessus), elle est toujours recommandée par l’Adobe.

1. **Fenêtre contextuelle -** Ceci est identique au flux de l’onglet Navigateur ci-dessus.

</br>

## Flux de connexion amélioré {#improved_logout}

Le nouveau flux de déconnexion sera exécuté dans un iFrame masqué, éliminant ainsi la redirection de la page entière.  Cela est possible car l’utilisateur n’a pas besoin d’effectuer une action spécifique sur la page de déconnexion du MVPD.

Une fois le flux de déconnexion terminé, il redirige l’iFrame vers un point de terminaison d’authentification Adobe Pass personnalisé. Cela diffusera un fragment de code JS qui exécute un `window.postMessage` au parent, informant AccessEnabler que la déconnexion est terminée. Les rappels suivants sont déclenchés : `setAuthenticationStatus()` et `sendTrackingData(AUTHENTICATION_DETECTION ...)`, ce qui indique que l’utilisateur n’est plus authentifié.

L’illustration ci-dessous présente le flux sans actualisation qui permet à un utilisateur de se connecter à son MVPD sans actualiser la page principale de votre application :

**Authentification/flux de déconnexion améliorés (sans actualisation)**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## Flux TempPass {#improved_temppas}

La connexion sans actualisation suit une approche différente pour les MVPD de type TempPass.

Comme le flux TempPass nécessite la création automatique d’une fenêtre et sa fermeture sans intervention explicite de l’utilisateur, cela peut poser un problème pour certains navigateurs (bloqueurs de fenêtres contextuelles). Par conséquent, AccessEnabler met en oeuvre la phase de connexion en arrière-plan, sans avoir besoin d’un conteneur web créé par le programmeur.

Voici les aspects dont le programmeur doit tenir compte lors de l’implémentation de TempPass pour une connexion et une déconnexion sans actualisation :

- Avant de commencer l’authentification, l’iFrame ou la fenêtre contextuelle ne doit être créée que pour les MVPD non TempPass. Le programmeur peut détecter si un MVPD est TempPass ou non en lisant la propriété `tempPass` de l’objet MVPD (renvoyée par `setConfig()` / `displayProviderDialog()`).

- Le rappel `createIFrame()` doit contenir une vérification de TempPass et n’exécuter sa logique que lorsque le MVPD n’est PAS TempPass.

- Le rappel `destroyIFrame()` doit contenir une vérification de TempPass et n’exécuter sa logique que lorsque le MVPD n’est PAS TempPass.

- Les rappels `setAuthenticationStatus()` et `sendTrackingData()` sont appelés une fois l’authentification terminée (exactement comme dans le flux Actualiser sans mise à jour pour les MVPD normaux).

>[!NOTE]
>
>Ce flux est disponible uniquement pour TempPass sans actualisation. Pour le flux d’actualisation, TempPass doit être géré explicitement (lorsque TempPass nécessite un iFrame/une fenêtre contextuelle).

</br>

L’exemple de code suivant montre comment gérer une fenêtre MVPD sur un site web de programmeur (pour les MVPD normaux et pour TempPass) :

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
