---
title: Métadonnées utilisateur
description: Métadonnées utilisateur
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '1902'
ht-degree: 0%

---

# Métadonnées utilisateur {#user-metadata}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

Les métadonnées utilisateur font référence à des [attributs](#attributes) spécifiques à l’utilisateur (par exemple, codes postaux, évaluations parentales, ID utilisateur, etc.) qui sont gérés par les MVPD et fournis aux programmeurs via l’API Adobe Pass Authentication [API REST V2](#apis).

Les métadonnées de l’utilisateur sont disponibles une fois le flux d’authentification terminé, mais certains attributs de métadonnées peuvent être mis à jour pendant le flux d’autorisation, selon le MVPD et l’attribut de métadonnées spécifique en question.

Les métadonnées utilisateur peuvent être utilisées pour améliorer la personnalisation des utilisateurs et utilisatrices, mais aussi pour les analyses. Par exemple, un programmeur peut utiliser le code postal d’un utilisateur pour diffuser des informations localisées ou des mises à jour météorologiques, ou pour appliquer le contrôle parental.

L’authentification Adobe Pass normalise les valeurs de métadonnées utilisateur lorsque les fichiers MVPD fournissent des données dans différents formats. En outre, pour certains attributs (par exemple, code postal), les valeurs peuvent être [chiffrées](#encryption) à l’aide d’un certificat de programmeur.

L’authentification Adobe Pass permet aux programmeurs de consulter les métadonnées de l’utilisateur mises à disposition dans leurs intégrations MVPD et de les [gérer](#management) via le [tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

## Attributs de métadonnées de l’utilisateur {#attributes}

Le tableau suivant répertorie certains attributs de métadonnées utilisateur mis à la disposition des programmeurs :

| Clé | Type | Sample | Nécessite un chiffrement | Description | Détails |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | String | « 1o7241p » | Non | Identifiant du compte. | La valeur d’attribut peut être un identifiant de foyer ou un identifiant de sous-compte. La valeur `userID` sera différente de la `householdID` si le MVPD prend en charge les sous-comptes et que l’utilisateur actuel n’est pas le titulaire du compte principal. |
| `upstreamUserID` | String | « 1o7241p » | Non | Identifiant de compte pour la surveillance de simultanéité. | La valeur d’attribut peut être utilisée pour appliquer des limites d’accès simultané sur les sites et applications MVPD et Programmer . La valeur `upstreamUserID` est identique à la valeur `userID` pour la plupart des MVPD. |
| `householdID` | String | « 1o7241p » | Non | Identifiant du compte pour le contrôle parental. | La valeur d’attribut peut être utilisée pour différencier l’utilisation des ménages et des sous-comptes. Parfois, il peut être utilisé comme un substitut du contrôle parental si les vraies notes ne sont pas disponibles, si l&#39;utilisateur a été connecté avec le compte du ménage, il peut regarder, sinon le contenu noté ne s&#39;afficherait pas. La représentation de ce paramètre varie beaucoup d’une MVPD à l’autre (par exemple, identifiant utilisateur de ménage, identifiant de ménage, indicateur de chef de ménage, etc.). Si le MVPD ne prend pas en charge les sous-comptes, la représentation sera identique à celle de `userID`. |
| `primaryOID` | String | « uuidd1e19ec9-012c-124f-b520-acaf118d16a0 » | Non | Identifiant du compte. | L’attribut est spécifique à AT&amp;T. La valeur `primaryOID` est identique à la valeur `userID` lorsque la valeur `typeID` est définie sur « Principal ». |
| `typeID` | String |  »Principal » | Non | Attribut qui indique si l’utilisateur actuel est un titulaire de compte principal ou secondaire. | L’attribut est spécifique à AT&amp;T. La valeur `primaryOID` est identique à la valeur `userID` lorsque la valeur `typeID` est définie sur « Principal ». |
| `is_hoh` | String | « 1 » | Non | Attribut qui indique si l’utilisateur actuel est le chef de ménage ou non. | L&#39;attribut est spécifique à Synacor. |
| `hba_status` | Booléen | « true » | Non | Attribut qui indique si l&#39;utilisateur actuel s&#39;est authentifié via l&#39;adaptateur HBA ou non. |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | Booléen | « true » | Non | Attribut qui indique si l’appareil actuel peut ou non mettre en miroir l’écran. | L&#39;attribut est spécifique au spectre. |
| `zip` | Tableau | \[ »77754 », « 12345« \] | Oui | Code postal de l’utilisateur. | La valeur d’attribut peut être utilisée pour diffuser des actualités localisées, des mises à jour météorologiques ou des événements sportifs. La valeur `zip` représente les données sensibles qui nécessitent des accords juridiques avec le MVPD. Lorsqu’elle est chiffrée, la représentation de la clé `zip` sera un `String` au lieu d’un `Array`. |
| `encryptedZip` | String | « » | Oui | Code postal chiffré de l’utilisateur. | L’attribut est spécifique à Comcast. |
| `channelID` | Tableau | \[« channel-1 », « channel-2 »\] | Non | Liste des canaux que l’utilisateur est autorisé à consulter. | La valeur d’attribut peut être utilisée pour filtrer différents canaux des portails qui agrègent plusieurs réseaux. Nous vous recommandons d’utiliser l’API [Preauthorize](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) au lieu de cet attribut de métadonnées utilisateur pour filtrer les canaux qui ne sont pas disponibles pour l’utilisateur. |
| `maxRating` | Objet | { MPAA : « NR », VCHIP : « X », URL : « http://manage.my/parental » } | Non | Évaluation parentale maximale pour l’utilisateur actuel. | La valeur d’attribut peut être utilisée pour filtrer le contenu qui n’est pas adapté à l’utilisateur actuel en fonction des évaluations « MPAA » ou « VCHIP ». |
| `language` | String | « Anglais » | Non | Paramètres de langue. | La valeur d’attribut peut être utilisée pour afficher des messages en fonction des préférences linguistiques de l’utilisateur ou de l’utilisatrice. |

Les attributs de métadonnées utilisateur mis à la disposition d’un programmeur dépendent de ce que fournit un MVPD. Le tableau suivant répertorie les attributs rendus disponibles par divers MVPD :

|                         | **Accord légal signé (zip uniquement)** | **ID utilisateur sur AuthN** | **ID d’utilisateur en amont sur AuthN** | **ID de ménage sur AuthN/Z** | **OID de Principal sur AuthN** | **ID de type sur AuthN** | **Chef de ménage sur AuthN** | **Statut du HBA** | **Autoriser la mise en miroir sur AuthZ** | **Code postal sur AuthN/Z** | **Identifiant du canal sur AuthN** | **Évaluation sur AuthN/Z** | **Langue** | **onNet** | **inHome** | **Notes** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **Nom officiel** | s.o. | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **Nécessite un chiffrement** | s.o. | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** |                                                                                                                                           |
| **Sensible** | s.o. | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** |                                                                                                                                           |
| IdP Adobe | **Oui** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Oui** | **Oui** | **Oui** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | Accord juridique non nécessaire. |
| Synacor | **Oui** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | Accord juridique ne couvrant pas tous les MVPD proxy. Il s&#39;agit d&#39;une prise en charge générique de Synacor, qui n&#39;est peut-être pas intégrée à tous leurs MVPD. |
| Plat | **Non** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | Il partage la même liste que tous les MVPD de Synacor, plus `upstreamUserID`. |
| Comcast | **Non** | **Oui** | **Oui** | **Oui (AuthZ uniquement)** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Oui (AuthZ uniquement)** | **Non** | **Non** | **Non** |                                                                                                                                           |
| AT&amp;T | **Oui** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | Accord légal signé. |
| DTV | **Oui** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** |                                                                                                                                           |
| COX | **Non** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** |                                                                                                                                           |
| Télévision Par Câble | **Oui** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Oui** | **Non** | **Non** | **Non** | **Non** | Accord légal signé. |
| Spectre | **Oui** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** |                                                                                                                                           |
| Charte | **Oui** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** |                                                                                                                                           |
| Version | **Non** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** |                                                                                                                                           |
| HTC | **Non** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** |                                                                                                                                           |
| Rogers | **Non** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** |                                                                                                                                           |
| RCN | **Oui** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** |                                                                                                                                           |
| Lien Est | **Non** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** |                                                                                                                                           |
| Cogeco | **Non** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** |                                                                                                                                           |
| Vidéotron | **Non** | **Oui** | **Oui** | **Oui*** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | Il expose les `householdID` avec la même valeur que les `userID`. |
| Mission du proxy | **Oui** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | Accord légal signé. |
| Proxy Clearleap | **Oui** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Oui (AuthZ uniquement)** | **Oui** | **Non** | **Non** | Accord légal signé. |
| GLDS du proxy | **Non** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** |                                                                                                                                           |
| Autres MVPD | **Non** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | Aucun accord juridique pour le moment, métadonnées sensibles non disponibles pour la production. Pour tous les MVPD, le `userID` est disponible sans travail supplémentaire. |

>[!IMPORTANT]
>
> Des accords juridiques doivent être signés avec les MVPD avant que des métadonnées d&#39;utilisateur sensibles (par exemple, le code postal) ne soient disponibles.

## Chiffrement des métadonnées de l’utilisateur {#encryption}

Pour chiffrer et déchiffrer les attributs de métadonnées de l’utilisateur, le programmeur doit générer un certificat (paire de clés publique/privée) et [s’auto-configurer](#management) le certificat via le [tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) ou partager la clé publique avec les représentants de l’authentification Adobe Pass.

Suivez les étapes ci-dessous pour vous assurer que le certificat est généré et configuré correctement :

1. Téléchargez et installez le kit d’outils OpenSSL (http://www.openssl.org).

1. Générez une demande de signature de certificat (CSR) :

   * Générez une paire de clés. Sur votre terminal de commande, exécutez ce qui suit :

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Générez la demande de signature de certificat. Sur votre terminal de commande, exécutez ce qui suit :

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     Vous serez invité à saisir le mot de passe de la clé privée.

   * Créez une copie de sauvegarde de votre clé privée et de votre mot de passe. Exemple de CSR :

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. Envoyez la demande de signature de certificat à une autorité de certification (par exemple, Verisign).

1. L&#39;autorité de certification vous enverra le certificat au format .p7b (PKCS#7, Cryptographic Message Syntax Standard).

1. Déployez le certificat .p7b. Convertissez le fichier PKCS#7 (.p7b) en fichier PKCS#12 (fichier PFX, Personal Information Exchange Syntax Standard) à l’aide de votre clé privée et générez le fichier PEM (fichier conteneur de certificats concaténés) :

   * Convertissez le fichier PKCS#7 en fichier PEM temporaire. Sur votre ligne de commande, exécutez ce qui suit :

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Convertissez le fichier PEM temporaire en fichier PFX. Sur votre ligne de commande, exécutez ce qui suit :

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Convertissez le fichier PEM temporaire en fichier PEM final. Sur votre ligne de commande, exécutez ce qui suit :

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Utilisez le fichier PEM pour [configurer](#management) le certificat via [le tableau de bord Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) ou envoyez le fichier PEM aux représentants de l’authentification Adobe Pass.

   * Pour plus d’informations sur la gestion des certificats via le tableau de bord [Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication), consultez la section suivante.

   * L’authentification Adobe Pass prend en charge un certificat principal et un certificat de sauvegarde. Si votre certificat principal est compromis de quelque manière que ce soit, vous pouvez le révoquer et passer au certificat secondaire. Cela garantit une transition fluide entre les certificats avec un impact minimal sur le client.

## Gestion des métadonnées de l’utilisateur {#management}

>[!IMPORTANT]
>
> Si vous n’avez pas accès au tableau de bord Adobe Pass TVE, créez un ticket via notre [Zendesk](https://adobeprimetime.zendesk.com) et demandez à votre gestionnaire de compte technique (TAM) d’apporter les modifications appropriées pour vous.

Le tableau de bord Adobe Pass TVE est un outil permettant aux clients du service d’authentification d’Adobe Pass (les programmeurs) de gérer leur configuration et leurs données. Ce tableau de bord en libre-service active un éventail de fonctionnalités qui sont décrites dans la documentation Guide de l’utilisateur du tableau de bord TVE d’Adobe Pass [](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md).

Pour passer en revue et gérer les attributs de métadonnées utilisateur rendus disponibles par un MVPD, suivez les étapes de la documentation du [Guide d’utilisation du tableau de bord TVE pour les intégrations](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata) .

Pour vérifier et gérer les certificats utilisés pour chiffrer les attributs de métadonnées utilisateur, suivez les étapes des documents [Guide d’utilisation du tableau de bord TVE pour les programmeurs](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates) ou [Guide d’utilisation du tableau de bord TVE pour les canaux](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates).

## API REST V2 {#rest-api-v2}

Les attributs de métadonnées de l’utilisateur peuvent être récupérés à l’aide des API suivantes :

* [Récupération des profils](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Récupération du profil pour un fichier mvpd spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Récupération du profil pour un code spécifique](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Reportez-vous aux sections **Réponse** et **Exemples** des API ci-dessus pour comprendre la structure des attributs de métadonnées de l’utilisateur.

>[!IMPORTANT]
>
> Les métadonnées de l’utilisateur sont disponibles une fois le flux d’authentification terminé. Par conséquent, l’application cliente n’a pas besoin d’interroger un point d’entrée distinct pour récupérer les informations [métadonnées de l’utilisateur](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), car elles sont déjà incluses dans les informations de profil.

Pour plus d’informations sur comment et à quel moment intégrer les API ci-dessus, reportez-vous aux documents suivants :

* [Flux de profils de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flux de profils de base exécuté dans l’application secondaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Certains attributs de métadonnées peuvent être mis à jour pendant le flux d’autorisation, selon le MVPD et l’attribut de métadonnées spécifique. Par conséquent, l’application cliente peut avoir besoin d’interroger à nouveau les API ci-dessus pour récupérer les dernières métadonnées de l’utilisateur.

>[!MORELIKETHIS]
>
> [FAQ sur la phase d’authentification](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
