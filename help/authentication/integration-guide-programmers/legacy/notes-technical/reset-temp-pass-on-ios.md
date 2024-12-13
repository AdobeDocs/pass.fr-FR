---
title: Réinitialiser la transmission temporaire sur iOS
description: Réinitialiser la transmission temporaire sur iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '721'
ht-degree: 0%

---

# (Hérité) Réinitialiser la passe temporaire sur iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>

L’application de démonstration iOS comprend un écran dédié à la réinitialisation de la TTL de passe temporaire. Les informations suivantes sont requises pour l’opération de réinitialisation :

- **Environnement :** spécifie le point d’entrée du serveur de passe de télévision à péage d’Adobe qui recevra l’appel réseau de la passe temporaire réinitialisée. Valeurs possibles : **Prequal** (*mgmt-prequal.auth-staging.adobe.com*), **Release** (*mgmt.auth.adobe.com*) ou **Custom** (réservé aux tests internes d’Adobe).
- Jeton du porteur **OAuth2 :** jeton OAuth2 est nécessaire pour autoriser le programmeur à effectuer l’authentification de la télévision à péage d’Adobe. Un tel jeton peut être obtenu à partir du point d’entrée OAuth2 d’authentification de télévision payante dédié (par exemple, *curl -u « \&lt;consumer\_key\>:\&lt;consumer\_secret\_key\>* » *»https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials »*).
- **ID du demandeur :** ID unique du programmeur actuel. Cette valeur est lue à partir de l’écran principal de l’application de démonstration (champ du demandeur).
- **Identifiant de passe temporaire :** l’identifiant unique du MVPD de passe temporaire.
- **Identifiant de l’appareil :** identifiant d’appareil haché calculé par l’application de démonstration.
- **Clé générique :** certaines MVPD de passe temporaire (c’est-à-dire la fonctionnalité de passe temporaire extensible suivante) prennent en charge une clé générique pour la réinitialisation de la passe temporaire (avec l’ID d’appareil).

Tous les paramètres ci-dessus (à l’exception de la *Clé générique*) sont obligatoires. Voici un exemple de paramètres et de l’appel réseau associé qui sera effectué par l’application de démonstration (l’exemple se présente sous la forme d’une commande *curl*) :

- **Environnement :** version (*mgmt.auth.adobe.com*)
- Jeton du porteur **OAuth2 :** H4j7cF3GtJX81BrsgDa10GwSizVz
- **ID du programmeur :** REF
- **Identifiant de passe temporaire :** TempPassREF
- **Identifiant d’appareil :** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **Clé générique :** null (aucune valeur fournie)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

une requête HTTP de DELETE sera envoyée au point d’entrée **/reset**, en transmettant le jeton du porteur *OAuth2* dans l’en-tête d’autorisation et les paramètres *ID d’appareil*, *ID de demandeur* et *ID de passe temporaire (ID de MVPD)*.

Si le programmeur fournit une valeur pour la *clé générique*, un autre appel HTTP sera effectué (cette fois vers le point d’entrée **/reset/generic**), en transmettant la *clé générique* dans le paramètre de requête *clé*.

Par exemple, définir la *Clé générique* sur un hachage d’adresse électronique (pour
Les MVPD de passe temporaire (qui prennent en charge ce type de fonctionnalité) produiront le
à la suite d’un appel HTTP (l’e-mail est `user@domain.com` son SHA-256
le hachage est `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`) :

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

Il est important de souligner que la réinitialisation de la passe temporaire dans l’application de démonstration peut ne pas avoir le même effet pour une application de programmation sur le même appareil. Cela est dû au fait que l’ID d’appareil (tel que calculé par l’application de démonstration et l’AccessEnabler) peut ne pas être le même pour toutes les applications sur l’appareil :

- iOS 6 et versions ultérieures : l’ID d’appareil est calculé à l’aide de l’adresse MAC (qui est unique pour toutes les applications). Par conséquent, la réinitialisation de la passe temporaire dans l’application de démonstration la réinitialisera dans toutes les autres applications de programmation de l’appareil.

- iOS 7 et versions ultérieures : l’ID d’appareil est calculé en fonction de la valeur IDFV (ID du fournisseur), qui est unique pour toutes les applications ayant le même préfixe d’ID de lot (c’est-à-dire tous les composants, à l’exception du dernier). Comme l’application de démonstration et une application de programmation ont des ID de bundle différents, la réinitialisation de la passe temporaire dans l’application de démonstration n’aura aucun effet sur une application de programmation.

Le dernier cas d’utilisation (iOS 7 et versions ultérieures) est le plus courant. Voyons donc comment les programmeurs peuvent réinitialiser la passe temporaire pour leurs applications dans cette situation. Il existe plusieurs options :

1. Transférez le code de l’application de démonstration vers l’application de programmation. Les classes *TempPassResetViewController* et *DeviceIdDemoApp* contiennent la logique de base de la réinitialisation de la passe de temp et peuvent être facilement modifiées et incluses dans l&#39;application du programmeur.

1. Exécutez la requête HTTP pour réinitialiser la passe temporaire avec *curl*. Le paramètre device\_Id peut être obtenu en calculant l’IDFV de l’application Programmer et en appliquant un hachage SHA-256 sur celle-ci (exemple de code dans la classe *DeviceIdDemoApp*).

1. Il vous suffit d’effectuer la réinitialisation à partir de l’application de démonstration en spécifiant l’IDFV haché de l’application du programmeur comme *clé générique*. Cela entraînera deux appels réseau : un pour la réinitialisation de la passe temporaire pour l’application de démonstration (sans rapport avec le programmeur) et un pour la réinitialisation de la passe temporaire pour l’application du programmeur.

Toutes les options ci-dessus sont similaires, il appartient au programmeur d&#39;en choisir une en fonction de la facilité d&#39;implémentation.
