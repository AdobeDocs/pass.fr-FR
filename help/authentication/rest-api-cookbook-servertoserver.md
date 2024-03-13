---
title: Guide pas à pas de l’API REST (serveur à serveur)
description: Redéfinissez le serveur de livre de cuisine de l’API sur le serveur .
exl-id: 36ad4a64-dde8-4a5f-b0fe-64b6c0ddcbee
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '1847'
ht-degree: 0%

---

# Guide pas à pas de l’API REST (serveur à serveur) {#rest-api-cookbook-server-to-server}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.


## Vue d’ensemble {#overview}

L’objectif de ce guide est de décrire en détail les bonnes pratiques pour implémenter l’authentification Adobe Pass à l’aide des architectures serveur à serveur.  Il fournit des exigences de base, une implémentation de flux détaillée et des considérations générales pour les environnements de production et le fonctionnement.

### Mécanisme de ralentissement

L’API REST d’authentification Adobe Pass est régie par une [Mécanisme de ralentissement](/help/authentication/throttling-mechanism.md).


## Composants {#components}

Dans une solution serveur à serveur opérationnelle, les composants suivants sont impliqués :


| Type | Composant | Description |
| --- | --- | --- |
| Appareil de diffusion en continu | Application de diffusion en continu | L’application de programmation qui réside sur l’appareil de diffusion en continu de l’utilisateur et lit la vidéo authentifiée. |
| | \[Facultatif\] Module AuthN | Si le périphérique de diffusion comporte un agent utilisateur (c’est-à-dire un navigateur web), le module AuthN est responsable de l’authentification de l’utilisateur sur le MVPD IdP. |
| \[Facultatif\] Périphérique AuthN | Application AuthN | Si le périphérique de diffusion en continu ne dispose pas d’un agent utilisateur (c’est-à-dire un navigateur web), l’application AuthN est une application web de programmeur accessible à partir d’un périphérique d’un utilisateur distinct à l’aide d’un navigateur web. |
| Infrastructure de programmation | Service de programmation | Service qui relie le périphérique de diffusion en continu au service Adobe Pass pour obtenir les décisions d’authentification et d’autorisation. |
| Infrastructure d’Adobe | Service Adobe Pass | Un service qui s’intègre au service MVPD IdP et AuthZ et qui fournit des décisions d’authentification et d’autorisation. |
| Infrastructure MVPD | MVPD IdP | Un point de terminaison MVPD qui fournit un service d’authentification basé sur les informations d’identification pour valider l’identité de l’utilisateur. |
| | Service MVPD AuthZ | Un point de terminaison MVPD qui fournit des décisions d’autorisation basées sur les abonnements de l’utilisateur, les contrôles parental, etc. |


Les termes supplémentaires utilisés dans le flux sont définis dans la variable
[Glossaire](/help/authentication/glossary.md).

## Flux {#flows}

### Enregistrement du client dynamique (DCR)


Adobe Pass utilise DCR pour sécuriser les communications client entre une application ou un serveur de programmation et les services Adobe Pass. Le flux DCR est distinct, dépendant et prérequis. Vous pouvez le trouver dans [Enregistrement du client dynamique](/help/authentication/dynamic-client-registration.md).


### Authentification (authN)

Le flux d’authentification permet à un utilisateur de s’identifier à son MVPD pour déterminer si l’utilisateur dispose d’un compte valide.

1. L’utilisateur démarre l’application d’appareil en flux continu et tente de se connecter ou d’afficher du contenu protégé.
2. L’application d’appareil en flux continu envoie une requête au service de programmation pour déterminer si l’appareil est déjà authentifié.
3. Le service de programmation enregistre l’application à l’aide de DCR.
4. Le service de programmation vérifie l’état d’authentification du périphérique de diffusion en appelant le service Adobe Pass. **checkauthn** API.
5. Pour le cas où la variable **checkauthn** L’appel renvoie l’état d’authentification de l’appareil utilisateur, puis l’application peut passer au flux d’autorisation.
6. Pour le cas où la variable **checkauthn** L’appel renvoie l’état indiquant que l’appareil utilisateur n’est PAS authentifié, alors l’application doit attendre qu’une demande d’utilisateur se connecte.
7. Lorsque l’utilisateur demande de se connecter directement (par exemple, sélectionne le bouton de connexion) ou indirectement (par exemple, sélectionne du contenu protégé lorsqu’il n’est pas déjà authentifié), l’application de périphérique de diffusion émet une requête au service de programmation pour lancer l’authentification de l’utilisateur. Le service de programmation demande et reçoit un code d’enregistrement unique (regcode) en appelant le service Adobe Pass. **regcode** API.
8. Le service de programmation récupère également la liste des MVPD et attributs actuels en appelant le service Adobe Pass **config** API. Remarque : cette API peut également être appelée plus tôt dans le flux et mise en cache.
9. Le service de programmation renvoie le regcode à l’application de périphérique de diffusion en continu et la liste MVPD traitée demandée à l’étape \#7. Remarque : Le format de liste MVPD traité est spécifié par le programmeur et peut être filtré pour autoriser ou bloquer explicitement des MVPD spécifiques (c’est-à-dire des listes autorisées ou bloquées).
10. Si est différent de l’appareil AuthN (c’est-à-dire &quot;deuxième écran&quot;), par choix ou par nécessité (c’est-à-dire que l’appareil en flux continu ne prend pas en charge un agent utilisateur), l’appareil en flux continu doit afficher le code postal et un URI pour que l’utilisateur puisse accéder à l’application AuthN. L’utilisateur saisit l’URI dans l’agent utilisateur sur le périphérique AuthN pour lancer l’application AuthN, puis saisit le code postal dans cette application. Si le périphérique de diffusion en continu est identique au périphérique AuthN, le code postal peut être transmis par programmation au module AuthN.
11. Le module AuthN lance l’authentification de l’utilisateur avec le MVPD en affichant un sélecteur MVPD. Une fois que l’utilisateur a sélectionné le MVPD, les appels du module AuthN **authentifier** avec le regcode, qui redirige l’agent utilisateur vers le MVPD IdP. Lorsque l’utilisateur s’authentifie correctement avec le MVPD, l’agent utilisateur est redirigé vers le service Adobe Pass, où l’authentification réussie est enregistrée avec le regcode, puis redirigé vers le module AuthN.
12. Si le périphérique de diffusion en continu est différent du périphérique AuthN, le périphérique AuthN doit afficher un message d’authentification réussi à l’utilisateur et les étapes à suivre (par exemple, &quot;Succès\! Vous pouvez maintenant revenir à votre console de jeu pour continuer \[..\]&quot;). Si le périphérique de diffusion en continu est identique au périphérique AuthN, il peut détecter par programmation la fin de l’authentification.



Le diagramme suivant illustre le flux d’authentification :

![](assets/authn-flow.png)

### Authorization (authZ)

Le flux d’autorisation permet de déterminer si un utilisateur est autorisé à accéder au contenu demandé.

1. Chaque fois que l’utilisateur tente d’afficher du contenu protégé sur l’application de périphérique de diffusion en continu, l’application de périphérique de diffusion en continu appelle le service de programmation qui identifie le contenu et demande les autorisations et les informations nécessaires pour démarrer la diffusion en continu.
1. Le service de programmation appelle Adobe Pass. **autoriser** API transmettant l’ID de ressource avec d’autres paramètres requis. Le service Adobe appelle le service MVPD AuthZ avec l’ID de ressource et reçoit la décision d’autorisation qui est ensuite renvoyée au service de programmation. Cette décision d’autorisation sera mise en cache par le service Adobe Pass pendant une période configurable. À venir **autoriser** les appels du service de programmation vers le service Adobe Pass, la valeur mise en cache est renvoyée tant qu’elle est valide.
1. Si l’autorisation est accordée, le service de programmation doit appeler Adobe Pass **/tokens/media** API, qui renverra un jeton multimédia signé. Le service de programmation doit valider le jeton multimédia à l’aide de la bibliothèque de vérification de jeton multimédia (JAR). S’il est valide, le service de programmation doit renvoyer l’autorisation et les conditions nécessaires pour démarrer le flux (URL de diffusion, par exemple) demandés à l’étape \#1.
1. Si l’autorisation est refusée, la variable **autoriser** L’appel renvoie un code d’erreur et une description au service de programmation. Le service de programmation doit renvoyer le code d’erreur et la description (ou un message modifié par le programmeur) à la demande à l’étape \#1.

Le diagramme suivant illustre le flux d’autorisation :

![](assets/authz-flow.png)

### Déconnexion

Le flux de déconnexion permet à un utilisateur de supprimer l’identité actuellement associée à l’application.

1. Lorsque l’utilisateur demande de se déconnecter (c’est-à-dire de supprimer de l’appareil le compte MVPD actuel associé à l’application), l’application de périphérique en flux continu appelle le service de programmation lui demandant de se déconnecter de l’appareil.
1. Le service de programmation doit appeler Adobe Pass **déconnexion** API.

Le diagramme suivant illustre le flux de déconnexion :

![](assets/logout-flow.png)

### \[Facultatif\] Préautorisation (c.-à-d. Pré-vol)

La préautorisation peut être utilisée pour déterminer rapidement à partir d’un ensemble de ressources celles auxquelles un utilisateur peut avoir accès.  Le résultat de cet appel est généralement utilisé pour personnaliser l’interface utilisateur d’un utilisateur individuel.

1. Une fois l’utilisateur authentifié, l’appareil en flux continu peut appeler le service de programmation pour demander le contenu auquel l’utilisateur est autorisé à diffuser.

1. Le service de programmation doit appeler Adobe Pass **preautoriser** API avec une liste d’ID de ressource, qui sont une chaîne simple qui représente généralement un canal dont un utilisateur peut avoir le droit de diffuser. *Remarque : Actuellement, la variable* ***preautoriser*** *est configuré pour limiter la liste à cinq (5) identifiants de ressource. Lorsque plus de cinq ressources sont nécessaires, plusieurs* ***preautoriser*** *des appels peuvent être effectués ou l’appel peut être configuré pour accepter plus de cinq ressources avec un accord des MVPD. Les implémentateurs doivent garder à l’esprit le coût d’une* ***preautoriser*** *appelez à la fois aux ressources MVPD ainsi que le temps de réponse au programmeur et structurez leur utilisation de l’appel de manière judicieuse.*

1. La variable **preautoriser** L’appel répond au service de programmation avec un objet JSON contenant une valeur TRUE ou FALSE pour chaque ID de ressource de la requête qui indique si l’utilisateur a ou non droit au canal associé. *Remarque : Si un MVPD ne fournit pas de réponse pour un ID de ressource donné (en raison d’erreurs réseau ou de dépassements de délai, par exemple), la valeur est définie par défaut sur FALSE.*

1. Le service de programmation doit utiliser la variable **preautoriser** appelez la réponse pour créer une réponse personnalisée définie par le programmeur au périphérique de diffusion, généralement pour personnaliser la présentation à l’utilisateur en fonction de ses droits.

Le diagramme suivant illustre le flux de préautorisation :

![](assets/preauthz-flow.png)


### Métadonnées \[facultatif\]

Les métadonnées peuvent être utilisées pour récupérer les informations utilisateur partagées par le MVPD.
Il peut s’agir d’un ID utilisateur, d’un code postal, etc.

1. Une fois l’utilisateur authentifié, le service de programmation peut appeler Adobe Pass. **usermetadata** API pour demander des informations sur l’utilisateur authentifié.

1. La réponse comprend toutes les métadonnées disponibles pour l’utilisateur donné. Les champs spécifiques sont configurés séparément pour chaque intégration Programmer/MVPD.

Le diagramme suivant illustre le flux de préautorisation :



![](assets/user-metadata-api-preauthz.png)



## Environnements et exigences fonctionnelles{#environments}



Un programmeur doit créer au moins deux environnements : un pour la production et un ou plusieurs pour l’évaluation.


### Production

L’environnement de production doit être hautement disponible et adapté aux pics importants ou inattendus (par exemple, les sports en direct, les actualités de dernière heure).



Le service Adobe Pass s’exécute sur plusieurs centres de données géographiquement dispersés aux États-Unis.  Pour obtenir le meilleur temps de réponse (c’est-à-dire la latence la plus faible) à partir du service Adobe Pass, le programmeur doit également créer une infrastructure de service géographiquement dispersée similaire.


Le service Programmer doit limiter le cache DNS à un maximum de 30 s au cas où l’Adobe aurait besoin d’annuler le trafic. Cela peut se produire si un centre de données n’est plus disponible.


Le programmeur doit fournir la plage IP publique de l’environnement de production. Elles seront entrées dans une liste autorisée d’adresses IP dans l’infrastructure Adobe Pass pour l’accès et gérées par les stratégies d’utilisation de l’API équitables d’Adobe.

### Évaluation

L’environnement d’évaluation peut être minimal, mais doit inclure tous les composants système et la logique métier. Il doit fonctionner de la même manière que la production et permettre le test des versions en dehors de la production. Idéalement, l’environnement d’évaluation peut être connecté aux environnements de test Adobe Pass pour être utilisé par le programmeur et par Adobe si nécessaire afin que nous puissions vous aider à tester et résoudre les problèmes.

### Exigences fonctionnelles

Le service Programmer doit transmettre des informations d’identification précises de l’appareil pour lequel il exécute les flux. De plus, le service Programmer doit transmettre l’adresse IP de l’appareil pour lequel il exécute les flux (dans un en-tête x-transfer-for) avec le port source de la connexion (dans le champ d’informations sur l’appareil) :

    **X-Forwarded-For : \&lt;client _ip=&quot;&quot;>**
    
    où&lt;client _ip=&quot;&quot;> est l’adresse IP publique du client ;
    
    
    
    L’en-tête doit être ajouté aux appels **regcode** et **authorized**.
    
    Exemples :
    
    POST /reggie/v1/{req\_id}/regcode HTTP/1.1
    
    X-Forwarded-For:203.45.101.20
    
    
    
    GET /api/v1/autoriser HTTP/1.1
    
    X-Forwarded-For:203.45.101.20



Le service de programmation doit envoyer les données et la mise en forme requises par les MVPD individuels ou les applications intégrées (par exemple, IP de l’appareil, port source, informations sur l’appareil, MRSS, données facultatives telles que l’ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.


Le service de programmation doit respecter les TTL authN et authZ lors de la mise en cache et de l’invalidation des sessions authN ou authZ en cas de notification.

Le programmeur doit conserver les certificats partagés avec Adobe.

<!--
## Related Information {#related}

* [REST API Reference](/help/authentication/rest-api-reference.md)
* [Glossary of Terms](/help/authentication/adobe-pass-glossary.md)
-->
