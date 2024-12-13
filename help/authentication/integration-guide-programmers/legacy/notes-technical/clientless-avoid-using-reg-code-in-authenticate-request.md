---
title: Évitez d’utiliser le code ’&’reg_code dans la requête /authenticate.
description: Évitez d’utiliser le code ’&’reg_code dans la requête /authenticate.
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---

# (Hérité) Évitez d’utiliser «&amp; »reg_code dans la requête /authenticate {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>



## Problème

Le navigateur IE 9 interprète &#39;\&amp;reg&#39; comme une commande spéciale et la convertit en ®.

## Explication

Si la requête `/authenticate` est composée comme suit...


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


...il sera interprété par le navigateur d’IE comme ci-dessous et sera envoyé à l’Adobe au format suivant :


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


Le demandeur\_id sera interprété comme univision®\_code=EKAFMFI, puisqu’il n’existe aucun «&amp; », et l’Adobe ne trouvera pas de paramètre `regCode` auquel associer le jeton.  Il est possible que le jeton AuthN ne soit pas créé du tout, auquel cas les appels `/checkauthn` ne trouveront aucun jeton.



## Solution

L’une des options suivantes devrait résoudre ce problème :

1. Évitez d’utiliser le paramètre `&reg_code` entre les autres paramètres de chaîne de requête.  Au lieu de cela, déplacez-la vers le premier paramètre de chaîne de requête dans l’URL de requête, de sorte que l’URL de requête ressemble à ceci :


       &lt;FQDN>authentifier?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   De cette manière, le paramètre `&reg` ne sera pas interprété de manière incorrecte.

1. Normalisez les `&reg_code` comme si vous utilisiez `&amp;reg_code`.

1. L’Adobe peut introduire une nouvelle fonctionnalité permettant de renvoyer un code d’erreur au deuxième écran en réponse à un appel d’authentification, si la création du jeton AuthN a échoué.
