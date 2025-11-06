---
title: Implémentation de l’API sans client - Codes d’erreur/messages avec raison/cause probable
description: Implémentation de l’API sans client - Codes d’erreur/messages avec raison/cause probable
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# Implémentation de l’API sans client (héritée) - Codes d’erreur/messages avec raison/cause probable {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>


## Erreur : non autorisé

### Causes :

1. En-tête d’autorisation manquant dans le POST
1. Problème lié à l’en-tête d’autorisation : vérifiez si le temps de requête est en millisecondes.

## Erreur : SC 400 lors de l&#39;authentification

### Causes :

1. Le serveur n’a pas trouvé le code d’enregistrement, qui a été créé pour un demandeur et un environnement spécifiques.
1. Vous pourriez rencontrer des problèmes de script entre domaines
1. Une usurpation correcte doit être ajoutée au fichier /etc/hosts

## Erreur : 400 Requête Incorrecte

### Causes :

1. URL incorrecte pour POST/GET
1. SAMLAssertionParserException - L’assertion SAML chiffrée n’a pas pu être déchiffrée à la fin de l’Adobe

## Erreur : 403 interdit

### Causes :

1. Trop de requêtes rapides : une fonctionnalité de gestion des API permettant d’empêcher les attaques par déni de service.
2. Si vous utilisez l’environnement préégal, ajoutez l’usurpation. Sinon, assurez-vous que l’usurpation a été supprimée du fichier /etc/hosts

## Erreur : impossible de se connecter à la page MVPD

### Causes :

1. Le nom d’utilisateur et le mot de passe ne correspondent pas.
2. La connexion peut avoir été désactivée
3. Vérifier si la connexion est à des fins de production ou d’évaluation


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
