---
title: Mises à jour des cookies - Indicateurs SameSite et Secure
description: Mises à jour des cookies - Indicateurs SameSite et Secure
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '956'
ht-degree: 0%

---

# Mises à jour des cookies (hérités) - Indicateurs SameSite et Secure {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>


## Mises à jour {#Updates}

Cette section présente les modifications introduites par le navigateur Chrome et l’authentification Adobe Pass pour gérer les cookies tiers.



### Mises à jour de Chrome 80 {#Chrome}

À partir de la version 80 de Chrome (à l’exception de la version 82), les cookies qui ne spécifient pas d’attribut *SameSite* sont traités comme s’ils étaient *SameSite=Lax*. Par conséquent, les cookies qui doivent être diffusés dans un contexte intersite doivent spécifier explicitement l’attribut *SameSite=None* et doivent également être marqués avec l’attribut *Secure* et diffusés via *HTTPS*. Plus de détails sur ces mises à jour peuvent être lus à partir de la page officielle sur le chrome : <https://www.chromium.org/updates/same-site> et aussi à partir de <https://web.dev/samesite-cookies-explained/>.


### Mises à jour de l’authentification Adobe Pass {#Pass-Updates}

Le service d’authentification d’Adobe Pass s’appuie actuellement sur quelques cookies, qui sont considérés comme des cookies tiers du point de vue du navigateur, notamment Chrome, pour fonctionner conjointement avec certaines plateformes et versions des SDK d’authentification d’Adobe Pass. Par conséquent, pour se conformer aux modifications à venir et continuer à diffuser ces cookies dans un contexte intersite à partir de ces anciens SDK, le service d’authentification Adobe Pass implémente les modifications requises dans la version *adobe-pass-2.55.1*.

Ces modifications par rapport à la version *adobe-pass-2.55.1* impliquent l’ajout des attributs *Secure* et *SameSite=None* pour tous ses cookies transmis à tous les SDK d’authentification Adobe Pass lors de l’utilisation de navigateurs Chrome commençant par la version 80 et ultérieure (à l’exception de la version 82).

La section suivante présente certains problèmes potentiels pour une liste de plateformes et de versions des SDK d’authentification Adobe Pass dans le cas où un utilisateur utilise le navigateur Chrome 80 et versions ultérieures (à l’exception de la version 82).

## Dépannage {#Troubleshooting}

Lorsque vous parcourez cette section, gardez à l’esprit que tous les cookies du service d’authentification Adobe Pass doivent avoir l’attribut *Secure* défini dans la version *adobe-pass-2.55.1* pour tous les navigateurs, tandis que l’attribut *SameSite=None* doit être défini uniquement pour les navigateurs Chrome version 80 et ultérieure (à l’exception de la version 82).


### Dépannage général {#General}

1. Il est important de noter que certains agents utilisateur sont connus pour être incompatibles avec l’attribut *SameSite=None*.

   - Versions de Chrome de Chrome 51 à Chrome 66 (incluses des deux côtés). Ces versions de Chrome rejettent un cookie avec *SameSite=None*. Cela affecte également les versions plus anciennes des navigateurs dérivés de Chromium, ainsi que les WebView d’Android. Ce comportement était correct selon la version de la spécification des cookies à ce moment-là, mais avec l’ajout de la nouvelle valeur « Aucun » à la spécification, ce comportement a été mis à jour dans Chrome 67 et les versions ultérieures. (Avant Chrome 51, l’attribut SameSite était entièrement ignoré et tous les cookies étaient traités comme s’ils étaient *SameSite=None*.)
   - Versions de UC Browser sur Android antérieures à la version 12.13.2. Les anciennes versions rejettent un cookie avec *SameSite=None*. Ce comportement était correct selon la version de la spécification des cookies à ce moment-là, mais avec l’ajout de la nouvelle valeur « Aucun » à la spécification, ce comportement a été mis à jour dans les versions plus récentes du navigateur UC.
   - Versions de Safari et des navigateurs incorporés sur MacOS 10.14 et de tous les navigateurs sur iOS 12. Ces versions traitent par erreur les cookies marqués d’*SameSite=None* comme s’ils étaient marqués d’*SameSite=Strict*. Ce bug a été corrigé sur les versions plus récentes d’iOS et de MacOS.


1. Il est important de noter que les cookies dotés de l’attribut *Secure* doivent être envoyés via *HTTPS*, sinon le cookie n’atteindra pas le service d’authentification Adobe Pass.

   - SDK JavaScript AccessEnabler :
      - Obligatoire pour que la communication avec *sp.auth.adobe.com* utilise *HTTPS* pour les versions *2.35* et *3.5.0*, avant d’introduire l’enregistrement client dynamique.
   - SDK AccessEnabler iOS/tvOS :
      - Obligatoire pour que la communication avec *sp.auth.adobe.com* utilise *HTTPS* pour les versions antérieures à *3.0.0*, avant d’introduire l’enregistrement client dynamique.
   - AccessEnabler Android SDK :
      - Obligatoire pour que la communication avec *sp.auth.adobe.com* utilise *HTTPS* pour les versions antérieures à *3.0.0*, avant d’introduire l’enregistrement client dynamique.
   - SDK FireOS AccessEnabler :
      - Obligatoire pour que la communication avec *sp.auth.adobe.com* utilise *HTTPS* pour la version *2.0.4*.

</br>

### Dépannage d’AccessEnabler JavaScript SDK version 2.35 {#235-Troubleshooting}

Le flux d’authentification de l’utilisateur peut être affecté dans Chrome 80 et les versions ultérieures (sauf la version 82). Pour vous assurer que l’utilisateur n’a pas de problèmes d’authentification en raison des mises à jour ci-dessus, vous pouvez :

- Vérifiez que le cookie *JSESSIONID* est défini dans le navigateur et que les attributs *SameSite=None* et *Secure* sont définis.
- Vérifiez que le cookie *JSESSIONID* de la requête réseau *https://sp.auth.adobe.com/authenticate/saml* correspond au cookie *JSESSIONID* de la requête réseau *https://sp.auth.adobe.com/session*.


### Dépannage d’AccessEnabler JavaScript SDK version 3.5.0 {#350-Troubleshooting}

Le flux d’authentification de l’utilisateur peut être affecté dans Chrome 80 et les versions ultérieures (sauf la version 82). Pour vous assurer que l’utilisateur n’a pas de problèmes d’authentification en raison des mises à jour ci-dessus, vous pouvez :

- Vérifiez que le cookie *JSESSIONID* est défini dans le navigateur et que les attributs *SameSite=None* et *Secure* sont définis.
- Vérifiez que le cookie *JSESSIONID* de la requête réseau *https://sp.auth.adobe.com/authenticate/saml* correspond au cookie *JSESSIONID* de la requête réseau *https://sp.auth.adobe.com/session*.
- Vérifiez que le cookie *pass\_sfp* est défini dans le navigateur et que les attributs *SameSite=None* et *Secure* sont définis.
- Vérifiez que le cookie *pass\_sfp* est défini dans la requête réseau *https://sp.auth.adobe.com/session*.


Le flux d’autorisation de l’utilisateur peut être affecté dans Chrome 80 et versions ultérieures (sauf la version 82). Pour vous assurer que l’utilisateur n’a aucun problème à surveiller une ressource protégée, après avoir été authentifié avec succès, en raison des mises à jour ci-dessus, vous pouvez :

- Vérifiez que le cookie *pass\_sfp* est défini dans le navigateur et que les attributs *SameSite=None* et *Secure* sont définis.
- Vérifiez que le cookie *pass\_sfp* est défini dans la requête réseau *https://sp.auth.adobe.com/adobe-services/authorize*.
- Vérifiez que le cookie *pass\_sfp* est défini dans la requête réseau *https://sp.auth.adobe.com/adobe-services/shortAuthorize*.
