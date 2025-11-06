---
title: Comment migrer la page de connexion MVPD d’iFrame vers une fenêtre contextuelle
description: Comment migrer la page de connexion MVPD d’iFrame vers une fenêtre contextuelle
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 0%

---

# (Hérité) Comment migrer la page de connexion MVPD d’iFrame vers une fenêtre contextuelle {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Fenêtre contextuelle par rapport à iFrame {#popup-vs-iframe}

Certains utilisateurs ont rencontré des problèmes de cookies tiers avec l’implémentation iFrame d’une page de connexion MVPD.
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

L’équipe Authentification Adobe Pass **recommande d’implémenter la page de connexion de la fenêtre contextuelle/nouvelle fenêtre** plutôt que la version iFrame sur Firefox et Safari.  Cependant, si vous implémentez une page de connexion pour Internet Explorer, vous pouvez rencontrer des problèmes avec l’implémentation de la fenêtre contextuelle. Les problèmes d’IE sont dus au fait que, une fois que l’utilisateur s’authentifie avec son MVPD dans la fenêtre contextuelle, l’authentification Adobe Pass force une redirection de la page parente, qui est considérée comme un bloqueur de fenêtres contextuelles par Internet Explorer. L’équipe Authentification Adobe Pass **recommande d’implémenter la connexion iFrame pour Internet Explorer**.

L’exemple de code présenté dans cette note technique utilise une implémentation hybride d’iFrame et d’une fenêtre contextuelle : ouverture d’un iFrame sur Internet Explorer et d’une fenêtre contextuelle sur les autres navigateurs.

Étant donné qu’une implémentation iFrame existe déjà, la première partie de la note technique présente le code de l’implémentation iFrame et la seconde partie présente les modifications pour prendre en compte l’implémentation de la fenêtre contextuelle comme valeur par défaut.


## Sélecteur MVPD avec page de connexion dans un iFrame {#mvpd-pickr-iframe}

Les exemples de code précédents illustraient une page HTML contenant la balise &lt;div> dans laquelle l’iFrame doit être créé avec le bouton de fermeture de l’iFrame :

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

Voici le code **JavaScript** associé :

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

Comme nous n’utiliserons plus d’iFrame **iFrame**, le code HTML ne contiendra pas l’iFrame ni le bouton de fermeture de l’iFrame. La balise div qui contenait auparavant l’iFrame (**mvpddiv**) est conservée et utilisée pour les opérations suivantes :

* pour informer l’utilisateur que la page de connexion de MVPD est déjà ouverte en cas de perte du focus de la fenêtre contextuelle :
* pour fournir un lien afin de réactiver le focus sur la fenêtre contextuelle :

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

La liste des fichiers MVPD s’affiche dans la balise div appelée **picker** sous la forme d’une **select-mvpdList**.

Un nouveau rappel API sera utilisé : **setConfig(configXML)**. Le rappel est déclenché après l’appel de la fonction setRequestor(requestorID). Ce rappel renvoie la liste des fichiers MVPD intégrés à l’ID de demandeur précédemment défini. Dans la méthode de rappel , le code XML entrant est analysé, et la liste des fichiers MVPD est mise en cache. Le sélecteur MVPD est également créé, mais ne s’affiche pas.

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

Une fois la fonction getAuthentication() ou getAuthorization() appelée, le rappel displayProviderDialog() est déclenché. Normalement, à l’intérieur de ce rappel, la liste MVPD aurait été créée et affichée. Étant donné que le sélecteur MVPD est déjà créé, il ne reste plus qu’à l’afficher à l’utilisateur ou à l’utilisatrice.

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

Une fois que l’utilisateur a sélectionné un MVPD dans le sélecteur, la fenêtre contextuelle doit être créée. Certains navigateurs peuvent bloquer la fenêtre contextuelle si elle est créée avec about:blank ou avec une page qui se trouve sur un autre domaine. Par conséquent, il est recommandé de l’ouvrir avec le nom d’hôte à partir duquel AccessEnabler est chargé.

Dans l’implémentation d’iFrame, la réinitialisation du flux d’authentification était effectuée par le bouton btnCloseIframe et la fonction JavaScript closeIframeAction(), mais il n’est plus possible de décorer l’iFrame. Ainsi, le même comportement est obtenu en surveillant le moment où la fenêtre contextuelle est fermée (soit par l’utilisateur ou l’utilisatrice, soit en terminant le flux d’authentification). Un fragment de code a été ajouté, ce qui permet également d’éviter que l’utilisateur ou l’utilisatrice ne perde le focus sur la fenêtre contextuelle :

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

Sur le rappel createIFrame() , la balise **mvpddiv** div s’affiche.

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
>* L’exemple de code contient une variable codée en dur pour l’ID de demandeur utilisé - « REF » qui doit être remplacé par un ID de demandeur de programmeur réel.
>* L’exemple de code s’exécute correctement uniquement à partir d’un domaine placé sur la liste autorisée associé à l’ID du demandeur utilisé.
>* Comme l’ensemble du code peut être téléchargé, le code présenté dans cette note technique a été tronqué. Pour obtenir un exemple complet, consultez **Exemple iFrame JS vs Popup**.
>* Les bibliothèques JavaScript externes ont été liées depuis [Google Hosted Services](https://developers.google.com/speed/libraries/).
