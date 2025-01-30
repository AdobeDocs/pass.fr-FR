---
title: Présentation de JavaScript SDK
description: Présentation de JavaScript SDK
exl-id: 8756c804-a4c1-4ee3-b2b9-be45f38bdf94
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---

# Présentation de JavaScript SDK (hérité) {#javascript-sdk-overview}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Introduction

Adobe vous recommande vivement de migrer vers la dernière version de JS v4.x de la bibliothèque AccessEnabler.

L’intégration de JavaScript d’authentification Adobe Pass offre aux programmeurs une solution TV-Everywhere dans l’environnement de développement d’applications web JS habituel. Les principaux composants de l’intégration sont votre application « de haut niveau » (interaction utilisateur, présentation vidéo) et la bibliothèque AccessEnabler « de bas niveau » fournie par l’Adobe, qui fournit votre entrée aux flux de droits et gère la communication avec les serveurs d’authentification Adobe Pass.

Les sections suivantes fournissent des descriptions et des exemples spécifiques à l’intégration JavaScript AccessEnabler.

>[!IMPORTANT]
>
>Ce document décrit l’implémentation d’une solution web de bureau. La bibliothèque JavaScript n’est pas prise en charge sur les plateformes mobiles (par exemple, Safari sur iOS, Chrome sur Android). Utilisez nos SDK natifs si vous souhaitez cibler une plateforme mobile (iOS, Android, Windows).

## Création de la boîte de dialogue de sélection MVPD {#creating-the-mvpd-selection-dialog}

Pour qu’un utilisateur puisse se connecter à son MVPD et s’authentifier, votre page ou lecteur doit lui permettre d’identifier son MVPD. Une version par défaut d’une boîte de dialogue de sélection MVPD est fournie pour le développement. Pour une utilisation en production, vous devez implémenter votre propre sélecteur MVPD.

Si vous savez déjà qui est le fournisseur du client, vous pouvez [définir le MVPD par programmation](/help/authentication/home.md) sans interaction de l’utilisateur. La technique est la même, mais contourne l’étape d’appel de la boîte de dialogue du sélecteur de fournisseur et de demande au client de sélectionner son MVPD.

## Affichage du fournisseur de services {#displaying-the-service-provider}

L’exemple de code suivant montre comment découvrir et afficher le fournisseur de services pour le client actuel :

**HTML** - Cette page ajoute une section à la page qui affiche le fournisseur choisi par le client, s&#39;il est déjà connecté :

```HTML
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
            "http://www.w3.org/TR/html4/loose.dtd"> 
    <html>
    <head>
        <title>Get the distributor that is used for the authentication</title>
        <script type="text/javascript" src="libs/jquery-1.4.4.min.js"></script>
        <script type="text/javascript" src="libs/swfobject.js"></script>
        <script type="text/javascript" src="scripts/sample6.js"></script>
    </head>
    <body>
        <div id="alternative">
        <a href="http://www.adobe.com/go/getflashplayer"> 
            <img src="http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif" 
                 alt="Get Adobe Flash player"/> </a>
        </div> 
        <div id="user_authenticated">Please wait while we verify your authentication status</div> 
        <div id="control">
        <button id="sign_in" style="display:none;">Sign in</button>
        <button id="sign_out" style="display:none;">Sign out</button>
        </div> 
        <div id="distributor"></div> 
        <div id="provider_selector" style="display:none;">
        <select id="provider_list"></select>
        <button id="login">Login</button>
        <button id="cancel">Cancel</button>
        </div> 
        <div id="video_player">This is a video player showing an image as a preview.  You are not yet authorized to see this content.  Make sure that you first sign in.
        </div> 
        <div id="get_authorization">
        <button id="request_access" style="display:none;">Request access</button>
        </div> 
    </body>
    </html>
```


**JavaScript** Ce fichier JavaScript interroge Access Enabler pour le fournisseur actuel si l&#39;utilisateur est déjà connecté et affiche le résultat dans la section de page qui lui est réservée. Il implémente également une boîte de dialogue de sélecteur MVPD :

```JS
    $(function() {
        embedAE();
    }); 
    function embedAE() {
        var flashvars = {};
        var params = {
            bgcolor: "#869ca7",
            quality: "high",
            allowscriptaccess: "always"
        };
        var attributes = {
            id: "ae_swf",
            align: "middle",
            name: "ae_swf"
        };
        swfobject.embedSWF(
                "https://entitlement.auth-staging.adobe.com/entitlement/AccessEnabler.swf",      
                "alternative", "1", "1", "10.1.53", "", flashvars, params, attributes);
    } 
    function getAE() {
        return document.getElementById("ae_swf");
    } 
    function swfLoaded() {
        var swf = getAE();
        initBindings();
        // don't load the default provider-selection dialog 
        swf.setProviderDialogURL("none");
        // identify yourself 
        swf.setRequestor("sample_requestor_Id");
        swf.checkAuthentication();
    } 
    // Define the button actions for the provider dialog
    function initBindings() {
        var provider_selector = $('#provider_selector');
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var login = $('#login');
        var cancel = $('#cancel');
        var request_access = $('#request_access'); 
        sign_out.click(function() {
            getAE().logout();
        });
        sign_in.click(function() {
            getAE().getAuthentication();
        }); 
        login.click(function() {
            var selected_mvpd_id = $("#provider_list option:selected").val();
            getAE().setSelectedProvider(selected_mvpd_id);
        }); 
        cancel.click(function() {
            getAE().setSelectedProvider(null);
            provider_selector.hide();
        }); 
        request_access.click(function(){
            getAE().getAuthorization("sample_requestor_Id");
        });
    }
    // Called if user is already authenticated; queries AE to find the current user's MVPD, 
    // Adds the result to the UI 
    function setDistributor() {
        var distributor = $("#distributor");
        var selectedProvider = getAE().getSelectedProvider();
        distributor.replaceWith('<div id="distributor">' +
                                'Courtesy of ' +
                                selectedProvider.MVPD +
                                '</div>');
    } 
    // Adjust the contents of the provider dialog, based on authentication status 
    // Adds the call to check and display the current distributor, if the user is already
    // logged in. 
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        var user_authenticated = $("#user_authenticated");
        var video_player = $("#video_player");
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var request_access = $('#request_access'); 
        if (isAuthenticated == 1) {
            setDistributor();
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are authenticated - if you wish you can sign out
                                           </div>');
            sign_out.show();
            sign_in.hide();
            video_player.replaceWith('<div id="video_player">Now you can ask for access.</div>');
            request_access.show();
        } else {
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are not authenticated please sign in
                                           </div>');
            sign_in.show();
            sign_out.hide();
        }
    } 
    // Allow user to choose a provider 
    function displayProviderDialog(providers) {
        var provider_list = $("#provider_list");
        var options = provider_list.attr('options');
        for (var index in providers) {
            options[index] = new Option(providers[index].displayName,
                                        providers[index].ID);
        }
        //select the first one by default 
        if (!$("#provider_list option:selected").length)
            $("#provider_list option[0]").attr('selected', 'selected'); 
        $('#provider_selector').show();
    } 
    // Handle the authorization response by sending token to video player 
    function setToken(requested_resource_id, token) {
        var video_player = $("#video_player");
        var request_access = $("#request_access");
        video_player.replaceWith('<div id="video_player">Now you are authorized to ' +
                                 requested_resource_id +
                                 ' content with ' + token + ' . </div>');
    }
```

## Déconnexion {#logout}

Appelez `logout()` pour lancer le processus de déconnexion. Cette méthode ne prend aucun argument. Il déconnecte l’utilisateur actuel, efface toutes les informations d’authentification et d’autorisation de cet utilisateur et supprime tous les jetons AuthN et AuthZ du système local.

Dans certains cas, votre lecteur n’est pas responsable de la gestion des déconnexions des utilisateurs :



- **Lorsque la déconnexion est lancée à partir d’un site qui n’est pas intégré à l’authentification Adobe Pass.** Dans ce cas, le MVPD peut appeler le service de déconnexion unique d’authentification Adobe Pass par le biais d’une redirection du navigateur. (L’appel de SLO via un appel de backchannel n’est actuellement pas pris en charge.)

>[!NOTE]
>
>Si l’utilisateur ou l’utilisatrice laisse son ordinateur inactif suffisamment longtemps pour que ses jetons expirent, il ou elle peut toujours revenir à sa session et lancer avec succès la déconnexion. L’authentification Adobe Pass s’assure que tous les jetons sont supprimés et avertit le MVPD de supprimer également sa session.

Le code JavaScript suivant illustre la déconnexion (désauthentification) d’un utilisateur actuellement authentifié :

```JS
    [...]
    /*
     * @param isAuthenticated authentication status: 1 - Authenticated, 0 - not authenticated 
     * @param errorCode Any error that occurred when determining the authentication status.
     * An empty string if no error occurred.
     */
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        if (isAuthenticated == 1 ) {
            /* User is authenticated – we can logout / deauthenticate */
            accessEnablerObject.logout();
            /* Logs out the current user, clearing all authentication
             * and authorization information for that user. Deletes
             * all authN and authZ tokens from the user's system.
             */
        } else {
            /*
             * User is NOT authenticated – we do not need to logout;
             * Show the log-in image/button/message
             */
        }
    }
```

<!--
**Related Information**

- [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
- [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
- [JavaScript Code Samples](#javascript-code-samples)
- [Understanding Tokens](#understanding_tokens)
-->
