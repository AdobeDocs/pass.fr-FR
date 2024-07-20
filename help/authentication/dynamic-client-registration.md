---
title: Enregistrement du client dynamique
description: Enregistrement du client dynamique
exl-id: 9bc2597d-b634-4542-849b-8e91a76cb8da
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Enregistrement du client dynamique {#dynamic-client-registration}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Contexte {#context}

Pour s’aligner sur les pratiques de sécurité modernes, amélioration des propriétaires d’UX et de plateformes
conditions requises, le SDK Android d’authentification Adobe Pass et le SDK iOS se dirigent vers l’adoption des [ onglets personnalisés Android Chrome ](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} et du [contrôleur de vue Safari Apple](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}.

La mise en oeuvre actuelle d’AdobePass utilise des vues web spécifiques à la plateforme afin de fournir un environnement web pour l’affichage de la page de connexion MVPD. Ces vues web ne partagent pas la gestion des informations d’identification avec les navigateurs de plateforme. Par conséquent, l’utilisateur ne peut pas utiliser de mot de passe enregistré dans un navigateur lors de l’utilisation d’une application d’authentification Adobe Pass. De plus, pour des raisons de sécurité, certaines plateformes sont en train de rendre obsolètes les contrôleurs WebView pour les tâches d’authentification. Google et Apple offrent des options alternatives telles que &quot;Onglets personnalisés Chrome&quot; et &quot;Contrôleur de vue Safari&quot;. Il s’agit essentiellement d’onglets à usage unique de leurs navigateurs respectifs. L’authentification Adobe Pass adoptera ces nouveaux composants en 2018.

## Détails {#details}

Actuellement, l’authentification Adobe Pass peut identifier et enregistrer les applications de deux manières différentes :

* les clients basés sur un navigateur sont enregistrés par le biais de listes de domaines autorisées
* les clients d’application natifs, tels que les applications iOS et Android, sont enregistrés par le biais du mécanisme de requête signé.

Afin de répondre aux nouveaux flux pour les onglets personnalisés Chrome et le contrôleur de vue Safari, Adobe Pass propose un nouveau mécanisme d’enregistrement client pour enregistrer de nouvelles applications. Ce mécanisme permettra un contrôle plus sécurisé et plus précis de vos applications et peut être utilisé pour enregistrer des applications sur toutes les plateformes.

<!--
## Related Information

- [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
- [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)
-->
