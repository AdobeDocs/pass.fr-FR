---
title: 'Mises à jour des cookies : indicateurs samesite et sécurisé'
description: 'Mises à jour des cookies : indicateurs samesite et sécurisé'
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
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

Cette section présente les modifications introduites par le navigateur Chrome et l’authentification Adobe Pass pour gérer les cookies tiers.



### Mises à jour de Chrome 80 {#Chrome}

À partir de la version 80 de Chrome (sauf la version 82), les cookies qui ne spécifient pas d’attribut *SameSite* seront traités comme s’ils étaient *SameSite=Lax*. Par conséquent, les cookies qui doivent être diffusés dans un contexte intersite doivent spécifier explicitement *SameSite=None*, et doivent également être marqués avec l’attribut *Secure* et distribués sur *HTTPS*. Pour plus d&#39;informations sur ces mises à jour, consultez la page officielle sur le chrome : <https://www.chromium.org/updates/same-site> et également <https://web.dev/samesite-cookies-explained/>.


### Mises à jour de l’authentification Adobe Pass {#Pass-Updates}

Le service d’authentification Adobe Pass repose actuellement sur deux cookies qui sont considérés comme des cookies tiers du point de vue du navigateur, y compris Chrome, afin de fonctionner en combinaison avec certaines plateformes et versions des SDK d’authentification Adobe Pass. Par conséquent, afin de se conformer aux modifications à venir et de continuer à diffuser ces cookies dans un contexte intersite à partir de ces anciens SDK, le service d’authentification Adobe Pass implémente les modifications requises dans la version *adobe-pass-2.55.1*.

Ces modifications de la version *adobe-pass-2.55.1* impliquent l’ajout des attributs *Secure* et *SameSite=None* pour tous ses cookies transmis à tous les SDK d’authentification Adobe Pass lors de l’utilisation de navigateurs Chrome commençant par la version 80 et ultérieure (sauf la version 82).

La section suivante présente quelques problèmes potentiels pour une liste de plateformes et de versions de SDK d’authentification Adobe Pass si un utilisateur utilise le navigateur Chrome 80 et versions ultérieures (à l’exception de la version 82).

## Dépannage {#Troubleshooting}

Lors du parcours de cette section, gardez à l’esprit que tous les cookies du service d’authentification Adobe Pass doivent avoir un attribut *Secure* défini dans la version *adobe-pass-2.55.1* pour tous les navigateurs, tandis que l’attribut *SameSite=None* doit être défini uniquement pour les navigateurs Chrome version 80 et ultérieure (sauf la version 82).


### Résolution des problèmes généraux {#General}

1. Important : Certains agents utilisateur sont réputés incompatibles avec l’attribut *SameSite=None*.

   - Versions de Chrome de Chrome 51 à Chrome 66 (y compris aux deux extrémités). Ces versions de Chrome rejetteront un cookie avec *SameSite=None*. Cela a également une incidence sur les anciennes versions des navigateurs dérivés de Chromium, ainsi que sur Android WebView. Ce comportement était correct en fonction de la version de la spécification du cookie à l’époque, mais avec l’ajout de la nouvelle valeur &quot;Aucun&quot; à la spécification, ce comportement a été mis à jour dans Chrome 67 et versions ultérieures. (Avant Chrome 51, l’attribut SameSite était entièrement ignoré et tous les cookies étaient traités comme s’ils étaient *SameSite=None*.)
   - Versions du navigateur UC sur Android antérieures à la version 12.13.2. Les versions plus anciennes rejettent un cookie avec *SameSite=None*. Ce comportement était correct en fonction de la version de la spécification du cookie à l’époque, mais avec l’ajout de la nouvelle valeur &quot;Aucun&quot; à la spécification, ce comportement a été mis à jour dans de nouvelles versions du navigateur UC.
   - Versions de Safari et navigateurs incorporés sur MacOS 10.14 et tous les navigateurs sur iOS 12. Ces versions traiteront par erreur les cookies marqués de *SameSite=None* comme s’ils étaient marqués *SameSite=Strict*. Ce bogue a été corrigé sur les versions plus récentes d’iOS et de MacOS.


1. Important : Notez que les cookies ayant l’attribut *Secure* doivent être envoyés par *HTTPS*, sinon le cookie n’atteindra pas le service d’authentification Adobe Pass.

   - SDK JavaScript AccessEnabler :
      - Obligatoire que la communication avec *sp.auth.adobe.com* utilise *HTTPS* pour les versions *2.35* et *3.5.0*, avant d’introduire l’enregistrement du client dynamique.
   - SDK AccessEnabler iOS/tvOS :
      - Obligatoire que la communication avec *sp.auth.adobe.com* utilise *HTTPS* pour les versions antérieures à *3.0.0*, avant d’introduire l’enregistrement du client dynamique.
   - AccessEnabler Android SDK :
      - Obligatoire que la communication avec *sp.auth.adobe.com* utilise *HTTPS* pour les versions antérieures à *3.0.0*, avant d’introduire l’enregistrement du client dynamique.
   - SDK AccessEnabler FireOS :
      - Obligatoire que la communication avec *sp.auth.adobe.com* utilise *HTTPS* pour la version *2.0.4*.

</br>

### Dépannage du SDK JavaScript AccessEnabler version 2.35 {#235-Troubleshooting}

Le flux d’authentification de l’utilisateur peut être affecté dans Chrome 80 et versions ultérieures (sauf dans la version 82). Pour vous assurer que l’utilisateur n’a pas de problèmes d’authentification en raison des mises à jour ci-dessus, vous pouvez :

- Vérifiez que le cookie *JSESSIONID* est défini dans le navigateur et que les attributs *SameSite=None* et *Secure* sont définis.
- Vérifiez que le cookie *JSESSIONID* de la requête réseau *https://sp.auth.adobe.com/authenticate/saml* correspond au cookie *JSESSIONID* de la requête réseau *https://sp.auth.adobe.com/session* .


### Résolution des problèmes du SDK JavaScript version 3.5.0 d’AccessEnabler {#350-Troubleshooting}

Le flux d’authentification de l’utilisateur peut être affecté dans Chrome 80 et versions ultérieures (sauf dans la version 82). Pour vous assurer que l’utilisateur n’a pas de problèmes d’authentification en raison des mises à jour ci-dessus, vous pouvez :

- Vérifiez que le cookie *JSESSIONID* est défini dans le navigateur et que les attributs *SameSite=None* et *Secure* sont définis.
- Vérifiez que le cookie *JSESSIONID* de la requête réseau *https://sp.auth.adobe.com/authenticate/saml* correspond au cookie *JSESSIONID* de la requête réseau *https://sp.auth.adobe.com/session* .
- Vérifiez que le cookie *pass\_sfp* est défini dans le navigateur et que les attributs *SameSite=None* et *Secure* sont définis.
- Vérifiez que le cookie *pass\_sfp* est défini dans la requête réseau *https://sp.auth.adobe.com/session*.


Le flux d’autorisation de l’utilisateur peut être affecté dans Chrome 80 et versions ultérieures (sauf dans la version 82). Afin de vous assurer que l’utilisateur n’a pas de difficultés à regarder une ressource protégée, après s’être authentifié correctement, en raison des mises à jour ci-dessus, vous pouvez :

- Vérifiez que le cookie *pass\_sfp* est défini dans le navigateur et que les attributs *SameSite=None* et *Secure* sont définis.
- Vérifiez que le cookie *pass\_sfp* est défini dans la requête réseau *https://sp.auth.adobe.com/adobe-services/authorize*.
- Vérifiez que le cookie *pass\_sfp* est défini dans la requête réseau *https://sp.auth.adobe.com/adobe-services/shortAuthorize*.
