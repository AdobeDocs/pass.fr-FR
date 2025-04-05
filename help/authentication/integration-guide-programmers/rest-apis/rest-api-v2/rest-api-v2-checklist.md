---
title: Liste de contrôle V2 de l’API REST
description: Liste de contrôle V2 de l’API REST
source-git-commit: f0001d86f595040f4be74f357c95bd2919dadf15
workflow-type: tm+mt
source-wordcount: '2535'
ht-degree: 0%

---

# Liste de contrôle V2 de l’API REST {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Ce document regroupe en un seul endroit les exigences obligatoires et les pratiques recommandées pour les programmeurs implémentant des applications clientes qui utilisent l’authentification Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

Ce document doit être considéré comme faisant partie de vos critères d’acceptation lors de la mise en œuvre de l’API REST V2 et doit être utilisé comme une liste de contrôle pour vous assurer que toutes les étapes nécessaires ont été prises pour obtenir une intégration réussie.

## Exigences obligatoires {#mandatory-requirements}

### 1. Phase d’enregistrement {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Conditions requises</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Limitation de la portée de l'application enregistrée</i></td>
      <td>Utilisez une application enregistrée avec la portée API REST v2 .</td>
      <td>Risques déclenchant des réponses d’erreur HTTP 401 « Non autorisé », surchargeant les ressources système et augmentant la latence.</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>Mise en cache des informations d’identification du client</i></td>
      <td>Stockez les informations d’identification du client dans un espace de stockage persistant et réutilisez-les pour chaque demande de jeton d’accès.</td>
      <td>Risque de perte de l’authentification lors de la régénération des informations d’identification du client.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Mise en cache des jetons d’accès</i></td>
      <td>Stockez les jetons d’accès dans un espace de stockage persistant et réutilisez-les jusqu’à leur expiration. Ne demandez pas de nouveau jeton pour chaque appel API REST v2.</td>
      <td>Risque de surcharger les ressources système, d’augmenter la latence et de déclencher potentiellement des réponses d’erreur HTTP 429 « Too many requests ».</td>
   </tr>
</table>

### 2. Phase de configuration {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Conditions requises</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Récupération de la configuration</i></td>
      <td>Récupérez la réponse de configuration uniquement lorsque vous devez inviter l’utilisateur à sélectionner son MVPD (fournisseur de télévision) avant la phase d’authentification.<br/><br/>Il n’est pas nécessaire de récupérer la réponse de configuration lorsque :<ul><li>L’utilisateur est déjà authentifié.</li><li>L’utilisateur se voit proposer un accès temporaire.</li><li>L’authentification de l’utilisateur a expiré, mais celui-ci peut être invité à confirmer qu’il est toujours abonné au MVPD sélectionné précédemment.</li></ul></td>
      <td>Risque de surcharger les ressources système et d’augmenter la latence.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Mise en cache de la sélection du fournisseur de télévision</i></td>
      <td>Stockez la sélection du fournisseur de télévision payante (MVPD) de l’utilisateur dans un espace de stockage persistant afin de l’utiliser lors de toutes les phases suivantes :<ul><li>Dans le magasin de réponse de configuration, l’utilisateur a sélectionné MVPD « id ».</li><li>Dans le magasin de réponse de configuration, l’utilisateur a sélectionné MVPD « displayName ».</li><li>Dans le magasin de réponse de configuration, l’utilisateur a sélectionné MVPD « logoUrl ».</li></ul></td>
      <td>Risque de surcharger les ressources système et d’augmenter la latence.</td>
   </tr>
</table>

### 3. Phase d’authentification {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Conditions requises</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Démarrage du mécanisme d'interrogation</i></td>
      <td>Démarrez le mécanisme d’interrogation dans les conditions suivantes :<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Authentification effectuée dans l’application principale (écran)</a></b><ul><li>L’application principale (de diffusion en continu) doit commencer à interroger l’utilisateur ou l’utilisatrice lorsqu’il ou elle atteint la page de destination finale, une fois que le composant de navigateur charge l’URL spécifiée pour le paramètre « redirectUrl » dans la requête de point d’entrée Sessions.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Authentification effectuée dans une application secondaire (écran)</a></b><ul><li>L’application principale (de diffusion en continu) doit commencer à interroger l’utilisateur dès qu’il lance le processus d’authentification, juste après avoir reçu la réponse du point d’entrée Sessions et affiché le code d’authentification à l’utilisateur.</li></ul></td>
      <td>Risque de surcharger les ressources système, d’augmenter la latence et de déclencher potentiellement des réponses d’erreur HTTP 429 « Too many requests ».</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Arrêt du mécanisme d'interrogation</i></td>
      <td>Arrêtez le mécanisme d’interrogation sous les conditions suivantes : <br/><br/><b>Authentification réussie</b><ul><li>Les informations de profil de l’utilisateur ont été récupérées avec succès, confirmant leur statut d’authentification. L’interrogation n’est donc plus nécessaire.</li></ul><br/><b>Session d’authentification et expiration du code</b><ul><li>La session d’authentification et le code expirent, l’utilisateur ou l’utilisatrice doit redémarrer le processus d’authentification et l’interrogation à l’aide du code d’authentification précédent doit être arrêtée immédiatement.</li></ul><br/><b>Nouveau code d’authentification généré</b><ul><li>Si l’utilisateur demande un nouveau code d’authentification, la session existante est invalidée et l’interrogation avec le code d’authentification précédent doit être arrêtée immédiatement.</li></ul></td>
      <td>Risque de surcharger les ressources système, d’augmenter la latence et de déclencher potentiellement des réponses d’erreur HTTP 429 « Too many requests ».</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Configuration du mécanisme d'interrogation</i></td>
      <td>Configurez la fréquence du mécanisme d’interrogation dans les conditions suivantes :<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Authentification effectuée dans l’application principale (écran)</a></b><ul><li>L’application principale (de diffusion en continu) doit effectuer une interrogation toutes les 3 à 5 secondes.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Authentification effectuée dans une application secondaire (écran)</a></b><ul><li>L’application principale (de diffusion en continu) doit effectuer une interrogation toutes les 3 à 5 secondes.</li></ul></td>
      <td>Risque de surcharger les ressources système, d’augmenter la latence et de déclencher potentiellement des réponses d’erreur HTTP 429 « Too many requests ».</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Mise en cache des profils</i></td>
      <td>Stockez des parties des informations de profil de l’utilisateur dans un stockage persistant afin d’améliorer les performances et de minimiser les appels API REST v2 inutiles.<br/><br/>La mise en cache doit se concentrer sur les champs de réponse des profils suivants :<br/><br/><b>mvpd</b><ul><li>L’application cliente peut l’utiliser pour suivre le fournisseur de télévision sélectionné par l’utilisateur et continuer à l’utiliser pendant les phases de préautorisation ou d’autorisation.</li><li>Lorsque le profil utilisateur actuel expire, l’application cliente peut utiliser la sélection MVPD mémorisée et demander à l’utilisateur de confirmer.</li></ul><br/><b>attributs</b><ul><li>Utilisé pour personnaliser l’expérience utilisateur en fonction de différentes clés de métadonnées d’utilisateur (par exemple, zip, maxRating, etc.).</li><li>Les métadonnées de l’utilisateur sont disponibles une fois le flux d’authentification terminé. Par conséquent, l’application cliente n’a pas besoin d’interroger un point d’entrée distinct pour récupérer les informations de métadonnées de l’utilisateur, car elles sont déjà incluses dans les informations de profil.</li><li>Certains attributs de métadonnées peuvent être mis à jour pendant la phase d’autorisation, selon le MVPD (par exemple, la charte) et l’attribut de métadonnées spécifique (par exemple, householdID). Par conséquent, l’application cliente peut avoir besoin d’interroger à nouveau les API Profiles après autorisation pour récupérer les dernières métadonnées de l’utilisateur.</li></ul></td>
      <td>Risque de surcharger les ressources système, d’augmenter la latence et de déclencher potentiellement des réponses d’erreur HTTP 429 « Too many requests ».</td>
   </tr>
</table>

### 4. (Facultatif) Phase De Préautorisation {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Conditions requises</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Récupération Des Décisions De Préautorisation</i></td>
      <td>Utilisez les décisions de préautorisation pour le filtrage de contenu et jamais pour les décisions de lecture.</td>
      <td>Risque de violer les accords contractuels entre Programmer, MVPD et Adobe.<br/><br/>Risques de contournement de nos systèmes de surveillance et d'alerte.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Reprise Des Décisions De Préautorisation</i></td>
      <td>Gérez les <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">codes d’erreur améliorés</a> de manière appropriée et utilisez le champ d’action pour déterminer les étapes de correction nécessaires.<br/><br/>Seul un nombre limité de codes d’erreur améliorés justifie une nouvelle tentative, alors que la plupart nécessitent d’autres résolutions, comme spécifié dans le champ d’action.<br/><br/>Assurez-vous que tout mécanisme de reprise implémenté pour récupérer les décisions de préautorisation n’entraîne pas une boucle sans fin et limite les reprises à un nombre raisonnable (c’est-à-dire 2-3).</td>
      <td>Risque de surcharger les ressources système, d’augmenter la latence et de déclencher potentiellement des réponses d’erreur HTTP 429 « Too many requests ».</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Mise En Cache Des Décisions De Préautorisation</i></td>
      <td>Mettre en cache les décisions d’autorisation réussies en mémoire pour améliorer les performances et minimiser les appels API REST v2 inutiles, car les mises à jour d’abonnement pendant l’exécution de l’application sont peu fréquentes.</td>
      <td>Risque de surcharger les ressources système, d’augmenter la latence et de déclencher potentiellement des réponses d’erreur HTTP 429 « Too many requests ».</td>
   </tr>
</table>

### 5. Phase d’autorisation {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Conditions requises</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Récupération des décisions d’autorisation</i></td>
      <td>Obtention des décisions d’autorisation avant la lecture, qu’une décision de préautorisation existe ou non.<br/><br/>Autorisez les flux à continuer sans interruption même si le jeton multimédia expire pendant la lecture et demandez une nouvelle décision d’autorisation contenant un jeton multimédia (frais) lorsque l’utilisateur effectue sa prochaine demande de lecture, que ce soit pour la même ressource ou pour une autre ressource.<br/><br/>Les diffusions en direct s’exécutant pendant des périodes prolongées peuvent choisir de demander une nouvelle décision d’autorisation à la suite d’opérations vidéo, telles que la suspension du contenu, le lancement d’interruptions commerciales ou la modification des configurations au niveau des ressources lorsque le MRSS est soumis à des modifications.</td>
      <td>Risque de violer les accords contractuels entre Programmer, MVPD et Adobe.<br/><br/>Risques de contournement de nos systèmes de surveillance et d'alerte.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Reprise De Décisions D’Autorisation</i></td>
      <td>Gérez les <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">codes d’erreur améliorés</a> de manière appropriée et utilisez le champ d’action pour déterminer les étapes de correction nécessaires.<br/><br/>Seul un nombre limité de codes d’erreur améliorés justifie une nouvelle tentative, alors que la plupart nécessitent d’autres résolutions, comme spécifié dans le champ d’action.<br/><br/>Assurez-vous que tout mécanisme de reprise implémenté pour récupérer les décisions d’autorisation n’entraîne pas une boucle sans fin et limite les reprises à un nombre raisonnable (c’est-à-dire 2-3).</td>
      <td>Risque de surcharger les ressources système, d’augmenter la latence et de déclencher potentiellement des réponses d’erreur HTTP 429 « Too many requests ».</td>
   </tr>
</table>

### 6. Phase de déconnexion {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Conditions requises</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Prise en charge de la déconnexion</i></td>
      <td>Implémentez l’API logout pour permettre aux utilisateurs de se déconnecter manuellement, d’arrêter leur profil authentifié et de suivre le nom d’action v2 de l’API REST spécifié pour chaque profil supprimé :<ul><li>Pour les fichiers MVPD prenant en charge un point d’entrée de déconnexion, l’application cliente doit accéder à l’« URL » fournie dans un user-agent.</li><li>Pour les profils de type « appleSSO », l’application cliente nécessite de guider l’utilisateur pour également se déconnecter au niveau du partenaire (paramètres système d’Apple).</li></ul></td>
      <td>Risque de dysfonctionnement de l’application cliente en raison d’une prise en charge manquante côté application cliente.</td>
   </tr>
</table>

### 7. Paramètres et en-têtes {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Conditions requises</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>En-tête d’autorisation Envoyer</i></td>
      <td>Envoyez l’en-tête <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a> pour chaque requête API REST v2.</td>
      <td>Risques déclenchant des réponses d’erreur HTTP 401 « Non autorisé », surchargeant les ressources système et augmentant la latence.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Envoi d’un en-tête identifiant-appareil-AP</i></td>
      <td>Envoyez l’en-tête <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> pour chaque requête API REST v2.<br/><br/>Même lorsque la requête provient d’un serveur pour le compte d’un appareil, la valeur de l’en-tête AP-Device-Identifier doit refléter l’identifiant réel de l’appareil de diffusion en continu.</td>
      <td>Risque de déclencher des réponses d'erreur HTTP 400 « Bad Request », de surcharger les ressources système et d'augmenter la latence.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Envoyer L’En-Tête X-Device-Info</i></td>
      <td>Envoyez l’en-tête <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> pour chaque requête v2 de l’API REST.<br/><br/>Même lorsque la requête provient d’un serveur pour le compte d’un appareil, la valeur de l’en-tête X-Device-Info doit refléter les informations réelles de l’appareil de diffusion en continu.</td>
      <td>Risques classés comme provenant d’une plateforme inconnue et traités comme non sécurisés, devenant ainsi soumis à des règles plus restrictives, telles que des TTL d’authentification plus courtes.<br/><br/>De plus, certains champs, comme connectionIp et connectionPort, sont obligatoires pour des fonctions comme l’authentification de base à domicile de Spectrum.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Identifiant d’appareil stable</i></td>
      <td>Calculez et stockez un identifiant d’appareil stable qui ne change pas lors des mises à jour ou des redémarrages pour l’en-tête <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.<br/><br/>Pour les plateformes sans identifiant matériel, générez un identifiant unique à partir des attributs de l’application et conservez-le.</td>
      <td>Risque de perte d’authentification lorsque l’identifiant de l’appareil change.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Suivre les références des API</i></td>
      <td>Assurez-vous de n’envoyer que les paramètres et en-têtes attendus de l’API REST v2.</td>
      <td>Risque de déclencher des réponses d'erreur HTTP 400 « Bad Request », de surcharger les ressources système et d'augmenter la latence.</td>
   </tr>
</table>

### 8. Gestion des erreurs {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Conditions requises</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Amélioration De La Prise En Charge De La Gestion Du Code D’Erreur</i></td>
      <td>Gérez les <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">codes d’erreur améliorés</a> de manière appropriée et utilisez le champ d’action pour déterminer les étapes de correction nécessaires.<br/><br/>Seul un nombre limité de codes d’erreur améliorés justifie une nouvelle tentative, alors que la plupart nécessitent d’autres résolutions, comme spécifié dans le champ d’action.<br/><br/>La plupart des codes d’erreur améliorés répertoriés dans la documentation <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2">Codes d’erreur améliorés - API REST V2</a> peuvent être entièrement évités s’ils sont correctement gérés pendant la phase de développement avant le lancement de l’application.</td>
      <td>Risque de surcharger les ressources système, d’augmenter la latence et de déclencher potentiellement des réponses d’erreur HTTP 429 « Too many requests ».<br/><br/>Risque de dysfonctionnement de l’application cliente en raison d’une gestion manquante des codes d’erreur améliorés, ce qui entraîne des messages d’erreur flous, des conseils d’utilisation incorrects ou un comportement de secours incorrect.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Prise en charge de la gestion des erreurs HTTP</i></td>
      <td>Différenciez la gestion des réponses d’erreur HTTP (par exemple, 400, 401, 403, 404, 405, 500) et des réponses de succès (par exemple, 200, 201) qui contiennent des payloads de codes d’erreur améliorés, comme expliqué ci-dessus.<br/><br/>Seul un nombre limité de codes d’erreur HTTP justifie une nouvelle tentative, alors que la plupart nécessitent d’autres résolutions.<br/><br/>La plupart des réponses d’erreur HTTP peuvent être entièrement évitées si elles sont correctement gérées pendant la phase de développement avant le lancement de l’application.</td>
      <td>Risque de surcharger les ressources système, d’augmenter la latence et de déclencher potentiellement des réponses d’erreur HTTP 429 « Too many requests ».<br/><br/>Risque de dysfonctionnement de l’application cliente en raison d’une gestion manquante des codes d’erreur améliorés, ce qui entraîne des messages d’erreur flous, des conseils d’utilisation incorrects ou un comportement de secours incorrect.</td>
   </tr>
</table>

### 9. Tests {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Conditions requises</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Test du cycle de vie</i></td>
      <td>Développez et testez l’application à l’aide des environnements hors production Adobe Pass Authentication officiels :<ul><li>Préproduction</li><li>Release-Staging</li></ul><br/>Effectuez une assurance qualité (QA) approfondie dans ces environnements avant de passer en version de production.<br/><br/>Les applications clientes ne doivent pas passer en version de production sans avoir d’abord effectué la validation de bout en bout dans les environnements hors production.</td>
      <td>Risques de lancement avec des défauts critiques et majeurs.<br/><br/>L’absence d’un chemin de débogage court et efficace peut empêcher l’assistance et l’ingénierie Adobe d’intervenir rapidement.</td>
   </tr>
</table>

## Pratiques recommandées {#recommended-practices}

### 1. Phase d’enregistrement {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiques</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validation des jetons d’accès</i></td>
      <td>Vérifiez de manière proactive la validité du jeton d’accès et actualisez-le après expiration.<br/><br/>Assurez-vous que tout mécanisme de reprise permettant de gérer les erreurs HTTP 401 « Non autorisé » actualise d’abord le jeton d’accès avant de retenter la requête d’origine.</td>
      <td>Risques déclenchant des réponses d’erreur HTTP 401 « Non autorisé », surchargeant les ressources système et augmentant la latence.</td>
   </tr>
</table>

### 2. Phase de configuration {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiques</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Mise en cache de configuration</i></td>
      <td>Stockez la réponse de configuration en mémoire ou dans un espace de stockage persistant pendant une courte période (par exemple, 3 à 5 minutes) afin d’améliorer les performances et de minimiser les appels v2 inutiles de l’API REST.</td>
      <td>Risque de surcharger les ressources système et d’augmenter la latence.</td>
   </tr>
</table>

### 3. Phase d’authentification {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiques</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validation du code d’authentification (authentification du deuxième écran)</i></td>
      <td>Validez le code d’authentification envoyé par le biais de l’entrée utilisateur sur la deuxième application secondaire (écran) avant d’appeler l’API /api/v2/authenticate dans les conditions suivantes :<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd">Authentification effectuée dans l’application secondaire (écran) avec mvpd présélectionné</a></b><ul><li>Utilisez la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md">Reprendre la session d’authentification</a> - POST /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd">Authentification effectuée dans l’application secondaire (écran) sans mvpd présélectionné</a></b><ul><li>Utilisez l’option <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md">Récupérer la session d’authentification</a> - GET /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/>L’application cliente recevait une erreur si le code d’authentification fourni était mal saisi ou si la session d’authentification arrivait à expiration.</td>
      <td>Risque diverses réponses d'erreur et problèmes de workflow lors de l'authentification.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Prise en charge de plusieurs profils</i></td>
      <td>Assurez-vous que l’application cliente peut gérer plusieurs profils en invitant l’utilisateur à sélectionner un profil ou en appliquant une logique personnalisée, telle que la sélection automatique du profil ayant la période de validité la plus longue.</td>
      <td>Risque de dysfonctionnement de l’application cliente en raison d’une prise en charge manquante côté application cliente.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>(Facultatif) Prise En Charge Des Flux Non Basiques</i></td>
      <td>Prendre en charge les flux non basiques si l’activité de l’application cliente l’exige :<ul><li>Flux d’accès dégradés (fonctionnalité Premium)</li><li>Flux d’accès temporaires (fonctionnalité Premium)</li><li>Flux d’accès à authentification unique (fonctionnalité standard)</li></ul></td>
      <td>Risque de créer une expérience utilisateur loin d’être idéale.</td>
   </tr>
</table>

### 4. (Facultatif) Phase De Préautorisation {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiques</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Expérience utilisateur</i></td>
      <td>Affichez des commentaires utilisateur clairs si une décision de préautorisation est refusée, en utilisant les messages fournis par les MVPD ou Adobe via des codes d’erreur améliorés.</td>
      <td>Risque de créer une expérience utilisateur loin d’être idéale.</td>
   </tr>
</table>

### 5. Phase d’autorisation {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiques</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validation des jetons multimédia</i></td>
      <td>Validez les jetons multimédias à l’aide de la bibliothèque <a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier">Vérificateur de jetons multimédias</a>.</td>
      <td>Risques de fraude tels que l'arrachage de flux.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Expérience utilisateur</i></td>
      <td>Affichez des commentaires utilisateur clairs si une décision d’autorisation est refusée, en utilisant les messages fournis par les MVPD ou Adobe via des codes d’erreur améliorés.</td>
      <td>Risque de créer une expérience utilisateur loin d’être idéale.</td>
   </tr>
</table>

### 6. Phase de déconnexion {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiques</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Expérience utilisateur</i></td>
      <td>Évitez d’appeler automatiquement (par programmation) l’API de déconnexion dans des scénarios tels que le refus de la préautorisation ou de l’autorisation, car l’API de déconnexion ne doit être appelée qu’en réponse à une demande directe de l’utilisateur.</td>
      <td>Risque de dérouter l’utilisateur en raison de l’échec de l’authentification.</td>
   </tr>
</table>

### 7. Paramètres et en-têtes {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiques</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Réutiliser le code</i></td>
      <td>Réutilisez le code de l’API REST v1 pour calculer l’identifiant de l’appareil et les informations sur l’appareil avec des ajustements mineurs, mais assurez-vous d’envoyer uniquement les paramètres et en-têtes attendus de l’API REST v2.<br/><br/>Réutilisez le code de l’API REST v1 pour appeler l’API DCR afin de récupérer un jeton d’accès.</td>
      <td>-</td>
   </tr>
</table>

### 8. Tests {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Pratiques</th>
      <th style="background-color: #EFF2F7;">Risques</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Couverture de test</i></td>
      <td>Vérifiez que les flux de base suivants sont testés sur les appareils et plateformes :<br/><br/><b>Flux d’authentification</b><ul><li>Scénario d’authentification de l’application de Principal (écran)</li><li>Scénario d’authentification de l’application Secondaire (écran)</li></ul><br/><b> (Facultatif) Flux de préautorisation</b><ul><li>Scénario de décisions d’autorisation de test</li><li>Test du scénario de refus de décisions</li></ul><br/><b> Flux d’autorisation </b><ul><li>Scénario de décisions d’autorisation de test</li><li>Test du scénario de refus de décisions</li></ul><br/><b>Flux de déconnexion</b><br/><br/>Testez également d’autres flux d’accès, le cas échéant :<br/><br/><ul><li>Flux d’accès dégradés (fonctionnalité Premium)</li><li>Flux d’accès temporaires (fonctionnalité Premium)</li><li>Flux d’accès à authentification unique (fonctionnalité standard)</li></ul><br/>Couverture des principales intégrations MVPD (couvrant les fournisseurs les plus utilisés).</td>
      <td>Risque de défaillances imprévues dans la production, notamment sur des plateformes moins fréquemment testées ou des flux rares comme des scénarios négatifs.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Outils de test</i></td>
      <td>Utilisez le site web <a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a>.</td>
      <td>-</td>
   </tr>
</table>

## Résumé {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Phase</th>
      <th style="background-color: #EFF2F7;">Obligatoire</th>
      <th style="background-color: #EFF2F7;">(Fortement) Recommandé</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Inscription</i></td>
      <td>Mettre en cache les informations d’identification du client<br/><br/>Mettre en cache les jetons d’accès</td>
      <td>Valider et actualiser les jetons d’accès</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Configuration</i></td>
      <td>Minimiser les récupérations de réponse de configuration</td>
      <td>Réponse de configuration du cache</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Authentification</i></td>
      <td>Réglage fin du mécanisme d’interrogation <br/><br/> parties du cache des profils</td>
      <td>Prise en charge de plusieurs profils<br/><br/>Prise en charge de la fonctionnalité de dégradation (si les besoins de l’entreprise)<br/><br/>Prise en charge de la fonctionnalité TempPass (si les besoins de l’entreprise)<br/><br/>Prise en charge de la fonctionnalité d’authentification unique (si les besoins de l’entreprise)</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Préautorisation</i></td>
      <td>Autorisation du cache décisions de préautorisation<br/><br/>réglage fin du mécanisme de reprise</td>
      <td>Améliorer l’expérience utilisateur en utilisant des codes d’erreur pour les décisions de préautorisation refusées</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autorisation</i></td>
      <td>Récupérer la décision d’autorisation lorsque l’utilisateur demande le réglage fin du mécanisme de lecture<br/><br/>reprise</td>
      <td>Améliorez l’expérience utilisateur en utilisant des codes d’erreur pour la validation des décisions d’autorisation refusées<br/><br/>Jeton multimédia</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Déconnexion</i></td>
      <td>Implémenter l’API de déconnexion pour permettre aux utilisateurs de se déconnecter manuellement</td>
      <td>Évitez d’appeler automatiquement l’API de déconnexion</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Obligatoire</th>
      <th style="background-color: #EFF2F7;">(Fortement) Recommandé</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Paramètres et en-têtes</i></td>
      <td>Respecter les spécifications des en-têtes obligatoires</td>
      <td>Réutilisation du code de l’API REST v1</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Gestion des erreurs</i></td>
      <td>Implémenter une gestion des erreurs améliorée<br/><br/>Implémenter la gestion des erreurs HTTP</td>
      <td>-</td>
   </tr>
</table>
