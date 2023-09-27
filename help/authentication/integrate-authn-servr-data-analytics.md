---
title: Intégration des données côté serveur d’authentification Adobe Pass dans Adobe Analytics
description: Intégration des données côté serveur d’authentification Adobe Pass dans Adobe Analytics
exl-id: c1f1f2a3-c98c-4aed-92ad-1f9bfd80b82b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1138'
ht-degree: 4%

---

# Intégration des données côté serveur d’authentification Adobe Pass dans Adobe Analytics

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Les clients de l’authentification Adobe Pass souhaitent afficher les données côté serveur de l’authentification Adobe Pass (Adobe Pass) dans le tableau de bord Adobe Analytics pour une consommation plus facile.

Les données serviront à effectuer le suivi de mesures TVE importantes, telles que les taux de conversion d’authentification par MVPD, les utilisateurs uniques en fonction de l’identifiant utilisateur MVPD, etc.

Il n’est pas prévu de remplacer une implémentation côté client si elle existe déjà, car le suivi de l’activité utilisateur ne peut pas être effectué au-delà des événements spécifiques ci-dessous en l’absence d’identifiant visiteur. Si les clients fournissent un identifiant visiteur sur les appels Pass (Transmettre), nous pouvons déverrouiller un autre type d’intégration Analytics (temps réel), qui peut joindre tous les événements Pass aux données client existantes, plus de détails sur ce nouveau type d’intégration possible ici : &quot;[Utilisation de l’ID Experience Cloud dans l’authentification Adobe Pass](/help/authentication/exp-cloud-id-authn.md)&quot;

## Mesures incluses {#metrics-included-int-authn-analyt}

| Événement | Description |
|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| AuthN demandée | Nombre de flux d’authentification lancés |
| AuthN en attente | Nombre de jetons d’authentification générés avec succès (sans tenir compte du fait que le client l’ait réellement obtenu) |
| AuthN OK | Nombre de jetons d’authentification obtenus avec succès par les utilisateurs |
| AuthZ demandée | Nombre de tentatives d’autorisation |
| AuthZ OK | Nombre d’autorisations réussies |
| AuthZ a échoué | Nombre d’autorisations refusées par les MVPD au niveau de l’application |
| Lecture de la requête | Nombre de jetons multimédias courts générés (qui s’assimilent au nombre de requêtes de lecture) |
| Déconnexion demandée | Nombre de flux de déconnexion initialisés |
| Déconnexion terminée | Nombre de flux de déconnexion terminés avec succès |
| Échec de la connexion | Nombre de flux de déconnexion ayant échoué |
| Préautorisation demandée | Nombre de flux de préautorisation lancés |
| Préautorisation OK | Nombre d’événements de préautorisation réussis avec les ressources préautorisées |
| Autorisation refusée | Nombre d’événements de préautorisation avec les ressources refusées |
| Échec de la préautorisation | Nombre d’événements de préautorisation ayant échoué |

| Nom Adobe Analytics | Description |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Canal | Identifiant du demandeur utilisé pour exécuter la demande de droit |
| MVPD | Le MVPD responsable de l’octroi des droits à l’utilisateur |
| Proxy | Le proxy MVPD (qui sera &quot;Direct&quot; pour les intégrations directes) |
| Type de SDK | Le SDK client utilisé (Flash, HTML5, Android natif, iOS, sans client, etc.) |
| Version du SDK | Version du SDK client d’authentification Adobe Pass |
| ID de ressource | Titre réel de la ressource impliquée dans la requête d’autorisation (extrait de la charge utile MRSS en tant qu’élément/titre s’il est fourni). |
| Type d’erreur AuthZ | Raison des échecs, comme indiqué par l’authentification Adobe Pass <br/> Voici les valeurs les plus courantes <br/> **noAuthZ** = le MVPD a répondu que l’utilisateur n’a pas le canal dans son package<br/> **network** = nous n’avons pas pu atteindre le MVPD (le MVPD a un problème au moment de l’appel et n’a pas répondu)<br/> **norefreshtoken** = ceci est strictement pour les implémentations OAuth et cela peut se produire si l’utilisateur a modifié son mot de passe ou si le MVPD l’a refusé pour une raison quelconque. Cela entraîne généralement une nouvelle authentification.<br/> **discordance** = si la requête est effectuée à partir d’un appareil différent de celui qui avait le jeton d’authentification. Cela peut se produire si les utilisateurs tentent de tromper le système, mais que la plupart d’entre eux se sont produits dans le contexte de notre ancien SDK JavaScript où l’identifiant de l’appareil utilisait l’adresse IP dans le cadre du calcul. Si un utilisateur regardait TVE à la maison puis au travail, cette erreur serait déclenchée et il devrait s’authentifier à nouveau.<br/> **non valide** = requête non valide, paramètres manquants ou non valides<br/>  **authzNone** = Les programmeurs peuvent refuser des autorisations pour une combinaison channelMVPD spécifique. Cela est déclenché par une API principale à laquelle les programmeurs ont accès.<br/> **fraude** = c&#39;est un mécanisme de protection de notre côté. Si l’utilisateur échoue à l’autorisation, puis le demande à nouveau un certain nombre de fois dans un court intervalle (secondes), nous refusons directement l’appel. Cela se produit généralement lorsqu’un programmeur a un bogue dans son implémentation qui demande constamment l’autorisation en cas d’échec. |
| Type de jeton | Lorsque des jetons sont créés en raison de AuthZ All et AuthN All, nous devons connaître les causes d’une mesure de dégradation.<br/> Ils sont :<br/> &quot;normal&quot; = cas normal<br/> &quot;authnall&quot; = Lorsque AuthN All est activé<br/> &quot;authzall&quot; = Lorsque AuthZ All est activé<br/>  &quot;hba&quot; = Lorsque l’adaptateur de bus hôte est activé |
| Type d’appareil sans client | Plateforme d’appareil (alternative), actuellement utilisée pour les clients sans client.<br/> Les valeurs peuvent être les suivantes :<br/> S/O : l’événement ne provient pas d’un SDK sans client<br/> Inconnu : depuis le paramètre deviceType d’un **API sans client** est facultatif, il existe des appels qui ne contiennent aucune valeur.<br/> Toute autre valeur envoyée via la variable **API sans client**. Par exemple, xbox, appletv et roku. |
| Identifiant utilisateur MVPD | Remplace l’identifiant visiteur basé sur les cookies |


## Détails {#details-int-authn-analyt}

* Les mesures seront insérées, événement par événement, dans la suite de rapports spécifique via l’API d’insertion de données Adobe Analytics.
* L&#39;insertion sera groupée et envoyée toutes les 30 minutes. En conséquence, le rapport doit être horodaté.
* Chaque client disposera d’une ou de plusieurs suites de rapports. Un identifiant du demandeur (canal) ne sera mappé qu’à une seule suite de rapports. Plusieurs ID de demandeur ne peuvent correspondre qu’à une seule suite de rapports.
* Des données historiques peuvent être fournies, mais une attention particulière doit être accordée en raison de problèmes de trafic/performances.
* La variable de visiteur unique est définie sur l’identifiant utilisateur MVPD.
* Le mappage des événements et des eVars n’est pas configurable.


## SLA {#sla-int-authn-serv-anal}

Comme ce n&#39;est pas un composant essentiel, il n&#39;y a aucune garantie SLA pour l&#39;intégration.

## Configuration de la suite de rapports {#report-suite-config}

Le rapport doit être horodaté, car les événements seront envoyés par lots.

### Événements {#report-suite-config-events}


>[!NOTE]
>Tous doivent être définis avec :
>
>* Compteur (sans sous-relations)

| Événement | Événement Adobe Analytics |
|---------------------------------------|-----------------------|
| AuthN demandée | event1 |
| AuthN en attente | event2 |
| AuthN OK | event3 |
| AuthZ demandée | event4 |
| AuthZ OK | event5 |
| AuthZ a échoué | event6 |
| Lecture de la requête | event7 |
| AuthN Failed | event8 |
| Demande de jeton AuthN sans client OK | event9 |
| Échec de la demande de jeton AuthN sans client | event10 |
| Échec de la requête de lecture | event11 |
| Demande de connexion | event12 |
| Déconnexion terminée | event13 |
| Échec de la connexion | event14 |
| Demande de contrôle en amont | event15 |
| Echec du contrôle en amont | event16 |
| Pré-contrôle accordé | event17 |
| Contrôle en amont refusé | event18 |


### eVars {#evars}


>[!NOTE]
>Tous doivent être définis avec :
>
>* Attribution : le plus récent (dernier)
>* Expire après : accès
>* Type : chaîne de texte

| Propriété | eVar |
|-----------------------------------|--------------------------------|
| Canal | eVar1 |
| MVPD | eVar2 |
| Proxy | eVar3 |
| Type de SDK | eVar4 |
| Version du SDK | eVar5 |
| ID de ressource | eVar6 |
| Type d’erreur AuthZ | eVar7 |
| Type de jeton | eVar8 |
| Type d’appareil sans client | eVar9 |
| Identifiant utilisateur MVPD | visitorID (fait automatiquement) |
| Identifiant utilisateur MVPD | eVar10 |
| Type de périphérique | eVar11 |
| Système d’exploitation | eVar12 |
| Type de matériel Principal | eVar13 |
| TTL | eVar14 |
| Type d’authentification | eVar15 |
| Version de l’architecture du serveur | eVar16 |
| Fournisseur d’authentification externe | eVar17 |
| Latence | eVar18 |
| Identifiant visiteur | eVar19 |
| Mécanisme SSO | eVar20 |
| Modèle de périphérique | eVar21 |
| Version du périphérique | eVar22 |
| Modèle matériel de périphérique | eVar23 |
| Fournisseur de matériel de périphérique | eVar24 |
| Fabricant du matériel du périphérique | eVar25 |
| Version du matériel du périphérique | eVar26 |
| Nom du système d’exploitation du périphérique | eVar27 |
| Famille du système d’exploitation de périphérique | eVar28 |
| Fournisseur du système d’exploitation de périphérique | eVar29 |
| Version du SE du périphérique | eVar30 |
| Nom du navigateur de l’appareil | eVar31 |
| Fournisseur de navigateur de périphérique | eVar32 |
| Version de l’explorateur de périphériques | eVar33 |
| Type d’appareil sans client normalisé | eVar34 |

## Tarifs {#pricing}

Pour plus d’informations, contactez votre TAM.
