---
title: Réinitialiser le transfert temporaire sur iOS
description: Réinitialiser le transfert temporaire sur iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---

# Réinitialiser le transfert temporaire sur iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

L’application de démonstration iOS comprend un écran dédié à la réinitialisation de la transition temporaire. Les informations suivantes sont requises pour l’opération de réinitialisation :

- **Environnement :** spécifie le point de terminaison du serveur de passe de télévision payante Adobe qui recevra l’appel réseau de réinitialisation de la passe temporaire. Valeurs possibles : **Préqual** (*mgmt-prequal.auth-staging.adobe.com*), **Version** (*mgmt.auth.adobe.com*) ou **Personnalisé** (réservé pour les tests internes par Adobe).
- **Jeton de porteur OAuth2 :** le jeton OAuth2 est nécessaire pour autoriser le programmeur pour l’authentification payante Adobe. Un tel jeton peut être obtenu à partir du point d’entrée OAuth2 de l’authentification payante dédiée (par exemple, *curl -u &quot;\&lt;consumer\_key\>:\&lt;consumer\_secret\_key\>*&quot; *&quot; &quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*).
- **Identifiant du demandeur :** l’identifiant unique du programmeur actuel. Cette valeur est lue à partir de l’écran principal de l’application de démonstration (champ du demandeur).
- **ID de transmission temporaire :** l’identifiant unique du MVPD de transmission temporaire.
- **ID de l’appareil :** ID de l’appareil haché calculé par l’application de démonstration.
- **Clé générique :** certains MVPD de transmission de température (c’est-à-dire la prochaine fonctionnalité de transmission de température extensible) prennent en charge une clé générique pour réinitialiser la transmission de température (avec l’ID de périphérique).

Tous les paramètres ci-dessus (à l’exception de la *clé générique*) sont obligatoires. Voici un exemple de paramètres et de l’appel réseau associé qui sera exécuté par l’application de démonstration (l’exemple se présente sous la forme d’une commande *curl*) :

- **Environnement :** Version (*mgmt.auth.adobe.com*)
- **Jeton de porteur OAuth2 :** H4j7cF3GtJX81BrsgDa10GwSizVz
- **Identifiant du programmeur :** REF
- **ID de transmission temporaire :** TempPassREF
- **ID de l’appareil :** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **Clé générique :** null (aucune valeur fournie)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

une requête HTTP DELETE sera envoyée au point de terminaison **/reset**, en transmettant le *jeton de porteur OAuth2* dans l’en-tête d’autorisation et les paramètres *ID de périphérique*, *ID de demandeur* et *ID de transmission temporaire (ID MVPD)* comme paramètres.

Si le programmeur fournit une valeur pour la *Clé générique*, un autre appel HTTP sera effectué (cette fois au point de terminaison **/reset/generic**), en transmettant la *Clé générique* à l’intérieur du paramètre de requête *key*.

Par exemple, définissez la *Clé générique* sur un hachage d’adresse électronique (pour
Les MVPD de transmission de température qui prennent en charge ce type de fonctionnalités) généreront le
appel HTTP suivant (le courriel est `user@domain.com` son SHA-256
hash est `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`) :

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

Il est important de souligner que la réinitialisation de Temp Pass dans l’application de démonstration peut ne pas avoir le même effet pour une application de programmation sur le même appareil. Cela est dû au fait que l’identifiant de l’appareil (tel que calculé par l’application de démonstration et AccessEnabler) peut ne pas être le même pour toutes les applications de l’appareil :

- iOS 6 et versions antérieures : l’identifiant de l’appareil est calculé à l’aide de l’adresse MAC (qui est unique pour toutes les applications). Par conséquent, la réinitialisation de la transmission de la température dans l’application de démonstration la réinitialise dans toutes les autres applications de programmation de l’appareil.

- iOS 7 et supérieur : l’identifiant de l’appareil est calculé à partir de la valeur IDFV (identifiant du fournisseur), qui est unique pour toutes les applications qui possèdent le même préfixe Bundle ID (c’est-à-dire tous les composants sauf le dernier). Comme l’application de démonstration et une application de programmation ont des ID de bundle différents, la réinitialisation de Temp Pass dans l’application de démonstration n’aura aucun effet sur une application de programmeur.

Le dernier cas d’utilisation (iOS 7 et version ultérieure) est le plus courant. Voyons donc comment les programmeurs peuvent réinitialiser Temp Pass pour leurs applications dans ce cas. Il existe plusieurs options :

1. Déplacez le code de l’application de démonstration vers l’application de programmation. Les classes *TempPassResetViewController* et *DeviceIdDemoApp* contiennent la logique principale de réinitialisation de Temp Pass, et elles peuvent facilement être modifiées et incluses dans l’application de programmation.

1. Exécutez la requête HTTP pour réinitialiser Temp Pass avec *curl*. Le paramètre device\_Id peut être obtenu en calculant l’IDFV de l’application Programmer et en appliquant un hachage SHA-256 sur celle-ci (exemple de code dans la classe *DeviceIdDemoApp*).

1. Il vous suffit d’effectuer la réinitialisation à partir de l’application de démonstration en spécifiant l’IDFV haché de l’application du programmeur en tant que *Clé générique*. Cela entraînera deux appels réseau : un pour la réinitialisation de Temp Pass pour l’application de démonstration (sans rapport avec le programmeur) et un autre pour la réinitialisation de Temp Pass pour l’application de programmation.

Toutes les options ci-dessus sont similaires. C’est au programmeur d’en choisir une selon la facilité de mise en oeuvre.
