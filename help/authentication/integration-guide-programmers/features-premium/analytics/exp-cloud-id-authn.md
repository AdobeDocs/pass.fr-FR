---
title: Utilisation de l’Experience Cloud ID dans l’authentification Adobe Pass
description: Utilisation de l’Experience Cloud ID dans l’authentification Adobe Pass
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

# Utilisation de l’Experience Cloud ID dans l’authentification Adobe Pass

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

## Qu’est-ce qu’Experience Cloud ID et comment l’obtenir ? {#what-exp-cloud-id-obtain}

L’Experience Cloud ID (ECID, en abrégé) est un identifiant unique généré par Adobe Experience Cloud pour chaque utilisateur de votre application/site web. L’ECID est fortement utilisé dans tous les rapports Experience Cloud utilisés pour lier les informations sur un utilisateur spécifique dans plusieurs applications/sites web.

Si vous avez déjà mis en place un système qui fournit un identifiant visiteur, vous devez utiliser le même identifiant pour la portée de ce document.

L’une des manières d’obtenir l’ECID consiste à utiliser le service Experience Cloud ID. Vous pouvez utiliser le type d’implémentation de votre choix, basé sur TDM, la bibliothèque JS, le côté serveur, l’intégration directe ou les bibliothèques natives pour les plateformes mobiles. Pour une vue d’ensemble complète des services, bibliothèques, guides de SDK et de mise en œuvre disponibles, consultez : <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html>

## Quel est l’avantage d’utiliser l’Experience Cloud ID dans l’authentification Adobe Pass ? {#benefit-ex-cloud-id}

Si vous configurez nos SDK et l’API REST sans client pour utiliser votre ECID, vous pourrez plus tard lier les données collectées par l’authentification Adobe Pass à vos solutions Experience Cloud existantes. Vous pourrez ainsi mieux comprendre le parcours et l’expérience de vos clients dans toutes les solutions fournies par Adobe.

## Comment utiliser l’Experience Cloud ID dans l’authentification Adobe Pass ? {#how-to-ex-cloud-id-authn}

Après avoir obtenu l’ECID (expliqué ci-dessus), vous devez transmettre ces informations à nos SDK et à notre API REST sans client. Ces informations seront ensuite transmises à nos serveurs à chaque appel réseau effectué par le SDK. Le processus de configuration est différent pour chaque SDK, comme suit :

### SDK JS {#js-sdk}

Pour JavaScript, vous devez transmettre l’ECID dans une carte en tant que troisième paramètre à l’appel setRequestor.

**Exemple d’utilisation :**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### SDK iOS/tvOS {#ios-sdk}

Pour iOS/tvOS SDK, il existe une méthode dédiée appelée setOptions.

**Exemple d’utilisation :**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Pour Android/fireTV SDK, le mécanisme est similaire à celui d’iOS. Seul le nom du paramètre est différent. L’API est documentée ici.

**Exemple d’utilisation :**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### API sans client {#clientless-api}

Lors de l’utilisation d’Adobe Pass via son API REST v1, la valeur **ECID** doit être envoyée **sur toutes les API** en tant que paramètre nommé **&#39;ap_vi&#39;**.

**Exemple d’utilisation :**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`

### API REST V2 {#rest-api-v2}

Lors de l’utilisation d’Adobe Pass via son API REST v2, la valeur **ECID** doit être envoyée **sur toutes les API** en tant qu’en-tête nommé **&#39;AP-Visitor-Identifier&#39;**.

**Exemple d’utilisation :**

`POST: https://api.auth.adobe.com/api/v2/${serviceProvider}/sessions/`\
En-têtes :\
`AP-Visitor-Identifier: THE_ECID_VALUE`

