---
title: SDK Amazon FireOS avec enregistrement client dynamique
description: SDK Amazon FireOS avec enregistrement client dynamique
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 0%

---


# SDK Amazon FireOS avec enregistrement client dynamique (hérité) {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>

## <span id=""></span>Introduction {#Intro}

FireOS AccessEnabler SDK for FireTV a été modifié pour activer l’authentification sans utiliser de cookies de session. Comme de plus en plus de navigateurs restreignent l’accès aux cookies, une autre méthode était nécessaire pour autoriser l’authentification.

**FireOS SDK 3.0.4** remplace le mécanisme actuel d’enregistrement de l’application basé sur l’ID du demandeur signé et l’authentification par cookie de session par [Présentation de l’enregistrement du client dynamique](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


## Modifications d’API {#API}

### Factory.getInstance

**Description :** instancie l&#39;objet Access Enabler. Il doit y avoir une seule instance Access Enabler par instance d&#39;application.

| Appel API : constructeur |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        renvoie AccessEnablerException |

**Disponibilité :** v3.0+

**Paramètres:**

- *appContext* : contexte de l’application Android
- *softwareStatement* : valeur obtenue à partir du tableau de bord TVE ou *null* si « software\_statement » est défini dans strings.xml
- *redirectUrl* : pour les implémentations FireTV, ce paramètre doit être nul. Tous les paramètres de cet attribut seront ignorés.

**Notes**

- softwareStatement non valide empêchera l&#39;application d&#39;initialiser AccessEnabler ou d&#39;enregistrer l&#39;application pour l&#39;authentification et l&#39;autorisation Adobe Pass
- Le paramètre redirectUrl pour FireTV est défini par le SDK sur adobepass://android.app car l’authentification est gérée par une instance AccessEnabler unique.

### setRequestor

**Description :** établit l’identité du canal. Chaque canal se voit attribuer un identifiant unique lors de l’enregistrement auprès de l’Adobe pour le système d’authentification Adobe Pass. Lorsque vous traitez avec des jetons SSO et distants, l&#39;état d&#39;authentification peut changer lorsque l&#39;application est en arrière-plan, setRequestor peut être appelé à nouveau lorsque l&#39;application est mise en premier plan afin de se synchroniser avec l&#39;état du système (récupérer un jeton distant si SSO est activé ou supprimer le jeton local si une déconnexion s&#39;est produite en attendant).

La réponse du serveur contient une liste de fichiers MVPD ainsi que des informations de configuration liées à l’identité du canal. La réponse du serveur est utilisée en interne par le code d’activation d’Access. Seul le statut de l’opération (c’est-à-dire SUCCÈS/ÉCHEC) est présenté à votre application via le rappel setRequestorComplete().

Si le paramètre *urls* n’est pas utilisé, l’appel réseau qui en résulte cible l’URL du fournisseur de services par défaut : l’environnement de production de publication d’Adobe.

Si une valeur est fournie pour le paramètre *urls*, l’appel réseau résultant cible toutes les URL fournies dans le paramètre *urls*. Toutes les demandes de configuration sont déclenchées simultanément dans des threads distincts. Le premier répondant est prioritaire lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, Access Enabler mémorise l’URL du fournisseur d’accès associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.

| Appel API : configuration du demandeur |
| --- |
| public void setRequestor(String requestorId) |

**Disponibilité :** v3.0+

| Appel API : configuration du demandeur |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Disponibilité :** v3.0+

**Paramètres:**

- *requestorID* : ID unique associé au canal. Transmettez l’ID unique attribué par Adobe à votre site lors de votre premier enregistrement auprès du service d’authentification d’Adobe Pass.
- *urls* : paramètre facultatif ; par défaut, le fournisseur d’accès Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points d’entrée pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances de fournisseur de services d’authentification Adobe Pass. Ce faisant, la liste MVPD est composée des points d’entrée de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.

Obsolète :

- *signedRequestorID* : copie de l’ID du demandeur signé numériquement avec votre clé privée. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Rappels déclenchés :** `setRequestorComplete()`

</br>

### déconnexion

**Description :** utilisez cette méthode pour lancer le flux de déconnexion. La déconnexion est le résultat d’une série d’opérations de redirection HTTP en raison du fait que l’utilisateur doit être déconnecté des serveurs d’authentification Adobe Pass et des serveurs de MVPD. Par conséquent, ce flux ouvre une fenêtre ChromeCustomTab pour exécuter la déconnexion.

| Appel API : lancement du flux de déconnexion |
| --- |
| public void logout() |

**Disponibilité :** v3.0+

**Paramètres:** Aucun

**Rappels déclenchés :** `setAuthenticationStatus()`

## Flux de mise en œuvre du programmeur {#Progr}

### **1. Enregistrer l’application**

1. Obtenir le logiciel\_instruction auprès d’Adobe Pass ( Tableau de bord TVE )
1. Il existe deux options pour transmettre ces valeurs à Adobe Pass SDK :
   - Dans strings.xml, ajoutez :

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - Appeler AccessEnabler.getInstance(appContext,softwareStatement, null)



### **2. Configurez l’application**

- a. setRequestor(requestor\_id)

  Le SDK va effectuer les opérations suivantes :

   - enregistrer l’application : à l’aide de **software\_statement**, le SDK obtiendra un **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**. Ces informations seront stockées dans le stockage interne de l’application.
   - obtenez un **access\_token** à l’aide de client\_id, client\_secret et grant\_type=« client\_credentials » . Ce jeton d’accès sera utilisé à chaque appel effectué par le SDK vers les serveurs Adobe Pass.

| Réponses d&#39;erreur de jeton : |  |  |
|--- | --- | --- |
| HTTP 400 (requête incorrecte) | {« error »: « invalid\_request »} | Il manque un paramètre obligatoire à la requête, la requête inclut une valeur de paramètre non prise en charge (autre que le type d’octroi), la requête répète un paramètre, inclut plusieurs informations d’identification, utilise plusieurs mécanismes pour authentifier le client ou est mal formée pour d’autres raisons. |
| HTTP 400 (requête incorrecte) | {« error »: « invalid\_client »} | Échec de l&#39;authentification du client car le client était inconnu. Le SDK *MUST* s’enregistre à nouveau auprès du serveur d’autorisation. |
| HTTP 400 (requête incorrecte) | {« error »: « authorized\_client »} | Le client authentifié n’est pas autorisé à utiliser ce type d’octroi d’autorisation. |

- si un MVPD nécessite une authentification passive, un WebView s’ouvre pour exécuter une authentification passive avec ce MVPD et se ferme une fois l’opération terminée

- b. checkAuthentication()

   - *true* : accédez à Autorisation .
   - *false* : accédez à Sélectionner le MVPD .

- c. getAuthentication : le SDK inclura **access_token** dans les paramètres d’appel

   - mvpd mémorisé : accéder à setSelectedProvider(mvpd\_id)
   - mvpd non sélectionné : displayProviderDialog
   - mvpd selected : accédez à setSelectedProvider(mvpd\_id).

- d. setSelectedProvider

   - L’URL d’authentification mvpd\_id est chargée dans ChromeCustomTabs.
   - connexion réussie : delegate.setAuthenticationStatus ( SUCCESS )
   - connexion annulée : réinitialiser la sélection MVPD
   - Le schéma d’URL est établi sous la forme « adobepass://android.app » pour capturer une fois l’authentification terminée

- e. get/checkAuthorization : le SDK inclura **access\_token** dans l’en-tête en tant qu’autorisation : porteur **access\_token**

- si l’autorisation est accordée, un appel est lancé pour obtenir le jeton du média

- f. déconnexion :

   - SDK supprimera le jeton valide pour le demandeur actuel (les authentifications obtenues par d’autres applications et non par SSO resteront valides).
   - SDK ouvrira les onglets personnalisés Chrome pour atteindre le point d’entrée de connexion mvpd\_id. Une fois l’opération terminée, les onglets personnalisés de Chrome seront fermés
   - Le schéma d’URL est établi en tant que « adobepass://logout » pour capturer le moment où la déconnexion est terminée
   - la déconnexion déclenche une action sendTrackingData(new Event(EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR) et un rappel : setAuthenticationStatus(0,« Logout »)



**Remarque :** comme chaque appel nécessite un **access_token**, les codes d’erreur possibles ci-dessous sont gérés dans le SDK.

| Réponses d’erreur |  |  |
|--- | --- | --- |
| invalid_request | 400 | La requête est incorrecte. Le SDK doit arrêter d’effectuer des appels au serveur . |
| invalid_client | 403 | L’identifiant client n’est plus autorisé à effectuer des requêtes. Le sdk DOIT à nouveau effectuer l’enregistrement du client. |
| access_deny | 401 | Le jeton access_token n&#39;est pas valide. Le sdk DOIT demander un nouveau jeton access_token. |
