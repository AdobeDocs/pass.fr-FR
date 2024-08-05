---
title: En-tête - X-Device-Info
description: API REST V2 - En-tête - X-Device-Info
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 3%

---


# En-tête - X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Vue d’ensemble {#overview}

L’en-tête de requête <b>X-Device-Info</b> contient les informations du client (appareil, connexion et application) liées au périphérique de diffusion en continu réel.

## Syntaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b> : &lt;device_information&gt;</td>
   </tr>
   <tr>
      <td>Type d’en-tête</td>
      <td>En-tête de requête</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Non</td>
   </tr>
</table>

## Directives {#directives}

<b>&lt;device_information></b>

La valeur `Base64-encoded` de l’élément JSON contenant au moins les attributs marqués comme requis par le tableau suivant.

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Clé</th>
        <th style="background-color: #EFF2F7;">Description</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Présence</th>
        <th style="background-color: #EFF2F7;">Valeurs possibles</th>
    </tr>
    <tr>
        <td>primaryHardwareType</td>
        <td>Type matériel principal du périphérique.</td>
        <td></td>
        <td>
            Les valeurs sont restreintes :
            <ul>
                <li>Appareil photo</li>
                <li>DataCollectionTerminal</li>
                <li>Bureau</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>GamesConsole</li>
                <li>GeolocationTracker</li>
                <li>Lunettes</li>
                <li>MediaPlayer</li>
                <li>MobilePhone</li>
                <li>PayTerminal</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>Tablette</li>
                <li>WirelessHotspot</li>
                <li>Wistwatch</li>
                <li>Inconnu</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>model</td>
        <td>Nom du modèle de l’appareil.</td>
        <td><i>required</i></td>
        <td>par exemple iPhone, SM-G930V, Apple TV, etc.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>Version de l’appareil.</td>
        <td></td>
        <td>par exemple, 2.0.1, etc.</td>
    </tr>
    <tr>
        <td>manufacturer</td>
        <td>Entreprise/organisation de fabrication de l’appareil.</td>
        <td></td>
        <td>Par exemple, Samsung, LG, ZTE, Huawei, Motorola, Apple, etc.</td>
    </tr>
    <tr>
        <td>vendor</td>
        <td>Entreprise/organisme de vente de l’appareil.</td>
        <td></td>
        <td>par exemple Apple, Samsung, LG, Google, etc.</td>
    </tr>
    <tr>
        <td>osName</td>
        <td>Nom du système d’exploitation du périphérique.</td>
        <td><i>required</i></td>
        <td>
            Les valeurs sont restreintes :
            <ul>
                <li>Android</li>
                <li>CHROME OS</li>
                <li>Linux</li>
                <li>Mac OS</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osFamily</td>
        <td>Nom du groupe du système d’exploitation du périphérique.</td>
        <td></td>
        <td>
            Les valeurs sont restreintes :
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>SE PlayStation</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVendor</td>
        <td>Fournisseur du système d’exploitation du périphérique.</td>
        <td></td>
        <td>
            Les valeurs sont restreintes :
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Projet Tizen</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVersion</td>
        <td>Version du système d’exploitation du périphérique.</td>
        <td></td>
        <td>par exemple, 10.2, 9.0.1, etc.</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>Nom du navigateur.</td>
        <td></td>
        <td>
            Les valeurs sont restreintes :
            <ul>
                <li>Explorateur Android</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Navigateur Symbian</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVendor</td>
        <td>L’entreprise/l’organisation de construction du navigateur.</td>
        <td></td>
        <td>
            Les valeurs sont restreintes :
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVersion</td>
        <td>Version du navigateur de l’appareil.</td>
        <td></td>
        <td>par exemple, 60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>L’agent utilisateur de l’appareil.</td>
        <td></td>
        <td>Par exemple, Mozilla/5.0 (Macintosh ; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, comme Gecko) Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>displayWidth</td>
        <td>Largeur d’écran physique de l’appareil.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayHeight</td>
        <td>Hauteur d’écran physique de l’appareil.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPpi</td>
        <td>Densité physique des pixels de l’écran de l’appareil.</td>
        <td></td>
        <td>par exemple, 294</td>
    </tr>
    <tr>
        <td>diagonalScreenSize</td>
        <td>Dimension diagonale de l’écran physique de l’appareil en pouces.</td>
        <td></td>
        <td>par exemple, 5.5, 10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>Adresse IP de l’appareil utilisée pour envoyer des requêtes HTTP.</td>
        <td></td>
        <td>par exemple, 8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>Port de l’appareil utilisé pour envoyer des requêtes HTTP.</td>
        <td></td>
        <td>Par exemple, 53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>Type de connexion réseau.</td>
        <td></td>
        <td>Par exemple, Wi-Fi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>État de sécurité de la connexion réseau.</td>
        <td></td>
        <td>
            Les valeurs sont restreintes :
            <ul>
                <li>true - dans le cas d’un réseau sécurisé</li>
                <li>false - dans le cas d’une zone réactive publique</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>Identifiant unique de l’application.</td>
        <td></td>
        <td>Par exemple, CNN</td>
    </tr>
</table>


## Exemples {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```
