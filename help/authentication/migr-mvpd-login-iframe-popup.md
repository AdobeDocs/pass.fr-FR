---
title: Comment migrer la page de connexion MVPD de l’iFrame à la fenêtre contextuelle
description: Comment migrer la page de connexion MVPD de l’iFrame à la fenêtre contextuelle
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---

# Comment migrer la page de connexion MVPD d’iFrame vers la fenêtre contextuelle {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Fenêtre contextuelle ou iFrame {#popup-vs-iframe}

Certains utilisateurs ont rencontré des problèmes de cookies tiers avec l’implémentation d’iFrame d’une page de connexion MVPD.
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

L’équipe d’authentification Adobe Pass **recommande de mettre en oeuvre la fenêtre contextuelle/la nouvelle page de connexion à la fenêtre.** plutôt que la version iFrame sous Firefox et Safari.  Cependant, si vous mettez en oeuvre une page de connexion pour Internet Explorer, vous pouvez rencontrer des problèmes avec la mise en oeuvre de la fenêtre contextuelle. Les problèmes d’IE sont dus au fait que, une fois que l’utilisateur s’est authentifié avec son MVPD dans la fenêtre contextuelle, l’authentification Adobe Pass force une redirection de page parente, qui est considérée comme un bloqueur de fenêtres contextuelles par Internet Explorer. L’équipe d’authentification Adobe Pass **recommande de mettre en oeuvre la connexion à l’iFrame pour Internet Explorer.**.

L’exemple de code présenté dans cette note technique utilise une implémentation hybride d’iFrame et de fenêtre contextuelle : ouverture d’un iFrame sur Internet Explorer et d’une fenêtre contextuelle sur les autres navigateurs.

Étant donné qu’une implémentation d’iFrame existe déjà, la première partie de la note technique présente le code pour l’implémentation d’iFrame et la seconde les modifications pour s’adapter à l’implémentation de fenêtre contextuelle comme valeur par défaut.


## Sélecteur MVPD avec page de connexion dans un iFrame {#mvpd-pickr-iframe}

Les exemples de code précédents présentaient une page de HTML contenant la variable &lt;div> balise dans laquelle l’iFrame doit être créé avec le bouton Fermer l’iFrame :

```HTML
<body> 
    <div id="mvpddiv">
        <div style="background: red">
            <input type="button" id="btnCloseIframe" value="X" onclick="closeIframeAction();">
        </div>
        <br/>
        <!-- We use the "about:blank" value so that when the iFrame loads
             we do not see the the parent page. -->
        <iframe id="mvpdframe" name="mvpdframe" src="about:blank"></iframe>
    </div> 
</body>
```

Voici le **JavaScript** code :

```JavaScript
/*
 * Callback indicating that the AccessEnabler swf has initialized
 */
function swfLoaded() {
    // AccessEnabler is loaded so we can use the API function it provides
    accessEnablerObject.setRequestor(requestorID); 
    accessEnablerObject.checkAuthentication();
} 
/*
* The code the correctly closes the opened IFrame and does not leave the
* AccessEnabler in an undefined state.
*/
function closeIframeAction () {
    accessEnablerObject.setSelectedProvider(null);
    /* We use the "about:blank" value so that when the iFrame loads
     * we do not see the the previous MVPD.
     */
    document.getElementById('mvpdframe').src="about:blank";
    document.getElementById('mvpddiv').style.visibility="hidden";
    document.getElementById('mvpddiv').style.display="none";
}
/*
* Some of the supported MVPDs require their login pages to be displayed within an iFrame.
* In your HTML page, you must include the following <div> tag: "<div id="mvpddiv"></div>".
* Called when the selected MVPD requires an iFrame in which to display its authentication UI.
*/
function createIFrame(inWidth, inHeight) {
     // Create the iFrame to the specified width and height for the auth page
    ifrm = document.createElement("IFRAME");
    ifrm.name = "mvpdframe";
    ...
    document.getElementById('mvpddiv').appendChild(ifrm);
    ...
    // Force the name into the DOM since it is still not refreshed, for IE
    window.frames["mvpdframe"].name = "mvpdframe";
}
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
    /* This is an example how previously the MVPD picker was build and will be replaced. */
}
/* 
* Sending the user selected MVPD of the custom dialog is achieved
* by calling the API function setSelectedProvider(providerID:String).
*/
function setSelectedProvider(providerID) {
    accessEnablerObject.setSelectedProvider(providerID);
}
```


## Sélecteur MVPD avec page de connexion dans une fenêtre contextuelle {#mvpd-pickr-popup}

Puisque nous n’utiliserons pas un **iFrame** le code du HTML ne contient plus l’iFrame ni le bouton permettant de fermer l’iFrame. La balise div qui contenait auparavant l’iFrame - **mvpddiv** - sera conservé et utilisé pour les éléments suivants :

* pour informer l’utilisateur que la page de connexion MVPD est déjà ouverte si la sélection contextuelle est perdue
* pour fournir un lien permettant de se concentrer à nouveau sur la fenêtre contextuelle

```HTML
<body onload="javascript:loadAccessEnabler();"> 
   <div id="aeHolder">
       <div name="contentAccessEnabler" id="contentAccessEnabler"></div>
   </div>
   <div id="mvpddiv">
      <p>
      <strong>No login window?</strong>
      <br/>
      <a href="javascript:mvpdWindow.focus();">Click here to open it.</a>
      <br/>
      Still not working? Check popup blockers!
      </p>
   </div> 
 
   <div id="picker" style="display:none">
      Choose MVPD: <br/>
      <select id="mvpdList" multiple></select>
      <br/>
         <a id="authenticateButton" onclick="javascript:authenticate();">Authenticate</a>
   </div>
</body>
```

La liste des MVPD s’affiche dans la balise div appelée **sélecteur** comme sélection **-mvpdList**.

Un nouveau rappel API sera utilisé - **setConfig(configXML)**. Le rappel est déclenché après l’appel de la fonction setRequestor(requestorID) . Ce rappel renvoie la liste des MVPD qui sont intégrés avec l’ID du demandeur précédemment défini. Dans la méthode de rappel, le code XML entrant est analysé et la liste des MVPD mis en cache. Le sélecteur MVPD est également créé, mais pas affiché.

```JavaScript
var mvpdList;  // The list of cached MVPDs
var mvpdWindow;  // The reference to the popup with the MVPD login page
var cancelTimer = 0;
/* Callback indicating that the AccessEnabler swf has initialized */
function swfLoaded() {
   accessEnablerObject = $('#AccessEnabler').get(0);
   // Using a custom implementation of the MVPD picker
   accessEnablerObject.setProviderDialogURL('none');
   accessEnablerObject.setRequestor(requestorID); 
}

function setConfig(configXML) {
   mvpdList = [];
   $.each($($.parseXML(configXML)).find('mvpd'), function (idx, item) {
      var mvpdId = $(item).find('id').text();
      mvpdList[mvpdId] = {
         displayName: $(item).find('displayName').text(),
         logo: $(item).find('logoUrl').text(),
         popup: $(item).find('iFrameRequired').text() == "true",
         width: $(item).find('iFrameWidth').text(),
         height: $(item).find('iFrameHeight').text()
      };

      $('#mvpdList').append($('<option value="' + mvpdId + '" title="' + mvpdId + '">' + mvpdList[mvpdId].displayName + '</option>'));
   });
   accessEnablerObject.getAuthentication();
}
```

Une fois la fonction getAuthentication() ou getAuthorization() appelée, le rappel displayProviderDialog() est déclenché. Normalement, dans ce rappel, la liste MVPD a été créée et affichée. Comme le sélecteur MVPD est déjà créé, il ne reste plus qu’à l’afficher à l’utilisateur.

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

Une fois que l’utilisateur a sélectionné un MVPD dans le sélecteur, la fenêtre contextuelle doit être créée. Certains navigateurs peuvent bloquer la fenêtre contextuelle si elle est créée avec about:blank ou avec une page se trouvant sur un autre domaine. Il est donc recommandé de l’ouvrir avec le nom d’hôte à partir duquel AccessEnabler est chargé.

Dans l’implémentation de l’iFrame, la réinitialisation du flux d’authentification était effectuée par le bouton btnCloseIframe et la fonction JavaScript closeIframeAction(), mais la décoration de l’iFrame n’est plus possible. Ainsi, le même comportement est obtenu en surveillant le moment où la fenêtre contextuelle est fermée (soit par l’utilisateur, soit en finissant le flux d’authentification). Ajout d’un fragment de code qui aide également en cas de perte de focus de la fenêtre contextuelle :

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

Sur le rappel createIFrame() , la fonction **mvpddiv** div s’affiche.

```JavaScript
function createIFrame(width, height) {
   $('#mvpddiv').show();
   if (useIframeLoginStyle) {
      ...
   }
}

/* Function called when user has selected the MVPD from the picker */
function authenticate() {
   var selectedMvpd = $('#mvpdList').val()[0];
   if (mvpdList[selectedMvpd].popup) {
      mvpdWindow = window.open("http://entitlement.auth-staging.adobe.com", "mvpdframe", "width=" + mvpdList[selectedMvpd].width + ",height=" + mvpdList[selectedMvpd].height, true);
      if (!mvpdWindow) {
         // Message to user that popup blocker is not allowing the authentication process to start
      }
      //watch the mvpd popup for close
      clearInterval(cancelTimer);
      cancelTimer = setInterval(checkClosed, 200);
   }
   accessEnablerObject.setSelectedProvider(selectedMvpd);
}

function checkClosed() {
   try {
      if (mvpdWindow && mvpdWindow["closed"]) {
         clearInterval(cancelTimer);
         accessEnablerObject.setSelectedProvider(null);
         accessEnablerObject.getAuthentication();
         $('#mvpddiv').hide();
      }
   } catch (error) {
      console.log(error);
   }
}
```

>[!IMPORTANT]
>
>* L’exemple de code contient une variable codée en dur pour l’ID de demandeur utilisé - &#39;REF&#39; qui doit être remplacée par un ID de demandeur de programmeur réel.
>* L’exemple de code ne s’exécute correctement qu’à partir d’un domaine whitelisté associé à l’ID de demandeur utilisé.
>* L’ensemble du code pouvant être téléchargé, le code présenté dans cette note technique a été tronqué. Pour obtenir un exemple complet, reportez-vous à la section **Exemple d’iFrame JS ou de fenêtre contextuelle**.
>* Les bibliothèques JavaScript externes ont été liées à partir de [Services hébergés Google](https://developers.google.com/speed/libraries/).
