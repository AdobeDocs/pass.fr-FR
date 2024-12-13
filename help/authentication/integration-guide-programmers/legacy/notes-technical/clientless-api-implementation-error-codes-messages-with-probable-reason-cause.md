---
title: Mise en œuvre de l’API sans client - Codes / Messages d’erreur avec raison/cause probable
description: Mise en œuvre de l’API sans client - Codes / Messages d’erreur avec raison/cause probable
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# (Hérité) Mise en œuvre de l’API sans client - Codes / Messages d’erreur avec raison/cause probable {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre informatif seulement. L’utilisation de cette API nécessite une licence en cours de validité Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Assurez-vous de rester informé des dernières annonces de produits Adobe Pass Authentication et des calendriers de mise hors service regroupés dans la [page Annonces de](/help/authentication/product-announcements.md) produits.

</br>


## Erreur : Non autorisé

### Causes:

1. En-tête d’autorisation manquant dans le POST
1. Problème avec l’en-tête d’autorisation - vérifiez si le délai d’exécution de la demande est en millisecondes.

## Erreur : SC 400 lors de l’authentification

### Causes:

1. Le serveur n’a pas trouvé le code d’enregistrement, qui a été créé pour un demandeur et un environnement spécifiques.
1. Vous pourriez rencontrer des problèmes de script inter-domaines
1. Une usurpation correcte doit être ajoutée au fichier /etc/hosts

## Erreur : 400 Requête Incorrecte

### Causes:

1. URL incorrecte pour POST/GET
1. SAMLAssertionParserException – L’assertion SAML chiffrée n’a pas pu être déchiffrée à la fin de Adobe.

## Erreur : 403 Forbidden

### Causes:

1. Trop de requêtes rapides - une fonctionnalité de la gestion des API pour empêcher les attaques DoS.
2. Si vous utilisez un environnement prequal, ajoutez l’usurpation d’identité, sinon assurez-vous que l’usurpation a été supprimée du fichier /etc/hosts

## Erreur : impossible de se connecter à la page MVPD

### Causes:

1. Le nom d’utilisateur et le mot de passe ne correspondent pas.
2. La connexion peut avoir été désactivée
3. Vérifiez si la connexion est destinée à la production ou à l’évaluation


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
