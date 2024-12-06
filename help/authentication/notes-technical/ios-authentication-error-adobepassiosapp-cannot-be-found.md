---
title: Erreur d’authentification iOS - Impossible de trouver adobepass.ios.app
description: Erreur d’authentification iOS - Impossible de trouver adobepass.ios.app
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Erreur d’authentification iOS - adobepass.ios.app introuvable {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Problème {#issue}

L’utilisateur passe par le flux d’authentification et une fois qu’il a saisi ses informations d’identification avec son fournisseur, il est redirigé vers une page d’erreur, une page de recherche ou une autre page personnalisée l’informant que `adobepass.ios.app` n’a pas pu être trouvé/résolu.

## Explication {#explanation}

Sur iOS, `adobepass.ios.app` est utilisé comme URL de redirection finale pour indiquer que le flux AuthN est terminé. À ce stade, l’application doit adresser une requête à AccessEnabler pour obtenir le jeton AuthN et finaliser le flux AuthN.

Le problème est que `adobepass.ios.app` n&#39;existe pas réellement et déclenchera un message d&#39;erreur dans le `webView`. Les anciennes versions de DemoApp d’iOS supposaient que cette erreur serait toujours déclenchée à la fin du flux AuthN et ont été configurées pour la gérer en conséquence (`indidFailLoadWithError`).

**Remarque :** Ce problème a été corrigé dans les versions ultérieures de DemoApp (incluses avec le téléchargement du SDK iOS).

Malheureusement, cette hypothèse n&#39;est PAS correcte. Il existe des serveurs DNS ou proxy &quot;intelligents&quot; qui ne se contentent pas de transmettre l’erreur générée, mais qui effectuent à la place l’une des opérations suivantes :

- Création d’une page d’erreur personnalisée
- Transférez vers une page de recherche ou tout autre type de page client ou de portail.

Dans ces cas, la réponse qui revient au webView d’iOS sera une réponse parfaitement valide en ce qui concerne le webView et NE déclenchera PAS l’erreur dont dépendait l’ancienne DemoApp.

## Solution {#solution}

Ne prenez PAS la même hypothèse que celle de DemoApp. Au lieu de cela, interceptez la requête avant qu’elle ne soit exécutée (dans `shouldStartLoadWithRequest`) et gérez-la de manière appropriée.

Exemple d’interception de la requête avant son exécution :

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

Quelques éléments à noter :

- N’utilisez JAMAIS `adobepass.ios.app` directement n’importe où dans le code. Utilisez plutôt la constante `ADOBEPASS_REDIRECT_URL`
- L’instruction `return NO;` empêchera le chargement de la page.
- Assurez-vous absolument que l’appel `getAuthenticationToken` est appelé une seule fois et une seule fois dans votre code. Plusieurs appels à `getAuthenticationToken` auront des résultats non définis.
