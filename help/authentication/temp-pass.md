---
title: Temp pass
description: Temp pass
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2243'
ht-degree: 0%

---

# Temp pass {#temp-pass}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Résumé des fonctionnalités {#tempass-featur-summary}

Temp Pass permet aux programmeurs d’offrir un accès temporaire à leur contenu protégé, pour les utilisateurs qui ne disposent pas d’informations d’identification de compte avec un MVPD.  Temp Pass comprend les fonctionnalités suivantes :

* Temp Pass peut être configuré de manière à fournir un accès temporaire pour couvrir divers scénarios, notamment :
   * Un programmeur peut proposer un court aperçu de l&#39;un de ses sites tous les jours (10 minutes, par exemple).
   * Un programmeur peut offrir une seule et longue présentation (par exemple, quatre heures) d&#39;un grand événement sportif comme les Jeux Olympiques, ou la folie de la Marche de NCAA.
   * Un programmeur peut fournir une combinaison des deux scénarios précédents ; par exemple, une période d’affichage initiale plus longue un jour, suivie d’une série de courtes périodes qui se répètent tous les jours pendant un certain nombre de jours consécutifs.
* Les programmeurs spécifient la durée (durée de vie, ou TTL) de leur Temp Pass.
* Temp Pass fonctionne par demandeur.  Par exemple, NBC peut configurer un laissez-passer temporaire de 4 heures pour le demandeur &quot;NBCOlympics&quot;.
* Les programmeurs peuvent réinitialiser tous les jetons accordés à un demandeur particulier.  Le &quot;MVPD temporaire&quot; utilisé pour implémenter la transmission temporaire doit être configuré avec l’option &quot;Authentification par demandeur&quot; activée.
* **L’accès à Temp Pass est accordé à des utilisateurs individuels sur des appareils spécifiques**. Une fois que l’accès à Temp Pass expire pour un utilisateur, celui-ci ne pourra pas obtenir un accès temporaire sur le même appareil tant que le [jeton d’autorisation](/help/authentication/glossary.md#authz-token) expiré de cet utilisateur n’aura pas été effacé du serveur d’authentification Adobe Pass.


>[!NOTE]
>
>Temp Pass fait partie du package de workflow Premium. Contactez votre représentant commercial Adobe Pass si vous souhaitez utiliser cette fonctionnalité.

## Détails des fonctionnalités {#tempass-featur-details}

* **Mode de calcul du temps d’affichage** : la durée de validité d’une transmission temporaire n’est pas corrélée au temps passé par un utilisateur à visionner le contenu sur l’application du programmeur.  Lors de la demande d’autorisation initiale de l’utilisateur via Temp Pass, un délai d’expiration est calculé en ajoutant l’heure de requête actuelle initiale au délai d’activation spécifié par le programmeur. Ce délai d’expiration est associé à l’identifiant de l’appareil de l’utilisateur et à l’identifiant du demandeur du programmeur, et stocké dans la base de données d’authentification Adobe Pass. Chaque fois que l’utilisateur tente d’accéder au contenu à l’aide de Temp Pass à partir du même appareil, l’authentification Adobe Pass compare le temps de demande du serveur au temps d’expiration associé à l’identifiant de l’appareil de l’utilisateur et à l’identifiant du demandeur du programmeur. Si le délai de demande du serveur est inférieur au délai d’expiration, l’autorisation est accordée ; dans le cas contraire, l’autorisation est refusée.
* **Paramètres de configuration** - Les paramètres Temp Pass suivants peuvent être spécifiés par un programmeur pour créer une règle Temp Pass :
   * **Jeton TTL** : durée pendant laquelle un utilisateur est autorisé à regarder sans se connecter à un MVPD. Cette fois-ci est basée sur l’horloge et expire si l’utilisateur regarde du contenu ou non.
  >[!NOTE]
  >Un ID de demandeur ne peut pas être associé à plusieurs règles de transmission temporaire.
* **Authentification/autorisation** - Dans le flux de transmission temporaire, vous spécifiez le MVPD comme &quot;Temp Pass&quot;.  L’authentification Adobe Pass ne communique pas avec un MVPD réel dans le flux de transmission temporaire. Le MVPD &quot;Temp Pass&quot; autorise donc toute ressource. Les programmeurs peuvent spécifier une ressource accessible à l’aide de Temp Pass comme ils le font pour le reste des ressources de leur site. La bibliothèque du vérificateur multimédia peut être utilisée comme d’habitude pour vérifier le jeton multimédia court Temp Pass et appliquer la vérification des ressources avant la lecture.
* **Suivi des données dans le flux de transmission temporaire** - Deux points concernant le suivi des données lors d’un flux de droits de transmission temporaire :
   * L’ID de suivi transmis de l’authentification Adobe Pass à votre rappel **sendTrackingData()** est un hachage de l’ID de périphérique.
   * Puisque l’identifiant MVPD utilisé dans le flux de transmission temporaire est &quot;Temp Pass&quot;, ce même identifiant MVPD est transmis à **sendTrackingData()**. La plupart des programmeurs voudront probablement traiter les mesures de transfert temporaire différemment des mesures MVPD réelles. Cela nécessite un travail supplémentaire dans votre mise en oeuvre d’Analytics.

L’illustration suivante présente le flux de transmission temporaire :

![Flux de transmission temporaire](assets/temp-pass-flow.png)

*Figure : Flux de transmission temporaire*

## Mise en oeuvre de Temp Pass {#implement-tempass}

Côté authentification Adobe Pass, Temp Pass est implémenté avec l’ajout d’un pseudo-MVPD nommé &quot;TempPass&quot; à la configuration du serveur du programmeur participant.  Ce pseudo-MVPD agit comme un MVPD réel qui accorde temporairement l&#39;accès au contenu protégé du programmeur.

Côté programmeur, Temp Pass est implémenté comme suit pour les deux scénarios que les MVPD utilisent pour l’authentification :

* **iFrame sur la page du programmeur**. Temp Pass fonctionne indépendamment du type d’authentification d’un MVPD, mais pour le scénario iFrame, des étapes supplémentaires sont requises pour annuler le flux d’authentification actuel et s’authentifier avec Temp Pass. Ces étapes sont présentées dans la [connexion à iFrame](/help/authentication/temp-pass.md) ci-dessous.
* **Rediriger vers la page de connexion MVPD**. Dans le cas plus classique où l’interface utilisateur de déclenchement de la transmission temporaire est présentée avant de commencer l’authentification avec un MVPD, aucune étape spéciale n’est à prendre. Temp Pass doit être traité comme un MVPD normal.

Les points suivants s’appliquent aux deux scénarios de mise en oeuvre :

* Le &quot;Temp Pass&quot; ne doit s’afficher dans le sélecteur MVPD que pour les utilisateurs qui n’ont pas encore demandé d’autorisation de Temp Pass. Le blocage de l’affichage pour les requêtes suivantes peut être réalisé en conservant un indicateur sur les cookies. Cela fonctionne tant que l’utilisateur n’efface pas le cache du navigateur. Si les utilisateurs effacent la mise en cache de leur navigateur, &quot;Temp Pass&quot; (Transmettre temporaire) s’affiche à nouveau dans le sélecteur et l’utilisateur peut le demander à nouveau. L’accès n’est accordé que si l’heure &quot;Temp Pass&quot; n’a pas encore expiré.
* Lorsqu’un utilisateur demande l’accès via Temp Pass, le serveur d’authentification Adobe Pass n’effectue pas sa requête SAML (Security Assertion Markup Language) habituelle au cours du processus d’authentification. Au lieu de cela, le point de terminaison d’authentification renvoie une réussite chaque fois qu’il est appelé alors que les jetons sont valides pour l’appareil.
* Lorsqu’un Temp Pass expire, son utilisateur ne sera plus authentifié, car dans le flux Temp Pass, le jeton d’authentification et le jeton d’autorisation ont la même date d’expiration. Pour expliquer aux utilisateurs que leur Temp Pass a expiré, les programmeurs doivent récupérer le MVPD sélectionné juste après avoir appelé `setRequestor()`, puis appeler `checkAuthentication()` comme d’habitude. Dans le rappel `setAuthenticationStatus()`, une vérification peut être effectuée pour déterminer si l’état d’authentification est 0. Ainsi, si le MVPD sélectionné était &quot;TempPass&quot; (TempPass), un message peut être présenté aux utilisateurs pour indiquer que leur session de Temp Pass a expiré.
* Si un utilisateur supprime le jeton Temp Pass avant expiration, les vérifications de droits suivantes génèrent un jeton dont le délai d’activation est égal au temps restant.
* Si l’utilisateur supprime le jeton Temp Pass après expiration, les vérifications de droits suivantes renvoient &quot;user not authorized&quot;.

Consultez les exemples de [code d’exemple](/help/authentication/temp-pass.md#tempass-sample-code) ci-dessous pour obtenir des exemples de codage des détails de mise en oeuvre décrits dans cette section.

## Exemple de code {#tempass-sample-code}

Les sections ci-dessous montrent comment appeler l’API d’authentification Adobe Pass pour mettre en oeuvre le flux de transmission temporaire :

* [Exemple de connexion iFrame](/help/authentication/temp-pass.md#iframe-login-sample)
* [Exemple de connexion automatique](/help/authentication/temp-pass.md#auto-login-sample)

### Exemple de connexion iFrame {#iframe-login-sample}

Cet exemple montre comment mettre en oeuvre Temp Pass pour les cas où les MVPD prennent en charge l’intégration d’iFrame :

```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript" src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var ae, ifrm, providersMenu, previousSelectedProvider;
        var tempassSelected = false;
 
        $(document).ready(function() {
            ifrm = $('#ifrm');
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {wmode: "transparent", allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded() {
            ae = $('#accessEnabler')[0];
            ae.setProviderDialogURL("none");
            ae.setRequestor("sample_requestor_Id");
            previousSelectedProvider = ae.getSelectedProvider(); 
            ae.checkAuthentication();
        }
 
        function createIFrame() {
            providersMenu.hide();
 
            // If the user already used TempPass once, hide the button
            if ($.cookie("TPSelected") == "1"){
                $('#tempassBtn').hide();
            }
            ifrm.show();
        }
 
        function displayProviderDialog(providers) {
            if (tempassSelected) {
                // Remember in a cookie that the user selected temp pass
                $.cookie("TPSelected", "1", {expires: 366, path: '/'});
 
                // Authenticate with temp pass
                ae.setSelectedProvider("TempPass");
            } else {
                $('#loginBtn').hide();
                providersMenu = $('<select></select>');
 
                providersMenu.change(function(event){
                    ae.setSelectedProvider(event.target.value);
                });
 
                $.each(providers, function(k, v) {
                    // Add the MVPDs to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if(v.ID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:v.ID}).text(v.displayName));                       
                    }
                });
                $('body').append(providersMenu);
            }
        }
 
        function setAuthenticationStatus(status, code) {
            loginBtn = $('#loginBtn');
            logoutBtn = $('#logoutBtn');
            console.log(previousSelectedProvider);
 
            if (status == 1) {
                $('#selectedProvider').text("Authenticated with " + ae.getSelectedProvider().MVPD + "   ");
                loginBtn.hide();
                logoutBtn.show();
 
                // Get authorization
                ae.getAuthorization("sample_requestor_Id");
            } else {
                // If selected provider is TempPass but the user is not authenticated,
                //   infer that the TempPass period has expired, so reset the MVPD selection
                if (previousSelectedProvider && previousSelectedProvider.MVPD == "TempPass") {
                    previousSelectedProvider = null;
                    ae.setSelectedProvider(null);
                    alert("Your Temp Pass has expired, please login with your regular cable provider!");
                }
                loginBtn.show();
                logoutBtn.hide();
            }
        }
 
        function selectTempPass() {
            ifrm.hide();
 
            // Signal the fact that the user selected temp pass
            tempassSelected = true;
 
            // Cancel the current authentication flow
            ae.setSelectedProvider(null);
 
            // Retry authentication
            ae.getAuthentication();
 
        }
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="ae.getAuthentication();">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
           style="display: none" onclick="ae.logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px; width: 400px; height: 400px; border: 2px solid red;">
        <button id="tempassBtn"
           onclick="selectTempPass();"
             style="float:left">Don't know your credentials? Click here to get a Temp Pass.
        </button>
        <button onclick="window.location.reload()" style="float:right">X</button>
        <br />
        <hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="90%" height="90%" frameborder="0"></iframe>
    </div>
    <br/>
    <div id="ae" style="display: none">
        <p>Loading Access Enabler...</p>
    </div>
</body>
</html>
```

#### Cas d’utilisation de la connexion iFrame {#iframe-login-use-cases}

**Pour demander une transition temporaire pour la première fois :**

1. Un utilisateur accède à la page du programmeur et clique sur le lien de connexion.
1. Le sélecteur MVPD s’ouvre et l’utilisateur choisit un MVPD dans la liste.
1. L’iFrame d’authentification s’affiche. Cet iFrame contient un lien &quot;Temp Pass&quot;.
1. L’utilisateur clique sur &quot;Temp Pass&quot;, de sorte que le programmeur ajoute un indicateur à un cookie pour empêcher l’utilisateur de voir le lien &quot;Temp Pass&quot; lors des visites ultérieures sur la page.
1. La demande d’authentification Temp Pass atteint les serveurs d’authentification Adobe Pass et génère un jeton d’authentification. La durée de vie est égale à la période définie par le programmeur pour la transmission temporaire.
1. La demande d’autorisation Temp Pass atteint les serveurs d’authentification Adobe Pass.
1. Les serveurs d’authentification Adobe Pass extraient les identifiants de l’appareil et du demandeur de la requête et les stockent dans la base de données avec le délai d’expiration. Le délai d’expiration est calculé comme suit : le délai initial de la demande de transmission temporaire plus le délai d’expiration (spécifié par le programmeur).
1. Les serveurs d’authentification Adobe Pass génèrent un jeton d’autorisation.
1. L’utilisateur accède au contenu protégé.

**Pour demander à nouveau une transmission temporaire après qu’un utilisateur de transmission temporaire qui revient a supprimé les cookies de navigateur :**

1. L’utilisateur accède à la page du programmeur et clique sur le lien de connexion.
1. Le sélecteur MVPD s’ouvre et l’utilisateur choisit un MVPD dans la liste.
1. L’iFrame d’authentification s’affiche. Cet iFrame contient un lien &quot;Temp Pass&quot; (l’utilisateur a supprimé le cookie d’origine, de sorte que le programmeur ne sait pas si l’utilisateur a déjà cliqué sur le lien &quot;Temp Pass&quot;).
1. L’utilisateur clique à nouveau sur &quot;Temp Pass&quot; (Passe temporaire), de sorte que le programmeur ajoute un indicateur à un cookie, afin d’empêcher l’utilisateur de voir le lien &quot;Temp Pass&quot; lors des visites ultérieures sur la page.
1. La demande d’authentification Temp Pass atteint les serveurs d’authentification Adobe Pass, qui génèrent un jeton d’authentification. La durée de vie (TTL) est désormais le temps restant pour la transmission temporaire (la différence entre l’heure actuelle et le temps d’expiration associé à l’identifiant de l’appareil).
1. La demande d’autorisation Temp Pass atteint les serveurs d’authentification Adobe Pass.
1. Les serveurs d’authentification Adobe Pass extraient les identifiants de l’appareil et du demandeur de la requête et les utilisent pour récupérer le délai d’expiration de la base de données d’authentification Adobe Pass. L’heure actuelle est comparée à l’heure d’expiration.
1. Si le Temp Pass de l’utilisateur n’a pas expiré, les serveurs d’authentification Adobe Pass génèrent un jeton d’autorisation.
1. Si la transmission temporaire de l’utilisateur n’a pas expiré, l’utilisateur pourra accéder au contenu protégé.

### Exemple de connexion automatique {#auto-login-sample}

L’exemple suivant illustre un cas où un utilisateur est automatiquement connecté avec TempPass lors de sa visite sur un site. L’utilisateur peut choisir de se connecter avec un MVPD standard à tout moment et est averti si TempPass a expiré :

```HTML
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript"
             src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript"
             src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript"
             src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var REQUESTOR = "REF";
        var RESOURCE = "sample_requestor_Id";
        var selectedProvider = null;
        var mvpds;
        var hasTempPassMVPD = false;
 
        // Used to cache the mvpd picker
        var picker;
 
        $(document).ready(function() {
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded(){
            console.log("AccessEnabler loaded");
            ae = $('#accessEnabler')[0];
 
            // Make sure the default picker is disabled
            ae.setProviderDialogURL("none");
 
            ae.setRequestor(REQUESTOR);
            ae.checkAuthentication();
        }
 
        /**
         * Callback received as a result of setRequestor()
         *
         * @param xml object holding the configuration for the current REQUESTOR
         * including the MVPD list
         */
        function setConfig(config) {
            // Save the mvpd list
            var mvpdList = $.parseXML(config);
            mvpds = $(mvpdList).find('mvpd');
 
            // Create the picker only once and cache it
            if(!picker) {
                picker = $('<div id="mvpdPicker"/>');
 
                var providersMenu = $('<select id="mvpdList" multiple></select>');
 
                $.each(mvpds, function(k, v) {
                    var mvpdID = $(v).find("id").text();
                    var mvpdName = $(v).find("displayName").text();
 
                    // Add the mvpd's to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if (mvpdID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:mvpdID}).text(mvpdName));
                    } else {
                        hasTempPassMVPD = true;
                    }
                });
                picker.append(providersMenu);
                picker.append($('<br/>'));
                picker.append($('<input type="button" onclick="login()" value="login" />'));
                picker.append($('<input type="button" onclick="cancelPicker()" value="cancel" />'));                  
            }
 
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");             
            }
        }
 
        /**
         * Callback triggered for iFramed MVPD's
         */
        function createIFrame() {
            $('#mvpdPicker').remove();
            $('#ifrm').show();
        }
 
        /**
         * Hides the MVPD picker
         * when the user clicks "Cancel"
         */
        function cancelPicker() {
            $('#video').show();
            $('#mvpdPicker').remove();
            $('#loginBtn').show();
        }
 
        /**
         * Pops up the MVPD picker
         */
        function showPicker() {
            $('#video').hide();
            $('#loginBtn').hide();
            $('body').append(picker);
        }
 
        function logout() {
            $.removeCookie('tempPassUsed');
            ae.logout();
        }
 
        /**
         * Performs login with the selected MVPD
         */
        function login() {
            selectedProvider = $('#mvpdList').val()[0];
 
            // Make sure we clear out previously
            // selected. This is a must if we want to force
            // login with a real MVPD while still logged in with
            // TempPass, without doing an ae.logout()
            ae.setSelectedProvider(null);
            ae.getAuthentication();
        }

        /**
         * Callback triggered by AccessEnabler. This is usually
         * triggered in order to display the MVPD picker, but
         * since we already constructed, cached, and displayed the
         * picker, and the user already picked the MVPD, we don't need
         * to do anything here but state management
         */
        function displayProviderDialog() {
            // If the selected MVPD is TempPass
            // store this fact in a cookie,
            // otherwise clear it
            if (selectedProvider != 'TempPass') {
                $.removeCookie('tempPassUsed');
            } else {
                $.cookie("tempPassUsed", 1);
            }
 
            // Since the picker was already shown
            // and the user picked an MVPD,
            // just proceed to login
            ae.setSelectedProvider(selectedProvider);
        }
 
        function setAuthenticationStatus(status, code) {
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");
            } else if(status == 1) {
                selectedProvider = ae.getSelectedProvider().MVPD;
                $('#selectedProvider').text("Authenticated with " + selectedProvider + "   ");
 
                // If authenticated with TempPass
                // allow the user to login with
                // a real MVPD
                if (selectedProvider == "TempPass") {
                    $('#loginBtn').show();
                    $('#logoutBtn').hide();
                } else {
                    $('#loginBtn').hide();
                    $('#logoutBtn').show();
                }
 
                // Get authorization
                // Note: This is mandatory in order to "start" the temp pass countdown
                ae.checkAuthorization(RESOURCE);
            } else if(code != "Provider not Selected Error") {
                // Auto-authenticate with TempPass only if we infer
                // that TempPass has not expired, otherwise we
                // inform the user that TempPass has expired
                if ($.cookie('tempPassUsed') == 1) {
                   $('#selectedProvider').text("Your Temp Pass has expired, please log in with your cable provider!");
                   $('#logoutBtn').show();
                   showPicker();
                } else {
                    selectedProvider = 'TempPass';
                    ae.getAuthentication();
                }
            }
        }
 
        /**
         * Displays the picker as a result
         * of user action
         */
        function loginClicked() {
            $('#loginBtn').hide();
            showPicker();
        }
 
        /**
         * Callback triggered in case of authorization success
         */
        function setToken(token) {
            console.log(token);
            $('#video').html('<img src=">');
        }
 
        /**
         * Callback triggered in case of authz failure
         */
        function tokenRequestFailed(resource, status, message) {
            console.log(resource);
            $('#video').html('<p style="color: red">' + status + ': ' + message + '</p>');
        }
 
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="loginClicked()">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
        style="display: none" onclick="logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px;
         width: 400px; height: 400px; border: 2px solid red;">
        <button onclick="window.location.reload()" style="float:right">Close this window</button>
        <br /><hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="80%" height="80%" frameborder="0"></iframe>  
    </div>
    <br/>
 
    <div id="video"></div>
    <div id="ae" style="display: none"><p>Loading Access Enabler...</p></div>
</body>
</html>
```

## Utilisation de plusieurs variables temporaires {#use-mult-tempass}

Certains événements nécessitent un accès libre et échelonné au contenu, comme un intervalle initial de libre accès (par exemple, 4 heures), suivi d’un accès gratuit quotidien (par exemple, 10 minutes chaque jour suivant).  Pour qu’un programmeur puisse mettre en oeuvre ce scénario, il doit l’organiser avec son contact d’Adobe afin de configurer deux MVPD temporaires pour le programmeur.

Pour cet exemple de scénario (une session gratuite initiale de 4 heures, suivie de sessions gratuites quotidiennes de 10 minutes), l’Adobe configure un MVPD appelé TempPass1 avec une durée de vie (TTL) de 4 heures et un TempPass2 avec une durée de vie de 10 minutes pour la période suivante.  Ces deux éléments sont associés à l’identifiant du demandeur du programmeur.

### Implémentation du programmeur {#mult-tempass-prog-impl}

Une fois que Adobe configure les deux instances TempPass, les deux MVPD supplémentaires (TempPass1 et TempPass2) apparaissent dans la liste MVPD du programmeur.  Le programmeur doit effectuer les étapes suivantes pour mettre en oeuvre les plusieurs passes temporaires :

1. Lors de la première visite de l’utilisateur sur le site, connectez-vous automatiquement avec TempPass1. Vous pouvez utiliser l’ exemple d’authentification ci-dessus comme point de départ pour cette tâche.
1. Lorsque vous détectez que TempPass1 a expiré, stockez le fait (dans un cookie/stockage local) et présentez l’utilisateur à votre sélecteur MVPD standard. **Veillez à filtrer TempPass1 et TempPass2 de cette liste**.
1. Chaque jour suivant, si TempPass1 expire, authentifiez cet utilisateur avec TempPass2.
1. Une fois que TempPass2 a expiré, stockez le fait (dans un cookie/stockage local) pour la journée et présentez l’utilisateur à votre sélecteur MVPD standard. Encore une fois, veillez à filtrer TempPass1 et TempPass2 de cette liste.
1. Chaque nouveau jour, à 00:00 heures, réinitialisez toutes les transmissions temporaires pour TempPass2, à l’aide de l’ [ API Web Reset TempPass](/help/authentication/temp-pass.md#reset-all-tempass).

>[!NOTE]
>**Note de programmation :** L’authentification Adobe Pass ne dispose pas d’un mécanisme intégré pour arrêter la diffusion en continu libre après les 10 minutes écoulées.  C&#39;est aux programmeurs de restreindre l&#39;accès une fois que TempPass2 arrive à expiration. Pour ce faire, les programmeurs peuvent implémenter dans leurs sites/applications un appel &quot;checkAuthorization&quot; toutes les X minutes, où X est la période que le programmeur détermine comme logique pour leurs applications.

## Réinitialiser toutes les transitions temporaires {#reset-all-tempass}

Certaines règles de fonctionnement nécessitent une purge régulière de Temp Pass ou une réinitialisation ad hoc de toutes les Temp Passes émises pour un identifiant de demandeur et un identifiant MVPD particulier. Cette fonctionnalité prend en charge des cas d’utilisation tels que les suivants :

* Une carte temporaire quotidienne de 10 minutes (la carte temporaire doit être réinitialisée au début de la journée)
* Un Temp Pass est disponible pour tous les utilisateurs lors des dernières nouvelles. (La transmission temporaire doit être réinitialisée pour tous les appareils dès que les dernières nouvelles démarrent.)
* Le scénario de plusieurs Temp Pass qui fournit une combinaison d’une période d’affichage initiale d’une certaine durée, suivie de périodes quotidiennes ultérieures d’une autre durée.

Pour réinitialiser toutes les transmissions temporaires, Adobe Pass Authentication fournit aux programmeurs une API web *public* :

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>L’URL ci-dessus remplace l’API de réinitialisation précédente. L’ancienne API reset (v1) n’est plus prise en charge.

* **Protocole :** HTTPS
* **Hôte :**
   * Version : mgmt.auth.adobe.com
   * Préqualification - mgmt-prequal.auth.adobe.com
* **Chemin d’accès :** /reset-tempass/v2/reset
* **Paramètres de requête :** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **En-têtes :** ApiKey - 1232293681726481
* **Réponse :**
   * Succès - HTTP 204
   * Échec :
      * HTTP 400 pour une requête incorrecte
      * HTTP 401 si l’ApiKey n’a pas été spécifié
      * HTTP 403 si l’ApiKey n’est pas valide

Par exemple :

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## Clients pris en charge {#supp-clients}

Prise en charge de l’outil de transfert et de réinitialisation de température par plateforme :

| Clients d’authentification Adobe Pass | Temp Pass | Outil Réinitialiser |
|:--------------------------------------:|:---------:|:----------:|
| JS AccessEnabler | OUI | OUI |
| IOS client natif | OUI | OUI |
| natif du client tvOS | OUI | OUI |
| Android client natif | OUI | OUI |
| Native Client fireTV | OUI | OUI |
| API sans client | OUI | OUI |

## Limites et problèmes connus {#limitations}

Cette section décrit les limites qui s’appliquent à l’implémentation actuelle de Temp Pass.

**SDK JavaScript** : prend en charge la fonctionnalité de transfert temporaire de réinitialisation à partir de la version **3.X et ultérieure**.

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
