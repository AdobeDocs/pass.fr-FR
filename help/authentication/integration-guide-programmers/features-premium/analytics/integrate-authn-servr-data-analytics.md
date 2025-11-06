---
title: Intégration des données côté serveur de l’authentification Adobe Pass dans Adobe Analytics
description: Intégration des données côté serveur de l’authentification Adobe Pass dans Adobe Analytics
exl-id: c1f1f2a3-c98c-4aed-92ad-1f9bfd80b82b
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# Intégration des données côté serveur de l’authentification Adobe Pass dans Adobe Analytics

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Les clients et clientes de l’authentification Adobe Pass souhaitent voir les données côté serveur de l’authentification Adobe Pass (Adobe Pass) dans le tableau de bord Adobe Analytics pour une consommation plus facile.

Les données serviront à effectuer le suivi de mesures TVE importantes telles que les taux de conversion d’authentification par MVPD, les utilisateurs uniques en fonction de l’identifiant utilisateur MVPD, etc.

Il n’est pas destiné à remplacer une implémentation côté client si elle existe déjà, car l’activité de l’utilisateur ne peut pas être suivie au-delà des événements spécifiques ci-dessous en l’absence d’identifiant visiteur. Si les clients fournissent un identifiant visiteur lors des appels Pass, nous pouvons déverrouiller un autre type d’intégration Analytics (en temps réel), qui peut joindre tous les événements Pass aux données client existantes. Vous trouverez plus d’informations sur ce nouveau type d’intégration ici : « [&#x200B; Utilisation de l’Experience Cloud ID dans l’authentification Adobe Pass &#x200B;](/help/authentication/integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md) »

## Mesures incluses {#metrics-included-int-authn-analyt}

| Événement | Description |
|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| Authentification demandée | Nombre de flux d’authentification lancés |
| Authentification en attente | Nombre de jetons d’authentification générés avec succès (que le client les ait obtenus ou non) |
| Authentification correcte | Nombre de jetons d’authentification obtenus par les utilisateurs |
| Authentification demandée | Nombre de tentatives d’autorisation |
| Authentification OK | Nombre d’autorisations réussies |
| Échec d’AuthZ | Nombre d&#39;autorisations refusées par les MVPD au niveau de l&#39;application |
| Requête de lecture | Nombre de jetons de médias courts générés (qui sont assimilés au nombre de requêtes de lecture) |
| Déconnexion demandée | Nombre de flux de déconnexion lancés |
| Déconnexion terminée | Nombre de flux de déconnexion terminés avec succès |
| Échec de la déconnexion | Nombre de flux de déconnexion ayant échoué |
| Préautorisation demandée | Nombre de flux de pré-autorisation lancés |
| Autorisation préalable OK | Nombre d’événements de préautorisation réussis avec les ressources qui ont été préautorisées |
| Préautorisation refusée | Nombre d’événements de préautorisation avec les ressources pour lesquels la préautorisation a été refusée |
| Échec de la préautorisation | Nombre d’événements de préautorisation ayant échoué |

| Nom d’Adobe Analytics | Description |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Canal | Identifiant du demandeur utilisé pour effectuer la demande de droit |
| MVPD | MVPD responsable de l’octroi des droits à l’utilisateur |
| Proxy | Le MVPD proxy (qui sera « Direct » pour les intégrations directes) |
| Type de SDK | Le SDK client utilisé (Flash, HTML5, Android natif, iOS, Clientless, etc.) |
| Version de SDK | Version du SDK du client d’authentification Adobe Pass |
| ID de ressource | Le titre réel de la ressource impliqué dans la demande d’autorisation (extrait de la payload MRSS comme l’élément/le titre, le cas échéant). |
| Type d’erreur AuthZ | La raison des échecs, telle qu’elle est signalée par le <br/> d’authentification Adobe Pass Voici les valeurs les plus courantes <br/> **noAuthZ** = le MVPD a répondu que l’utilisateur n’a pas le canal dans son package<br/> **network** = nous n’avons pas pu accéder au MVPD (MVPD a un problème au moment de l’appel et n’a pas répondu)<br/> **norefreshtoken** = ceci est strictement réservé aux implémentations OAuth et peut se produire si l’utilisateur modifie son mot de passe ou si le MVPD le lui refuse pour une raison quelconque. Cela entraîne généralement une nouvelle authentification<br/> **dismatch** = si la requête est effectuée à partir d’un appareil différent de celui qui avait le jeton d’authentification. Cela peut se produire si les utilisateurs tentent de tromper le système, mais que la plupart de ces événements se sont produits dans le cadre de notre ancien SDK JavaScript où l’identifiant de l’appareil utilisait l’adresse IP dans le cadre du calcul. Si un utilisateur regardait TVE à la maison, puis au travail, cette erreur se déclencherait et il devrait s’authentifier à nouveau<br/> **non valide** = requête non valide, paramètres manquants ou non valides<br/>  **authzNone** = Les programmeurs ont la possibilité de refuser les autorisations pour une combinaison channelxMVPD spécifique. Cela est déclenché par une API principale à laquelle les programmeurs ont accès<br/> **fraude** = c&#39;est un mécanisme de protection de notre côté. Si l’utilisateur ou l’utilisatrice ne parvient pas à obtenir l’autorisation, puis la demande à nouveau plusieurs fois dans un court intervalle (secondes), nous refusons directement l’appel. Cela se produit généralement lorsqu’un programmeur présente un bug dans son implémentation qui demande constamment une autorisation en cas d’échec. |
| Type de jeton | Lorsque des jetons sont créés en raison d’AuthZ All et AuthN All, nous devons savoir ce qui est créé par une mesure de dégradation.<br/> : <br/> « normal » = Le cas normal <br/> « authnall » = Lorsque AuthN All est activé <br/> « authzall » = Lorsque AuthZ All est activé <br/> « hba » = Lorsque HBA est activé |
| Type d’appareil sans client | Plateforme d’appareil (alternative) actuellement utilisée pour le sans client.<br/> Les valeurs peuvent être les suivantes : <br/> S/O - l’événement n’provient pas d’un SDK sans client <br/> Inconnu - Étant donné que le paramètre deviceType d’une **API sans client** est facultatif, certains appels ne contiennent aucune valeur.<br/> Toute autre valeur envoyée par l’intermédiaire de l’**API sans client**. Par exemple, xbox, appletv et roku. |
| ID d’utilisateur MVPD | Remplace l’identifiant visiteur basé sur un cookie |


## Détails {#details-int-authn-analyt}

* Les mesures seront insérées, événement par événement, dans la suite de rapports spécifique via l’API Data Insertion d’Adobe Analytics
* L’insertion sera effectuée par lots et envoyée toutes les 30 minutes. Pour cette raison, le rapport doit être horodaté
* Chaque client dispose d’une ou de plusieurs suites de rapports. Un identifiant de demandeur (canal) ne sera mappé qu’à une seule suite de rapports. Plusieurs identifiants de demandeur ne peuvent mapper qu’à une seule suite de rapports.
* Des données historiques peuvent être fournies, mais une attention particulière devra être accordée en raison de problèmes de trafic/performances.
* La variable de visiteur unique est définie sur l’ID d’utilisateur MVPD
* Le mappage des événements et des eVars n’est pas configurable.


## SLA {#sla-int-authn-serv-anal}

Comme il ne s’agit pas d’un composant critique, SLA ne garantit pas l’intégration.

## Configuration de la suite de rapports {#report-suite-config}

Le rapport doit être horodaté, car les événements seront envoyés par lots.

### Événements {#report-suite-config-events}


>[!NOTE]
>Tout doit être défini avec :
>
>* Compteur (pas de sous-relations)

| Événement | Événement Adobe Analytics |
|---------------------------------------|-----------------------|
| Authentification demandée | event1 |
| Authentification en attente | event2 |
| Authentification correcte | event3 |
| Authentification demandée | event4 |
| Authentification OK | event5 |
| Échec d’AuthZ | event6 |
| Requête de lecture | event7 |
| Échec de l’authentification | event8 |
| Demande de jeton d’authentification sans client OK | event9 |
| Échec de la demande de jeton d’authentification sans client | event10 |
| Échec de la requête de lecture | event11 |
| Demande de déconnexion | event12 |
| Déconnexion terminée | event13 |
| Échec de la déconnexion | event14 |
| Demande de contrôle en amont | event15 |
| Échec du contrôle en amont | event16 |
| Contrôle en amont accordé | event17 |
| Contrôle en amont refusé | event18 |


### eVars {#evars}


>[!NOTE]
>Tout doit être défini avec :
>
>* Attribution : la plus récente (dernière)
>* Expire après : accès
>* Type : chaîne de texte

| Propriété | eVar |
|-----------------------------------|--------------------------------|
| Canal | EVAR1 |
| MVPD | EVAR2 |
| Proxy | EVAR3 |
| Type de SDK | EVAR4 |
| Version de SDK | EVAR5 |
| ID de ressource | EVAR6 |
| Type d’erreur AuthZ | EVAR7 |
| Type de jeton | EVAR8 |
| Type d’appareil sans client | EVAR9 |
| ID d’utilisateur MVPD | visitorID (effectué automatiquement) |
| ID d’utilisateur MVPD | eVar10 |
| Type d’appareil | eVar11 |
| Système d’exploitation | eVar12 |
| type de matériel du Principal | eVar13 |
| TTL | eVar14 |
| Type d’authentification | eVar15 |
| Version de l’architecture du serveur | eVar16 |
| Fournisseur d’authentification externe | eVar17 |
| Latence | eVar18 |
| Identifiant Visiteur | eVar19 |
| Mécanisme SSO | eVar20 |
| Modèle d’appareil | eVar21 |
| Version de l’appareil | eVar22 |
| Modèle matériel du périphérique | eVar23 |
| Fournisseur du matériel de l’appareil | eVar24 |
| Fabricant du matériel du périphérique | eVar25 |
| Version du matériel du périphérique | eVar26 |
| Nom du système d’exploitation de l’appareil | eVar27 |
| Famille du système d’exploitation de l’appareil | eVar28 |
| Fournisseur du système d’exploitation de l’appareil | eVar29 |
| Version du système d’exploitation de l’appareil | eVar30 |
| Nom du navigateur d’appareils | eVar31 |
| Fournisseur du navigateur d’appareils | eVar32 |
| Version du navigateur d’appareils | eVar33 |
| Type d’appareil sans client normalisé | eVar34 |

## Tarification {#pricing}

Veuillez contacter votre TAM pour plus d&#39;informations.
