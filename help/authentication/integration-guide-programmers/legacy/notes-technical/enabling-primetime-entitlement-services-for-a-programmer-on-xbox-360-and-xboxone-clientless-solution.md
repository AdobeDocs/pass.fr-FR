---
title: Activation des services Adobe Pass Entitlement pour un programmeur sur Xbox 360 et XboxOne sans client
description: Activation des services Adobe Pass Entitlement pour un programmeur sur Xbox 360 et XboxOne sans client
exl-id: ff7254de-9ea4-4c27-a186-d1c2eea12222
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# (Hérité) Activation des services de droits Adobe Pass pour un programmeur sur Xbox 360 et XboxOne sans client {#enabling-primetime-entitlement-services-for-a-programer-on-xbox-360-and-xboxone-clientless}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).


1. Le programmeur crée un ticket Zendesk pour activer Xbox 360/One pour la solution sans client d’authentification Adobe Pass en fournissant les informations suivantes :

   1. Plateforme : ex : Xbox 360, Xbox One

   1. ID du demandeur : par exemple netgeo, CNN, etc.

1. Adobe crée les certificats X509 et configure la clé privée et le mot de passe à son extrémité.

1. Adobe fournira le certificat public (du certificat X509) au programmeur dans le ticket ou par e-mail.

1. Le programmeur devra ensuite installer ce certificat public sur le portail GDNP pour l’application enregistrée auprès de Microsoft.

1. Le programmeur demandera alors le jeton JWT (Java Web Token) ou STS pour XboxOne ou 360 respectivement auprès du service Microsoft Xbox Live, qui serait chiffré à l&#39;aide du certificat public X509 fourni à l&#39;étape 3.

1. Il s’agit des jetons qui contiennent l’ID d’appareil unique pour les appareils Xbox. Insérez le jeton (JWT ou STS) dans l’en-tête d’autorisation à l’aide d’un paramètre « x » comme ci-dessous :

   1. Pour la Xbox 360, le jeton XSTS doit être codé en Base64 avant d&#39;être envoyé à l&#39;authentification de la télévision payante Adobe Pass.
   1. Pour Xbox One, le JWT est déjà correctement encodé, aucun encodage supplémentaire ne doit donc se produire.

1. Tous les appels API de l’appareil Xbox doivent contenir l’en-tête d’autorisation avec le jeton mentionné ci-dessus dans un paramètre .



>[!NOTE]
>
>La Xbox en particulier a des exigences uniques liées à la signature numérique. L’ID d’appareil de la console XBox est inclus dans le jeton XSTS.  Pour la Xbox 360, il s’agit d’une assertion SAML chiffrée ; pour la Xbox One, il s’agit d’un JWT chiffré. L’application de console XBox envoie l’intégralité du jeton XSTS à l’authentification de télévision payante Adobe Pass. L’authentification de la télévision payante Adobe Pass déchiffre le jeton à l’aide de sa clé publique, analyse le jeton et en extrait l’ID d’appareil.

>[!NOTE]
>
>En raison de la grande longueur du jeton XSTS, la console XBox présente une limitation technique : elle ne peut pas envoyer le jeton en tant que paramètre HTTP GET aux API d’authentification de télévision à péage d’Adobe Pass. Pour gérer ce problème, l’authentification de la télévision payante Adobe Pass permet d’envoyer le jeton XSTS dans le cadre de l’en-tête HTTP « Authorization » lors de l’appel des API. Le jeton XSTS doit être chiffré à l’aide de la clé publique du certificat X.509 émis au programmeur par l’authentification de la télévision payante Adobe Pass. L’authentification de la télévision payante Adobe Pass stocke la clé privée associée et l’utilise pour déchiffrer le jeton XSTS et en extraire l’ID d’appareil.
