---
title: Fonction TempPass
description: Fonction TempPass
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# Fonction TempPass {#temp-pass-feature}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

TempPass est une fonctionnalité polyvalente qui permet aux programmeurs d’offrir un accès temporaire à leur contenu protégé aux utilisateurs sans informations d’identification de compte MVPD valides. Il s’agit d’un outil efficace pour mobiliser les téléspectateurs, que ce soit par le biais de scénarios d’accès de base ou de campagnes promotionnelles ciblées.

TempPass est une solution puissante pour les programmeurs pour :

* **Engager les visionneuses :** proposez un avant-goût du contenu premium pour attirer de nouveaux abonnés.
* **Stimuler les promotions :** exécutez des campagnes ciblées pour augmenter l’exposition du contenu et fidéliser la marque.
* **Conserver le contrôle :** gérez les périodes d’accès, appliquez des limites et réinitialisez l’accès selon les besoins pour vous aligner sur les objectifs commerciaux.

La fonctionnalité TempPass est fournie en introduisant un pseudo-MVPD (appelé « Temp Pass ») dans la configuration du serveur d’authentification Adobe Pass en tant qu’intégration au programmeur participant. La fonction TempPass est disponible dans deux configurations :

* [TempPass de base](#basic-temp-pass) pour l’accès temporel.
* [Promotional TempPass](#promotional-temp-pass) pour un accès flexible piloté par les campagnes.

>[!IMPORTANT]
>
> La fonction TempPass est une fonction premium qui nécessite une licence actuelle d’Adobe.

Le tableau suivant présente une brève comparaison des fonctionnalités TempPass de base et promotionnelles :

| **Fonction** | **TempPass de base** | **TempPass promotionnel** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **Accès au contenu** | <ul><li>Basé sur l&#39;heure</li></ul> | <ul><li>Basé sur l&#39;heure</li><li>Limité à un nombre maximal de ressources</li></ul> |
| **Sécurité D’Accès Basée Sur** | <ul><li>Identifiant de l’appareil</li></ul> | <ul><li>Identifiant de l’appareil</li><li>Hachage des informations d’identifiant utilisateur fournies (par exemple, e-mail)</li></ul> |
| **Codes d’erreur améliorés** | Available | Available |
| **Fonction de réinitialisation de TempPass** | Available | Available |

>[!IMPORTANT]
> 
> L’authentification Adobe Pass n’inclut pas de mécanisme intégré pour arrêter automatiquement le flux en cours une fois le temps alloué (X minutes) écoulé. Il est de la responsabilité des programmeurs d’appliquer des restrictions d’accès une fois que le TempPass expire au cours d’un flux continu.

Que vous fournissiez un aperçu de votre bibliothèque de contenu ou que vous fassiez la promotion d’un événement de renom, TempPass vous fournit les outils nécessaires pour élargir votre audience tout en gardant le contrôle sur l’accès.

## TempPass de base {#basic-temp-pass}

La fonctionnalité de base de TempPass permet aux programmeurs de fournir un accès limité dans le temps au contenu en fonction de divers scénarios :

* **Prévisualisations courtes :** proposez des prévisualisations courtes, telles qu’une période d’accès quotidienne de 10 minutes, pour attirer les abonnés potentiels.
* **Accès basé sur les événements :** activez un accès plus long pour les événements majeurs, comme une session de 4 heures.
* **Accès combiné :** associez et faites correspondre des durées, telles qu’une période de visionnage initiale étendue suivie de prévisualisations quotidiennes plus courtes sur plusieurs jours.

Certains événements peuvent nécessiter un accès gratuit et échelonné au contenu, par exemple une période d’accès libre initiale prolongée (par exemple, 4 heures), suivie d’intervalles d’accès libre quotidiens plus courts (par exemple, 10 minutes par jour). Pour mettre en œuvre ce scénario, les programmeurs doivent se coordonner avec leur représentant Adobe pour configurer deux fichiers MVPD TempPass adaptés à leurs besoins.

Par exemple, pour proposer une première session gratuite de 4 heures suivie de sessions gratuites quotidiennes de 10 minutes, Adobe peut configurer pour le programmeur :

* **TempPass1** : configuré avec une durée de vie (TTL) de 4 heures pour couvrir la période d’accès gratuit initiale.
* **TempPass2** : configuré avec une durée de vie (TTL) de 10 minutes pour les intervalles d’accès libre quotidiens suivants.

Pour garantir un accès quotidien correct, TempPass2 doit être réinitialisé pour tous les appareils à 00:00 heures chaque jour.

### Détails des fonctionnalités {#basic-temp-pass-feature-details}

**Paramètres de configuration :**

* **TTL (Time-To-Live) :** les programmeurs peuvent spécifier la durée de l’accès. Cette TTL basée sur l’horloge expire quelle que soit l’heure d’affichage réelle.

**Identification de l&#39;utilisateur :**

La fonction TempPass de base utilise l’identifiant de l’appareil comme paramètre d’identification de l’utilisateur.

Le tableau suivant vous aide à comprendre comment les paramètres d’identification des utilisateurs influencent l’expérience d’évaluation des utilisateurs :

| Identifiant de l’appareil | Résultat |
|-------------------|----------------|
| Nouveau | Nouvelle version d’essai |
| Existant | Essai existant |

**Afficher le calcul de l’heure :**

La TTL représente la durée entre le temps de demande d’autorisation initiale et le temps d’expiration, indépendamment du temps réel passé à visionner le contenu. Chaque requête ultérieure vérifie l’heure actuelle du serveur par rapport à l’heure d’expiration stockée pour autoriser l’accès.

**Authentification:**

L’authentification n’est pas requise pour le TempPass de base, ce qui vous permet de passer directement à l’étape d’autorisation.

**Autorisation:**

Comme il n’y a pas d’interaction avec un MVPD réel, le MVPD « Temp Pass » de base autorise toute ressource étant donné que le TempPass est valide. En cas d’autorisation réussie, la bibliothèque du vérificateur de jeton multimédia reste applicable pour vérifier le jeton multimédia et assurer la validation des ressources avant de lancer la lecture du contenu.

La décision d’autorisation est basée sur les paramètres d’identification de l’utilisateur et la TTL configurée. Pour obtenir une autorisation réussie pour une ressource, les conditions suivantes doivent être remplies par une requête valide :

* **Durée non consommée :** la durée d’expiration est calculée en ajoutant la durée de demande d’autorisation initiale (enregistrée dans nos bases de données) à la TTL configurée. L’heure actuelle du serveur est comparée à cette heure d’expiration pour déterminer si le TempPass est toujours valide.

Si un utilisateur dépasse la durée de vie configurée, il ne pourra plus afficher le contenu sur le même appareil, sauf si son TempPass est réinitialisé.

**Préautorisation :**

Lorsqu’une demande de préautorisation est effectuée pour un MVPD « Temp Pass » de base, la réponse renvoie la liste complète des ressources de la demande comme préautorisées avec succès. Ce comportement reflète la logique d’autorisation, étant donné que les conditions d’autorisation sont basées sur des limites de temps, plutôt que sur des ressources spécifiques. Tant que la contrainte de temps est valide, les ressources demandées seront autorisées.

**Déconnexion:**

La déconnexion n’est pas requise pour le TempPass de base, ce qui vous permet de passer directement à l’étape d’authentification à l’aide d’un MVPD utilisateur réel.

**Données de tracking et analyses :**

Pendant le flux TempPass de base, les données de suivi utilisent une version hachée de l’identifiant de l’appareil, avec l’identifiant MVPD défini sur « Temp Pass ». Les programmeurs doivent différencier les mesures TempPass des mesures MVPD standard dans leurs implémentations d&#39;analyse.

## TempPass promotionnel {#promotional-temp-pass}

La fonction promotionnelle TempPass étend les fonctionnalités du TempPass de base, conçu spécifiquement pour l&#39;exécution de campagnes promotionnelles. Cette fonctionnalité permet aux programmeurs d’interagir avec les utilisateurs en leur permettant d’accéder à un nombre prédéfini de titres VOD pendant une période spécifiée après la collecte d’une identification utilisateur valide, telle qu’une adresse e-mail.

Le TempPass promotionnel comprend toutes les fonctionnalités du TempPass de base, avec une flexibilité accrue pour :

* Définir le nombre maximal de titres VOD accessibles pendant la période promotionnelle.
* Définir la période pendant laquelle l’accès promotionnel est valide.

Une fois que l’utilisateur dépasse les limites d’accès prédéfinies (nombre de titres VOD ou durée), il ne peut plus afficher le contenu sur le même appareil ou avec le même identifiant utilisateur, sauf si son TempPass est réinitialisé.

### Détails des fonctionnalités {#promotional-temp-pass-feature-details}

**Paramètres de configuration :**

* **Clé d’information de l’utilisateur :** clé utilisée pour communiquer l’identifiant fourni par l’utilisateur, tel qu’une adresse e-mail (c’est-à-dire que la clé est un e-mail).
* **Nombre de ressources :** définit le nombre de titres VOD auxquels un utilisateur peut accéder.
* **TTL (Time-To-Live) :** durée pendant laquelle l’utilisateur ou l’utilisatrice peut consommer les ressources autorisées.

**Identification de l&#39;utilisateur :**

La fonction Promotionnel TempPass utilise le hachage de l&#39;identifiant fourni par l&#39;utilisateur en plus de l&#39;identifiant de l&#39;appareil comme paramètres d&#39;identification de l&#39;utilisateur.

>[!IMPORTANT]
>
> La validation et le hachage de l’identifiant fourni par l’utilisateur sont gérés par le programmeur, et non par Adobe. Adobe ne stocke aucune information d’identification personnelle (PII). À ce titre, le programmeur est chargé de générer et d’envoyer un hachage de l’identifiant unique fourni par l’utilisateur lors de l’interaction avec les API d’authentification d’Adobe Pass.

Adobe recommande d’utiliser la famille **SHA-2** ou ses fonctions **SHA-256**, **SHA-512** spécifiques sur les données avant leur envoi à Adobe. Par exemple, la mention **SHA-256** sous **»user@domain.com »** est **« f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7 »**.

Le tableau suivant vous aide à comprendre comment les paramètres d’identification des utilisateurs influencent l’expérience d’évaluation des utilisateurs :

| Hachage D’Identifiant Fourni Par L’Utilisateur | Identifiant de l’appareil | Résultat |
|-------------------------------|-------------------|---------------------------------------------------------|
| Nouveau | Nouveau | Nouvelle version d’essai |
| Existant | Nouveau | Version d’évaluation existante (basée sur le hachage d’identifiant fourni par l’utilisateur) |
| Nouveau | Existant | Version d’évaluation existante (basée sur l’identifiant de l’appareil) |
| Existant | Existant | Essai existant |

**Afficher le calcul de l’heure :**

La TTL représente la durée entre le temps de demande d’autorisation initiale et le temps d’expiration, indépendamment du temps réel passé à visionner le contenu. Chaque requête ultérieure vérifie l’heure actuelle du serveur par rapport à l’heure d’expiration stockée pour autoriser l’accès.

**Authentification:**

L’authentification n’est pas requise pour le TempPass promotionnel, ce qui vous permet de passer directement à l’étape d’autorisation.

Pour prendre en charge l’implémentation de l’application du programmeur, le TempPass promotionnel expose les informations de métadonnées utilisateur suivantes, accessibles par le biais des clés correspondantes :

* **`remaining_resources`** : indique le nombre de ressources que l’utilisateur est toujours autorisé à consommer.
* **`used_assets`** : fournit une liste des ressources que l’utilisateur a déjà utilisées.
* **`expiration_date`** : affiche la date d’expiration de la passe temporaire promotionnelle de l’utilisateur.

**Autorisation:**

Comme il n’y a pas d’interaction avec un MVPD réel, le MVPD « Temp Pass » promotionnel autorise toute ressource étant donné que le TempPass est valide. En cas d’autorisation réussie, la bibliothèque du vérificateur de jeton multimédia reste applicable pour vérifier le jeton multimédia et assurer la validation des ressources avant de lancer la lecture du contenu.

La décision d’autorisation est basée sur les paramètres d’identification de l’utilisateur, ainsi que sur le nombre de ressources et la durée de vie configurés. Pour obtenir une autorisation réussie pour une ressource, les conditions suivantes doivent être remplies par une requête valide :

* **Durée non consommée :** la durée d’expiration est calculée en ajoutant la durée de demande d’autorisation initiale (enregistrée dans nos bases de données) à la TTL configurée. L’heure actuelle du serveur est comparée à cette heure d’expiration pour déterminer si le TempPass est toujours valide.
* **Ressources non consommées :** le nombre de ressources consommées est tracké (enregistré dans nos bases de données). Le nombre de ressources consommées est comparé au nombre de ressources configuré pour déterminer si le TempPass est toujours valide.

Si un utilisateur dépasse la durée de vie configurée ou le nombre de ressources, il ne pourra plus afficher le contenu sur le même appareil ou avec le même identifiant fourni par l’utilisateur, sauf si son TempPass est réinitialisé.

**Préautorisation :**

Lorsqu’une demande de préautorisation est effectuée pour un MVPD « Temp Pass » promotionnel, la réponse renvoie la liste complète des ressources de la demande comme préautorisées avec succès. Ce comportement reflète la logique d’autorisation, étant donné que les conditions d’autorisation sont basées sur des limites temporelles et le nombre total de ressources accessibles, plutôt que sur des ressources spécifiques. Tant que la contrainte de temps est valide et que la limite de ressources n&#39;a pas été dépassée, les ressources demandées seront autorisées.

**Déconnexion:**

La déconnexion n’est pas requise pour le TempPass promotionnel, ce qui vous permet de passer directement à l’étape d’authentification à l’aide d’un MVPD utilisateur réel.

**Données de tracking et analyses :**

Pendant le flux promotionnel de TempPass, les données de suivi utilisent une version hachée de l’identifiant de l’appareil, avec l’identifiant MVPD défini sur « Temp Pass ». Les programmeurs doivent différencier les mesures TempPass des mesures MVPD standard dans leurs implémentations d&#39;analyse.

## Réinitialiser l’accès à l’API TempPass {#reset-tempass-api-access}

Avant d’accéder à l’API Reset TempPass, vous devez suivre les étapes requises dans le processus d’enregistrement client dynamique (DCR). Ce processus obligatoire garantit que vous disposez du jeton d’accès nécessaire pour interagir avec l’API Reset TempPass.

Pour obtenir des instructions complètes, reportez-vous à la documentation [ Présentation de l’enregistrement client dynamique ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Réinitialiser l’API TempPass - DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

Pour réinitialiser un TempPass spécifique pour un ou tous les appareils, l’authentification Adobe Pass fournit aux programmeurs une API qui fonctionne pour les TempPass de base et promotionnels.

### Requête {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">hôte</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">méthode</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Paramètres de requête</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>Identifiant unique interne associé au fournisseur de services lors du processus d’intégration.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>Identifiant unique interne associé au TempPass pendant le processus d’intégration.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            Identifiant de l’appareil pour lequel cette opération de réinitialisation est valide.
            <br/><br/>
            Si aucune valeur n’est fournie, l’opération de réinitialisation s’applique à tous les appareils.
      </td>
      <td>facultatif</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisation</td>
      <td>La génération de la payload du jeton porteur est décrite dans la documentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Récupérer le jeton d’accès</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

### Réponse {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Texte</th>
      <th style="background-color: #EFF2F7;">Description</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Aucun contenu</td>
      <td>
        La réinitialisation a réussi.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Requête incorrecte</td>
      <td>
        La requête n’est pas valide, le client doit la corriger et réessayer.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non Autorisé</td>
      <td>
        Le jeton d’accès n’est pas valide, le client doit obtenir un nouveau jeton d’accès et réessayer. Pour plus d’informations, consultez la documentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> Présentation de l’enregistrement client dynamique </a> .
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Interdit</td>
      <td>
        Le jeton d’accès n’est pas valide, le client doit obtenir de nouvelles informations d’identification du client et un nouveau jeton d’accès, puis réessayez. Pour plus d’informations, consultez la documentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> Présentation de l’enregistrement client dynamique </a> .
      </td>
   </tr>
</table>

### Exemples {#reset-tempass-v3-reset-samples}

#### Réinitialiser TempPass pour un appareil spécifique {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### Réinitialiser TempPass pour tous les appareils {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## Réinitialiser l’API TempPass - DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

Pour réinitialiser un TempPass spécifique pour une clé générique (hachage d’identifiant fourni par l’utilisateur) ou toutes les clés, l’authentification Adobe Pass fournit aux programmeurs une API qui fonctionne pour le TempPass promotionnel.

### Requête {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">hôte</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">chemin</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">méthode</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Paramètres de requête</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>Identifiant unique interne associé au fournisseur de services lors du processus d’intégration.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>Identifiant unique interne associé au TempPass pendant le processus d’intégration.</td>
      <td><i>obligatoire</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">clé</td>
      <td>
            Hachage de l’identifiant fourni par l’utilisateur pour lequel cette opération de réinitialisation est valide.
            <br/><br/>
            Si aucune valeur n’est fournie, l’opération de réinitialisation s’applique à tous les utilisateurs.
      </td>
      <td>facultatif</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">En-têtes</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisation</td>
      <td>La génération de la payload du jeton porteur est décrite dans la documentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Récupérer le jeton d’accès</a>.</td>
      <td><i>obligatoire</i></td>
   </tr>
</table>

### Réponse {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Texte</th>
      <th style="background-color: #EFF2F7;">Description</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Aucun contenu</td>
      <td>
        La réinitialisation a réussi.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Requête incorrecte</td>
      <td>
        La requête n’est pas valide, le client doit la corriger et réessayer.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Non Autorisé</td>
      <td>
        Le jeton d’accès n’est pas valide, le client doit obtenir un nouveau jeton d’accès et réessayer. Pour plus d’informations, consultez la documentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> Présentation de l’enregistrement client dynamique </a> .
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Interdit</td>
      <td>
        Le jeton d’accès n’est pas valide, le client doit obtenir de nouvelles informations d’identification du client et un nouveau jeton d’accès, puis réessayez. Pour plus d’informations, consultez la documentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> Présentation de l’enregistrement client dynamique </a> .
      </td>
   </tr>
</table>

### Exemples {#reset-tempass-v3-reset-generic-samples}

#### Réinitialiser TempPass pour une clé spécifique {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### Réinitialiser TempPass pour toutes les clés {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## API REST V2 {#rest-api-v2}

L’utilisation de la fonction TempPass nécessite la mise en œuvre de mises à jour du code pour modifier la manière dont votre application TV Everywhere (TVE) interagit avec l’authentification Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

Pour obtenir un guide complet sur ces mises à jour et les workflows associés, reportez-vous à la documentation [Flux d’accès temporaires](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).
