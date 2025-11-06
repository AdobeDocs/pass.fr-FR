---
title: Erreur d’authentification iOS - adobepass.ios.app introuvable
description: Erreur d’authentification iOS - adobepass.ios.app introuvable
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---

# Erreur d’authentification iOS (héritée) - adobepass.ios.app introuvable {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Problème {#issue}

L’utilisateur passe par le flux d’authentification et, après avoir saisi ses informations d’identification avec son fournisseur, il est redirigé vers une page d’erreur, une page de recherche ou une autre page personnalisée l’informant que `adobepass.ios.app` n’a pas pu être trouvé/résolu.

## Explication {#explanation}

Sur iOS, `adobepass.ios.app` est utilisé comme URL de redirection finale pour indiquer que le flux AuthN est terminé. À ce stade, l’application doit envoyer une requête à AccessEnabler pour obtenir le jeton AuthN et finaliser le flux AuthN.

Le problème est que `adobepass.ios.app` n’existe pas réellement et déclenchera un message d’erreur dans le `webView`. Les versions plus anciennes de l’application de démonstration iOS supposaient que cette erreur était toujours déclenchée à la fin du flux AuthN et ont été configurées pour la gérer en conséquence (`indidFailLoadWithError`).

**Remarque :** ce problème a été corrigé dans les versions ultérieures de DemoApp (incluses avec le téléchargement iOS SDK).

Malheureusement, cette hypothèse n’est PAS correcte. Certains serveurs DNS ou proxy dits « intelligents » ne se contentent pas de transmettre l’erreur générée, mais effectuent plutôt l’une des opérations suivantes :

- Créer une page d’erreur personnalisée
- Accédez à une page de recherche ou à un autre type de page ou de portail client.

Dans ce cas, la réponse qui revient à iOS webView sera parfaitement valide en ce qui concerne webView et ne déclenchera PAS l’erreur dont dépendait l’ancienne DemoApp.

## Solution {#solution}

Ne partez PAS de la même hypothèse que celle de DemoApp. Au lieu de cela, interceptez la requête avant qu’elle ne soit exécutée (en `shouldStartLoadWithRequest`) et gérez-la correctement.

Exemple d&#39;interception de la requête avant son exécution :

```obj-c
- (BOOL)webView:(UIWebView*)localWebView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {

NSString *absolutePath = [[request URL] absoluteString]; 
if ([absolutePath isEqualToString:ADOBEPASS_REDIRECT_URL] && ![APP_DELEGATE getAuthenticationWasCalled]) {

// user was logged ok => call getAuthenticationToken() 
[APP_DELEGATE setGetAuthenticationWasCalled:YES]; 
[[APP_DELEGATE accessEnabler] getAuthenticationToken];
return NO;

}

return YES;

}
```

Quelques points à noter :

- N’utilisez JAMAIS `adobepass.ios.app` directement dans le code. Utilisez plutôt l’`ADOBEPASS_REDIRECT_URL` constante .
- L’instruction `return NO;` empêche le chargement de la page
- Assurez-vous absolument que l’appel `getAuthenticationToken` est appelé une seule fois dans votre code. Plusieurs appels à `getAuthenticationToken` entraîneront des résultats non définis.
