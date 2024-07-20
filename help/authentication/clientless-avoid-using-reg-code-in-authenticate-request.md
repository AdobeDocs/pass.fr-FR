---
title: Évitez d’utiliser '&'reg_code dans /authenticRequest
description: Évitez d’utiliser '&'reg_code dans /authenticRequest
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Évitez d’utiliser &#39;&amp;&#39;reg_code dans /authenticRequest {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>



## Problème

Le navigateur IE 9 interprète &quot;\&amp;reg&quot; comme une commande spéciale et la convertit en ®.

## Explication

Si la requête `/authenticate` est composée comme suit...


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


...il sera interprété par le navigateur IE comme ci-dessous et envoyé à Adobe dans ce format :


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


Le demandeur\_id sera interprété comme univision®\_code=EKAFMFI, puisqu’il n’existe pas de &#39;&amp;&#39; et que l’Adobe ne trouvera pas de paramètre `regCode` avec lequel associer le jeton.  Il est possible que le jeton AuthN ne soit pas créé du tout, auquel cas les appels `/checkauthn` ne trouveront aucun jeton.



## Solution

L’une des options suivantes devrait résoudre ce problème :

1. Évitez d’utiliser le paramètre `&reg_code` entre les autres paramètres de chaîne de requête.  Au lieu de cela, déplacez-le vers le premier paramètre de chaîne de requête de l’URL de requête, en rendant l’URL de requête comme suit :


       &lt;FQDN>authentifier?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   De cette manière, le paramètre `&reg` ne sera pas interprété incorrectement.

1. Normalisez `&reg_code` en utilisant `&amp;reg_code`.

1. Adobe peut introduire une nouvelle fonctionnalité pour renvoyer un code d’erreur au 2e écran en réponse à un appel d’authentification, si la création du jeton AuthN a échoué.
