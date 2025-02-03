---
title: Utilisation du proxy Charles
description: Utilisation du proxy Charles
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: 175755aa7463257487b29c5f4da989cf34e91bfd
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# (Hérité) Utilisation du proxy Charles {#using-charles-proxy}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

**Charles:** <http://charlesproxy.com>


## Télécharger, installer et commencer avec Charles Proxy {#download-install-and-get-stared-with-charles-proxy}

- **Télécharger** - <http://www.charlesproxy.com/download/>
- **Installer** - <http://www.charlesproxy.com/documentation/installation/>
- **Prise en main** - <http://www.charlesproxy.com/documentation/getting-started/>


## Onglets Structure et Séquence {#structure-vs-sequence-tabs}

Il existe deux manières différentes d’afficher le trafic :

1. **Structure** - Les demandes sont regroupées par hôte
1. **Séquence** - Les requêtes sont répertoriées dans l’ordre dans lequel elles sont appelées


## SSL et certificats {#ssl-and-certificates}

Activer la `\[ *Proxy -\> Proxy Settings... -\> SSL* \]` de proxy SSL

Cochez la case « Activer le proxy SSL » et ajoutez tous les emplacements HTTPS.

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)
-->

- Proxy SSL - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- Certificats SSL - <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- Proxy SSL à partir d’appareils mobiles - Voir ci-dessous.


## Ignorer / Exclure les hôtes {#ignore-/-exclude-hosts}

Si votre sortie devient trop encombrée, vous pouvez choisir d’ignorer ou d’exclure des emplacements. Vous pouvez ignorer ou exclure des emplacements en effectuant l’une des opérations suivantes :

- Cliquez avec le bouton droit sur les demandes que vous souhaitez ignorer, puis sélectionnez Ignorer
- Ajoutez manuellement les emplacements à exclure des `\[ *Proxy -\> Recording Settings... -\> Exclude* \]`


## Usurpation DNS {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



L’usurpation DNS est très utile lorsque vous essayez de rediriger une requête vers une autre adresse IP, en particulier lorsque vous utilisez des appareils mobiles :

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## Mapper à distance {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



Avec Map Remote, vous pouvez rediriger une requête « entrante » vers un autre point d’entrée. Le cas d’utilisation le plus courant de cette fonctionnalité est de « mapper » les `AccessEnabler.swf` aux `AccessEnablerDebug.swf:`

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## Proxy Inverse {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## Mobile {#mobile}

### Utiliser Charles sur un appareil iOS (iPhone/iPad) {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### Connexion SSL depuis iPhone {#ssl-connection-from-iphone}

Accédez à <http://charlesproxy.com/charles.crt> à partir de votre appareil iOS.  La boîte de dialogue d’installation du certificat démarre :

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)
-->

</br>

Cliquez sur `\[ *Install*... *Install*... *Done* \]` pour terminer l’installation du certificat.

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### Utilisation de Charles à partir d’un appareil iOS {#using-charles-from-an-ios-device}

Sur votre appareil iOS, sélectionnez `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`. Cliquez sur la petite flèche bleue à côté de votre réseau, puis accédez à Proxy HTTP et sélectionnez « Manuel » :


</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)
-->

</br>
Ici, vous devez spécifier l’adresse IP et le port de la machine sur laquelle vous exécutez Charles. <span style="line-height: 1.6em;">Si vous ouvrez désormais Safari sur votre appareil iOS et que vous tentez d’ouvrir une page web, la fenêtre contextuelle ci-dessous s’affiche sur l’ordinateur exécutant Charles :

</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)
-->

</br>
Cliquez sur « Autoriser » pour permettre à l’appareil d’utiliser Charles pour tous ses
demandes.

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS - Approbation de certificats {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### Erreur d’authentification iOS - adobepass.ios.app est introuvable

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Utiliser Charles pour Android

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Accédez au [proxy Charles](http://charlesproxy.com/charles.crt) à partir de votre appareil Android.
