---
title: Jetons de média
description: Jetons de média
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# Jetons de média {#media-tokens}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Le jeton multimédia est un jeton généré par l’authentification Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) à la suite d’une décision d’autorisation censée fournir un accès en lecture seule au contenu protégé (ressource). Le jeton de média est valide pendant une période limitée et courte (quelques minutes) spécifiée au moment de l’émission, indiquant la durée pendant laquelle il doit être vérifié et utilisé par l’application cliente.

Le jeton de média se compose d’une chaîne signée basée sur l’infrastructure à clé publique (PKI) envoyée en texte clair. Avec la protection basée sur l’ICP, le jeton est signé à l’aide d’une clé asymétrique émise à l’Adobe par une autorité de certification (CA).

Le jeton multimédia est transmis au programmeur, qui peut ensuite le valider à l’aide du vérificateur de jeton multimédia avant de démarrer le flux vidéo afin de garantir la sécurité de l’accès à cette ressource.

Le vérificateur de jeton multimédia est une bibliothèque distribuée par l’authentification Adobe Pass qui est chargée de vérifier l’authenticité d’un jeton multimédia.

## Vérificateur de jeton de média {#media-token-verifier}

L’authentification Adobe Pass recommande aux programmeurs d’envoyer le jeton multimédia à un service principal intégrant la bibliothèque du vérificateur de jeton multimédia pour garantir un accès sécurisé avant de lancer le flux vidéo. La durée de vie (TTL) du jeton multimédia est conçue pour tenir compte des problèmes potentiels de synchronisation des horloges entre le serveur de génération de jetons et le serveur de validation.

L’authentification Adobe Pass déconseille vivement d’analyser le jeton multimédia et d’extraire directement ses données, car le format du jeton n’est pas garanti et peut changer à l’avenir. La bibliothèque du vérificateur de jeton de média doit être le seul outil utilisé pour analyser le contenu du jeton.

La bibliothèque du vérificateur de jetons multimédia peut être téléchargée à l’aide du lien suivant :

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

La bibliothèque du vérificateur de jeton de média nécessite la version 1.5 du JDK ou une version ultérieure et prend en charge l’utilisation d’un fournisseur préféré d’extension de chiffrement Java (JCE) pour l’algorithme de signature (`SHA256WithRSA`).

La bibliothèque du vérificateur de jetons multimédia représentée par l’archive Java `mediatoken-verifier-VERSION.jar` comprend les éléments suivants :

* Adobe de la clé publique.
* API de vérification des jetons (`ITokenVerifier.java`).
* Implémentation de référence (`com.adobe.entitlement.test.EntitlementVerifierTest.java`).
* Dépendances et fichiers de stockage des clés de certificat.

>[!IMPORTANT]
> 
> Le mot de passe par défaut du fichier de stockage des clés de certificat inclus est `123456`.

### Méthodes {#methods}

La classe `ITokenVerifier` définit les méthodes suivantes :

* Méthode `isValid()` utilisée pour valider le jeton de média. Elle accepte un seul argument, l’[identifiant de ressource](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier). Si l’identifiant de ressource fourni est `null`, la méthode valide uniquement l’authenticité et la période de validité du jeton multimédia.

  La méthode `isValid()` renvoie l’une des valeurs de statut suivantes :

  | VALID_TOKEN | Validations des jetons réussies |
  |----------------------|-------------------------------------------|
  | INVALID_TOKEN_FORMAT | Format de jeton non valide |
  | INVALID_SIGNATURE | Impossible de valider l’authenticité du jeton |
  | TOKEN_EXPIRED | TTL de jeton non valide |
  | INVALID_RESOURCE_ID | Jeton non valide pour la ressource donnée |
  | ERROR_UNKNOWN | Le jeton n’a pas encore été validé |

* Méthode `getResourceID()` utilisée pour récupérer l’identifiant de ressource associé au jeton de média et le comparer à l’identifiant renvoyé par la réponse de décision d’autorisation.

* Méthode `getTimeIssued()` utilisée pour récupérer l’heure d’émission du jeton de média.

* Méthode `getTimeToLive()` utilisée pour récupérer la durée de vie du jeton de média.

* Méthode `getUserSessionGUID()` utilisée pour récupérer un GUID anonymisé défini par le MVPD.

* Méthode `getMvpdId()` utilisée pour récupérer l’identifiant du MVPD qui a authentifié l’utilisateur.

* Méthode `getProxyMvpdId()` utilisée pour récupérer l’identifiant du MVPD proxy qui a authentifié l’utilisateur.

### Sample {#sample}

L’archive du vérificateur de jeton de média contient une implémentation de référence (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) et un exemple d’appel de l’API avec la classe de test. Cet exemple (`com.adobe.entitlement.text.EntitlementVerifierTest.java`) illustre l’intégration de la bibliothèque du vérificateur de jetons multimédia dans un serveur de médias.

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## API REST V2 {#rest-api-v2}

Le jeton de média peut être récupéré à l’aide de l’API suivante :

* [Récupérer des décisions d’autorisation à l’aide de mvpd spécifiques](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Reportez-vous aux sections **Réponse** et **Exemples** de l’API ci-dessus pour comprendre la structure des décisions d’autorisation et des jetons multimédia.

Pour plus d’informations sur comment et à quel moment intégrer l’API ci-dessus, reportez-vous au document suivant :

* [Flux d’autorisation de base exécuté dans l’application principale](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!IMPORTANT]
>
> L’application cliente doit transmettre la valeur `serializedToken` du `token` renvoyé au [vérificateur de jeton multimédia](#media-token-verifier) pour validation.
