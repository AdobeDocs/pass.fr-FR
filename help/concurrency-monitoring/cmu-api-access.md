---
title: Accès à l’API CMU
description: Accès à l’API CMU
exl-id: 8d216703-aabc-489e-93fe-d4d105616b1d
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 0%

---

# Accès à l’API de surveillance de la simultanéité {#cmu-api-usage-access}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée. Contactez votre représentant Adobe pour toute question concernant la disponibilité.

## Présentation des procédures d’accès {#api-access-procedure-overview}

Nous avons mis à jour l’accès aux rapports CMU pour qu’il soit compatible avec le protocole d’enregistrement dynamique du client OAuth 2.0. Un serveur d’autorisation OAuth 2.0 personnalisé est déployé pour répondre aux besoins de l’application de surveillance de la simultanéité. \
Pour que les applications clientes utilisent l’autorisation OAuth 2.0, le serveur doit s’enregistrer dynamiquement afin d’obtenir des informations spécifiques (informations d’identification du client) pour pouvoir interagir avec celle-ci. Dans le cadre du processus d’enregistrement, le client doit présenter un ensemble de métadonnées intégrées au point de terminaison d’enregistrement du client.
Ces métadonnées sont communiquées sous la forme d’une instruction logicielle, qui contient un &quot;software_id&quot; pour permettre à notre serveur d’autorisations de mettre en corrélation différentes instances d’une application à l’aide de la même instruction logicielle.
Une instruction logicielle est un jeton Web JSON (JWT) qui affirme les valeurs de métadonnées du logiciel client sous la forme d’un lot. Lorsqu’elle est présentée au serveur d’autorisations dans le cadre d’une demande d’enregistrement du client, l’instruction logicielle doit être signée numériquement ou au format MAC à l’aide de la signature web JSON (JWS). \
Vous trouverez une explication plus détaillée des instructions logicielles et de leur fonctionnement dans la documentation officielle <a href="https://datatracker.ietf.org/doc/html/rfc7591" target="_blank">[RFC7591]</a>.
Suivez les étapes des sections ci-dessous pour y accéder.

## Etapes de la procédure d’accès {#access-procedure-steps}

1. posséder une application enregistrée sur le serveur Adobe Pass DCR. Pour cette étape, contactez notre [équipe d&#39;assistance](mailto:tve-support@adobe.com).

2. Obtention de l’instruction logicielle
   1. Accédez à [Tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication)
   2. Sélectionner un programmeur
   3. Accédez à l’onglet *Applications enregistrées*
   4. Sélectionner une application
   5. Cliquez sur Télécharger sur la ligne de l’application enregistrée pour laquelle vous souhaitez obtenir un relevé logiciel et enregistrez-le sous forme de fichier sur votre ordinateur local.

      <figure>
          <img src="assets/programmer-download-software-statement-button.png"
               alt="Télécharger le relevé logiciel">
      </figure>

      <figure>
          <img src="assets/software_statement_2.png"
               alt="Exemple de relevé logiciel">
      </figure>

3. Obtention du jeton d’accès
   1. Obtenez les informations d’identification du client à l’aide de l’instruction logicielle obtenue ci-dessus et effectuez l’appel ci-dessous. De cette manière, une paire client_id - client_secret sera obtenue, qui peut être utilisée pour obtenir le jeton d’accès.

      *Cette étape ne doit pas être effectuée à chaque fois. Cela ne doit être fait que lorsque les informations d’identification expirent.*
      <figure>
          <img src="assets/dcr_request_1_get_client_credentials.png"
               alt="Obtention des informations d’identification du client">
       </figure>

   2. Obtenez un jeton d’accès à l’aide de l’appel ci-dessous. Utilisez ce jeton d’accès pour appeler toute API CMU jusqu’à ce que le jeton expire.

      *Cette étape ne doit être effectuée que si le dernier jeton généré a expiré.*
      <figure>
          <img src="assets/dcr_get_access_token_call.png"
               alt="Obtenir le jeton d’accès">
       </figure>

4. Appel de l’API CMU - voir les informations connexes ci-dessous.
   <figure>
          <img src="assets/call_cmu_reports_sample.png"
               alt="API d’appel CMU">
       </figure>

## Informations connexes {#related-information}

* [Présentation de CMU](/help/concurrency-monitoring/cm-usage-reports.md)
* [API CMU](/help/concurrency-monitoring/cmu-api.md)
