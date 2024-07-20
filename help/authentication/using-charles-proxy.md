---
title: Utilisation du proxy Charles
description: Utilisation du proxy Charles
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Utilisation du proxy Charles {#using-charles-proxy}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


**Charles:** <http://charlesproxy.com>


## Téléchargement, installation et prise en main du proxy Charles {#download-install-and-get-stared-with-charles-proxy}

- **Télécharger** - <http://www.charlesproxy.com/download/>
- **Installer** - <http://www.charlesproxy.com/documentation/installation/>
- **Prise en main** - <http://www.charlesproxy.com/documentation/getting-started/>


## Onglets Structure et Séquence {#structure-vs-sequence-tabs}

Il existe deux manières différentes d’afficher le trafic :

1. **Structure** - Les requêtes sont regroupées par hôte
1. **Séquence** - Les requêtes sont répertoriées dans l’ordre dans lequel elles sont appelées


## SSL et certificats {#ssl-and-certificates}

Activer le proxy SSL `\[ *Proxy -\> Proxy Settings... -\> SSL* \]`

Cochez la case &quot;Activer le proxy SSL&quot; et ajoutez tous les emplacements HTTPS.


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)



- Proxie SSL - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- Certificats SSL - <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- Proxie SSL depuis des périphériques mobiles - Voir ci-dessous.


## Ignorer / Exclure les hôtes {#ignore-/-exclude-hosts}

Si votre sortie devient trop encombrée, vous pouvez choisir d’ignorer ou d’exclure des emplacements. Vous pouvez ignorer ou exclure des emplacements en effectuant l’une des opérations suivantes :

- Cliquez avec le bouton droit sur les requêtes que vous souhaitez ignorer, puis sélectionnez &quot;Ignorer&quot;.
- Ajouter manuellement les emplacements à exclure de `\[ *Proxy -\> Recording Settings... -\> Exclude* \]`


## usurpation DNS {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



L’usurpation DNS est très utile pour tenter de rediriger une requête vers une autre adresse IP, en particulier lorsque vous utilisez des appareils mobiles :

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## Mapper à distance {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



Avec la télécommande map, vous pouvez rediriger une requête &quot;entrante&quot; vers un autre point de terminaison. Le cas d’utilisation le plus courant de cette fonctionnalité est de &quot;mapper&quot; `AccessEnabler.swf` sur `AccessEnablerDebug.swf:`

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## Reverse Proxy {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## Mobile {#mobile}

### Utiliser Charles sur un appareil iOS (iPhone/iPad) {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### Connexion SSL depuis iPhone {#ssl-connection-from-iphone}

Accédez à <http://charlesproxy.com/charles.crt> à partir de votre appareil iOS.  La boîte de dialogue d’installation du certificat s’ouvre alors :

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)

</br>

Cliquez sur `\[ *Install*... *Install*... *Done* \]` pour terminer l’installation du certificat.

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### Utilisation de Charles depuis un appareil iOS {#using-charles-from-an-ios-device}

Sur votre appareil iOS, sélectionnez `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`. Cliquez sur la petite flèche bleue en regard de votre réseau, puis accédez à HTTP Proxy et sélectionnez &quot;Manuel&quot; :


</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)


</br>
Ici, vous devez spécifier l’adresse IP et le port de la machine sur laquelle vous exécutez Charles. <span style="line-height: 1.6em;">Si vous ouvrez maintenant Safari sur votre appareil iOS et essayez d’ouvrir une page web, vous devriez obtenir la fenêtre contextuelle suivante sur la machine qui exécute Charles :

</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)

</br>
Cliquez sur "Autoriser" pour permettre à l’appareil d’utiliser Charles pour proxy tous ses
requêtes.

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS - Approbation de certificats {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### Erreur d’authentification iOS - adobepass.ios.app introuvable

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Utilisation de Charles pour Android

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Accédez à [Charles proxy](http://charlesproxy.com/charles.crt) à partir de votre appareil Android.
