---
title: SDK Amazon FireOS avec enregistrement du client dynamique
description: SDK Amazon FireOS avec enregistrement du client dynamique
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 0%

---


# SDK Amazon FireOS avec enregistrement du client dynamique {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

## <span id=""></span> Introduction {#Intro}

Le SDK FireOS AccessEnabler pour FireTV a été modifié pour activer l’authentification sans utiliser de cookies de session. Comme de plus en plus de navigateurs restreignent l’accès aux cookies, une autre méthode était nécessaire pour autoriser l’authentification.

**Le SDK FireOS 3.0.4** remplace le mécanisme d’enregistrement actuel de l’application en fonction de l’ID de demandeur signé et de l’authentification de cookie de session par [Présentation de l’enregistrement du client dynamique](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


## Modifications des API {#API}

### Factory.getInstance

**Description :** instancie l’objet Access Enabler. Il doit y avoir une instance Access Enabler unique par instance d’application.

| Appel API : constructeur |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        renvoie AccessEnablerException |

**Disponibilité :** v3.0+

**Paramètres :**

- *appContext* : contexte de l’application Android
- *softwareStatement* : valeur obtenue à partir du tableau de bord TVE ou *null* si &quot;software\_statement&quot; est défini dans strings.xml
- *redirectUrl* : pour les implémentations FireTV, ce paramètre doit être nul. Tous les paramètres de cet attribut seront ignorés.

**Notes**

- L’instruction logicielle non valide empêchera l’application d’initialiser AccessEnabler ou d’enregistrer l’application pour l’authentification et l’autorisation Adobe Pass.
- Le paramètre redirectUrl pour FireTV est défini par le SDK sur adobepass://android.app, car l’authentification est gérée par l’instance AccessEnabler unique.

### setRequestor

**Description :** Établit l’identité du canal. Un identifiant unique est attribué à chaque canal lors de l’enregistrement auprès d’Adobe pour le système d’authentification Adobe Pass. Lorsque vous traitez de SSO et de jetons distants, l’état d’authentification peut changer lorsque l’application est en arrière-plan, setRequestor peut être appelé de nouveau lorsque l’application est mise en premier plan afin de se synchroniser avec l’état du système (récupération d’un jeton distant si SSO est activé ou suppression du jeton local si une déconnexion s’est produite entre-temps).

La réponse du serveur contient une liste de MVPD ainsi que certaines informations de configuration jointes à l’identité du canal. La réponse du serveur est utilisée en interne par le code Access Enabler. Seul l’état de l’opération (c.-à-d. SUCCESS/FAIL) est présenté à votre application via le rappel setRequestorComplete() .

Si le paramètre *urls* n’est pas utilisé, l’appel réseau obtenu cible l’URL du fournisseur de services par défaut : l’environnement de production de version d’Adobe.

Si une valeur est fournie pour le paramètre *urls* , l’appel réseau obtenu cible toutes les URL fournies dans le paramètre *urls* . Toutes les requêtes de configuration sont déclenchées simultanément dans des threads distincts. Le premier participant a la priorité lors de la compilation de la liste des MVPD. Pour chaque MVPD de la liste, Access Enabler mémorise l&#39;URL du prestataire associé. Toutes les demandes de droits suivantes sont dirigées vers l’URL associée au fournisseur de services qui a été associé au MVPD cible pendant la phase de configuration.

| Appel API : configuration du demandeur |
| --- |
| public void setRequestor(String requestorId) |

**Disponibilité :** v3.0+

| Appel API : configuration du demandeur |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Disponibilité :** v3.0+

**Paramètres :**

- *requestorID* : identifiant unique associé au canal. Transmettez l’identifiant unique attribué par Adobe à votre site lorsque vous vous inscrivez pour la première fois auprès du service d’authentification Adobe Pass.
- *urls* : paramètre facultatif ; par défaut, le fournisseur de services Adobe est utilisé (http://sp.auth.adobe.com/). Ce tableau vous permet de spécifier des points de terminaison pour les services d’authentification et d’autorisation fournis par Adobe (différentes instances peuvent être utilisées à des fins de débogage). Vous pouvez l’utiliser pour spécifier plusieurs instances du fournisseur de services d’authentification Adobe Pass. Dans ce cas, la liste MVPD est composée des points de terminaison de tous les fournisseurs de services. Chaque MVPD est associé au fournisseur de services le plus rapide, c’est-à-dire le fournisseur qui a répondu en premier et qui prend en charge ce MVPD.

Obsolète :

- *signedRequestorID* : copie de l’ID du demandeur signé numériquement avec votre clé privée. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Rappels déclenchés :** `setRequestorComplete()`

</br>

### déconnexion

**Description :** Utilisez cette méthode pour lancer le flux de déconnexion. La déconnexion est le résultat d’une série d’opérations de redirection HTTP en raison du fait que l’utilisateur doit être déconnecté des serveurs d’authentification Adobe Pass et des serveurs du MVPD. Par conséquent, ce flux ouvre une fenêtre ChromeCustomTab pour exécuter la déconnexion.

| Appel API : lancer le flux de déconnexion |
| --- |
| public void logout() |

**Disponibilité :** v3.0+

**Paramètres :** Aucun

**Rappels déclenchés :** `setAuthenticationStatus()`

## Flux de mise en oeuvre du programmeur {#Progr}

### **1. Enregistrer l’application**

1. Procurez-vous l’instruction logicielle\_d’Adobe Pass ( tableau de bord TVE )
1. Il existe deux options pour transmettre ces valeurs au SDK Adobe Pass :
   - Dans strings.xml, ajoutez :

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - Appelez AccessEnabler.getInstance(appContext,softwareStatement, null)



### **2. Configuration de l’application**

- a. setRequestor(requestor\_id)

  Le SDK effectue les opérations suivantes :

   - application d’enregistrement : à l’aide de **software\_statement**, le SDK obtient un **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**. Ces informations seront stockées dans le stockage interne de l&#39;application.
   - obtenez un **access\_token** en utilisant client\_id, client\_secret et grant\_type=&quot;client\_credentials&quot; . Cet access\_token sera utilisé pour chaque appel effectué par le SDK aux serveurs Adobe Pass.

| Réponses d’erreur de jeton : |  |  |
|--- | --- | --- |
| HTTP 400 (Requête incorrecte) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Un paramètre requis est manquant dans la requête, inclut une valeur de paramètre non prise en charge (autre que le type d’octroi), répète un paramètre, inclut plusieurs informations d’identification, utilise plusieurs mécanismes pour authentifier le client ou est incorrectement formé. |
| HTTP 400 (Requête incorrecte) | {&quot;error&quot;: &quot;invalid\_client&quot;} | L’authentification du client a échoué car le client était inconnu. Le SDK *MUST* s’enregistre à nouveau auprès du serveur d’autorisation. |
| HTTP 400 (Requête incorrecte) | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | Le client authentifié n’est pas autorisé à utiliser ce type d’autorisation. |

- si un MVPD nécessite une authentification passive, un WebView s’ouvre pour s’exécuter de manière passive avec ce MVPD et se ferme une fois terminé.

- b. checkAuthentication()

   - *true* : accédez à Autorisation
   - *false* : accédez à Sélectionner MVPD

- c. getAuthentication : le SDK inclura **access_token** dans les paramètres d’appel

   - mvpd mémorisé : accédez à setSelectedProvider(mvpd\_id)
   - mvpd non sélectionné : displayProviderDialog
   - mvpd selected : accédez à setSelectedProvider(mvpd\_id)

- d. setSelectedProvider

   - L’URL d’authentification mvpd\_id est chargée dans ChromeCustomTabs
   - connexion réussie : delegate.setAuthenticationStatus ( SUCCESS )
   - connexion annulée : réinitialisation de la sélection MVPD
   - Le schéma d’URL est défini sur &quot;adobepass://android.app&quot; pour capturer une fois l’authentification terminée.

- e. get/checkAuthorization : le SDK inclura **access\_token **dans l’en-tête Authorization: Bearer **access\_token{1)**

- si l’autorisation a réussi, un appel est effectué pour obtenir le jeton multimédia

- f. logout :

   - Le SDK supprime le jeton valide pour le demandeur actuel (les authentifications obtenues par d’autres applications et non par SSO resteront valides)
   - Le SDK ouvre Chrome Custom Onglets pour atteindre le point de terminaison mvpd\_id logout . Une fois l’opération terminée, les onglets personnalisés de Chrome sont fermés.
   - Le schéma d’URL est défini sur &quot;adobepass://logout&quot; pour capturer le moment où la déconnexion est terminée.
   - La déconnexion déclenche un sendTrackingData(new Event(EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR) et un rappel : setAuthenticationStatus(0,&quot;Logout&quot;).



**Remarque :** comme chaque appel nécessite un **access_token**, les codes d’erreur possibles ci-dessous sont gérés dans le SDK.

| Réponses d’erreur |  |  |
|--- | --- | --- |
| invalid_request | 400 | La requête est incorrecte. Le SDK doit cesser d’effectuer des appels au serveur. |
| invalid_client | 403 | L’ID client n’est plus autorisé à effectuer des requêtes. Le sdk DOIT effectuer à nouveau l’enregistrement du client. |
| access_denied | 401 | La valeur access_token n’est pas valide. Le sdk DOIT demander un nouveau access_token. |
