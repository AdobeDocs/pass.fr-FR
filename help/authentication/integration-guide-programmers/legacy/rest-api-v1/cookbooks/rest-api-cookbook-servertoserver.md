---
title: Manuel de l’API REST (serveur à serveur)
description: Serveur du guide pas à pas de l’API REST à serveur.
exl-id: 36ad4a64-dde8-4a5f-b0fe-64b6c0ddcbee
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1856'
ht-degree: 0%

---

# Manuel de l’API REST (hérité) (serveur à serveur) {#rest-api-cookbook-server-to-server}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Vue d’ensemble {#overview}

L’objectif de ce guide pas à pas est de détailler les bonnes pratiques pour la mise en œuvre de l’authentification Adobe Pass à l’aide des architectures serveur à serveur.  Il fournit des exigences de base, une implémentation étape par étape et des considérations générales pour les environnements de production et l’exploitation.

### Mécanisme de limitation

L’API REST d’authentification Adobe Pass est régie par un [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md).


## Composants {#components}

Dans une solution de serveur à serveur opérationnelle, les composants suivants sont impliqués :


| Type | Composant | Description |
| --- | --- | --- |
| Appareil De Diffusion En Continu | Application de streaming | L’application Programmeur qui réside sur l’appareil de diffusion en continu de l’utilisateur et lit une vidéo authentifiée. |
| | \[Facultatif\] Module AuthN | Si l’appareil de diffusion en continu dispose d’un agent utilisateur (c’est-à-dire un navigateur web), le module AuthN est chargé d’authentifier l’utilisateur sur l’IdP MVPD. |
| \[Facultatif\] Périphérique AuthN | Application AuthN | Si l’appareil de diffusion en continu ne dispose pas d’un agent utilisateur (c’est-à-dire un navigateur web), l’application AuthN est une application web de programmation accessible à partir de l’appareil d’un utilisateur distinct à l’aide d’un navigateur web. |
| Infrastructure du programmeur | Service de programmation | Service qui relie l’appareil de diffusion en continu au service Adobe Pass afin d’obtenir des décisions d’authentification et d’autorisation. |
| Infrastructure d&#39;Adobe | Service Adobe Pass | Un service qui s’intègre aux services MVPD IdP et AuthZ et fournit des décisions d’authentification et d’autorisation. |
| Infrastructure MVPD | IdP MVPD | Point d’entrée MVPD qui fournit un service d’authentification basé sur les informations d’identification pour valider l’identité de l’utilisateur. |
| | Service MVPD AuthZ | Point d’entrée MVPD qui fournit des décisions d’autorisation basées sur les abonnements des utilisateurs, le contrôle parental, etc. |

## Flux {#flows}

### Enregistrement de client dynamique (DCR)


Adobe Pass utilise le DCR pour sécuriser les communications client entre une application ou un serveur de programmation et les services Adobe Pass. Le flux DCR est distinct et décrit dans la documentation [&#x200B; Présentation de l’enregistrement du client dynamique &#x200B;](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


### Authentification (authN)

Le flux d’authentification est utilisé pour permettre à l’utilisateur de s’identifier
à leur MVPD afin de déterminer si l’utilisateur dispose d’un compte valide.

1. L’utilisateur démarre l’application Appareil de diffusion en continu et tente de se connecter ou d’afficher du contenu protégé.
2. L’application Appareil de diffusion en continu effectue une requête au service de programmation pour déterminer si l’appareil est déjà authentifié.
3. Le service de programmation enregistre l’application à l’aide de DCR.
4. Le service de programmation vérifie l’état d’authentification de l’appareil de diffusion en continu en appelant l’API du service Adobe Pass **checkauthn**.
5. Dans le cas où l’appel **checkauthn** renvoie le statut selon lequel l’appareil utilisateur est authentifié, l’application peut passer au flux d’autorisation.
6. Dans le cas où l’appel **checkauthn** renvoie l’état indiquant que l’appareil utilisateur n’est PAS authentifié, l’application doit attendre qu’une demande d’utilisateur se connecte.
7. Lorsque l’utilisateur demande à se connecter directement (par exemple, sélectionne le bouton de connexion) ou indirectement (par exemple, sélectionne le contenu protégé lorsqu’il n’est pas déjà authentifié), l’application Streaming Device demande au service de programmation de lancer l’authentification de l’utilisateur. Le service de programmation demande et reçoit un code d’enregistrement unique (regcode) en appelant l’API du service Adobe Pass **regcode**.
8. Le service de programmation récupère également la liste des MVPD et attributs actuels en appelant l’API du service Adobe Pass **config**. Remarque : cette API peut également être appelée plus tôt dans le flux et mise en cache.
9. Le service de programmation renvoie le regcode à l’application Streaming Device et la liste MVPD traitée demandée à l’étape \#7. Remarque : le format de liste MVPD traité est spécifié par le programmeur et peut être filtré pour autoriser ou bloquer explicitement des MVPD spécifiques (c’est-à-dire des listes autorisées ou bloquées).
10. Si le est différent du périphérique AuthN (c’est-à-dire du « deuxième écran »), par choix ou par nécessité (c’est-à-dire que le périphérique de diffusion en continu ne prend pas en charge un agent utilisateur), le périphérique de diffusion en continu doit afficher le regcode et un URI pour que l’utilisateur puisse accéder à l’application AuthN. L’utilisateur saisit l’URI dans l’agent utilisateur sur l’appareil AuthN pour lancer l’application AuthN, puis saisit le regcode dans cette application. Si l’appareil de diffusion en continu est identique à l’appareil AuthN, le regcode peut être transmis par programmation au module AuthN.
11. Le module AuthN lance l’authentification de l’utilisateur avec le MVPD en affichant un sélecteur MVPD. Une fois que l’utilisateur a sélectionné le MVPD, le module AuthN appelle **authenticate** avec le regcode, qui redirige l’agent utilisateur vers l’IdP MVPD. Lorsque l’utilisateur s’authentifie avec succès auprès du MVPD, l’agent utilisateur est redirigé via le service Adobe Pass, où l’authentification réussie est enregistrée avec le regcode, puis est redirigé vers le module AuthN.
12. Si l’appareil de diffusion en continu est différent de l’appareil AuthN, l’appareil AuthN doit afficher un message d’authentification réussie à l’utilisateur et indiquer les étapes à suivre (par exemple, « Succès »! Vous pouvez maintenant revenir à votre console de jeux pour continuer \[...\] »). Si le périphérique de diffusion en continu est identique au périphérique AuthN, le périphérique de diffusion en continu peut détecter par programmation la fin de l’authentification.



Le diagramme suivant illustre le flux d’authentification :

![](../../../../assets/authn-flow.png)

### Autorisation (authZ)

Le flux d’autorisation est utilisé pour déterminer si un utilisateur a le droit d’accéder au contenu demandé.

1. Chaque fois que l’utilisateur tente de consulter du contenu protégé sur l’application Appareil de diffusion en continu, l’application Appareil de diffusion en continu appelle le service de programmation pour identifier le contenu et demander l’autorisation et les informations nécessaires au démarrage du flux.
1. Le service de programmation appelle l’API Adobe Pass **authorize** en transmettant l’ID de ressource avec d’autres paramètres requis. Le service Adobe appelle le service MVPD AuthZ avec l’ID de ressource et reçoit une décision d’autorisation qui est ensuite transmise au service de programmation. Cette décision d’autorisation sera mise en cache par le service Adobe Pass pendant une période configurable. Lors des appels **authorize** suivants du service de programmation au service Adobe Pass, la valeur mise en cache sera renvoyée tant qu’elle est valide.
1. Si l’autorisation est accordée, le service de programmation doit appeler l’API **/tokens/media** d’Adobe Pass, qui renverra un jeton de média signé. Le service de programmation doit valider le jeton multimédia à l’aide de la bibliothèque JAR (Media Token Verifier Library). Si elle est valide, le service de programmation doit renvoyer l’autorisation et les nécessaires pour démarrer le flux (par exemple, l’URL du flux) demandés à l’étape \#1.
1. Si l’autorisation est refusée, l’appel **authorize** renvoie un code d’erreur et une description au service de programmation. Le service de programmation doit renvoyer le code et la description de l’erreur (ou un message modifié par le programmeur) à la requête de l’étape \#1.

Le diagramme suivant illustre le flux d’autorisations :

![](../../../../assets/authz-flow.png)

### Déconnexion

Le flux de déconnexion permet à un utilisateur de supprimer l’identité actuelle
associé à l’application.

1. Lorsque l’utilisateur demande de se déconnecter (c’est-à-dire de supprimer de l’appareil le compte MVPD associé à l’application), l’application Appareil de diffusion en continu appelle le service de programmation pour lui demander de déconnecter l’appareil.
1. Le service de programmation doit appeler l’API Adobe Pass **déconnexion**.

Le diagramme suivant illustre le flux de déconnexion :

![](../../../../assets/logout-flow.png)

### \[Facultatif\] Autorisation préalable (ou pré-vol)

La préautorisation peut être utilisée pour déterminer rapidement à partir d’un ensemble de ressources celles auxquelles un utilisateur peut avoir accès.  Le résultat de cet appel est généralement utilisé pour personnaliser l’interface utilisateur pour un utilisateur individuel.

1. Une fois l&#39;utilisateur authentifié, le dispositif de diffusion en continu peut appeler le service de programmation pour demander le contenu auquel l&#39;utilisateur est autorisé à diffuser.

1. Le service de programmation doit appeler l’API Adobe Pass **preauthorize** avec une liste d’ID de ressources, qui sont une simple chaîne représentant généralement un canal qu’un utilisateur peut être autorisé à diffuser. *Remarque : actuellement, l’appel* ***preauthorize*** *est configuré pour limiter la liste à cinq (5) identifiants de ressource. Lorsque plus de cinq ressources sont nécessaires* plusieurs appels ***préautorisés*** *peuvent être effectués ou l’appel peut être configuré pour accepter plus de cinq ressources avec un accord des MVPD. Les personnes responsables de l’implémentation doivent garder à l’esprit le coût d’un appel* ***préautoriser*** *à la fois aux ressources de MVPD et le temps de réponse au programmeur, et structurer leur utilisation de l’appel de manière judicieuse.*

1. L’appel **preauthorize** répond au service de programmation avec un objet JSON contenant une valeur TRUE ou FALSE pour chaque ID de ressource dans la requête qui indique si l’utilisateur a droit ou non au canal associé. *Remarque : si un MVPD ne fournit pas de réponse pour un ID de ressource donné (par exemple en raison d’erreurs réseau ou de dépassements de délai), la valeur par défaut est FALSE.*

1. Le service de programmation doit utiliser la réponse d’appel **preauthorize** pour créer une réponse personnalisée définie par le programmeur pour l’appareil de diffusion en continu, généralement pour personnaliser la présentation à l’utilisateur en fonction de ses droits.

Le diagramme suivant illustre le flux de préautorisation :

![](../../../../assets/preauthz-flow.png)


### \[Facultatif\] Métadonnées

Les métadonnées peuvent être utilisées pour récupérer les informations utilisateur partagées par le MVPD.
Par exemple, l’identifiant utilisateur, le code postal, etc.

1. Une fois l’utilisateur authentifié, le service de programmation peut appeler l’API Adobe Pass **usermetadata** pour demander des informations sur l’utilisateur authentifié.

1. La réponse inclut toutes les métadonnées disponibles pour l’utilisateur donné. Les champs spécifiques sont configurés séparément pour chaque intégration Programmer/MVPD.

Le diagramme suivant illustre le flux de préautorisation :



![](../../../../assets/user-metadata-api-preauthz.png)



## Environnements et exigences fonctionnelles{#environments}



Un programmeur doit créer au moins deux environnements : un pour la production et un ou plusieurs pour l’évaluation.


### Production

L’environnement de production doit être hautement disponible et dimensionné de manière appropriée en cas de pics importants ou inattendus (sports en direct, rupture, etc.).
actualités).



Le service Adobe Pass s’exécute sur plusieurs centres de données répartis géographiquement à travers les États-Unis.  Pour obtenir le meilleur temps de réponse (c’est-à-dire la latence la plus faible) du service Adobe Pass, le programmeur doit également créer un service géographiquement dispersé similaire
les infrastructures.


Le service de programmation doit limiter le cache DNS à un maximum de 30 s au cas où l’Adobe devrait réacheminer le trafic. Cela peut se produire si un centre de données devient indisponible.


Le programmeur doit fournir la plage d’adresses IP publique de l’environnement de production. Ils seront entrés dans une liste autorisée d’adresses IP dans l’infrastructure Adobe Pass pour l’accès et gérés par les politiques d’utilisation équitable des API d’Adobe.

### Évaluation

L’environnement d’évaluation peut être minimal, mais il doit inclure tous les composants système et la logique commerciale. Il doit fonctionner de la même manière que la production et permettre de tester les versions en dehors de la production. Idéalement, l’environnement d’évaluation peut être connecté aux environnements de test Adobe Pass pour être utilisé par le programmeur et par Adobe si nécessaire, afin que nous puissions vous aider à effectuer les tests et à résoudre les problèmes.

### Exigences fonctionnelles

Le service Programmer doit transmettre des informations d’identification d’appareil précises pour l’appareil pour lequel il exécute les flux. En outre, le service Programmer doit transmettre l’adresse IP de l’appareil pour lequel il exécute les flux (dans un en-tête x-forwarded-for) avec le port source de connexion (dans le champ informations sur l’appareil ) :

    **X-Forwarded-For : \&lt;client\_ip\>**
    
    où \&lt;client\_ip\> est l’adresse IP publique du client
    
    
    
    L’en-tête doit être ajouté sur **regcode** et **authorize**&#x200B;appels
    
    Exemples :
    
    POST /reggie/v1/{req\_id}/regcode HTTP/1.1
    
    X-Forwarded-For:203.45.101.20
    
    
    
    GET /api/v1/authorize HTTP/1.1.1
    
    X-Forwarded-For:203.45.101.20



Le service Programmer doit envoyer les données et le formatage requis par les MVPD individuelles ou les applications intégrées (par exemple, l’adresse IP de l’appareil, le port source, les informations sur l’appareil, le MRSS, les données facultatives telles que l’ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.


Le service de programmation doit respecter les TTL authN et authZ lors de la mise en cache et de l’invalidation des sessions authN ou authZ lorsqu’elles sont notifiées.

Le programmeur doit conserver les certificats partagés avec l’Adobe.
