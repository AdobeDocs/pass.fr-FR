---
title: Passage temp
description: Passage temp
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '2243'
ht-degree: 0%

---

# Passage temp {#temp-pass}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

## Résumé des fonctionnalités {#tempass-featur-summary}

Temp Pass permet aux programmeurs d&#39;offrir un accès temporaire à leur contenu protégé, pour les utilisateurs qui n&#39;ont pas d&#39;identifiants de compte avec un MVPD.  Temp Pass offre les fonctionnalités suivantes :

* La passe temporaire peut être configurée pour fournir un accès temporaire afin de couvrir divers scénarios, notamment les suivants :
   * Un programmeur peut proposer une courte présentation quotidienne (par exemple, un aperçu de 10 minutes) de l’un de ses sites.
   * Un programmeur peut offrir une seule longue présentation (par exemple, quatre heures) d&#39;un événement sportif majeur tel que les Jeux olympiques ou la NCAA March Madness.
   * Un programmeur peut fournir une combinaison des deux scénarios précédents ; par exemple, une période de visionnage initiale plus longue d’un jour, suivie d’une série de courtes périodes qui se répètent quotidiennement pendant un certain nombre de jours suivants.
* Les programmeurs spécifient la durée (Time-To-Live, ou TTL) de leur passe de temp.
* La passe temporaire fonctionne par demandeur.  Par exemple, NBC pourrait établir un laissez-passer temporaire de 4 heures pour le demandeur « NBCOlympics ».
* Les programmeurs peuvent réinitialiser tous les jetons accordés à un demandeur particulier.  Le « MVPD temporaire » utilisé pour implémenter la passe temporaire doit être configuré avec « Authentification par demandeur » activé.
* **L’accès de passe temporaire est accordé à des utilisateurs individuels sur des appareils spécifiques**. Une fois que l’accès de passe temporaire a expiré pour un utilisateur, celui-ci ne peut plus obtenir d’accès temporaire sur le même appareil tant que le jeton d’autorisation arrivé à expiration de cet utilisateur n’a pas été effacé du serveur d’authentification Adobe Pass.


>[!NOTE]
>
>La passe temporaire fait partie du package de workflow Premium . Contactez votre représentant Adobe Pass si vous souhaitez utiliser cette fonctionnalité.

## Détails des fonctionnalités {#tempass-featur-details}

* **Calcul de la durée de visionnage** - La durée de validité d’une passe de temp n’est pas corrélée à la durée de visionnage du contenu de l’application du programmeur par l’utilisateur.  Lors de la demande initiale d’autorisation de l’utilisateur via un Temp Pass, un délai d’expiration est calculé en ajoutant le délai de demande actuel initial à la TTL spécifiée par le programmeur. Ce délai d’expiration est associé à l’ID d’appareil de l’utilisateur et à l’ID de demandeur du programmeur, et stocké dans la base de données d’authentification d’Adobe Pass. Chaque fois que l’utilisateur ou l’utilisatrice tente d’accéder au contenu à l’aide d’un Temp Pass sur le même appareil, l’authentification Adobe Pass compare le temps de requête au serveur avec le temps d’expiration associé à l’identifiant de l’appareil de l’utilisateur ou de l’utilisatrice et à l’identifiant du demandeur du programmeur. Si le délai de requête du serveur est inférieur au délai d’expiration, l’autorisation est accordée ; dans le cas contraire, l’autorisation est refusée.
* **Paramètres de configuration** - Les paramètres de passe temporaire suivants peuvent être spécifiés par un programmeur pour créer une règle de passe temporaire :
   * **TTL de jeton** - Durée pendant laquelle un utilisateur est autorisé à regarder sans se connecter à un MVPD. Cette durée est basée sur l’horloge et expire, que l’utilisateur regarde du contenu ou non.
  >[!NOTE]
  >Un ID de demandeur ne peut pas être associé à plusieurs règles de transfert temporaire.
* **Authentification/Autorisation** - Dans le flux de passe temporaire, vous spécifiez le MVPD en tant que « passe temporaire ».  L’authentification Adobe Pass ne communique pas avec un MVPD réel dans le flux de passe temporaire. Par conséquent, le MVPD « Passe temporaire » autorise toute ressource. Les programmeurs peuvent spécifier une ressource accessible à l’aide de Temp Pass comme ils le font pour le reste des ressources de leur site. La bibliothèque du vérificateur de médias peut être utilisée comme d’habitude pour vérifier le jeton de média court Temp Pass et appliquer la vérification des ressources avant la lecture.
* **Données de suivi dans le flux de passe temporaire** - Deux points concernant les données de suivi pendant un flux de droits de passe temporaire :
   * L’identifiant de suivi transmis par l’authentification Adobe Pass à votre rappel **sendTrackingData()** est un hachage de l’identifiant de l’appareil.
   * Comme l’identifiant MVPD utilisé dans le flux de passe temporaire est « Temp Pass », ce même identifiant MVPD est renvoyé à **sendTrackingData()**. La plupart des programmeurs voudront probablement traiter les mesures de transfert temporaire différemment des mesures MVPD réelles. Cela nécessite un travail supplémentaire dans votre implémentation d’analyses.

L’illustration suivante présente le flux de passe temporaire :

![Flux de transfert temporaire](../../../assets/temp-pass-flow.png)

*Image : flux de transfert temporaire*

## Implémenter la passe temporaire {#implement-tempass}

Du côté de l’authentification Adobe Pass, la passe temporaire est implémentée avec l’ajout d’un pseudo-MVPD nommé « TempPass » à la configuration du serveur du programmeur participant.  Ce pseudo-MVPD agit comme un véritable MVPD qui accorde temporairement l’accès au contenu protégé du programmeur.

Du côté du programmeur, la passe temporaire est implémentée comme suit pour les deux scénarios que les MVPD utilisent pour l’authentification :

* **iFrame sur la page du programmeur**. La passe temporaire fonctionne quel que soit le type d’authentification MVPD. Cependant, pour le scénario iFrame, des étapes supplémentaires sont nécessaires pour annuler le flux d’authentification actuel et s’authentifier avec la passe temporaire. Ces étapes sont présentées dans la section [Connexion iFrame](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) ci-dessous.
* **Rediriger vers la page de connexion de MVPD**. Dans le cas plus traditionnel où l’interface utilisateur permettant de déclencher la passe temporaire est présentée avant de commencer l’authentification avec un MVPD, aucune étape particulière n’est à effectuer. Temp Pass doit être traité comme un MVPD normal.

Les points suivants s’appliquent aux deux scénarios d’implémentation :

* Le « Temp Pass » ne doit s’afficher dans le sélecteur MVPD que pour les utilisateurs et utilisatrices qui n’ont pas encore demandé d’autorisation de Temp Pass. Le blocage de l’affichage pour les requêtes suivantes peut être effectué en conservant un indicateur sur les cookies. Cela fonctionne tant que l’utilisateur n’efface pas la mémoire cache du navigateur. Si les utilisateurs et utilisatrices effacent leurs caches de navigateur, la « passe temporaire » réapparaît dans le sélecteur et l’utilisateur ou l’utilisatrice peut la demander à nouveau. L’accès ne sera accordé que si le délai de « passe temporaire » n’a pas encore expiré.
* Lorsqu’un utilisateur ou une utilisatrice demande l’accès via une passe temporaire, le serveur d’authentification Adobe Pass n’effectue pas sa demande SAML (Security Assertion Markup Language) habituelle pendant le processus d’authentification. Au lieu de cela, le point d’entrée d’authentification renvoie un succès chaque fois qu’il est appelé alors que les jetons sont valides pour l’appareil.
* Lorsqu’une passe temporaire expire, son utilisateur n’est plus authentifié, car dans le flux de passe temporaire, le jeton d’authentification et le jeton d’autorisation ont la même date d’expiration. Afin d’expliquer aux utilisateurs que leur passe temporaire a expiré, les programmeurs doivent récupérer le MVPD sélectionné juste après l’avoir `setRequestor()`, puis appeler `checkAuthentication()` comme d’habitude. Dans le rappel `setAuthenticationStatus()`, une vérification peut être effectuée pour déterminer si le statut d’authentification est 0, de sorte que si le MVPD sélectionné était « TempPass », un message peut être présenté aux utilisateurs indiquant que leur session de TempPass a expiré.
* Si un utilisateur supprime le jeton de passe temporaire avant l’expiration, les vérifications d’autorisations suivantes génèrent un jeton dont la durée de vie est égale au temps restant.
* Si l’utilisateur ou l’utilisatrice supprime le jeton de passe temporaire après expiration, les vérifications de droits suivantes renvoient la valeur « utilisateur non autorisé ».

Consultez les exemples dans [Exemple de code](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#tempass-sample-code) ci-dessous pour obtenir des exemples de codage des détails d’implémentation décrits dans cette section.

## Exemple de code {#tempass-sample-code}

Les sections ci-dessous montrent comment appeler l’API d’authentification Adobe Pass pour implémenter le flux de transfert temporaire :

* [Exemple de connexion iFrame](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#iframe-login-sample)
* [Exemple de connexion automatique](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#auto-login-sample)

### Exemple de connexion iFrame {#iframe-login-sample}

Cet exemple montre comment implémenter la passe temporaire dans le cas où les fichiers MVPD prennent en charge l’intégration iFrame :

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

**Pour demander une passe temporaire pour la première fois :**

1. Un utilisateur accède à la page du programmeur et clique sur le lien de connexion.
1. Le sélecteur MVPD s’ouvre et l’utilisateur choisit un MVPD dans la liste.
1. L’iFrame d’authentification s’affiche. Cet iFrame contient un lien « Temp Pass ».
1. L’utilisateur clique sur « Temp Pass », de sorte que le programmeur ajoute un indicateur à un cookie pour empêcher l’utilisateur de voir le lien « Temp Pass » lors de ses visites ultérieures sur la page.
1. La demande d’authentification Temp Pass atteint les serveurs d’authentification d’Adobe Pass qui génèrent un jeton d’authentification. La TTL est égale à la période définie par le programmeur pour la passe temporaire.
1. La demande d’autorisation de transfert temporaire atteint les serveurs d’authentification d’Adobe Pass.
1. Les serveurs d’authentification d’Adobe Pass extraient les identifiants d’appareil et de demandeur de la requête et les stockent dans la base de données avec le délai d’expiration. Le délai d’expiration est calculé comme suit : délai de requête initial de transfert temporaire plus la TTL (spécifiée par le programmeur).
1. Les serveurs d’authentification d’Adobe Pass génèrent un jeton d’autorisation.
1. L’utilisateur accède au contenu protégé.

**Pour demander à nouveau un Temp Pass après qu’un utilisateur Temp Pass récurrent a supprimé les cookies du navigateur, procédez comme suit**

1. L’utilisateur accède à la page du programmeur et clique sur le lien de connexion.
1. Le sélecteur MVPD s’ouvre et l’utilisateur choisit un MVPD dans la liste.
1. L’iFrame d’authentification s’affiche. Cet iFrame contient un lien « Temp Pass » (l&#39;utilisateur a supprimé le cookie d&#39;origine, ainsi le programmeur ne sait pas si l&#39;utilisateur a cliqué sur le lien « Temp Pass » auparavant).
1. L’utilisateur clique à nouveau sur « Temp Pass », de sorte que le programmeur ajoute à nouveau un indicateur à un cookie, pour empêcher l’utilisateur de voir le lien « Temp Pass » lors de ses visites ultérieures sur la page.
1. La demande d’authentification Temp Pass atteint les serveurs d’authentification d’Adobe Pass, qui génèrent un jeton d’authentification. La TTL est désormais le temps restant pour la passe de temp (la différence entre l’heure actuelle et le temps d’expiration associé à l’ID d’appareil).
1. La demande d’autorisation de transfert temporaire atteint les serveurs d’authentification d’Adobe Pass.
1. Les serveurs d’authentification Adobe Pass extraient les identifiants d’appareil et de demandeur de la requête et les utilisent pour récupérer le délai d’expiration de la base de données d’authentification Adobe Pass. L’heure actuelle est comparée à l’heure d’expiration.
1. Si la passe temporaire de l’utilisateur n’a pas expiré, les serveurs d’authentification d’Adobe Pass génèrent un jeton d’autorisation.
1. Si la passe temporaire de l’utilisateur n’a pas expiré, l’utilisateur pourra accéder au contenu protégé.

### Exemple de connexion automatique {#auto-login-sample}

L’exemple suivant illustre un cas où un utilisateur est automatiquement connecté avec TempPass lors de sa visite d’un site. L’utilisateur peut choisir de se connecter à tout moment à l’aide d’un MVPD standard. Il est averti si le TempPass a expiré :

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

## Utiliser plusieurs passes temporaires {#use-mult-tempass}

Certains événements nécessitent un accès gratuit échelonné au contenu, comme un intervalle initial d’accès gratuit (par exemple, 4 heures), suivi d’accès gratuits quotidiens (par exemple, 10 minutes chaque jour suivant).  Pour qu’un programmeur puisse mettre en œuvre ce scénario, il doit organiser ce scénario avec son contact d’Adobe afin de configurer deux MVPD temporaires pour le programmeur.

Dans cet exemple de scénario (une session gratuite initiale de 4 heures, suivie de sessions libres quotidiennes de 10 minutes), l’Adobe configure un MVPD appelé TempPass1 avec une durée de vie (TTL) de 4 heures et un TempPass2 avec une durée de vie de 10 minutes pour la période suivante.  Ces deux éléments sont associés à l’ID du demandeur du programmeur.

### Implémentation du programmeur {#mult-tempass-prog-impl}

Une fois que Adobe a configuré les deux instances TempPass, les deux MVPD supplémentaires (TempPass1 et TempPass2) apparaissent dans la liste MVPD du programmeur.  Le programmeur doit effectuer les étapes suivantes pour implémenter les multiples passes temporaires :

1. Lors de la première visite de l’utilisateur sur le site, connectez-le automatiquement avec TempPass1. Vous pouvez utiliser l’exemple de connexion automatique ci-dessus comme point de départ pour cette tâche.
1. Lorsque vous détectez que TempPass1 a expiré, stockez le fait (dans un cookie/stockage local) et présentez à l’utilisateur votre sélecteur MVPD standard. **Veillez à exclure TempPass1 et TempPass2 de cette liste**.
1. Chaque jour suivant, si TempPass1 expire, connectez automatiquement cet utilisateur avec TempPass2.
1. Lorsque TempPass2 expire, stockez le fait (dans un cookie/stockage local) pour la journée, et présentez à l&#39;utilisateur votre sélecteur MVPD standard. Là encore, veillez à exclure TempPass1 et TempPass2 de cette liste.
1. Chaque nouveau jour, à 00:00 heures, réinitialisez tous les laissez-passer temporaires pour TempPass2, à l’aide de l’[API web Reset TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#reset-all-tempass).

>[!NOTE]
>**Remarque sur la programmation :** l’authentification Adobe Pass ne dispose pas d’un mécanisme intégré pour arrêter la diffusion en continu gratuite après l’expiration du délai de 10 minutes.  Il appartient aux programmeurs de restreindre l’accès une fois que TempPass2 expire. Pour ce faire, les programmeurs peuvent implémenter dans leurs sites/applications un appel « checkAuthorization » toutes les X minutes, où X est la période que le programmeur détermine comme étant logique pour leurs applications.

## Réinitialiser toutes les passes temporaires {#reset-all-tempass}

Certaines règles métier nécessitent une purge régulière des laissez-passer temporaires ou une réinitialisation ad hoc de tous les laissez-passer temporaires émis pour un ID de demandeur et un ID MVPD spécifiques. Cette fonctionnalité prend en charge des cas d’utilisation tels que :

* Une passe temporaire quotidienne de 10 minutes (la passe temporaire doit être réinitialisée au début de la journée)
* Un Temp Pass disponible pour tous les utilisateurs pendant les dernières nouvelles. (La passe temporaire doit être réinitialisée pour tous les appareils dès que les dernières nouvelles commencent.)
* Scénario à passes temporaires multiples qui fournit une combinaison d&#39;une période d&#39;affichage initiale d&#39;une certaine longueur, suivie de périodes quotidiennes ultérieures d&#39;une autre longueur.

Pour réinitialiser toutes les passes temporaires, l’authentification Adobe Pass fournit aux programmeurs une API web *publique* :

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>L’URL ci-dessus remplace l’API de réinitialisation précédente. L’ancienne API reset (v1) n’est plus prise en charge.

* **Protocol:** HTTPS
* **Host:**
   * Version - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Chemin:** /reset-tempass/v2/reset
* **Paramètres de requête :** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **En-têtes :** ApiKey - 1232293681726481
* **Réponse:**
   * Réussite - HTTP 204
   * Échec :
      * HTTP 400 pour une requête incorrecte
      * HTTP 401 si la clé API n’a pas été spécifiée
      * HTTP 403 si la clé API n’est pas valide

Par exemple :

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## Clients pris en charge {#supp-clients}

Prise en charge de l’outil Temp Pass et Reset par la plateforme :

| Clients d’authentification Adobe Pass | Temp Pass | Réinitialiser l’outil |
|:--------------------------------------:|:---------:|:----------:|
| JS AccessEnabler | OUI | OUI |
| Native Client iOS | OUI | OUI |
| tvOS client natif | OUI | OUI |
| Native Client Android | OUI | OUI |
| Native Client fireTV | OUI | OUI |
| API sans client | OUI | OUI |

## Limites et problèmes connus {#limitations}

Cette section décrit les limites qui s’appliquent à l’implémentation actuelle de Temp Pass.

**JavaScript SDK** : prend en charge la fonctionnalité de réinitialisation de la passe temporaire à partir de la version **3.X et ultérieure**.

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
