---
title: Accès à l’API CMU
description: Accès à l’API CMU
exl-id: 8d216703-aabc-489e-93fe-d4d105616b1d
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 0%

---

# Accès à l’API d’utilisation de la surveillance de simultanéité {#cmu-api-usage-access}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée. Contactez votre représentant Adobe pour toute question sur la disponibilité.

## Présentation de la procédure d’accès {#api-access-procedure-overview}

Nous avons mis à jour l’accès aux rapports CMU pour qu’ils soient compatibles avec le protocole d’enregistrement client dynamique OAuth 2.0. Un serveur d’autorisation OAuth 2.0 personnalisé est déployé pour répondre aux besoins de l’application de surveillance de simultanéité. \
Pour que les applications clientes puissent utiliser l’autorisation OAuth 2.0, le serveur doit s’enregistrer de manière dynamique afin d’obtenir des informations spécifiques (informations d’identification client) et pouvoir interagir avec celles-ci. Dans le cadre du processus d’enregistrement, le client doit présenter un ensemble de métadonnées intégrées au point d’entrée d’enregistrement du client.
Ces métadonnées sont communiquées sous la forme d&#39;une instruction logicielle, qui contient un « software_id » pour permettre à notre serveur d&#39;autorisation de corréler différentes instances d&#39;une application utilisant la même instruction logicielle.
Une instruction de logiciel est un jeton Web JSON (JWT) qui affirme des valeurs de métadonnées sur le logiciel client sous la forme d’un lot. Lorsqu’elle est présentée au serveur d’autorisation dans le cadre d’une demande d’enregistrement du client, la déclaration du logiciel doit être signée numériquement ou MACed à l’aide de la signature web JSON (JWS). \
Vous trouverez une explication plus détaillée des instructions de logiciel et de leur fonctionnement dans la documentation officielle <a href="https://datatracker.ietf.org/doc/html/rfc7591" target="_blank">[RFC7591]</a>.
Suivez les étapes des sections ci-dessous pour y accéder.

## Étapes de la procédure d’accès {#access-procedure-steps}

1. disposer d’une application enregistrée dans le serveur Adobe Pass DCR ; Pour cette étape, veuillez contacter notre [équipe d’assistance](mailto:tve-support@adobe.com).

2. Obtenir le relevé du logiciel
   1. Accédez au tableau de bord Adobe Pass TVE [](https://experience.adobe.com/#/pass/authentication)
   2. Sélectionner le programmeur
   3. Accédez à l’onglet *Applications enregistrées*
   4. Sélectionner une application
   5. Cliquez sur Télécharger sur la ligne de l’application enregistrée pour laquelle vous souhaitez obtenir une déclaration de logiciel et enregistrez-la en tant que fichier sur votre ordinateur local
      <figure>
          <img src="../assets/programmer-download-software-statement-button.png"
               alt="Download Software Statement">
      </figure>

      <figure>
          <img src="../assets/software_statement_2.png"
               alt="Exemple d’instruction logicielle">
      </figure>

3. Obtention du jeton d’accès
   1. Obtenez les informations d’identification du client en utilisant l’instruction logicielle obtenue ci-dessus et en effectuant l’appel ci-dessous. Ainsi, une paire client_id - client_secret est obtenue, qui peut être utilisée pour obtenir le jeton d’accès.
      *Cette étape ne doit pas être effectuée à chaque fois. Elle ne doit être effectuée à nouveau qu’à l’expiration des informations d’identification.*
      <figure>
          <img src="../assets/dcr_request_1_get_client_credentials.png"
               alt="Obtenir les informations d’identification du client">
       </figure>

   2. Obtenez le jeton d’accès à l’aide de l’appel ci-dessous. Utilisez ce jeton d’accès pour appeler n’importe quelle API CMU jusqu’à ce que le jeton expire.
      *Cette étape ne doit être effectuée que si le dernier jeton généré a expiré.*
      <figure>
          <img src="../assets/dcr_get_access_token_call.png"
               alt="Obtenir le jeton d’accès">
       </figure>

4. Appeler l’API CMU - voir les informations connexes ci-dessous.
   <figure>
          <img src="../assets/call_cmu_reports_sample.png"
               alt="Appel de l’API CMU">
       </figure>

## Informations connexes {#related-information}

* [Présentation de CMU](/help/concurrency-monitoring/reports/cm-usage-reports.md)
* [API CMU](/help/concurrency-monitoring/reports/cmu-api.md)
