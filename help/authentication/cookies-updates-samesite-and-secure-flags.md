---
title: 'Mises à jour des cookies : indicateurs samesite et sécurisé'
description: 'Mises à jour des cookies : indicateurs samesite et sécurisé'
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# Mises à jour des cookies : indicateurs samesite et sécurisé {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>


## Mises à jour {#Updates}

Cette section met en évidence les modifications introduites par le navigateur Chrome et Adobe Pass Authentication pour gérer les cookies tiers.



### Mises à jour de Chrome 80 {#Chrome}

À partir de la version 80 de Chrome (à l’exception de la version 82), les cookies qui ne spécifient pas de *SameSite* est traité comme s’ils étaient *SameSite=Lax*. Par conséquent, les cookies qui doivent être diffusés dans un contexte intersite doivent spécifier explicitement la variable *SameSite=None*, et doivent également être marqués par la variable *Sécurisé* Attribut et remis *HTTPS*. Pour plus d&#39;informations sur ces mises à jour, consultez la page officielle sur le chrome : <https://www.chromium.org/updates/same-site> et également de <https://web.dev/samesite-cookies-explained/>.


### Mises à jour de l’authentification Adobe Pass {#Pass-Updates}

Le service d’authentification Adobe Pass repose actuellement sur deux cookies qui sont considérés comme des cookies tiers du point de vue du navigateur, y compris Chrome, afin de fonctionner en combinaison avec certaines plateformes et versions des SDK d’authentification Adobe Pass. Par conséquent, pour se conformer aux modifications à venir et continuer à diffuser ces cookies dans un contexte intersite à partir de ces anciens SDK, le service d’authentification Adobe Pass implémente les modifications requises dans la variable *adobe-pass-2.55.1* version.

Ces modifications de la fonction *adobe-pass-2.55.1* la version implique l’ajout de la variable *Sécurisé* et *SameSite=None* attributs pour tous ses cookies transmis à tous les SDK d’authentification Adobe Pass lors de l’utilisation de navigateurs Chrome commençant avec la version 80 et ultérieure (sauf la version 82).

La section suivante présente quelques problèmes potentiels pour une liste de plateformes et de versions de SDK d’authentification Adobe Pass si un utilisateur utilise le navigateur Chrome 80 et versions ultérieures (à l’exception de la version 82).

## Dépannage {#Troubleshooting}

Lorsque vous parcourez cette section, gardez à l’esprit que tous les cookies du service d’authentification Adobe Pass doivent avoir *Sécurisé* défini dans *adobe-pass-2.55.1* pour tous les navigateurs, tandis que la variable *SameSite=None* doit être défini uniquement pour les navigateurs Chrome version 80 et ultérieure (à l’exception de la version 82).


### Résolution des problèmes généraux {#General}

1. Important : Certains agents utilisateur sont réputés incompatibles avec la variable *SameSite=None* attribut.

   - Versions de Chrome de Chrome 51 à Chrome 66 (inclus aux deux extrémités). Ces versions de Chrome rejettent un cookie avec *SameSite=None*. Cela a également une incidence sur les anciennes versions des navigateurs dérivés de Chromium, ainsi que sur Android WebView. Ce comportement était correct en fonction de la version de la spécification du cookie à l’époque, mais avec l’ajout de la nouvelle valeur &quot;Aucun&quot; à la spécification, ce comportement a été mis à jour dans Chrome 67 et versions ultérieures. (Avant Chrome 51, l’attribut SameSite était entièrement ignoré et tous les cookies étaient traités comme s’ils étaient *SameSite=None*.)
   - Versions du navigateur UC sur Android antérieures à la version 12.13.2. Les anciennes versions rejettent un cookie avec *SameSite=None*. Ce comportement était correct en fonction de la version de la spécification du cookie à l’époque, mais avec l’ajout de la nouvelle valeur &quot;Aucun&quot; à la spécification, ce comportement a été mis à jour dans de nouvelles versions du navigateur UC.
   - Versions de Safari et navigateurs incorporés sur MacOS 10.14 et tous les navigateurs sur iOS 12. Ces versions traiteront par erreur les cookies marqués par *SameSite=None* comme s’ils étaient marqués *SameSite=Strict*. Ce bogue a été corrigé sur les versions plus récentes d’iOS et de MacOS.


1. Important : Notez que les cookies qui contiennent la variable *Sécurisé* doit être envoyé *HTTPS*, sinon le cookie n’atteindra pas le service d’authentification Adobe Pass.

   - SDK JavaScript AccessEnabler :
      - Obligatoire pour la communication avec *sp.auth.adobe.com* uses *HTTPS* pour les versions *2,35* et *3.5.0*, avant d’introduire l’enregistrement du client dynamique.
   - SDK AccessEnabler iOS/tvOS :
      - Obligatoire pour la communication avec *sp.auth.adobe.com* uses *HTTPS* pour les versions antérieures à *3.0.0*, avant d’introduire l’enregistrement du client dynamique.
   - SDK Android AccessEnabler :
      - Obligatoire pour la communication avec *sp.auth.adobe.com* uses *HTTPS* pour les versions antérieures à *3.0.0*, avant d’introduire l’enregistrement du client dynamique.
   - SDK AccessEnabler FireOS :
      - Obligatoire pour la communication avec *sp.auth.adobe.com* uses *HTTPS* pour la version *2.0.4*.

</br>

### Dépannage du SDK JavaScript AccessEnabler version 2.35 {#235-Troubleshooting}

Le flux d’authentification de l’utilisateur peut être affecté dans Chrome 80 et versions ultérieures (à l’exception de la version 82). Pour vous assurer que l’utilisateur n’a pas de problèmes d’authentification en raison des mises à jour ci-dessus, vous pouvez :

- Vérifiez que la variable *JSESSIONID* est défini dans le navigateur et la variable *SameSite=None* et *Sécurisé* ensemble d’attributs.
- Vérifiez que la variable *JSESSIONID* du cookie *https://sp.auth.adobe.com/authenticate/saml* la requête réseau correspond à *JSESSIONID* du cookie *https://sp.auth.adobe.com/session* requête réseau.


### Dépannage du SDK JavaScript AccessEnabler version 3.5.0 {#350-Troubleshooting}

Le flux d’authentification de l’utilisateur peut être affecté dans Chrome 80 et versions ultérieures (à l’exception de la version 82). Pour vous assurer que l’utilisateur n’a pas de problèmes d’authentification en raison des mises à jour ci-dessus, vous pouvez :

- Vérifiez que la variable *JSESSIONID* est défini dans le navigateur et la variable *SameSite=None* et *Sécurisé* ensemble d’attributs.
- Vérifiez que la variable *JSESSIONID* du cookie *https://sp.auth.adobe.com/authenticate/saml* la requête réseau correspond à *JSESSIONID* du cookie *https://sp.auth.adobe.com/session* requête réseau.
- Vérifiez que la variable *pass\_sfp* est défini dans le navigateur et la variable *SameSite=None* et *Sécurisé* ensemble d’attributs.
- Vérifiez que la variable *pass\_sfp* est défini dans *https://sp.auth.adobe.com/session* requête réseau.


Le flux d’autorisation de l’utilisateur peut être affecté dans Chrome 80 et versions ultérieures (à l’exception de la version 82). Afin de vous assurer que l’utilisateur n’a pas de difficultés à regarder une ressource protégée, après s’être authentifié correctement, en raison des mises à jour ci-dessus, vous pouvez :

- Vérifiez que la variable *pass\_sfp* est défini dans le navigateur et la variable *SameSite=None* et *Sécurisé* ensemble d’attributs.
- Vérifiez que la variable *pass\_sfp* est défini dans *https://sp.auth.adobe.com/adobe-services/authorize* requête réseau.
- Vérifiez que la variable *pass\_sfp* est défini dans *https://sp.auth.adobe.com/adobe-services/shortAuthorize* requête réseau.
