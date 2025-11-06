---
title: Annexe B « Conseils de débogage »
description: Annexe B « Conseils de débogage »
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---

# (Hérité) Annexe B : Conseils de débogage {#appendix-b-debugging-tips}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Effacement des données temporaires {#clearing-temporary-data}

L’authentification Adobe Pass stocke des données temporaires telles que le cache du navigateur, le cache des LSO et les cookies. Il est important d’effacer les données temporaires pour vous assurer d’avoir une ardoise propre lors du test.

- [Effacement du cache et des cookies du navigateur](#clearing-the-browser-cache-and-cookies)
- [Effacement du cache des LSO](#clearing-lsos-cache)


## Effacement du cache et des cookies du navigateur {#clearing-the-browser-cache-and-cookies}

Il est fiable par le navigateur, mais dans Firefox : « Tools » -\> « Clear Recent History... » -\> Sur « Time range to clear: » sélectionnez « Tout »; et sur « Details » : vérifiez les « Cookies » et « Cache » -\> Cliquez sur « Clear Now ».


## Effacement du cache des LSO {#clearing-lsos-cache}

Accédez à l&#39;aide de [Flash Player](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html).

Sélectionnez le ```entitlement.\*``` (en fonction de ce qui a été testé) et cliquez sur « Supprimer le site web ».


## Outils de débogage {#tools}

Les ingénieurs Adobe Pass Authentication utilisent les outils de débogage suivants :

- Firebug - <http://www.getfirebug.com/>
- Flashbug (fonctionne avec la version de débogage du lecteur flash)
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>
